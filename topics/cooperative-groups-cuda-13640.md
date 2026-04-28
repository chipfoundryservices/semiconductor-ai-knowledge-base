# Cooperative Groups

**Keywords**: cooperative groups cuda,thread block groups,grid synchronization,multi device cooperative,cooperative launch cuda

---

**Cooperative Groups** is **the CUDA programming model extension that provides explicit, composable abstractions for thread collectives — enabling synchronization and communication at multiple granularities (thread block, multi-block grid, multi-GPU) through a unified API that replaces implicit assumptions with explicit group objects, supporting advanced patterns like grid-wide synchronization, persistent kernels, and multi-device cooperation**.

**Group Hierarchy:**
- **Thread Block (thread_block)**: represents all threads in a CUDA block; thread_block g = this_thread_block(); provides g.sync() (equivalent to __syncthreads()), g.size(), g.thread_rank(); makes block-level operations explicit and composable
- **Thread Block Tile (thread_block_tile<Size>)**: partitions thread block into tiles of Size threads (typically 32 for warps); auto tile = tiled_partition<32>(this_thread_block()); provides tile.shfl(), tile.any(), tile.all() for warp-level operations with cleaner syntax than intrinsics
- **Grid Group (grid_group)**: represents all threads across all blocks in a kernel launch; grid_group g = this_grid(); enables grid-wide synchronization via g.sync() — all blocks must reach the sync point before any proceed; requires cooperative launch
- **Multi-Grid Group (multi_grid_group)**: spans multiple devices; enables synchronization across GPUs; multi_grid_group g = this_multi_grid(); g.sync() synchronizes all participating GPUs; requires multi-device cooperative launch

**Cooperative Launch:**
- **Single-Device Cooperative Kernel**: cudaLaunchCooperativeKernel() launches kernel with grid-synchronization capability; all blocks must be resident simultaneously on the GPU; maximum grid size limited by SM count and resource usage — typically 100-200 blocks on modern GPUs
- **Occupancy Requirements**: cooperative kernels require sufficient resources (registers, shared memory) to fit all blocks simultaneously; cudaOccupancyMaxActiveBlocksPerMultiprocessor() calculates maximum blocks; total_blocks ≤ SM_count × blocks_per_SM
- **Multi-Device Cooperative Launch**: cudaLaunchCooperativeKernelMultiDevice() launches synchronized kernels across multiple GPUs; requires peer-to-peer access enabled; all GPUs must reach multi_grid.sync() before any proceed
- **Device Support**: query cudaDevAttrCooperativeLaunch and cudaDevAttrCooperativeMultiDeviceLaunch; all modern GPUs (Volta+) support single-device cooperative launch; multi-device requires NVLink or PCIe peer-to-peer

**Advanced Patterns:**
- **Persistent Kernels**: kernel runs for entire application lifetime; grid.sync() between iterations; eliminates kernel launch overhead (5-20 μs per launch); work queue pattern: load work from global queue, process, sync, repeat; achieves <1 μs iteration latency
- **Grid-Wide Reductions**: each block reduces to partial result; grid.sync(); single block reduces partial results; eliminates need for multiple kernel launches; 2-5× faster than launch-based synchronization for small reductions
- **Producer-Consumer**: producer blocks generate data, grid.sync(), consumer blocks process data; enables complex multi-stage pipelines within a single kernel; avoids global memory round-trips through L2 cache persistence
- **Dynamic Parallelism Alternative**: cooperative groups enable parent-child coordination without dynamic parallelism overhead; parent blocks launch work, children process, grid.sync() for coordination; lower overhead than cudaLaunchDevice()

**Tiled Partitioning:**
- **Binary Partitioning**: auto tile = tiled_partition(parent_group); recursively splits groups; enables hierarchical algorithms (multi-level reductions, tree-based operations); each level operates on its partition independently
- **Labeled Partitioning**: auto tile = labeled_partition(parent_group, label); groups threads with the same label; enables data-dependent grouping (e.g., group threads processing the same hash bucket); dynamic work distribution based on runtime data
- **Coalesced Groups**: auto active = coalesced_threads(); groups currently active threads in a warp; handles divergence automatically; enables efficient operations on irregular data (sparse matrices, variable-length sequences)

**Memory Consistency:**
- **Group Synchronization Semantics**: g.sync() provides acquire-release semantics; all memory operations before sync are visible to all threads after sync; ensures correct ordering of shared memory and global memory accesses
- **Fence Operations**: __threadfence_block(), __threadfence(), __threadfence_system() provide memory ordering without synchronization; required when using atomics or lock-free algorithms; cooperative groups sync includes implicit fence
- **Weak Memory Model**: GPUs have relaxed memory consistency; without explicit synchronization or fences, memory operations may be reordered; cooperative groups provide structured synchronization that enforces correct ordering

**Performance Considerations:**
- **Grid Sync Overhead**: grid.sync() requires all blocks to reach the barrier; stragglers (blocks delayed by load imbalance or hardware variation) delay all blocks; overhead typically 1-10 μs depending on grid size and load balance
- **Occupancy Impact**: cooperative launch requires all blocks resident simultaneously; reduces maximum grid size compared to non-cooperative launch; may limit parallelism for resource-intensive kernels
- **Launch Overhead Elimination**: persistent kernels with grid.sync() eliminate 5-20 μs kernel launch overhead; beneficial for fine-grained tasks (<100 μs per iteration); enables microsecond-latency iterative algorithms
- **Multi-Device Sync Cost**: multi_grid.sync() requires cross-GPU communication; NVLink provides 50-100 GB/s bandwidth with ~5 μs latency; PCIe adds 10-20 μs latency; minimize sync frequency in multi-GPU algorithms

**Comparison with Traditional Approaches:**
- **vs __syncthreads()**: cooperative groups make synchronization scope explicit; enable composition (sync within tiles, then sync tiles); provide uniform API across granularities; __syncthreads() is implicit block-level only
- **vs Multiple Kernel Launches**: grid.sync() is 10-100× faster than launching new kernel (1-10 μs vs 5-20 μs); avoids global memory round-trips; maintains L2 cache state across iterations
- **vs Atomics**: cooperative groups enable structured synchronization; atomics provide unstructured coordination; groups have lower overhead for bulk synchronization; atomics better for fine-grained, irregular coordination

Cooperative Groups is **the modern CUDA programming model that makes thread collectives explicit, composable, and scalable — enabling advanced patterns like persistent kernels, grid-wide synchronization, and multi-GPU cooperation that were previously impossible or required complex workarounds, fundamentally expanding the algorithmic possibilities of GPU computing**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/cooperative-groups-cuda-13640) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
