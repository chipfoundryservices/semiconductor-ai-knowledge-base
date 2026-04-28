# Cooperative Groups

**Keywords**: cooperative groups cuda,cuda thread synchronization,grid wide sync,warp level primitives,flexible cuda synchronization

---

**Cooperative Groups** is **the CUDA programming model extension that provides flexible, composable thread synchronization primitives beyond __syncthreads()** — enabling synchronization at multiple granularities (thread block, grid, warp, tile) through a unified API that supports grid-wide barriers (all threads across all blocks), warp-level operations (__shfl, __ballot), and arbitrary thread groupings, achieving 2-10× performance improvement over traditional synchronization through reduced overhead and better expressiveness, making Cooperative Groups essential for advanced GPU algorithms like multi-block reductions, dynamic parallelism alternatives, and warp-specialized kernels where __syncthreads() is insufficient and manual synchronization is error-prone and inefficient.


**Cooperative Groups Hierarchy:**
- **Thread Block Group**: equivalent to __syncthreads(); synchronizes all threads in block; this_thread_block(); most common usage
- **Grid Group**: synchronizes all threads across all blocks; requires cooperative launch; this_grid(); enables multi-block algorithms
- **Warp Group**: synchronizes threads in warp (32 threads); tiled_partition<32>(); implicit synchronization; warp-level primitives
- **Tile Group**: arbitrary power-of-2 subset of threads; tiled_partition<N>(); flexible grouping; N = 1, 2, 4, 8, 16, 32

**Thread Block Groups:**
- **Creation**: auto block = this_thread_block(); represents current thread block; 128-1024 threads typical
- **Synchronization**: block.sync(); equivalent to __syncthreads(); explicit barrier; all threads must reach
- **Size Query**: block.size(); returns number of threads in block; block.thread_rank(); returns thread index within block
- **Use Cases**: shared memory synchronization, block-level reductions, cooperative loading; same as traditional __syncthreads()

**Grid Groups:**
- **Creation**: auto grid = this_grid(); represents all threads in grid; requires cooperative launch
- **Cooperative Launch**: cudaLaunchCooperativeKernel(); all blocks must fit on GPU simultaneously; limited by SM count
- **Grid Sync**: grid.sync(); synchronizes all threads across all blocks; expensive (100-1000 μs); use sparingly
- **Use Cases**: multi-block reductions, global barriers, iterative algorithms requiring global consistency; 20-50% faster than multi-kernel approach

**Warp Groups:**
- **Creation**: auto warp = tiled_partition<32>(block); represents 32-thread warp; implicit synchronization
- **Warp Primitives**: warp.shfl(), warp.ballot(), warp.any(), warp.all(); efficient warp-level operations; 2-10× faster than shared memory
- **No Explicit Sync**: warp operations implicitly synchronized; no need for sync() call; SIMT execution model
- **Use Cases**: warp-level reductions, prefix sums, data exchange; 2-5× faster than shared memory for small data

**Tile Groups:**
- **Creation**: auto tile = tiled_partition<N>(block); N = 1, 2, 4, 8, 16, 32; power-of-2 sizes only
- **Synchronization**: tile.sync(); synchronizes threads in tile; lower overhead than block sync; 2-5× faster for small tiles
- **Shuffle**: tile.shfl(), tile.shfl_down(), tile.shfl_up(), tile.shfl_xor(); exchange data within tile; no shared memory needed
- **Use Cases**: hierarchical algorithms, multi-level reductions, flexible parallelism; 20-40% performance improvement

**Warp-Level Primitives:**
- **Shuffle**: tile.shfl(var, srcLane); broadcasts from source lane to all lanes; 2-10× faster than shared memory
- **Shuffle Down**: tile.shfl_down(var, delta); shifts data down by delta lanes; useful for reductions; tree-based patterns
- **Shuffle Up**: tile.shfl_up(var, delta); shifts data up by delta lanes; prefix sum patterns
- **Shuffle XOR**: tile.shfl_xor(var, mask); butterfly exchange pattern; FFT, bitonic sort; optimal communication

**Collective Operations:**
- **Ballot**: tile.ballot(predicate); returns bitmask of predicate results; identifies active threads; 10-100× faster than shared memory
- **Any**: tile.any(predicate); returns true if any thread's predicate is true; early exit optimization
- **All**: tile.all(predicate); returns true if all threads' predicate is true; convergence detection
- **Match**: tile.match_any(value), tile.match_all(value); finds threads with same value; grouping operations

**Reduction Patterns:**
- **Warp Reduction**: use shfl_down() in loop; log2(32) = 5 iterations; 2-5× faster than shared memory; no synchronization needed
- **Block Reduction**: warp reduction + shared memory for inter-warp; 20-40% faster than pure shared memory
- **Grid Reduction**: cooperative groups grid sync; single-kernel multi-block reduction; 20-50% faster than multi-kernel
- **Performance**: warp reduction 500-1000 GB/s; block reduction 300-600 GB/s; grid reduction 200-400 GB/s

**Grid-Wide Synchronization:**
- **Cooperative Launch**: cudaLaunchCooperativeKernel(); ensures all blocks resident simultaneously; required for grid.sync()
- **Grid Sync Cost**: 100-1000 μs depending on GPU size; expensive but cheaper than kernel launch (5-20 ms with data transfer)
- **Use Cases**: iterative algorithms (Jacobi, conjugate gradient), global reductions, multi-block algorithms
- **Limitations**: all blocks must fit on GPU; limits grid size; check cudaDevAttrCooperativeLaunch

