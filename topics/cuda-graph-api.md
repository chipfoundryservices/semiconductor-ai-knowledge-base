# CUDA Graph API

**Keywords**: cuda graph api,cuda graph optimization,graph capture cuda,cuda graph launch,kernel launch overhead reduction

---

**CUDA Graph API** is **the mechanism for capturing sequences of GPU operations into an executable graph that can be launched with minimal overhead** — reducing kernel launch latency from 5-20 μs per kernel to <1 μs for entire graph, achieving 10-50% throughput improvement for workloads with many small kernels or repeated execution patterns, making CUDA Graphs essential for inference serving where launching hundreds of small kernels per request dominates latency (30-60% of total time) and graph capture enables batching of operations that improves throughput by 2-4× through reduced CPU overhead and better GPU scheduling, used in production systems like TensorRT, PyTorch, and TensorFlow for optimizing inference pipelines that execute the same computation pattern repeatedly.


**Graph Fundamentals:**
- **Graph Structure**: directed acyclic graph (DAG) of GPU operations; nodes represent kernels, memory copies, synchronization; edges represent dependencies
- **Capture vs Manual**: stream capture (automatic) records operations from stream; manual construction (explicit) builds graph programmatically; capture easier, manual more flexible
- **Instantiation**: graph template instantiated into executable graph; instantiation cost 1-10ms; amortized over many launches; reuse instantiated graph
- **Launch**: cudaGraphLaunch() executes entire graph; <1 μs overhead vs 5-20 μs per kernel; 10-100× lower overhead for graphs with many kernels

**Stream Capture:**
- **Begin Capture**: cudaStreamBeginCapture(stream, mode); starts recording operations on stream; mode controls cross-stream dependencies
- **Record Operations**: kernel launches, memory copies, synchronization recorded into graph; operations execute normally during capture
- **End Capture**: cudaStreamEndCapture(stream, &graph); stops recording; returns graph object; graph can be instantiated and launched
- **Capture Modes**: cudaStreamCaptureModeGlobal (strict), cudaStreamCaptureModeThreadLocal (per-thread), cudaStreamCaptureModeRelaxed (cross-stream)

**Manual Graph Construction:**
- **Create Graph**: cudaGraphCreate(&graph); empty graph object
- **Add Nodes**: cudaGraphAddKernelNode(), cudaGraphAddMemcpyNode(), cudaGraphAddMemsetNode(); explicit node creation
- **Add Dependencies**: cudaGraphAddDependencies(); explicit edge creation; defines execution order
- **Use Cases**: dynamic graphs, conditional execution, complex dependencies; more control than capture

**Graph Instantiation:**
- **Instantiate**: cudaGraphInstantiate(&graphExec, graph, NULL, NULL, 0); creates executable graph; 1-10ms cost
- **Optimization**: instantiation performs optimizations; kernel fusion, memory coalescing, scheduling; improves performance 10-30%
- **Reuse**: instantiate once, launch many times; amortizes instantiation cost; critical for performance
- **Update**: cudaGraphExecUpdate() updates parameters without re-instantiation; useful for changing inputs; 10-100× faster than re-instantiation

**Graph Launch:**
- **Launch**: cudaGraphLaunch(graphExec, stream); executes entire graph; <1 μs overhead; asynchronous like kernel launch
- **Synchronization**: cudaStreamSynchronize(stream) waits for graph completion; cudaEventRecord() for fine-grained sync
- **Concurrency**: multiple graphs can execute concurrently in different streams; enables pipelining; 2-4× throughput improvement
- **Overhead Reduction**: 10-100× lower overhead vs individual kernel launches; critical for small kernels (<100 μs)

**Performance Benefits:**
- **Launch Overhead**: reduces from 5-20 μs per kernel to <1 μs for entire graph; 10-50% throughput improvement for many small kernels
- **CPU Overhead**: frees CPU from launching kernels; enables higher throughput; 20-40% CPU utilization reduction
- **GPU Scheduling**: better scheduling decisions; kernel fusion opportunities; 10-30% GPU utilization improvement
- **Inference Serving**: 2-4× throughput improvement; 30-60% latency reduction; critical for real-time applications

**Graph Update:**
- **Parameter Update**: cudaGraphExecKernelNodeSetParams() updates kernel parameters; avoids re-instantiation; 10-100× faster
- **Use Cases**: changing input pointers, batch sizes, hyperparameters; same graph structure, different data
- **Limitations**: can't change graph topology; can't add/remove nodes; only parameter updates; re-instantiate for structural changes
- **Performance**: parameter update costs 10-100 μs; re-instantiation costs 1-10ms; update preferred when possible

**Conditional Execution:**
- **Conditional Nodes**: cudaGraphConditionalHandle; enables if-then-else in graphs; dynamic execution paths
- **Use Cases**: early exit, adaptive algorithms, dynamic batch sizes; avoids re-capturing for different paths
- **Performance**: minimal overhead vs static graphs; 5-10% overhead for conditional logic
- **Limitations**: limited nesting depth; complex conditionals may require multiple graphs

**Graph Cloning:**
- **Clone**: cudaGraphClone(&clonedGraph, originalGraph); creates copy of graph; independent modification
- **Use Cases**: similar graphs with small differences; template pattern; multi-tenant serving
- **Performance**: cloning faster than re-capture; 10-100× faster; instantiation still required

