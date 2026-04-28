# CUDA Kernel Optimization

**Keywords**: cuda kernel optimization,gpu kernel tuning,cuda performance optimization,warp efficiency optimization,cuda memory coalescing

---

**CUDA Kernel Optimization** is **the systematic tuning of GPU kernels to maximize throughput, minimize latency, and achieve peak hardware utilization** — where optimizations like memory coalescing (achieving 80-100% memory bandwidth), occupancy tuning (70-100% SM utilization), warp divergence elimination (reducing branch penalties by 50-90%), and instruction-level parallelism (ILP) increase performance by 2-10× over naive implementations through techniques like shared memory tiling that reduces global memory accesses by 80-95%, register optimization that enables 50-100% more active warps, and loop unrolling that improves ILP by 2-4×, making kernel optimization critical for achieving 50-80% of theoretical peak performance (20-40 TFLOPS on A100, 60-80 TFLOPS on H100) where unoptimized kernels typically achieve only 5-20% of peak and systematic optimization following the CUDA performance guidelines can improve performance by 5-20× through memory, compute, and control flow optimizations.

**Memory Coalescing:**
- **Aligned Access**: threads in warp access consecutive memory addresses; 128-byte aligned; achieves 100% memory bandwidth utilization
- **Stride Patterns**: unit stride (consecutive) optimal; stride-2 achieves 50% bandwidth; stride-32 achieves 3% bandwidth; avoid non-unit strides
- **Structure of Arrays (SoA)**: prefer SoA over AoS; enables coalesced access; 5-10× memory bandwidth improvement
- **Padding**: add padding to avoid bank conflicts; align to 128 bytes; 10-30% performance improvement

**Occupancy Optimization:**
- **Register Usage**: reduce registers per thread; enables more active warps; 32-64 registers optimal; >128 registers limits occupancy
- **Shared Memory**: balance shared memory usage; 48KB per SM on A100; excessive usage reduces occupancy; 16-32KB per block typical
- **Block Size**: 128-256 threads per block optimal; too small wastes resources; too large limits occupancy; multiple of 32 (warp size)
- **Occupancy Calculator**: use CUDA occupancy calculator; predicts occupancy from resource usage; target 50-100% occupancy

**Warp Divergence:**
- **Branch Elimination**: remove branches when possible; use arithmetic instead; 2-5× speedup for divergent branches
- **Warp-Uniform Branches**: ensure all threads in warp take same path; predicate execution; eliminates divergence penalty
- **Thread Coarsening**: assign multiple elements per thread; reduces divergence; 20-50% performance improvement
- **Ballot/Shuffle**: use warp-level primitives; avoid explicit synchronization; 2-10× faster than shared memory

**Shared Memory Optimization:**
- **Tiling**: load data into shared memory; reuse across threads; reduces global memory accesses by 80-95%; 5-20× speedup
- **Bank Conflicts**: avoid accessing same bank simultaneously; 32 banks on modern GPUs; stride by 33 to avoid conflicts
- **Padding**: add padding to shared memory arrays; prevents bank conflicts; 1-2 elements padding typical
- **Synchronization**: minimize __syncthreads(); only when necessary; 10-30% overhead per sync

**Register Optimization:**
- **Register Pressure**: monitor register usage; nvcc --ptxas-options=-v shows usage; reduce to increase occupancy
- **Loop Unrolling**: #pragma unroll; reduces loop overhead; increases ILP; 20-50% speedup; but increases register usage
- **Constant Memory**: use __constant__ for read-only data; cached; broadcast to all threads; 2-5× faster than global memory
- **Texture Memory**: use texture cache for spatial locality; 2D/3D access patterns; 2-10× speedup for irregular access

**Instruction-Level Parallelism:**
- **Independent Operations**: reorder instructions; expose ILP; GPU can issue 2-4 instructions per cycle per warp
- **Loop Unrolling**: unroll loops by 2-4×; increases ILP; reduces loop overhead; 20-50% speedup
- **Multiple Accumulators**: use multiple accumulators in reductions; reduces dependency chains; 30-60% speedup
- **Fused Multiply-Add (FMA)**: use FMA instructions; 2× throughput vs separate multiply and add; automatic in most cases

**Memory Hierarchy:**
- **L1 Cache**: 128KB per SM on A100; automatic caching; prefer shared memory for explicit control
- **L2 Cache**: 40MB on A100, 50MB on H100; shared across SMs; benefits from temporal locality
- **Global Memory**: 40-80GB HBM2/HBM3; 1.5-3 TB/s bandwidth; minimize accesses; coalesce when accessing
- **Unified Memory**: automatic migration; convenient but slower; explicit management preferred for performance

**Compute Optimization:**
- **Tensor Cores**: use for matrix operations; 312 TFLOPS (FP16) on A100, 989 TFLOPS on H100; 10-20× faster than CUDA cores
- **Mixed Precision**: FP16 for compute, FP32 for accumulation; 2× throughput; maintains accuracy; automatic mixed precision (AMP)
- **Math Libraries**: use cuBLAS, cuDNN, cuFFT; highly optimized; 2-10× faster than custom kernels
- **Warp-Level Primitives**: __shfl, __ballot, __any, __all; faster than shared memory; 2-5× speedup for reductions

