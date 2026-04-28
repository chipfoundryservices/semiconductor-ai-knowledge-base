# GPU Atomic Operations

**Keywords**: gpu atomic operations,cuda atomics performance,atomic memory operations,gpu synchronization primitives,cuda atomic optimization

---

**GPU Atomic Operations** are **the hardware-supported read-modify-write operations that enable thread-safe updates to shared memory locations without explicit locking** — including atomicAdd, atomicMax, atomicMin, atomicCAS (compare-and-swap), atomicExch that guarantee indivisible execution even with thousands of concurrent threads, achieving 100-500 GB/s throughput for low-contention scenarios but degrading to 1-10 GB/s under high contention (1000+ threads accessing same location), making atomic optimization critical for algorithms like histograms, reductions, and graph processing where proper techniques like warp aggregation (reduces atomic calls by 32×), hierarchical atomics (block-level then global), and atomic-free alternatives (warp primitives, privatization) can improve performance by 5-100× and determine whether applications achieve 10% or 80% of theoretical throughput.


**Atomic Operation Types:**
- **Arithmetic**: atomicAdd, atomicSub; add/subtract value; most common; FP32, FP64, INT32, INT64 supported
- **Bitwise**: atomicAnd, atomicOr, atomicXor; bitwise operations; useful for flags, bitmasks; INT32, INT64 only
- **Comparison**: atomicMin, atomicMax; update if new value is min/max; useful for reductions; FP32, INT32, INT64
- **Exchange**: atomicExch; unconditional swap; atomicCAS (compare-and-swap); conditional swap; building block for complex atomics

**Performance Characteristics:**
- **Low Contention**: 100-500 GB/s throughput; few threads per location; near-optimal performance; <10 threads per location
- **Medium Contention**: 10-100 GB/s; 10-100 threads per location; serialization begins; performance degrades linearly
- **High Contention**: 1-10 GB/s; 100-1000+ threads per location; severe serialization; 10-100× slowdown
- **Latency**: 100-400 cycles per atomic; hidden by high occupancy; but serialization makes latency visible

**Atomic Scopes:**
- **Global Atomics**: atomicAdd(&global_var, val); visible to all threads across all blocks; slowest; highest contention
- **Block Atomics**: atomicAdd_block(&shared_var, val); visible within block; 10-100× faster than global; lower contention
- **System Atomics**: atomicAdd_system(&var, val); visible to CPU and GPU; slowest; use for CPU-GPU coordination
- **Warp Atomics**: warp aggregation + single atomic; 32× fewer atomics; 5-20× faster than per-thread atomics

**Warp Aggregation:**
- **Pattern**: reduce within warp using __shfl_down_sync(); lane 0 performs single atomic; 32× fewer atomic operations
- **Code**: int sum = warp_reduce(val); if (lane == 0) atomicAdd(&global_counter, sum);
- **Performance**: 5-20× faster than per-thread atomics; 300-600 GB/s vs 10-50 GB/s; critical optimization
- **Use Cases**: histograms, counters, reductions; any accumulation pattern; 40-70% of peak bandwidth

**Hierarchical Atomics:**
- **Two-Level**: warp aggregation → block-level atomic (shared memory) → global atomic; 100-1000× fewer global atomics
- **Pattern**: warp reduces to shared memory; block reduces shared memory; single thread performs global atomic
- **Performance**: 10-50× faster than direct global atomics; 400-800 GB/s; near-optimal for high contention
- **Use Cases**: global histograms, global counters; any global accumulation; 50-80% of peak bandwidth

**Privatization:**
- **Concept**: each thread/warp/block maintains private copy; merge at end; eliminates contention during computation
- **Pattern**: private histogram per block in shared memory; merge to global at end; 10-100× fewer atomics
- **Performance**: 5-50× faster than direct global atomics; 500-1000 GB/s during computation; merge cost amortized
- **Use Cases**: histograms with many bins, sparse accumulation; any pattern with high contention

**Atomic-Free Alternatives:**
- **Warp Primitives**: __shfl, __ballot for warp-level operations; 10-100× faster than atomics; no contention
- **Reductions**: use warp primitives + shared memory; 2-10× faster than atomic reductions; 500-1000 GB/s
- **Scan**: prefix sum without atomics; 400-800 GB/s; 2-5× faster than atomic accumulation
- **Sorting**: sort then reduce; 100-300 GB/s; faster than atomic histogram for some patterns

**Histogram Optimization:**
- **Naive**: per-thread atomicAdd to global histogram; 1-10 GB/s; severe contention; 100-1000× slower than optimal
- **Warp Aggregation**: warp reduces, lane 0 atomics; 5-20× faster; 50-200 GB/s; simple optimization
- **Privatization**: per-block histogram in shared memory; merge at end; 10-50× faster; 300-600 GB/s; best for many bins
- **Hybrid**: warp aggregation + privatization; 20-100× faster; 500-1000 GB/s; optimal for most cases

