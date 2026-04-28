# CUDA Streams and Concurrency

**Keywords**: cuda streams concurrency,gpu stream programming,cuda concurrent execution,asynchronous cuda operations,cuda stream synchronization

---

**CUDA Streams and Concurrency** is **the programming model that enables overlapping of kernel execution, memory transfers, and host operations through asynchronous task queues** — where streams provide independent execution contexts that allow multiple kernels to run simultaneously on different SMs, memory copies to overlap with computation, and CPU code to continue while GPU works, achieving 2-4× throughput improvement through concurrent execution of independent operations, making streams essential for maximizing GPU utilization in production applications where naive sequential execution leaves 50-80% of GPU resources idle and proper stream management can saturate all available hardware resources.


**Stream Fundamentals:**
- **Stream Definition**: sequence of operations that execute in order; operations in different streams can execute concurrently; default stream (stream 0) serializes with all other streams; explicit streams enable concurrency
- **Stream Creation**: cudaStreamCreate(&stream) creates stream; cudaStreamDestroy(stream) destroys; lightweight objects; create once, reuse many times; typical applications use 2-8 streams
- **Asynchronous Operations**: kernel launches, memory copies (cudaMemcpyAsync), memory sets; return immediately to host; GPU executes asynchronously; enables CPU-GPU overlap
- **Synchronization**: cudaStreamSynchronize(stream) waits for stream completion; cudaDeviceSynchronize() waits for all streams; cudaStreamQuery() checks completion without blocking

**Concurrent Kernel Execution:**
- **Multi-SM Utilization**: modern GPUs have 80-132 SMs (A100: 108, H100: 132); single kernel may not saturate all SMs; concurrent kernels from different streams utilize idle SMs
- **Kernel Concurrency Limits**: limited by SM resources (registers, shared memory); small kernels (low resource usage) enable more concurrency; 2-8 concurrent kernels typical
- **Launch Configuration**: smaller grid sizes enable more concurrency; balance between kernel efficiency and concurrency; profile to find optimal
- **Throughput Improvement**: 2-4× throughput with concurrent kernels vs sequential; depends on resource usage and kernel characteristics

**Overlapping Compute and Memory:**
- **Copy Engines**: separate DMA engines for memory transfers; 2 copy engines on modern GPUs (H2D and D2H); operate independently from compute SMs
- **Async Memory Copy**: cudaMemcpyAsync() with pinned host memory; overlaps with kernel execution in different stream; hides transfer latency
- **Pinned Memory**: cudaHostAlloc() or cudaMallocHost(); required for async transfers; 2-10× faster than pageable memory; limited resource (system RAM)
- **Overlap Pattern**: stream 1 copies data while stream 2 computes; pipeline stages; 30-60% throughput improvement; critical for data-intensive applications

**Stream Priorities:**
- **Priority Levels**: cudaStreamCreateWithPriority(); higher priority streams scheduled first; range from -1 (high) to 0 (normal); useful for latency-critical kernels
- **Use Cases**: interactive rendering (high priority) vs background computation (normal); real-time inference (high) vs batch training (normal)
- **Scheduling**: GPU scheduler favors high-priority streams; doesn't guarantee preemption; best-effort scheduling
- **Performance Impact**: 10-30% latency reduction for high-priority streams; depends on workload mix

**Stream Synchronization:**
- **Events**: cudaEventCreate(), cudaEventRecord(event, stream), cudaEventSynchronize(event); lightweight synchronization; measure elapsed time
- **Stream Wait Event**: cudaStreamWaitEvent(stream, event); stream waits for event from another stream; enables inter-stream dependencies
- **Host Callbacks**: cudaLaunchHostFunc() executes CPU function when stream reaches callback; useful for logging, notifications
- **Implicit Synchronization**: cudaMemcpy() (non-async), cudaMalloc(), cudaFree() synchronize all streams; avoid in performance-critical code

**Pipeline Patterns:**
- **Three-Stage Pipeline**: stage 1 copies H2D, stage 2 computes, stage 3 copies D2H; three streams; each stage processes different data batch; 2-3× throughput vs sequential
- **Depth**: pipeline depth = number of concurrent stages; deeper pipelines (4-8 stages) improve throughput but increase latency; balance based on requirements
- **Steady State**: after initial ramp-up, all stages busy; achieves maximum throughput; ramp-up and ramp-down reduce average efficiency
- **Buffer Management**: requires multiple buffers (one per pipeline stage); memory overhead; pre-allocate to avoid allocation overhead

**Default Stream Behavior:**
- **Legacy Default Stream**: stream 0; serializes with all other streams; blocks on all previous operations; convenient but prevents concurrency
- **Per-Thread Default Stream**: --default-stream per-thread flag; each host thread has independent default stream; enables concurrency across threads
- **Null Stream**: cudaStreamLegacy or cudaStreamPerThread; explicit specification; avoid legacy for performance-critical code
- **Best Practice**: always use explicit streams for performance; default stream for simple prototypes only

