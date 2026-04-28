# GPU Reduction Patterns

**Keywords**: gpu reduction patterns,parallel reduction cuda,warp reduction optimization,cuda reduce performance,hierarchical reduction gpu

---

**GPU Reduction Patterns** are **the parallel algorithms for combining array elements into single value through associative operations** — where hierarchical reduction using warp primitives (__shfl_down_sync) for intra-warp (500-1000 GB/s), shared memory for inter-warp (300-600 GB/s), and atomic operations for inter-block (200-400 GB/s) achieves 60-80% of peak memory bandwidth and 2-10× speedup over naive implementations, making reduction optimization critical for applications like sum, max, min, dot product that appear in 40-80% of GPU kernels and proper implementation using warp-level primitives instead of shared memory, minimizing synchronization, and hierarchical patterns determines whether reductions achieve 100 GB/s or 1000 GB/s throughput.


**Reduction Fundamentals:**
- **Associative Operations**: sum, max, min, product, AND, OR, XOR; order doesn't affect result; enables parallelization
- **Commutative**: most reductions also commutative; further optimization opportunities; non-commutative requires careful ordering
- **Tree Pattern**: log2(N) steps to reduce N elements; each step halves active threads; optimal work complexity O(N)
- **Memory Bound**: reductions are memory-bound; limited by bandwidth (1.5-3 TB/s); not compute; optimize memory access

**Warp-Level Reduction:**
- **Shuffle Down**: use __shfl_down_sync() in loop; 5 iterations for 32 threads; no shared memory; 2-10× faster than shared memory
- **Code Pattern**: for (int offset = 16; offset > 0; offset /= 2) { val += __shfl_down_sync(0xffffffff, val, offset); }
- **Performance**: 500-1000 GB/s; 60-80% of peak bandwidth; 2-5× faster than shared memory; no synchronization overhead
- **Use Cases**: reduce within warp; building block for block and grid reductions; critical optimization

**Block-Level Reduction:**
- **Two-Stage**: warp reduction → shared memory → warp reduction; optimal at each level; 300-600 GB/s
- **Pattern**: each warp reduces to single value; write to shared memory; first warp reduces shared memory; single thread has result
- **Shared Memory**: 32-64 elements in shared memory (one per warp); minimal usage; enables high occupancy
- **Performance**: 300-600 GB/s; 40-60% of peak bandwidth; 2-5× faster than pure shared memory

**Grid-Level Reduction:**
- **Three-Stage**: warp reduction → block reduction → atomic or multi-kernel; 200-400 GB/s
- **Atomic Approach**: each block atomics final result; simple but contention; 200-400 GB/s for low contention
- **Multi-Kernel**: first kernel reduces to per-block results; second kernel reduces blocks; no contention; 300-600 GB/s
- **Cooperative Groups**: grid.sync() enables single-kernel multi-block; 200-400 GB/s; simpler than multi-kernel

**Optimization Techniques:**
- **Warp Primitives**: always use __shfl for intra-warp; 2-10× faster than shared memory; 500-1000 GB/s
- **Minimize Sync**: reduce synchronization points; use warp primitives (no sync needed); 20-40% improvement
- **Coalesced Access**: ensure coalesced memory reads; 128-byte aligned; achieves 100% bandwidth
- **Multiple Elements Per Thread**: each thread processes multiple elements; reduces overhead; 20-50% improvement

**Unrolling and Specialization:**
- **Loop Unrolling**: unroll reduction loops; reduces overhead; 10-20% speedup; #pragma unroll
- **Template Specialization**: specialize for block size; enables compile-time optimization; 10-30% speedup
- **Warp Unrolling**: fully unroll warp reduction (5 iterations); eliminates loop overhead; 10-20% speedup
- **Compile-Time Constants**: use template parameters for sizes; enables aggressive optimization; 20-40% improvement

**Multiple Accumulators:**
- **Pattern**: use multiple accumulators to increase ILP; reduces dependency chains; 30-60% speedup
- **Code**: float sum1 = 0, sum2 = 0; for (int i = tid; i < N; i += stride) { sum1 += data[i]; sum2 += data[i+offset]; }
- **Merge**: combine accumulators at end; single reduction; amortizes overhead
- **Performance**: 30-60% improvement; exposes instruction-level parallelism; hides latency

**Reduction with Transformation:**
- **Pattern**: transform elements during reduction; fused operation; eliminates temporary storage
- **Example**: sum of squares: reduce(x * x); dot product: reduce(a * b); L2 norm: sqrt(reduce(x * x))
- **Performance**: 2-5× faster than separate transform and reduce; eliminates memory traffic; 500-1000 GB/s
- **Implementation**: thrust::transform_reduce() or custom kernel with inline transformation

