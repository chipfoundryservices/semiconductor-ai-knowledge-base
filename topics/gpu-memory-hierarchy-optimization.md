# GPU Memory Hierarchy Optimization

**Keywords**: gpu memory hierarchy optimization,cuda memory types,gpu cache optimization,shared memory optimization,gpu memory bandwidth

---

**GPU Memory Hierarchy Optimization** is **the systematic tuning of data placement and access patterns across GPU's multi-level memory system to maximize bandwidth utilization and minimize latency** — where understanding the hierarchy from registers (20,000 GB/s effective bandwidth) through shared memory (19 TB/s on H100), L1/L2 caches (10-15 TB/s), to global HBM memory (1.5-3 TB/s) enables 5-20× performance improvements through techniques like shared memory tiling that reduces global memory accesses by 80-95%, register blocking that keeps frequently accessed data in fastest storage, and memory coalescing that achieves 80-100% of theoretical bandwidth, making memory hierarchy optimization the most impactful optimization for memory-bound kernels that dominate GPU workloads where 60-80% of kernels are memory-limited rather than compute-limited.


**Memory Hierarchy Levels:**
- **Registers**: fastest storage; 32-bit registers; 65,536 registers per SM on A100; 20,000+ GB/s effective bandwidth; private to each thread; limited quantity (255 registers per thread max); excessive usage reduces occupancy
- **Shared Memory**: on-chip SRAM; 164KB per SM on A100, 228KB on H100; 19 TB/s bandwidth on H100; shared across thread block; explicit programmer control; 32 banks for parallel access; 100× faster than global memory
- **L1 Cache**: 128KB per SM on A100; combined with shared memory; automatic caching; benefits from spatial and temporal locality; cache line size 128 bytes; write-through to L2
- **L2 Cache**: 40MB on A100, 50MB on H100; shared across all SMs; 10-15 TB/s bandwidth; benefits from reuse across thread blocks; victim cache for L1; configurable persistence for critical data
- **Global Memory**: 40-80GB HBM2/HBM3; 1.5-3 TB/s bandwidth; highest capacity but slowest; 400-800 cycle latency; requires coalescing for efficiency; all threads can access

**Shared Memory Optimization:**
- **Tiling Strategy**: divide data into tiles that fit in shared memory; load tile cooperatively; reuse across threads; reduces global memory accesses by 80-95%; matrix multiplication: 5-20× speedup with tiling
- **Bank Conflicts**: 32 banks on modern GPUs; simultaneous access to same bank serializes; stride by 33 elements to avoid conflicts; padding arrays prevents conflicts; 2-10× slowdown from conflicts
- **Cooperative Loading**: all threads in block load data collaboratively; maximizes memory bandwidth; coalesced global loads; synchronize with __syncthreads() after loading
- **Double Buffering**: overlap computation with next tile load; use two shared memory buffers; hide memory latency; 20-40% performance improvement; requires careful synchronization
- **Capacity Planning**: 48KB per block typical; balance between occupancy and tile size; larger tiles reduce global accesses but limit occupancy; profile to find optimal size

**Register Optimization:**
- **Register Pressure**: monitor with nvcc --ptxas-options=-v; shows registers per thread; high usage limits occupancy; target 32-64 registers per thread for good occupancy
- **Register Spilling**: when exceeding register limit, spills to local memory (slow); 10-100× slowdown for spilled accesses; reduce by simplifying code, using fewer variables
- **Loop Unrolling**: #pragma unroll increases register usage but improves ILP; unroll factor 2-4 typical; balance between ILP and occupancy; measure impact with profiler
- **Constant Memory**: use __constant__ for read-only data; 64KB per kernel; cached; broadcast to all threads; 2-5× faster than global memory for uniform access
- **Texture Memory**: use for spatial locality; 2D/3D access patterns; cached; interpolation hardware; 2-10× speedup for irregular access patterns

**Cache Optimization:**
- **L1 Cache Hints**: use __ldg() for read-only data; forces L1 caching; improves temporal locality; 20-50% speedup for reused data
- **L2 Persistence**: cudaStreamSetAttribute() sets L2 persistence; keeps critical data in L2; benefits data reused across kernels; 30-60% speedup for multi-kernel workloads
- **Cache Line Utilization**: 128-byte cache lines; access consecutive data to utilize full line; 4-8× improvement vs scattered access; structure data for sequential access
- **Streaming Access**: use streaming loads for data accessed once; bypasses L1 cache; prevents cache pollution; improves performance for other data

**Memory Access Patterns:**
- **Coalescing**: threads in warp access consecutive addresses; 128-byte aligned; achieves 100% bandwidth; stride-1 access optimal; stride-2 achieves 50%; stride-32 achieves 3%
- **Structure of Arrays (SoA)**: prefer SoA over AoS; enables coalesced access; 5-10× memory bandwidth improvement; example: x[N], y[N], z[N] instead of point[N].x, point[N].y, point[N].z
- **Alignment**: align data to 128 bytes; cudaMalloc provides automatic alignment; manual alignment with __align__(128); misalignment causes 2-10× slowdown
- **Padding**: add padding to avoid bank conflicts and improve coalescing; 1-2 elements padding typical; 10-30% performance improvement