**Compare-and-Swap (CAS):**
- **Atomic CAS**: atomicCAS(&addr, compare, val); updates if current value equals compare; returns old value
- **Use Cases**: lock-free data structures, custom atomics, conditional updates; building block for complex operations
- **Performance**: same as other atomics; 100-500 GB/s low contention, 1-10 GB/s high contention
- **Pattern**: do { old = *addr; new = f(old); } while (atomicCAS(addr, old, new) != old); retry loop for complex updates

**Floating-Point Atomics:**
- **FP32 Add**: atomicAdd(&fp32_var, val); native on compute capability 2.0+; same performance as integer
- **FP64 Add**: atomicAdd(&fp64_var, val); native on compute capability 6.0+; same performance as FP32
- **FP16**: no native support; use atomicCAS with conversion; 2-5× slower; or use integer atomics on bits
- **Precision**: atomics are exact; no rounding errors from parallelism; but order non-deterministic

**Memory Ordering:**
- **Relaxed**: default; no ordering guarantees; fastest; sufficient for most cases
- **Acquire/Release**: memory fence semantics; ensures visibility; use for synchronization; slight overhead
- **Sequential Consistency**: strongest guarantees; highest overhead; rarely needed; use explicit fences instead
- **Scope**: block, device, system; determines visibility; narrower scope is faster

**Contention Reduction:**
- **Warp Aggregation**: 32× fewer atomics; 5-20× speedup; always use for high contention
- **Privatization**: per-block copies; 10-100× fewer global atomics; 10-50× speedup
- **Randomization**: randomize access order; reduces hot spots; 20-40% improvement for some patterns
- **Padding**: pad arrays to avoid false sharing; 128-byte alignment; 10-30% improvement

**Profiling Atomics:**
- **Nsight Compute**: Atomic Throughput metric; shows achieved throughput; identifies contention
- **Atomic Replay**: indicates serialization; high replay (>10) means severe contention; optimize access pattern
- **Memory Throughput**: low throughput with atomics indicates contention; compare with non-atomic version
- **Warp Stall**: atomic stalls show in warp state statistics; high stalls indicate contention

**Common Patterns:**
- **Counter**: global counter; warp aggregation essential; 5-20× speedup; 300-600 GB/s
- **Histogram**: per-block privatization + merge; 10-50× speedup; 500-1000 GB/s; critical for performance
- **Reduction**: warp primitives + block atomics; 2-10× speedup; 500-1000 GB/s; faster than pure atomics
- **Max/Min**: atomicMax/atomicMin; warp aggregation helps; 5-20× speedup; 300-600 GB/s

**Best Practices:**
- **Warp Aggregation**: always aggregate within warp before atomic; 5-20× speedup; 32× fewer atomics
- **Hierarchical**: use block-level atomics before global; 10-50× speedup; 100-1000× fewer global atomics
- **Privatization**: per-block copies for high contention; 10-50× speedup; merge cost amortized
- **Avoid When Possible**: use warp primitives, reductions, scans instead; 10-100× faster; no contention
- **Profile**: measure atomic throughput; identify contention; optimize based on data

**Performance Targets:**
- **Low Contention**: 100-500 GB/s; <10 threads per location; near-optimal performance
- **With Warp Aggregation**: 300-600 GB/s; 5-20× speedup; 32× fewer atomics
- **With Privatization**: 500-1000 GB/s; 10-50× speedup; near-optimal for high contention
- **Atomic Replay**: <2 ideal; <5 acceptable; >10 indicates severe contention; optimize

**Real-World Examples:**
- **Histogram**: privatization + warp aggregation; 500-1000 GB/s; 20-100× faster than naive; 50-80% of peak
- **Graph Algorithms**: atomic updates to vertex data; warp aggregation critical; 300-600 GB/s; 5-20× speedup
- **Particle Simulation**: atomic updates to grid cells; privatization helps; 400-800 GB/s; 10-50× speedup
- **Sparse Matrix**: atomic accumulation; warp aggregation essential; 300-600 GB/s; 5-20× speedup

GPU Atomic Operations represent **the necessary evil of parallel programming** — while enabling thread-safe updates without explicit locking, atomics suffer from severe performance degradation under high contention (1-10 GB/s vs 100-500 GB/s), making optimization techniques like warp aggregation (32× fewer atomics), hierarchical atomics (100-1000× fewer global atomics), and atomic-free alternatives (warp primitives, privatization) essential for achieving 5-100× performance improvement and determining whether applications achieve 10% or 80% of theoretical throughput where proper atomic optimization is the difference between unusable and production-ready performance.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/gpu-atomic-operations) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