**Partitioning Strategies:**
- **Static Partitioning**: tiled_partition<N>() at compile time; N known at compile; optimal performance
- **Dynamic Partitioning**: tiled_partition(block, N) at runtime; N determined dynamically; 10-20% overhead
- **Hierarchical**: partition block into warps, warps into tiles; multi-level algorithms; 20-40% performance improvement
- **Coalesced Groups**: coalesced_threads(); groups active threads; handles divergence; useful for irregular algorithms

**Performance Benefits:**
- **Reduced Overhead**: warp-level operations 2-10× faster than shared memory; no memory traffic; register-based
- **Better Expressiveness**: explicit grouping clarifies intent; easier to reason about; fewer bugs
- **Flexibility**: arbitrary groupings enable new algorithms; not limited to block-level sync; 20-50% performance improvement
- **Composability**: groups can be nested, partitioned, combined; modular algorithm design

**Memory Consistency:**
- **Fence Operations**: tile.sync() includes memory fence; ensures visibility of memory operations; critical for correctness
- **Scope**: block-level fence for block groups; grid-level fence for grid groups; warp-level implicit
- **Ordering**: operations before sync() visible to all threads after sync(); sequential consistency within group

**Use Cases and Patterns:**
- **Warp-Level Reduction**: sum, max, min across warp; 2-5× faster than shared memory; 5-10 lines of code
- **Multi-Block Reduction**: grid.sync() enables single-kernel reduction; 20-50% faster than multi-kernel; simpler code
- **Prefix Sum**: warp shuffle for intra-warp, shared memory for inter-warp; 30-60% faster than pure shared memory
- **Histogram**: warp-level atomics + block-level atomics; 40-70% faster than global atomics; reduces contention

**Integration with Existing Code:**
- **Backward Compatible**: this_thread_block().sync() equivalent to __syncthreads(); drop-in replacement
- **Incremental Adoption**: replace __syncthreads() with cooperative groups gradually; mix old and new code
- **Performance**: no overhead vs __syncthreads() for block-level sync; benefits come from warp-level and grid-level operations
- **Compilation**: requires C++11; --std=c++11 flag; supported on compute capability 3.0+

**Advanced Patterns:**
- **Warp Specialization**: different warps perform different tasks; reduces divergence; 20-40% speedup for heterogeneous workloads
- **Hierarchical Reduction**: warp reduction → block reduction → grid reduction; optimal at each level; 30-60% faster than flat reduction
- **Dynamic Grouping**: coalesced_threads() groups active threads; handles divergence; useful for irregular algorithms
- **Multi-Level Tiling**: partition at multiple levels; cache blocking; 20-50% performance improvement

**Debugging and Profiling:**
- **Nsight Compute**: shows warp efficiency, divergence; identifies synchronization bottlenecks; guides optimization
- **Assertions**: use assert() within groups; helps catch synchronization bugs; disabled in release builds
- **CUDA_LAUNCH_BLOCKING=1**: serializes operations; easier debugging; disables async; use only for debugging
- **Validation**: verify group sizes, ranks; check cooperative launch support; cudaDevAttrCooperativeLaunch

**Limitations:**
- **Cooperative Launch**: requires all blocks fit on GPU; limits grid size; check device capability
- **Warp Size**: assumes 32-thread warps; future GPUs may differ; use warp_size() for portability
- **Divergence**: tile operations assume convergence; divergent tiles may have undefined behavior; use coalesced_threads() for divergence
- **Overhead**: dynamic partitioning has 10-20% overhead; prefer static partitioning when possible

**Best Practices:**
- **Use Warp Primitives**: prefer shfl over shared memory for warp-level operations; 2-10× faster; no memory traffic
- **Static Partitioning**: use compile-time tile sizes when possible; eliminates overhead; optimal performance
- **Grid Sync Sparingly**: grid.sync() expensive; use only when necessary; consider multi-kernel alternative
- **Profile**: use Nsight Compute to verify performance improvement; measure warp efficiency; target >90%
- **Explicit Groups**: use cooperative groups instead of implicit assumptions; clearer code; easier maintenance

**Performance Targets:**
- **Warp Reduction**: 500-1000 GB/s; 2-5× faster than shared memory; 5-10 lines of code
- **Block Reduction**: 300-600 GB/s; 20-40% faster than pure shared memory; optimal for 256-512 threads
- **Grid Reduction**: 200-400 GB/s; 20-50% faster than multi-kernel; single-kernel simplicity
- **Warp Efficiency**: >90% with cooperative groups; reduced divergence; better resource utilization

**Real-World Examples:**
- **Reduction**: warp shuffle + block sync; 2-5× faster than pure shared memory; 60-80% of peak bandwidth
- **Scan/Prefix Sum**: hierarchical with warp shuffle; 30-60% faster; 400-800 GB/s
- **Histogram**: warp-level atomics; 40-70% faster than global atomics; 300-600 GB/s
- **Matrix Multiplication**: warp-level data exchange; 10-20% faster; 80-95% of peak TFLOPS

Cooperative Groups represent **the evolution of CUDA synchronization** — by providing flexible, composable primitives that work at multiple granularities from warp to grid, developers achieve 2-10× performance improvement over traditional __syncthreads() and enable algorithms that were previously impossible or inefficient, making Cooperative Groups essential for modern GPU programming where warp-level operations eliminate memory traffic and grid-wide synchronization enables single-kernel multi-block algorithms that are 20-50% faster than multi-kernel approaches.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/cooperative-groups-cuda) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
