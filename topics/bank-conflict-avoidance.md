# Bank Conflict Avoidance

**Keywords**: bank conflict avoidance,shared memory bank conflicts,padding shared memory,conflict free access,shared memory optimization

---

**Bank Conflict Avoidance** is **the shared memory optimization technique that eliminates serialization caused by multiple threads simultaneously accessing different addresses within the same memory bank — using padding, address permutation, and access pattern redesign to ensure conflict-free access where all 32 threads in a warp access different banks in parallel, achieving the full 20 TB/s shared memory bandwidth instead of suffering 2-32× slowdowns from bank conflicts**.

**Bank Conflict Mechanism:**
- **Bank Organization**: shared memory is divided into 32 banks (on modern GPUs) with 4-byte width; bank index = (address / 4) % 32; consecutive 4-byte words map to consecutive banks; bank 0 contains addresses 0, 128, 256, ...; bank 1 contains addresses 4, 132, 260, ...
- **Conflict Definition**: when multiple threads in a warp access different addresses in the same bank simultaneously, the accesses serialize; 2-way conflict causes 2× slowdown; 32-way conflict causes 32× slowdown; conflicts are detected and resolved by hardware
- **Broadcast Exception**: all threads reading the same address is conflict-free (broadcast mechanism); hardware detects identical addresses and serves all threads in a single transaction; useful for loading shared constants or parameters
- **Conflict Detection**: nsight compute reports shared_load_bank_conflict and shared_store_bank_conflict; reports number of replays (additional cycles) due to conflicts; zero replays indicates conflict-free access

**Common Conflict Patterns:**
- **Stride-32 Access**: thread i accesses shared[i * 32]; all threads access bank 0 (address 0, 128, 256, ...); 32-way conflict causes 32× slowdown; common in naive matrix transpose and reduction implementations
- **Power-of-2 Strides**: stride-16, stride-8 cause 16-way and 8-way conflicts respectively; any stride that is a divisor of 32 creates conflicts; stride-1 (consecutive access) is always conflict-free
- **Column-Major Access**: accessing shared[col][row] with consecutive threads accessing consecutive rows causes conflicts if row dimension is power-of-2; thread i accesses shared[0][i], shared[1][i], ... — stride equals row dimension
- **Diagonal Access**: accessing shared[i][i] (diagonal elements) is conflict-free if dimension is not a multiple of 32; but shared[i][(i+k)%N] patterns can create conflicts depending on k and N

**Padding Solutions:**
- **Single-Element Padding**: declare shared memory as __shared__ float tile[TILE_SIZE][TILE_SIZE+1]; adds one element per row; shifts each row to start at a different bank offset; eliminates conflicts in transpose operations
- **Padding Calculation**: for dimension N, pad to N+1 if N is power-of-2 or multiple of 32; for non-power-of-2 dimensions, padding may not be necessary; measure with profiler to confirm
- **Memory Overhead**: padding 32×32 tile to 32×33 adds 3% memory overhead; padding 64×64 to 64×65 adds 1.5% overhead; negligible cost for large performance gain (10-30× speedup in conflict-heavy kernels)
- **Multi-Dimensional Padding**: for 3D arrays, pad innermost dimension; __shared__ float data[D1][D2][D3+1]; padding only the innermost dimension is sufficient to eliminate most conflicts

**Access Pattern Redesign:**
- **Transpose in Shared Memory**: load data in row-major order (coalesced global memory access), store in column-major order (or vice versa); use padding to avoid conflicts during the transpose; enables coalesced access in both global and shared memory
- **Cyclic Distribution**: distribute data across banks using modulo arithmetic; address = (row * stride + col) where stride is coprime to 32; ensures different rows map to different bank patterns
- **Swizzling**: XOR-based address permutation; address = row * N + (col ^ (row & mask)); used in CUTLASS and high-performance libraries; eliminates conflicts without padding but requires complex addressing
- **Sequential Addressing in Reductions**: in later iterations of parallel reduction, use sequential addressing (thread i accesses shared[i] and shared[i + stride]) instead of interleaved (thread i accesses shared[2*i] and shared[2*i+1]); eliminates conflicts as active threads decrease

**Matrix Transpose Example:**
- **Naive Transpose**: load tile in row-major (coalesced), store in column-major (coalesced); but reading from shared memory for column-major write causes conflicts; __shared__ float tile[32][32]; tile[threadIdx.y][threadIdx.x] = input[...]; output[...] = tile[threadIdx.x][threadIdx.y]; — second access has conflicts
- **Padded Transpose**: __shared__ float tile[32][33]; eliminates conflicts; each row starts at different bank offset; column-major read becomes conflict-free; achieves 80-90% of peak shared memory bandwidth
- **Performance Impact**: naive transpose: 50-100 GB/s; padded transpose: 800-1200 GB/s; 10-20× speedup from single-element padding; critical for high-performance linear algebra kernels

**Reduction Optimization:**
- **Interleaved Addressing (Bad)**: for (int s=1; s<blockDim.x; s*=2) {if (tid % (2*s) == 0) shared[tid] += shared[tid + s];} — creates bank conflicts as stride increases; thread 0, 2, 4, ... access banks 0, 2, 4, ... with stride-2 pattern
- **Sequential Addressing (Good)**: for (int s=blockDim.x/2; s>0; s>>=1) {if (tid < s) shared[tid] += shared[tid + s];} — consecutive active threads access consecutive addresses; conflict-free throughout; 2-4× faster than interleaved
- **Warp-Level Reduction**: use shuffle operations instead of shared memory for final warp (32 elements); eliminates shared memory access entirely; combined with sequential addressing achieves optimal reduction performance

**Profiling and Validation:**
- **Nsight Compute Metrics**: shared_load_transactions and shared_store_transactions show actual transaction count; compare to theoretical minimum (number of warps × accesses per thread); ratio >1 indicates conflicts
- **Replay Overhead**: shared_load_bank_conflict_replays / shared_load_transactions shows conflict severity; 0% is perfect; >50% indicates serious conflict problems requiring redesign
- **Bandwidth Measurement**: measure effective shared memory bandwidth; compare to peak 20 TB/s (per SM); conflict-free kernels achieve 15-18 TB/s; conflicted kernels achieve 1-5 TB/s

Bank conflict avoidance is **the shared memory optimization that transforms slow, serialized access into parallel, high-bandwidth operations — by adding strategic padding, redesigning access patterns, or using address swizzling, developers eliminate 2-32× performance penalties and achieve the full potential of shared memory, making conflict-free access essential for any kernel that relies on shared memory for performance**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/bank-conflict-avoidance) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