**Launch Configuration:**
- **Grid Size**: enough blocks to saturate GPU; 100-1000 blocks typical; more blocks than SMs for load balancing
- **Block Size**: 128-256 threads optimal; multiple of 32; balance occupancy and resource usage
- **Dynamic Parallelism**: launch kernels from device; reduces CPU-GPU synchronization; 20-50% overhead; use sparingly
- **Streams**: overlap compute and memory transfers; 2-4 streams typical; 20-50% throughput improvement

**Profiling Tools:**
- **Nsight Compute**: detailed kernel profiling; memory, compute, occupancy metrics; identifies bottlenecks
- **Nsight Systems**: timeline view; CPU-GPU interaction; kernel launches, memory transfers; system-level optimization
- **nvprof**: command-line profiler; deprecated but still useful; quick performance overview
- **Metrics**: achieved occupancy, memory throughput, compute throughput, warp execution efficiency; guide optimization

**Common Bottlenecks:**
- **Memory Bound**: <50% memory bandwidth; optimize coalescing, use shared memory, reduce accesses
- **Compute Bound**: <50% compute throughput; use Tensor Cores, increase ILP, reduce divergence
- **Latency Bound**: low occupancy; reduce register usage, increase block size, optimize shared memory
- **Instruction Bound**: high instruction overhead; reduce branches, use warp primitives, optimize control flow

**Optimization Workflow:**
- **Profile**: identify bottleneck; memory, compute, or latency; use Nsight Compute
- **Optimize**: apply relevant optimizations; memory coalescing, shared memory, occupancy tuning
- **Measure**: verify improvement; compare metrics; iterate if needed
- **Iterate**: repeat for next bottleneck; diminishing returns after 3-5 iterations; 2-10× total speedup typical

**Advanced Techniques:**
- **Cooperative Groups**: flexible thread synchronization; grid-wide sync; warp-level primitives; more expressive than __syncthreads()
- **Warp Specialization**: different warps perform different tasks; reduces divergence; 20-40% speedup for heterogeneous workloads
- **Persistent Threads**: threads loop over work items; reduces kernel launch overhead; 10-30% speedup for small kernels
- **Asynchronous Copy**: async memory copy; overlaps with compute; 20-50% speedup; requires compute capability 8.0+

**Performance Targets:**
- **Memory Bandwidth**: 80-100% of peak (1.5-3 TB/s); coalesced access, minimal bank conflicts
- **Compute Throughput**: 50-80% of peak (20-40 TFLOPS FP32, 60-80 TFLOPS FP16); use Tensor Cores, high ILP
- **Occupancy**: 50-100%; balance register and shared memory usage; 256 threads per block typical
- **Warp Efficiency**: >90%; minimize divergence; uniform control flow

**Case Studies:**
- **Matrix Multiplication**: 80-95% of peak with tiling and Tensor Cores; 10-20 TFLOPS on A100
- **Reduction**: 60-80% of peak with warp primitives and multiple accumulators; 500-1000 GB/s
- **Convolution**: 70-90% of peak with cuDNN or custom kernels; 15-30 TFLOPS on A100
- **Sorting**: 40-60% of peak with radix sort; 100-300 GB/s; memory-bound operation

**Common Mistakes:**
- **Uncoalesced Access**: stride access patterns; 10-100× slowdown; use SoA, align data
- **Excessive Synchronization**: too many __syncthreads(); 10-30% overhead each; minimize usage
- **Low Occupancy**: too many registers or shared memory; limits active warps; reduce resource usage
- **Branch Divergence**: divergent branches within warps; 2-32× slowdown; eliminate or make uniform

**Best Practices:**
- **Start Simple**: get correct implementation first; then optimize; premature optimization wastes time
- **Profile-Guided**: always profile before optimizing; focus on bottlenecks; 80/20 rule applies
- **Incremental**: optimize one aspect at a time; measure impact; easier to debug
- **Use Libraries**: cuBLAS, cuDNN, Thrust; highly optimized; 2-10× faster than custom code

**Performance Portability:**
- **Compute Capability**: code for target GPU; A100 (8.0), H100 (9.0); use __CUDA_ARCH__ for conditional compilation
- **Tuning Parameters**: block size, tile size, unroll factors; auto-tune for different GPUs; 20-50% performance variation
- **Tensor Cores**: available on Volta (7.0) and newer; check capability; fallback to CUDA cores
- **Memory Bandwidth**: varies by GPU; A100 (1.5 TB/s), H100 (3 TB/s); adjust algorithms accordingly

CUDA Kernel Optimization represents **the art and science of GPU programming** — by applying memory coalescing, occupancy tuning, warp divergence elimination, and shared memory tiling, developers achieve 2-10× performance improvement and 50-80% of theoretical peak performance, making systematic kernel optimization essential for competitive GPU applications where unoptimized kernels achieve only 5-20% of peak and following CUDA best practices can improve performance by 5-20× through memory, compute, and control flow optimizations.');

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/cuda-kernel-optimization) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