**Multi-GPU Streams:**
- **Device Context**: each stream associated with device; cudaSetDevice() before stream operations; streams don't cross devices
- **Peer-to-Peer**: cudaMemcpyPeerAsync() for direct GPU-to-GPU transfer; requires NVLink or PCIe P2P; overlaps with computation
- **Multi-Device Concurrency**: separate streams per device; enables concurrent execution across GPUs; critical for multi-GPU applications
- **Synchronization**: cudaDeviceSynchronize() only affects current device; synchronize each device separately

**Stream Callbacks:**
- **Host Function**: cudaLaunchHostFunc(stream, callback, userData); executes on host when stream reaches callback point
- **Use Cases**: logging, notifications, dynamic scheduling, resource management; avoid heavy computation (blocks stream)
- **Execution Context**: callback runs on driver thread; not application thread; thread-safe implementation required
- **Performance**: minimal overhead (<10 μs); useful for orchestration without blocking host thread

**Graph Capture:**
- **Stream Capture**: cudaStreamBeginCapture(), cudaStreamEndCapture(); records stream operations into graph; replay with cudaGraphLaunch()
- **Benefits**: reduces kernel launch overhead by 10-50%; useful for repeated execution patterns; 20-40% throughput improvement for small kernels
- **Limitations**: captured operations must be deterministic; no host synchronization during capture; not all operations supported
- **Use Cases**: inference serving (same graph repeated), iterative algorithms, fixed computation patterns

**Profiling Streams:**
- **Nsight Systems**: timeline view shows stream concurrency; identifies idle periods; visualizes overlaps; guides optimization
- **Metrics**: stream occupancy, concurrent kernel count, copy-compute overlap; target 80-100% utilization
- **Bottlenecks**: serialization points, insufficient concurrency, resource limits; profile to identify
- **Optimization**: increase concurrency, reduce synchronization, balance resource usage

**Common Patterns:**
- **Batch Processing**: each stream processes one batch; N streams for N concurrent batches; 2-4× throughput improvement
- **Producer-Consumer**: one stream produces data, another consumes; event-based synchronization; pipeline pattern
- **Priority Queues**: high-priority stream for latency-critical work, normal streams for throughput; 10-30% latency reduction
- **Persistent Kernels**: long-running kernel processes work from multiple streams; reduces launch overhead; 20-50% improvement for small tasks

**Performance Considerations:**
- **Launch Overhead**: kernel launch costs 5-20 μs; concurrent launches amortize overhead; graph capture eliminates repeated overhead
- **Resource Limits**: concurrent kernels limited by SM resources; profile to find optimal concurrency level
- **Memory Bandwidth**: concurrent operations share bandwidth; may not achieve linear speedup; measure achieved bandwidth
- **Synchronization Cost**: cudaStreamSynchronize() costs 5-10 μs; minimize synchronization points; use events for fine-grained control

**Best Practices:**
- **Use Explicit Streams**: avoid default stream; create 2-8 streams for concurrency; reuse streams across iterations
- **Pinned Memory**: always use pinned memory for async transfers; pre-allocate to avoid allocation overhead
- **Pipeline**: structure code as pipeline; overlap compute and memory; 2-3× throughput improvement
- **Profile**: use Nsight Systems to visualize concurrency; identify idle periods; optimize based on data
- **Minimize Sync**: reduce synchronization points; use events instead of stream sync when possible; async operations everywhere

**Advanced Techniques:**
- **Dynamic Parallelism**: kernels launch other kernels; creates streams on device; reduces CPU-GPU synchronization; 20-50% overhead; use sparingly
- **Cooperative Groups**: grid-wide synchronization; enables new algorithms; requires all blocks in same stream
- **Multi-Process Service (MPS)**: multiple processes share GPU; each process has independent streams; improves utilization for small workloads
- **CUDA Graphs**: capture and replay stream operations; 10-50% overhead reduction; optimal for repeated patterns

**Debugging Streams:**
- **CUDA_LAUNCH_BLOCKING=1**: serializes all operations; easier debugging; disables concurrency; use only for debugging
- **cuda-memcheck**: detects race conditions between streams; identifies synchronization bugs
- **Nsight Compute**: detailed kernel analysis; shows resource usage; helps optimize for concurrency
- **Assertions**: use assert() in kernels; helps catch logic errors; disabled in release builds

CUDA Streams and Concurrency represent **the key to unlocking full GPU potential** — by enabling overlapping of independent operations through asynchronous execution and proper stream management, developers achieve 2-4× throughput improvement and 80-100% GPU utilization, making streams essential for production applications where naive sequential execution wastes 50-80% of available hardware resources and proper concurrency management determines whether applications achieve 20% or 90% of theoretical throughput.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/cuda-streams-concurrency) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