**Memory Management in Graphs:**
- **Allocations**: cudaMalloc/cudaFree not allowed during capture; allocate before capture; use pre-allocated buffers
- **Memory Nodes**: cudaGraphAddMemAllocNode(), cudaGraphAddMemFreeNode(); explicit memory management in graph
- **Virtual Memory**: CUDA 11.2+ supports virtual memory in graphs; enables dynamic allocation; 10-30% overhead
- **Best Practice**: pre-allocate all memory; reuse across graph launches; eliminates allocation overhead

**Integration with Frameworks:**
- **PyTorch**: torch.cuda.make_graphed_callables() captures PyTorch operations; automatic graph optimization; 20-40% inference speedup
- **TensorFlow**: tf.function with experimental_compile=True uses graphs; XLA compilation; 30-60% speedup
- **TensorRT**: automatically uses CUDA Graphs for inference; 2-4× throughput improvement; transparent to user
- **ONNX Runtime**: graph optimization and execution; CUDA Graph backend; 20-50% speedup

**Profiling Graphs:**
- **Nsight Systems**: visualizes graph execution; shows node timing; identifies bottlenecks; timeline view
- **Nsight Compute**: detailed kernel analysis within graph; memory, compute metrics; optimization guidance
- **Graph Metrics**: total graph time, per-node time, launch overhead, CPU time; target <1% overhead
- **Optimization**: identify slow nodes; optimize kernels; consider kernel fusion; balance parallelism

**Common Patterns:**
- **Inference Pipeline**: capture entire inference forward pass; launch graph per request; 2-4× throughput improvement
- **Iterative Algorithms**: capture single iteration; launch graph repeatedly; 20-50% speedup; examples: optimization, simulation
- **Multi-Stage Processing**: capture each stage as graph; pipeline stages with streams; 30-60% throughput improvement
- **Batch Processing**: capture processing for single item; launch graph for each item in batch; 10-30% speedup

**Limitations and Constraints:**
- **Deterministic**: graph operations must be deterministic; no host synchronization during capture; no CPU-dependent control flow
- **Supported Operations**: kernels, memory copies, memset, synchronization; not all CUDA operations supported; check documentation
- **Cross-Stream**: limited cross-stream dependencies during capture; use appropriate capture mode; may require manual construction
- **Dynamic Shapes**: fixed shapes during capture; dynamic shapes require re-capture or conditional nodes; limits flexibility

**Best Practices:**
- **Capture Once**: capture graph once, launch many times; amortizes capture and instantiation cost; critical for performance
- **Pre-Allocate Memory**: allocate all memory before capture; reuse across launches; eliminates allocation overhead
- **Update Parameters**: use cudaGraphExecUpdate() for parameter changes; 10-100× faster than re-instantiation
- **Profile**: use Nsight Systems to verify overhead reduction; measure launch time; target <1% overhead
- **Batch Operations**: capture multiple operations into single graph; reduces overhead; improves scheduling

**Advanced Techniques:**
- **Graph Partitioning**: split large graphs into smaller sub-graphs; enables partial updates; reduces instantiation cost
- **Hierarchical Graphs**: graphs containing sub-graphs; modular design; reuse sub-graphs; 20-40% development time reduction
- **Persistent Graphs**: long-lived graphs for repeated use; cache instantiated graphs; eliminates re-instantiation
- **Multi-GPU Graphs**: capture operations across multiple GPUs; requires careful synchronization; 70-85% scaling efficiency

**Debugging Graphs:**
- **CUDA_LAUNCH_BLOCKING=1**: serializes operations; easier debugging; disables async; use only for debugging
- **Graph Validation**: cudaGraphDebugDotPrint() exports graph to DOT format; visualize with Graphviz; verify structure
- **Error Handling**: check return codes; cudaGetLastError() after graph operations; errors may be deferred
- **Incremental Capture**: capture small portions; verify correctness; gradually expand; easier debugging

**Performance Targets:**
- **Launch Overhead**: <1 μs for graph launch vs 5-20 μs per kernel; target 10-100× reduction
- **Throughput**: 10-50% improvement for workloads with many small kernels; 2-4× for inference serving
- **CPU Utilization**: 20-40% reduction; frees CPU for other work; critical for high-throughput serving
- **GPU Utilization**: 10-30% improvement from better scheduling; kernel fusion opportunities

**Real-World Impact:**
- **BERT Inference**: 2-3× throughput improvement with CUDA Graphs; 30-40% latency reduction; critical for real-time NLP
- **ResNet Inference**: 20-40% speedup; reduces launch overhead from 30% to <5%; enables higher batch throughput
- **Video Processing**: 30-60% improvement; many small kernels per frame; graph capture amortizes overhead
- **Recommendation Systems**: 2-4× throughput; hundreds of small embedding lookups; graph batching critical

CUDA Graph API represents **the key to eliminating CPU overhead in GPU computing** — by capturing sequences of operations into executable graphs that launch with <1 μs overhead, developers achieve 10-50% throughput improvement and 2-4× higher serving capacity, making CUDA Graphs essential for production inference systems where kernel launch overhead dominates latency and proper graph optimization determines whether applications achieve 100 or 1000 requests per second on the same hardware.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/cuda-graph-api) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
