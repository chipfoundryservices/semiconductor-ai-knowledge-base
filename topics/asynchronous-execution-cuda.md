# Asynchronous Execution in CUDA

**Keywords**: asynchronous execution cuda,cuda events timing,non blocking operations,gpu cpu overlap,asynchronous memory copy

---

**Asynchronous Execution in CUDA** is **the programming model where GPU operations return control to the CPU immediately without waiting for completion — enabling the CPU to perform useful work, launch additional GPU operations, or manage multiple GPUs while kernels execute and data transfers occur, achieving 2-5× application-level speedup by eliminating CPU idle time and maximizing CPU-GPU overlap through careful orchestration of asynchronous operations and synchronization points**.

**Asynchronous Operations:**
- **Kernel Launches**: kernel<<<grid, block, sharedMem, stream>>>(args); returns immediately to CPU; kernel executes asynchronously on GPU; CPU continues to next instruction without waiting; GPU and CPU work in parallel
- **Memory Copies**: cudaMemcpyAsync(dst, src, size, kind, stream); initiates transfer and returns immediately; requires pinned (page-locked) host memory; cudaMemcpy() is synchronous (blocks CPU until complete)
- **Memory Operations**: cudaMemsetAsync(), cudaMemcpy2DAsync(), cudaMemcpy3DAsync() all have asynchronous variants; enable pipelining of memory operations with compute
- **Synchronization**: cudaDeviceSynchronize() blocks CPU until all GPU operations complete; cudaStreamSynchronize(stream) blocks until specific stream completes; cudaEventSynchronize(event) blocks until event is recorded

**CUDA Events:**
- **Event Creation**: cudaEvent_t event; cudaEventCreate(&event); creates event object; events mark points in stream execution; used for timing, synchronization, and inter-stream dependencies
- **Recording Events**: cudaEventRecord(event, stream); places event in stream; event is "complete" when all operations before it in the stream finish; non-blocking operation (returns immediately)
- **Waiting on Events**: cudaEventSynchronize(event); blocks CPU until event completes; cudaStreamWaitEvent(stream, event); makes stream wait for event (GPU-side wait, CPU continues)
- **Event Queries**: cudaEventQuery(event); returns cudaSuccess if event complete, cudaErrorNotReady if pending; enables polling without blocking; useful for CPU-GPU coordination

**GPU Timing with Events:**
- **Timing Pattern**: cudaEventRecord(start, stream); kernel<<<..., stream>>>(); cudaEventRecord(stop, stream); cudaEventSynchronize(stop); cudaEventElapsedTime(&ms, start, stop); — measures kernel execution time with microsecond precision
- **Advantages**: events measure GPU time (excludes CPU overhead); accurate for asynchronous operations; measures time between any two points in stream; hardware-based timing (no CPU involvement)
- **Multiple Timers**: create multiple event pairs to time different sections; events in same stream maintain order; events in different streams measure concurrent execution
- **Overhead**: event recording has ~1 μs overhead; negligible for kernels >10 μs; for micro-benchmarking, use many iterations and average

**CPU-GPU Overlap Patterns:**
- **Compute Overlap**: launch kernel; while GPU computes, CPU performs preprocessing, I/O, or launches operations on other GPUs; cudaStreamSynchronize() when CPU needs results; achieves 2× speedup if CPU and GPU work are balanced
- **Multi-GPU Management**: CPU launches kernels on GPU 0; cudaSetDevice(1); launches kernels on GPU 1; both GPUs execute concurrently; CPU orchestrates without blocking; scales to 4-8 GPUs
- **Pipelined Processing**: CPU prepares batch N+1 while GPU processes batch N; when GPU finishes N, immediately start N+1 (already prepared); eliminates CPU preparation latency from critical path
- **Callback Functions**: cudaStreamAddCallback(stream, callback, userData); CPU function executes when stream reaches callback; enables complex CPU-GPU coordination without polling

**Pinned Memory for Async Transfers:**
- **Allocation**: cudaMallocHost(&ptr, size); allocates page-locked host memory; guaranteed to remain in physical RAM (not swapped to disk); required for asynchronous transfers
- **Performance**: pinned memory enables DMA (direct memory access); GPU can transfer data without CPU involvement; achieves full PCIe bandwidth (16-32 GB/s)
- **Limitations**: pinned memory is scarce resource; excessive pinning reduces available RAM for OS and applications; typical limit: 50-80% of system RAM; use for frequently transferred data only
- **Portable Pinned Memory**: cudaHostAlloc(&ptr, size, cudaHostAllocPortable); accessible from all CUDA contexts; useful for multi-GPU applications

**Synchronization Strategies:**
- **Coarse-Grained Sync**: launch many operations; single cudaDeviceSynchronize() at end; maximizes asynchrony but provides no intermediate results; suitable for batch processing
- **Fine-Grained Sync**: synchronize after each critical operation; enables CPU to react to intermediate results; reduces parallelism; suitable for interactive applications
- **Event-Based Sync**: use events to create dependencies between streams; enables complex DAG (directed acyclic graph) execution; GPU operations proceed without CPU involvement; optimal for throughput
- **Polling**: cudaEventQuery() or cudaStreamQuery() in loop; CPU performs useful work between polls; enables responsive applications without blocking

**Common Pitfalls:**
- **Implicit Synchronization**: cudaMemcpy() (without Async) synchronizes entire device; cudaMalloc()/cudaFree() may synchronize; memory copies to/from pageable memory synchronize; use asynchronous variants and pinned memory
- **Default Stream Synchronization**: legacy default stream (NULL) synchronizes with all other streams; operations in default stream block until all streams complete; use explicit streams or per-thread default stream
- **Premature Synchronization**: synchronizing too early serializes execution; launch all independent operations before synchronizing; use events to express only necessary dependencies
- **Ignoring Errors**: asynchronous operations may fail silently; errors reported at next synchronization point; check cudaGetLastError() after launches; use cudaStreamQuery() to detect errors early

**Performance Measurement:**
- **Wall-Clock Time**: measures total application time including CPU and GPU; use for end-to-end performance; doesn't distinguish CPU vs GPU bottlenecks
- **GPU Time (Events)**: measures pure GPU execution time; excludes CPU overhead and synchronization; use for kernel optimization; doesn't capture CPU-GPU transfer time
- **Profiler Timeline**: nsight systems shows CPU and GPU timelines; visualizes overlap and idle time; identifies synchronization bottlenecks; essential for optimizing asynchronous execution
- **Overlap Percentage**: (overlapped_time / total_time) × 100%; target >70% for well-optimized applications; <30% indicates insufficient asynchrony or load imbalance

**Advanced Patterns:**
- **Graph Execution**: cudaGraph captures sequence of operations; cudaGraphLaunch() replays graph with minimal overhead; reduces launch overhead from 5-20 μs to <1 μs; ideal for repeated execution patterns
- **Stream Capture**: cudaStreamBeginCapture(stream); launch operations; cudaStreamEndCapture(stream, &graph); automatically creates graph from recorded operations; simplifies graph creation
- **Persistent Kernels**: kernel runs indefinitely; CPU enqueues work via device-side queues; eliminates launch overhead entirely; achieves <1 μs latency for small tasks

Asynchronous execution is **the fundamental technique for achieving high performance in CUDA applications — by eliminating CPU-GPU synchronization bottlenecks, overlapping compute with data transfer, and enabling concurrent multi-GPU execution, developers transform applications from sequential CPU-GPU ping-pong into fully pipelined, parallel systems that achieve 2-5× speedups through maximal hardware utilization**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/asynchronous-execution-cuda) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
