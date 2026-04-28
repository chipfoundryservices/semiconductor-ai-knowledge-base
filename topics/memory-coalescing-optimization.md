# Memory Coalescing Optimization

**Keywords**: memory coalescing optimization,coalesced memory access,structure of arrays soa,memory access patterns gpu,stride memory access

---

**Memory Coalescing Optimization** is **the critical technique of arranging memory access patterns so that threads within a warp access consecutive memory addresses — enabling the GPU to combine 32 individual memory requests into a single 128-byte transaction, achieving 32× bandwidth efficiency compared to non-coalesced access where each thread generates a separate transaction, making coalescing the single most important factor in memory-bound kernel performance**.

**Coalescing Fundamentals:**
- **Warp Memory Transactions**: when threads in a warp access global memory, the hardware coalesces requests into 32-byte, 64-byte, or 128-byte transactions; perfectly coalesced access (32 threads accessing consecutive 4-byte words) generates one 128-byte transaction; non-coalesced access generates up to 32 separate 32-byte transactions
- **Alignment Requirements**: transactions are aligned to their size (128-byte transaction must start at 128-byte boundary); misaligned access spanning a boundary requires multiple transactions; cudaMalloc guarantees 256-byte alignment; manual allocation should align to at least 128 bytes
- **Access Patterns**: stride-1 pattern (thread i accesses address base + i×sizeof(element)) is perfectly coalesced; stride-2 wastes 50% bandwidth (loads 2× required data); stride-32 generates 32 separate transactions (32× bandwidth waste); random access is worst case
- **Bandwidth Impact**: coalesced access achieves 70-90% of peak HBM bandwidth (1.3-1.7 TB/s on A100); non-coalesced access achieves 5-10% of peak (50-100 GB/s); 10-20× performance difference for memory-bound kernels

**Structure of Arrays (SoA) vs Array of Structures (AoS):**
- **AoS Layout**: struct Particle {float x, y, z, vx, vy, vz;}; Particle particles[N]; thread i accessing particles[i].x generates stride-6 access (each thread skips 5 floats to next x); only 1/6 of loaded data is used — 6× bandwidth waste
- **SoA Layout**: struct Particles {float x[N], y[N], z[N], vx[N], vy[N], vz[N];}; thread i accessing x[i] generates stride-1 access; perfectly coalesced; all loaded data is used; 6× bandwidth improvement over AoS
- **Conversion Cost**: converting AoS to SoA requires data restructuring; one-time cost amortized over many kernel launches; for persistent data structures, SoA is always preferred; for temporary data, consider access patterns
- **Hybrid Approaches**: SoA for frequently accessed fields, AoS for rarely accessed fields; struct {float3 position[N]; float3 velocity[N]; ComplexData metadata[N];} balances coalescing with data locality

**Access Pattern Optimization:**
- **Transpose for Coalescing**: if algorithm naturally produces column-major access (stride-N), transpose data to row-major; transpose kernel cost (1-2 ms for 1M elements) amortized over many accesses; shared memory transpose avoids bank conflicts
- **Padding for Alignment**: add padding to ensure each row starts at aligned boundary; for 2D arrays, pad width to multiple of 32 or 64 elements; prevents misalignment from odd-sized rows; small memory overhead (1-3%) for large bandwidth gain
- **Vectorized Loads**: use float4, int4 for loading 16 bytes per thread; reduces instruction count and improves coalescing; thread i loads float4 at address base + i×16; requires 16-byte alignment; 2-4× speedup for bandwidth-bound kernels
- **Texture Memory**: texture cache optimized for 2D spatial locality; use for non-coalesced access patterns (e.g., image filtering with arbitrary strides); provides 2-4× speedup over global memory for irregular access; limited to read-only data

**Bank Conflict Avoidance (Shared Memory):**
- **Bank Structure**: shared memory divided into 32 banks (4-byte width); simultaneous access to different addresses in the same bank by multiple threads serializes; N-way conflict causes N× slowdown (up to 32×)
- **Conflict Patterns**: stride-32 access (thread i accesses address i×32) causes 32-way conflict (all threads access bank 0); stride-1 access is conflict-free; power-of-2 strides often create conflicts due to bank count (32)
- **Padding Solution**: add 1 element to each row; float shared[TILE_SIZE][TILE_SIZE+1]; shifts columns to different banks; eliminates conflicts in matrix transpose; minimal memory overhead (3% for 32×32 tile)
- **Broadcast Exception**: all threads reading the same address is conflict-free (broadcast mechanism); useful for loading shared constants; single transaction serves all threads

**Profiling and Diagnosis:**
- **Global Memory Efficiency**: nsight compute reports gld_efficiency and gst_efficiency; target >80% for coalesced access; <50% indicates non-coalesced patterns; metric shows percentage of loaded data actually used
- **L1/L2 Cache Hit Rates**: high L1 hit rate (>80%) can mask coalescing issues; disable L1 caching (compile with -Xptxas -dlcm=cg) to measure true coalescing efficiency; L2 hit rate >60% indicates good temporal locality
- **Memory Throughput**: compare achieved memory throughput to peak bandwidth; coalesced kernels reach 70-90% of peak; non-coalesced kernels reach 5-20% of peak; large gap indicates coalescing problems
- **Warp Stall Reasons**: nsight compute shows stall reasons; high "memory throttle" or "long scoreboard" stalls indicate memory bottleneck; combined with low memory efficiency confirms coalescing issues

**Advanced Techniques:**
- **Swizzling**: permute memory addresses to improve cache utilization; used in CUTLASS for GEMM; complex addressing but eliminates bank conflicts and improves L2 hit rate; 10-20% speedup for large matrix operations
- **Sector Caching**: Ampere+ GPUs cache in 32-byte sectors; partial coalescing (e.g., stride-2) still benefits from sector caching; less severe penalty than pre-Ampere architectures
- **Async Copy**: cp.async instruction bypasses L1 cache and loads directly to shared memory; improves coalescing by avoiding L1 cache line conflicts; used in high-performance GEMM implementations

Memory coalescing optimization is **the foundational technique that determines whether GPU kernels achieve 10% or 90% of peak memory bandwidth — by restructuring data layouts from AoS to SoA, ensuring stride-1 access patterns, and eliminating bank conflicts, developers unlock 10-30× performance improvements, making coalescing mastery the first and most important optimization for any memory-bound GPU kernel**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/memory-coalescing-optimization) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
