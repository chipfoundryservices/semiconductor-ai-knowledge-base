# Kernel Fusion

**Keywords**: kernel fusion optimization,operator fusion deep learning,fused kernels cuda,memory traffic reduction,kernel launch overhead

---

**Kernel Fusion** is **the optimization technique that combines multiple sequential GPU kernels into a single kernel — eliminating intermediate global memory writes and reads, reducing kernel launch overhead (5-20 μs per launch), and improving data locality by keeping intermediate results in registers or shared memory, achieving 2-10× speedups for sequences of element-wise operations common in deep learning inference and scientific computing**.

**Fusion Opportunities:**
- **Element-Wise Operations**: sequences like ReLU → BatchNorm → Add → ReLU can be fused into a single kernel; each element is loaded once, all operations applied, result written once; unfused version: 4 kernel launches, 8 global memory accesses (4 reads + 4 writes); fused version: 1 launch, 2 accesses (1 read + 1 write)
- **Reduction Chains**: sum → square → sum (L2 norm) fused into single reduction kernel; intermediate squared values stay in registers; unfused: 3 kernels, 2 full passes over data; fused: 1 kernel, 1 pass over data
- **Stencil Operations**: convolution → bias add → activation fused; convolution output stays in registers, bias and activation applied immediately; eliminates storing/loading intermediate feature maps
- **Producer-Consumer**: when kernel A's output is kernel B's input and no other kernel uses A's output, fuse A and B; producer computes value, consumer uses it immediately from register; zero memory traffic for intermediate data

**Memory Traffic Reduction:**
- **Bandwidth Savings**: unfused element-wise chain with N operations: 2N global memory accesses (N reads + N writes); fused: 2 accesses (1 read + 1 write); N=10 operations: 10× bandwidth reduction
- **Intermediate Tensor Elimination**: fused kernels don't materialize intermediate tensors in global memory; saves memory allocation and bandwidth; critical for memory-constrained workloads (large batch sizes, high-resolution images)
- **Cache Utilization**: fused operations on same data improve L2 cache hit rate; data loaded once serves multiple operations; unfused kernels may evict data from cache between launches
- **Effective Bandwidth**: unfused element-wise operations achieve 10-30% of peak bandwidth (launch overhead dominates); fused operations achieve 60-80% of peak bandwidth; 3-8× effective bandwidth improvement

**Launch Overhead Elimination:**
- **Launch Cost**: each kernel launch incurs 5-20 μs overhead (CPU-side scheduling, GPU command queue processing); for 1 μs kernels, launch overhead is 5-20× the compute time; fusion eliminates N-1 launches for N-kernel sequence
- **Latency Reduction**: unfused: 10 kernels × 10 μs launch = 100 μs overhead; fused: 1 kernel × 10 μs = 10 μs overhead; 90 μs saved; critical for real-time inference (target <10 ms latency)
- **CPU-GPU Synchronization**: fewer launches reduce CPU-GPU synchronization points; improves pipelining and overlap of CPU and GPU work; reduces overall application latency
- **Batch Size Sensitivity**: small batch sizes (1-32) make launch overhead dominant; fusion provides 5-10× speedup; large batch sizes (1024+) amortize launch overhead; fusion provides 1.5-3× speedup

**Fusion Patterns:**
- **Vertical Fusion**: fuse sequential operations on same tensor; input → op1 → op2 → op3 → output; single kernel applies all operations; maximizes data reuse
- **Horizontal Fusion**: fuse independent operations on different tensors; parallel branches in computation graph executed by same kernel; improves GPU utilization by increasing parallelism
- **Loop Fusion**: fuse loops iterating over same data; for (i) A[i] = B[i] + C[i]; for (i) D[i] = A[i] * E[i]; → for (i) {A[i] = B[i] + C[i]; D[i] = A[i] * E[i];} — eliminates intermediate array A
- **Sliding Window Fusion**: fuse operations with overlapping access patterns; convolution layers with stride < kernel_size reuse input data; fused kernel loads shared input once for multiple output positions

**Implementation Techniques:**
- **Template Metaprogramming**: C++ templates generate fused kernels at compile time; template<typename Op1, typename Op2> __global__ void fused_kernel(Op1 op1, Op2 op2, ...); compiler inlines operations; zero runtime overhead
- **JIT Compilation**: runtime code generation creates fused kernels for specific operation sequences; PyTorch JIT, TensorFlow XLA, TVM compile computation graphs into optimized fused kernels; adapts to dynamic shapes and operation sequences
- **Kernel Generators**: libraries like CUTLASS provide building blocks for fused kernels; compose operations using C++ abstractions; generates efficient CUDA code with optimal memory access patterns
- **Manual Fusion**: hand-written fused kernels for critical paths; full control over register allocation, shared memory usage, and memory access patterns; highest performance but requires expert knowledge

**Compiler Support:**
- **XLA (Accelerated Linear Algebra)**: TensorFlow's JIT compiler; automatically fuses element-wise operations, reductions, and broadcasts; generates optimized GPU kernels; achieves 2-5× speedup on inference workloads
- **TorchScript JIT**: PyTorch's JIT compiler; fuses operations in traced or scripted models; limited fusion compared to XLA but improving; enables deployment optimization without manual kernel writing
- **TVM**: open-source compiler for deep learning; aggressive fusion across operation types; generates fused kernels for multiple hardware backends (CUDA, ROCm, CPU); research-level performance
- **NVFUSER**: NVIDIA's fusion compiler for PyTorch; specializes in element-wise and reduction fusion; generates highly optimized CUDA code; integrated into PyTorch 2.0+ as default fusion backend

**Limitations and Trade-offs:**
- **Register Pressure**: fused kernels use more registers (hold intermediate values); may reduce occupancy; balance between fusion benefits and occupancy loss; sometimes partial fusion is optimal
- **Code Complexity**: fused kernels are harder to write, debug, and maintain; template metaprogramming and JIT compilation add complexity; use compiler-based fusion when possible
- **Fusion Scope**: can't fuse across synchronization points (reductions, scans); can't fuse when intermediate results needed by multiple consumers; fusion opportunities limited by data dependencies
- **Diminishing Returns**: fusing 2-3 operations provides large gains; fusing 10+ operations provides smaller incremental gains; optimal fusion depth depends on register pressure and memory bandwidth

**Performance Analysis:**
- **Memory Traffic**: compare global memory reads/writes before and after fusion; nsight compute reports dram_read_throughput and dram_write_throughput; fusion should reduce by 2-10×
- **Kernel Count**: count kernel launches in profiler; fusion reduces launch count; measure total kernel time including launch overhead; fusion should reduce by 2-5×
- **Occupancy Impact**: check if fusion reduces occupancy due to register pressure; if occupancy drops below 50%, consider partial fusion or register optimization

Kernel fusion is **the high-impact optimization that transforms sequences of memory-bound operations into compute-efficient fused kernels — by eliminating intermediate memory traffic and launch overhead, fusion achieves 2-10× speedups for deep learning inference, making it the primary optimization target for deployment frameworks and the foundation of modern JIT compilers like XLA and TorchScript**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/kernel-fusion-optimization) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