**Bandwidth Optimization:**
- **Measure Bandwidth**: use Nsight Compute; reports achieved bandwidth vs peak; target 80-100% for memory-bound kernels; identifies bottlenecks
- **Vectorized Loads**: use float4, int4 for 128-bit loads; 2-4× fewer transactions; improves bandwidth utilization; requires aligned data
- **Asynchronous Copy**: async memory copy (compute capability 8.0+); overlaps with compute; 20-50% speedup; uses copy engines separate from compute
- **Prefetching**: load next iteration's data while computing current; hides latency; software pipelining; 15-30% improvement

**Latency Hiding:**
- **High Occupancy**: more active warps hide memory latency; target 50-100% occupancy; balance register and shared memory usage; 256 threads per block typical
- **Instruction-Level Parallelism**: independent operations hide latency; reorder instructions; multiple accumulators; 20-40% improvement
- **Warp Scheduling**: GPU schedules ready warps while others wait for memory; sufficient warps (8-16 per SM) ensure full utilization
- **Memory-Compute Overlap**: structure kernels to overlap memory access with computation; double buffering; asynchronous operations

**Unified Memory:**
- **Automatic Migration**: CUDA Unified Memory migrates pages between CPU and GPU; convenient but slower than explicit management; 2-5× overhead vs explicit
- **Prefetching**: cudaMemPrefetchAsync() prefetches to GPU; reduces page faults; 50-80% of explicit performance; good for prototyping
- **Access Counters**: track which processor accesses data; optimizes placement; reduces migration overhead; improves performance by 30-60%
- **When to Use**: rapid prototyping, irregular access patterns, CPU-GPU collaboration; production code prefers explicit management for performance

**Memory Bandwidth Bottlenecks:**
- **Identification**: Nsight Compute shows memory throughput; <50% of peak indicates memory bound; optimize memory access patterns first
- **Arithmetic Intensity**: FLOPs per byte; low intensity (<10) is memory bound; high intensity (>50) is compute bound; tiling increases intensity
- **Roofline Model**: plots performance vs arithmetic intensity; shows whether memory or compute limited; guides optimization strategy
- **Bandwidth Saturation**: achieved bandwidth / peak bandwidth; target 80-100%; below 50% indicates access pattern problems

**Advanced Techniques:**
- **Shared Memory Atomics**: faster than global atomics; 10-100× speedup; use for reductions within block; warp-level primitives even faster
- **Warp Shuffle**: exchange data between threads in warp; no shared memory needed; 2-5× faster than shared memory; __shfl_sync(), __shfl_down_sync()
- **Cooperative Groups**: flexible synchronization; grid-wide sync; warp-level operations; more expressive than __syncthreads()
- **Multi-Level Tiling**: tile at multiple levels (L2, shared memory, registers); maximizes reuse at each level; 10-30× speedup for complex algorithms

**Profiling and Tuning:**
- **Nsight Compute Metrics**: Memory Throughput, L1/L2 Hit Rate, Global Load/Store Efficiency, Shared Memory Bank Conflicts; guide optimization
- **Memory Replay**: indicates uncoalesced access; high replay (>1.5) means poor coalescing; restructure data layout
- **Occupancy vs Performance**: higher occupancy doesn't always mean better performance; balance with resource usage; profile to find optimal
- **Iterative Optimization**: optimize one aspect at a time; measure impact; memory coalescing first, then shared memory, then registers

**Common Patterns:**
- **Matrix Multiplication**: shared memory tiling; 80-95% of peak; 10-20 TFLOPS on A100; load tiles into shared memory, compute, repeat
- **Reduction**: warp primitives + shared memory; 60-80% of peak bandwidth; 500-1000 GB/s; minimize global memory accesses
- **Stencil**: shared memory halo; load neighbors into shared memory; 70-90% of peak; 1-2 TB/s; reduces redundant global loads
- **Histogram**: shared memory atomics + global atomics; 40-60% of peak; 500-800 GB/s; balance between shared and global atomics

**Best Practices:**
- **Profile First**: identify bottleneck before optimizing; memory or compute bound; use Nsight Compute
- **Coalesce Always**: ensure coalesced access; SoA layout; aligned data; 5-10× improvement
- **Use Shared Memory**: for data reused across threads; 100× faster than global; tile algorithms
- **Balance Resources**: registers, shared memory, occupancy; find optimal trade-off; profile-guided tuning
- **Measure Impact**: verify each optimization improves performance; some optimizations hurt; iterate based on data

GPU Memory Hierarchy Optimization is **the art of data orchestration across multiple storage levels** — by understanding the 1000× performance difference between registers and global memory and applying techniques like shared memory tiling, memory coalescing, and register blocking, developers achieve 5-20× performance improvements and 80-100% of theoretical bandwidth, making memory hierarchy optimization the most critical skill for GPU programming where the vast majority of kernels are memory-bound and proper data placement determines whether applications achieve 5% or 80% of peak performance.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/gpu-memory-hierarchy-optimization) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
