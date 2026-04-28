# Shared Memory Programming Patterns

**Keywords**: shared memory programming patterns,cuda shared memory,cooperative loading threads,shared memory synchronization,tile based computation

---

**Shared Memory Programming Patterns** are **the algorithmic techniques that exploit the fast, programmer-managed shared memory (20 TB/s, 128 KB per SM) available to thread blocks in CUDA — enabling efficient data sharing, reduction operations, and cooperative computation by loading data once from slow global memory and reusing it many times within the block, achieving 10-100× speedups for memory-bound kernels**.

**Fundamental Patterns:**
- **Cooperative Data Loading**: all threads in a block collaboratively load a tile of data from global memory into shared memory; each thread loads one or more elements using its thread ID to compute the source address; __syncthreads() barrier ensures all threads complete loading before any thread begins computation on the shared data
- **Data Reuse Through Tiling**: decompose large problem into tiles that fit in shared memory; matrix multiplication tile (32×32 elements = 4 KB) is loaded once and reused 32 times in dot product computation; without tiling, each element would be loaded 32 times from global memory — 32× bandwidth reduction
- **Halo Exchange**: stencil operations require neighbor data; load tile plus halo region (boundary elements from adjacent tiles) into shared memory; threads at tile boundaries load extra elements; enables all threads to access neighbors from shared memory without additional global reads
- **Privatization**: each thread block maintains private accumulation buffers in shared memory; threads update local buffers without contention; final reduction combines per-block results; avoids expensive atomic operations to global memory during accumulation phase

**Synchronization Patterns:**
- **Barrier Synchronization (__syncthreads)**: ensures all threads in a block reach the barrier before any proceed; required after cooperative loading (before computation) and before writing results (after computation); incorrect barrier placement causes race conditions or deadlock
- **Warp-Synchronous Programming**: threads within a warp (32 threads) execute in lockstep on Volta+ architectures; can share data through shared memory without explicit synchronization if access patterns are carefully designed; dangerous and architecture-dependent — use __syncwarp() for explicit warp-level synchronization
- **Double Buffering**: overlap computation on one tile with loading of the next tile; requires two shared memory buffers; while threads compute on buffer A, they load next tile into buffer B; alternate buffers each iteration; hides memory latency behind computation
- **Conditional Synchronization**: __syncthreads() must be reached by all threads in the block or none; placing __syncthreads() inside an if statement that not all threads execute causes deadlock; use predication (compute but discard results) instead of branching around barriers

**Advanced Patterns:**
- **Parallel Reduction**: sum/max/min across thread block using shared memory tree reduction; iteration k: threads 0 to N/(2^k) add pairs from shared memory; log₂(N) iterations reduce N elements; final result in shared[0]; bank conflict-free implementation uses sequential addressing in later iterations
- **Parallel Scan (Prefix Sum)**: compute cumulative sum using up-sweep (reduction) and down-sweep (distribution) phases; requires 2×log₂(N) iterations; enables parallel stream compaction, radix sort, and dynamic work allocation; Blelloch scan algorithm is work-efficient O(N) vs naive O(N log N)
- **Transpose**: load tile in row-major order, write in column-major order (or vice versa); naive implementation suffers bank conflicts (all threads in warp access same bank); padding shared memory array by 1 element shifts columns to different banks: __shared__ float tile[TILE_SIZE][TILE_SIZE+1]
- **Histogram**: each block computes local histogram in shared memory using atomic operations; shared memory atomics are 10-100× faster than global atomics; final reduction combines per-block histograms; privatization reduces contention by giving each warp its own histogram copy

**Memory Layout Considerations:**
- **Bank Conflicts**: shared memory has 32 banks (4-byte width on modern GPUs); simultaneous access to different addresses in the same bank by multiple threads serializes — 32-way conflict causes 32× slowdown; stride-32 access patterns (common in matrix operations) create conflicts
- **Conflict-Free Access**: stride-1 access (consecutive threads access consecutive addresses) is conflict-free; padding arrays to non-power-of-2 width eliminates conflicts in transpose and matrix operations; conflict-free addressing formulas: address = (row * (TILE_SIZE+1) + col)
- **Broadcast**: all threads reading the same address is conflict-free (broadcast mechanism); useful for loading constants or shared parameters; single transaction serves all threads in the warp
- **Capacity**: 48-164 KB shared memory per SM (configurable); must be divided among concurrent blocks; using 64 KB per block limits occupancy to 2 blocks per SM (on 128 KB SM); balance shared memory usage vs occupancy for optimal performance

**Performance Optimization:**
- **Occupancy vs Shared Memory**: more shared memory per block reduces occupancy (fewer concurrent blocks per SM); lower occupancy reduces latency hiding; optimal balance depends on compute vs memory intensity — compute-bound kernels tolerate lower occupancy, memory-bound kernels need high occupancy
- **Dynamic vs Static Allocation**: static allocation (__shared__ float data[SIZE]) determined at compile time; dynamic allocation (extern __shared__ float data[]) specified at kernel launch; dynamic allocation enables runtime tuning but prevents compiler optimizations
- **Shared Memory Bandwidth**: 128 KB shared memory with 20 TB/s bandwidth = 160 GB/s per KB; fully utilizing shared memory bandwidth requires high arithmetic intensity (many operations per loaded element); matrix multiplication achieves 100+ FLOPs per shared memory access

Shared memory programming patterns are **the essential techniques that transform GPU kernels from memory-bound to compute-bound — by carefully orchestrating cooperative data loading, synchronization, and reuse, developers can reduce global memory traffic by 10-100× and achieve performance within 80-90% of theoretical peak, making shared memory mastery the hallmark of expert CUDA programming**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/shared-memory-programming-patterns) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
