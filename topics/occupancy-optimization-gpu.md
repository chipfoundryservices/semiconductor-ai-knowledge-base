# Occupancy Optimization

**Keywords**: occupancy optimization gpu,register pressure cuda,shared memory occupancy,thread block sizing,occupancy calculator

---

**Occupancy Optimization** is **the technique of maximizing the number of active warps per streaming multiprocessor (SM) to hide memory latency through warp scheduling — balancing register usage, shared memory consumption, and thread block size to achieve 50-100% occupancy (16-64 active warps per SM on modern GPUs), enabling the GPU to switch between warps while some wait for memory, maintaining high compute unit utilization despite 200-400 cycle memory latencies**.

**Occupancy Fundamentals:**
- **Definition**: occupancy = active_warps / max_warps_per_SM; modern GPUs support 32-64 warps per SM (1024-2048 threads); 50% occupancy = 16-32 active warps; higher occupancy provides more warps to hide latency but doesn't always improve performance
- **Latency Hiding**: memory access takes 200-400 cycles; with 32 active warps, the scheduler can switch to a different warp every cycle; requires 200-400 warps to fully hide latency — impossible on single SM, but multiple SMs and instruction-level parallelism help
- **Resource Limits**: occupancy limited by registers per thread, shared memory per block, threads per block, and blocks per SM; the most restrictive resource determines actual occupancy; modern GPUs have 65,536 registers and 100-164 KB shared memory per SM
- **Diminishing Returns**: increasing occupancy from 25% to 50% often provides 20-40% speedup; 50% to 75% provides 5-15% speedup; 75% to 100% provides 0-5% speedup; compute-bound kernels benefit less from high occupancy than memory-bound kernels

**Register Pressure:**
- **Register Allocation**: each SM has 65,536 32-bit registers (Ampere/Hopper); divided among active threads; 64 registers/thread × 1024 threads = 65,536 (100% occupancy); 128 registers/thread limits to 512 threads (50% occupancy)
- **Register Spilling**: when kernel uses >255 registers/thread, excess registers spill to local memory (cached in L1); each spilled register access costs 20-100 cycles vs 1 cycle for register; 10-100× slowdown for register-heavy kernels
- **Compiler Optimization**: use --maxrregcount=N to limit registers; forces compiler to spill or optimize; --maxrregcount=64 may increase occupancy but decrease per-thread performance; balance between occupancy and register spilling
- **Profiling**: nsight compute reports registers_per_thread and achieved_occupancy; compare to theoretical_occupancy; large gap indicates register pressure; check local_memory_overhead for spilling

**Shared Memory Constraints:**
- **Capacity**: 100-164 KB shared memory per SM (configurable); divided among concurrent blocks; 48 KB/block limits to 2 blocks/SM (on 100 KB SM); 16 KB/block allows 6 blocks/SM
- **Configuration**: cudaFuncSetAttribute(kernel, cudaFuncAttributePreferredSharedMemoryCarveout, 50); sets shared memory vs L1 cache split; 50% shared memory = 64 KB on 128 KB SM; adjust based on kernel needs
- **Dynamic Allocation**: kernel<<<blocks, threads, shared_mem_bytes>>> specifies shared memory at launch; enables runtime tuning; but prevents some compiler optimizations; static allocation (__shared__ float data[SIZE]) is preferred when size is known
- **Occupancy Trade-off**: reducing shared memory per block increases blocks per SM; but may reduce per-block performance; optimal balance depends on whether kernel is compute-bound or memory-bound

**Thread Block Sizing:**
- **Warp Alignment**: block size must be multiple of 32 (warp size); 31-thread block wastes 1 thread slot per warp; 64-thread block uses 2 full warps; 96-thread block uses 3 full warps; always use multiples of 32
- **Common Sizes**: 128, 256, 512 threads per block are typical; 256 is often optimal (8 warps); 128 may be better for register-heavy kernels; 512 may be better for simple, memory-bound kernels; 1024 (maximum) rarely optimal due to resource constraints
- **2D/3D Blocks**: blockDim.x × blockDim.y × blockDim.z must be multiple of 32; prefer (32, 8, 1) or (16, 16, 1) for 2D; (8, 8, 8) for 3D; ensures warp alignment and good memory access patterns
- **Grid Size**: total blocks should be 2-4× the number of SMs for load balancing; too few blocks leaves SMs idle; too many blocks is fine (queued and executed as resources become available)

**Occupancy Calculator:**
- **CUDA API**: cudaOccupancyMaxActiveBlocksPerMultiprocessor(&numBlocks, kernel, blockSize, dynamicSharedMem); returns maximum blocks per SM given resource usage; multiply by SMs to get total concurrent blocks
- **Optimal Block Size**: cudaOccupancyMaxPotentialBlockSize(&minGridSize, &blockSize, kernel, dynamicSharedMem, maxBlockSize); suggests block size that maximizes occupancy; starting point for tuning
- **Spreadsheet Calculator**: CUDA toolkit includes Excel spreadsheet; input registers, shared memory, block size; calculates occupancy and identifies limiting resource; useful for manual tuning
- **Nsight Compute**: reports achieved_occupancy, theoretical_occupancy, and limiting factors; shows which resource (registers, shared memory, blocks) limits occupancy; provides optimization suggestions

**Optimization Strategies:**
- **Reduce Register Usage**: simplify expressions, recompute instead of storing, use smaller data types (half instead of float); compiler flag --maxrregcount forces reduction; measure impact on performance (may hurt if causes spilling)
- **Reduce Shared Memory**: use smaller tiles, recompute instead of caching, use registers for thread-private data; balance between shared memory usage and global memory traffic
- **Increase Block Size**: larger blocks improve occupancy if resources allow; but may reduce parallelism if total blocks < SMs; test multiple block sizes (128, 256, 512) and measure performance
- **Kernel Fusion**: combine multiple small kernels into one larger kernel; amortizes launch overhead and improves data reuse; but may increase register pressure; balance between fusion benefits and occupancy loss

**When Occupancy Doesn't Matter:**
- **Compute-Bound Kernels**: if compute units are fully utilized (>80% SM efficiency), higher occupancy won't help; focus on instruction-level parallelism and arithmetic optimization instead
- **High Arithmetic Intensity**: kernels with 100+ FLOPs per memory access are compute-bound; latency is hidden by instruction pipelining; occupancy >25% is often sufficient
- **Tensor Core Workloads**: Tensor Core operations have high throughput and low latency; occupancy >50% provides diminishing returns; focus on Tensor Core utilization instead

Occupancy optimization is **the balancing act between resource usage and parallelism — by carefully tuning register allocation, shared memory consumption, and block size, developers maximize the number of active warps that hide memory latency, achieving 20-50% performance improvements for memory-bound kernels while avoiding the trap of optimizing occupancy at the expense of per-thread efficiency**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/occupancy-optimization-gpu) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