**Segmented Reduction:**
- **Concept**: reduce multiple independent segments; each segment reduced separately; useful for batched operations
- **Implementation**: CUB DeviceSegmentedReduce; segment offsets specify boundaries; 200-400 GB/s
- **Use Cases**: per-row sum in matrix, per-group aggregation; graph algorithms; database operations
- **Performance**: 200-400 GB/s; 40-60% of peak; depends on segment sizes; small segments have overhead

**Thrust Reduce:**
- **API**: float sum = thrust::reduce(d_vec.begin(), d_vec.end(), 0.0f, thrust::plus<float>());
- **Performance**: 500-1000 GB/s; 70-90% of hand-tuned; 1 line vs 50-100 for custom implementation
- **Customization**: custom operators supported; thrust::maximum<float>(), custom functors
- **Use Cases**: rapid development, general-purpose reduction; acceptable 10-30% performance gap

**CUB Reduce:**
- **API**: cub::DeviceReduce::Sum(d_temp_storage, temp_storage_bytes, d_in, d_out, N);
- **Performance**: 500-1000 GB/s; 80-95% of hand-tuned; 10-20% faster than Thrust; lower-level API
- **Features**: sum, min, max, custom operators; segmented reduce; flexible
- **Use Cases**: performance-critical reductions; fine-grained control; production systems

**Atomic Reduction:**
- **Pattern**: each thread/warp/block atomics to global result; simple but contention-prone
- **Warp Aggregation**: reduce within warp first; lane 0 atomics; 32× fewer atomics; 5-20× speedup
- **Performance**: 200-400 GB/s with warp aggregation; 10-50 GB/s without; contention limits performance
- **Use Cases**: simple reductions, low contention; when multi-kernel overhead unacceptable

**Hierarchical Patterns:**
- **Three-Level**: warp → block → grid; optimal at each level; 300-600 GB/s
- **Warp Level**: __shfl_down_sync(); 500-1000 GB/s; no shared memory; no sync
- **Block Level**: shared memory for inter-warp; 300-600 GB/s; minimal shared memory usage
- **Grid Level**: atomic or multi-kernel; 200-400 GB/s; depends on contention

**Performance Profiling:**
- **Nsight Compute**: shows memory bandwidth, warp efficiency, occupancy; identifies bottlenecks
- **Metrics**: achieved bandwidth / peak bandwidth; target 60-80% for reductions; memory-bound
- **Bottlenecks**: uncoalesced access, excessive synchronization, low occupancy; optimize patterns
- **Tuning**: adjust block size, elements per thread, unrolling; profile to find optimal

**Common Pitfalls:**
- **Shared Memory Only**: not using warp primitives; 2-10× slower; always use __shfl for intra-warp
- **Excessive Sync**: too many __syncthreads(); 10-30% overhead each; minimize synchronization
- **Uncoalesced Access**: stride access patterns; 10-100× slowdown; ensure coalesced reads
- **Single Accumulator**: not using multiple accumulators; limits ILP; 30-60% slower

**Best Practices:**
- **Warp Primitives**: always use __shfl for intra-warp reduction; 2-10× faster than shared memory
- **Hierarchical**: warp → block → grid; optimal at each level; 300-600 GB/s
- **Multiple Accumulators**: use 2-4 accumulators; increases ILP; 30-60% improvement
- **Coalesced Access**: ensure coalesced memory reads; 128-byte aligned; achieves 100% bandwidth
- **Profile**: measure actual bandwidth; compare with peak; optimize only if bottleneck

**Performance Targets:**
- **Warp Reduction**: 500-1000 GB/s; 60-80% of peak; 2-5× faster than shared memory
- **Block Reduction**: 300-600 GB/s; 40-60% of peak; optimal for 256-512 threads
- **Grid Reduction**: 200-400 GB/s; 30-50% of peak; limited by atomic contention or multi-kernel overhead
- **Overall**: 500-1000 GB/s for large arrays (>1M elements); 60-80% of peak bandwidth

**Real-World Applications:**
- **Sum**: array sum, vector norm; 500-1000 GB/s; 60-80% of peak; critical building block
- **Dot Product**: vector dot product; 500-1000 GB/s; fused multiply-add and reduce; 60-80% of peak
- **Max/Min**: find maximum/minimum; 500-1000 GB/s; 60-80% of peak; used in normalization
- **Statistics**: mean, variance, standard deviation; 400-800 GB/s; multiple reductions; 50-70% of peak

GPU Reduction Patterns represent **the fundamental parallel primitive** — by using hierarchical reduction with warp primitives for intra-warp (500-1000 GB/s), shared memory for inter-warp (300-600 GB/s), and atomic operations for inter-block (200-400 GB/s), developers achieve 60-80% of peak memory bandwidth and 2-10× speedup over naive implementations, making reduction optimization critical for GPU applications where reductions appear in 40-80% of kernels and proper implementation using warp-level primitives instead of shared memory determines whether reductions achieve 100 GB/s or 1000 GB/s throughput.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/gpu-reduction-patterns) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
