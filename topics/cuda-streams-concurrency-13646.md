# CUDA Streams and Concurrency

**Keywords**: cuda streams concurrency,asynchronous execution gpu,stream synchronization,concurrent kernel execution,multi stream programming

---

**CUDA Streams and Concurrency** are **the mechanisms for overlapping independent GPU operations — enabling simultaneous execution of multiple kernels, concurrent data transfers and kernel execution, and pipelined processing of batches by organizing operations into independent streams that execute asynchronously, achieving 2-4× throughput improvements through hardware utilization that would otherwise remain idle**.

**Stream Fundamentals:**
- **Stream Definition**: a stream is a sequence of operations (kernel launches, memory copies, synchronization) that execute in order; operations in different streams can execute concurrently if hardware resources allow; default stream (stream 0) serializes with all other operations
- **Stream Creation**: cudaStream_t stream; cudaStreamCreate(&stream); creates a non-blocking stream; operations in this stream execute independently of other streams; cudaStreamDestroy(stream) releases resources
- **Asynchronous Operations**: kernel<<<grid, block, 0, stream>>>(args); launches kernel in stream; cudaMemcpyAsync(dst, src, size, kind, stream); asynchronous memory copy; both return immediately to CPU; GPU executes asynchronously
- **Synchronization**: cudaStreamSynchronize(stream) blocks CPU until all operations in stream complete; cudaDeviceSynchronize() blocks until all streams complete; cudaStreamQuery(stream) checks if stream is idle without blocking

**Concurrent Kernel Execution:**
- **Hardware Requirements**: modern GPUs support 32-128 concurrent kernels (Ampere: 128); each kernel requires available SMs; small kernels (using few SMs) enable more concurrency; large kernels (using all SMs) prevent concurrency
- **Resource Partitioning**: concurrent kernels share SM resources; kernel A using 50% of SMs allows kernel B to use remaining 50%; if kernel A uses 100% of SMs, no concurrency possible
- **Hyper-Q**: Kepler+ GPUs have 32 hardware work queues; enables true concurrent execution of kernels from different streams; pre-Kepler GPUs serialize kernels even in different streams
- **Occupancy Impact**: concurrent kernels reduce per-kernel occupancy; kernel A and B each get 50% of SMs instead of 100%; acceptable if kernels are memory-bound (latency-hiding doesn't require full GPU)

**Overlapping Compute and Memory Transfer:**
- **Copy Engines**: GPUs have dedicated DMA engines for memory transfers; 2 copy engines (host-to-device and device-to-host) operate independently of compute SMs; enables simultaneous kernel execution and data transfer
- **Pinned Memory**: cudaMallocHost(&ptr, size) allocates page-locked host memory; required for asynchronous transfers; pageable memory forces synchronous transfer; pinned memory enables full overlap
- **Bidirectional Transfer**: cudaMemcpyAsync(d_A, h_A, size, H2D, stream1); kernel<<<..., stream2>>>(); cudaMemcpyAsync(h_B, d_B, size, D2H, stream3); — three operations execute concurrently; achieves 2-3× throughput vs sequential execution
- **PCIe Bandwidth**: host-device transfer limited by PCIe bandwidth (16-32 GB/s on PCIe 4.0 x16); overlapping transfer with compute hides this latency; critical for data-intensive applications

**Stream Synchronization Patterns:**
- **Events**: cudaEvent_t event; cudaEventCreate(&event); cudaEventRecord(event, stream1); cudaStreamWaitEvent(stream2, event); — stream2 waits for stream1 to reach event; enables fine-grained inter-stream dependencies
- **Callbacks**: cudaStreamAddCallback(stream, callback_func, userData); executes CPU function when stream reaches callback point; enables CPU-GPU coordination without blocking
- **Stream Priorities**: cudaStreamCreateWithPriority(&stream, flags, priority); higher priority streams preempt lower priority; priority range: -1 (high) to 0 (normal); useful for latency-critical kernels
- **Default Stream Behavior**: legacy default stream (NULL) synchronizes with all streams; per-thread default stream (compile with --default-stream per-thread) is non-blocking; use explicit streams for best control

**Pipeline Parallelism:**
- **Batch Processing**: divide large batch into chunks; stream 1: copy chunk 1 H2D; stream 2: process chunk 1; stream 3: copy chunk 1 D2H; stream 1: copy chunk 2 H2D; ... — three stages execute concurrently
- **Steady State**: after initial ramp-up, all three stages execute simultaneously; throughput = max(copy_time, compute_time); if compute_time > copy_time, transfer is fully hidden; achieves 2-3× speedup vs sequential processing
- **Depth**: number of concurrent chunks (streams); depth 3-4 typically optimal; deeper pipelines provide diminishing returns and increase memory usage (more chunks in flight)
- **Load Balancing**: ensure chunks have similar compute time; imbalanced chunks cause pipeline stalls; dynamic chunk sizing adapts to workload variation

**Multi-GPU Streams:**
- **Per-Device Streams**: each GPU has independent streams; cudaSetDevice(0); cudaStreamCreate(&stream0); cudaSetDevice(1); cudaStreamCreate(&stream1); — streams on different GPUs execute independently
- **Peer-to-Peer Transfer**: cudaMemcpyPeerAsync(dst, dstDevice, src, srcDevice, size, stream); direct GPU-to-GPU transfer via NVLink or PCIe; overlaps with compute on both GPUs
- **Multi-GPU Pipeline**: GPU 0 processes chunk N while GPU 1 processes chunk N+1; results transferred peer-to-peer; achieves near-linear scaling for independent workloads

**Performance Optimization:**
- **Stream Depth**: too few streams underutilize hardware; too many streams increase overhead and memory usage; 3-8 streams typically optimal; measure with profiler
- **Kernel Size**: small kernels (<10 μs) enable more concurrency but have higher launch overhead; large kernels (>1 ms) limit concurrency; balance between kernel granularity and concurrency
- **Memory Copy Granularity**: small copies (<1 MB) have high overhead; large copies (>10 MB) reduce concurrency; chunk size 1-10 MB typically optimal for pipelined transfers
- **Synchronization Overhead**: minimize cudaStreamSynchronize() calls; use events for fine-grained dependencies; excessive synchronization serializes execution

**Profiling and Analysis:**
- **Nsight Systems**: visualizes stream timeline; shows concurrent kernel execution and memory transfers; identifies pipeline bubbles and synchronization bottlenecks
- **Concurrent Kernel Metric**: reports number of concurrent kernels; compare to theoretical maximum; low concurrency indicates resource contention or insufficient parallelism
- **Copy-Compute Overlap**: measures percentage of time where transfer and compute overlap; target >80% for pipelined workloads; <50% indicates insufficient overlap

CUDA streams and concurrency are **the essential techniques for maximizing GPU utilization — by organizing operations into independent streams and carefully orchestrating kernel execution, memory transfers, and synchronization, developers achieve 2-4× throughput improvements by keeping all GPU resources (SMs, copy engines, memory controllers) busy simultaneously, transforming underutilized GPUs into fully saturated high-performance accelerators**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/cuda-streams-concurrency-13646) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
