# GPU Scan (Prefix Sum)

**Keywords**: gpu scan prefix sum,parallel scan cuda,cuda prefix sum optimization,inclusive exclusive scan,scan algorithm gpu

---

**GPU Scan (Prefix Sum)** is **the parallel algorithm that computes cumulative sums or other associative operations across array elements** — where inclusive scan produces [a0, a0+a1, a0+a1+a2, ...] and exclusive scan produces [0, a0, a0+a1, ...], achieving 400-800 GB/s throughput (50-70% of peak bandwidth) through hierarchical implementation using warp primitives (__shfl_up_sync) for intra-warp (500-1000 GB/s), shared memory for inter-warp (300-600 GB/s), and multi-pass algorithms for large arrays, making scan essential for algorithms like stream compaction (removing elements), radix sort (computing output positions), and sparse matrix operations where scan appears in 30-60% of advanced GPU algorithms and proper implementation using warp-level primitives and minimizing global memory accesses determines whether applications achieve 100 GB/s or 800 GB/s throughput.

**Scan Fundamentals:**
- **Inclusive Scan**: output[i] = input[0] + input[1] + ... + input[i]; includes current element; natural for cumulative sums
- **Exclusive Scan**: output[i] = input[0] + input[1] + ... + input[i-1]; excludes current element; useful for computing positions
- **Associative Operations**: sum, max, min, product, AND, OR, XOR; order matters for scan; enables parallelization
- **Applications**: stream compaction, radix sort, sparse matrix, work distribution; fundamental building block

**Warp-Level Scan:**
- **Shuffle Up**: use __shfl_up_sync() in loop; 5 iterations for 32 threads; no shared memory; 2-5× faster than shared memory
- **Code Pattern**: for (int offset = 1; offset < 32; offset *= 2) { int temp = __shfl_up_sync(0xffffffff, val, offset); if (lane >= offset) val += temp; }
- **Performance**: 500-1000 GB/s; 60-80% of peak bandwidth; 2-5× faster than shared memory; no synchronization overhead
- **Use Cases**: scan within warp; building block for block and grid scans; critical optimization

**Block-Level Scan:**
- **Two-Stage**: warp scan → inter-warp scan → add offsets; optimal at each level; 300-600 GB/s
- **Pattern**: each warp scans independently; scan warp sums; add warp offsets to elements; requires two syncs
- **Shared Memory**: store warp sums (32-64 elements); minimal usage; enables high occupancy
- **Performance**: 300-600 GB/s; 40-60% of peak bandwidth; 2-5× faster than pure shared memory

**Large Array Scan:**
- **Multi-Pass**: first pass scans blocks independently; second pass scans block sums; third pass adds offsets; 400-800 GB/s
- **Three-Kernel**: kernel 1 scans blocks; kernel 2 scans block sums; kernel 3 adds offsets; no global sync needed
- **Single-Kernel**: use cooperative groups grid.sync(); simpler but requires all blocks fit on GPU; 400-800 GB/s
- **Performance**: 400-800 GB/s for large arrays (>1M elements); 50-70% of peak bandwidth; memory-bound

**Optimization Techniques:**
- **Warp Primitives**: always use __shfl_up for intra-warp; 2-5× faster than shared memory; 500-1000 GB/s
- **Bank Conflict Avoidance**: pad shared memory arrays; prevents bank conflicts; 10-30% improvement
- **Coalesced Access**: ensure coalesced memory reads/writes; 128-byte aligned; achieves 100% bandwidth
- **Multiple Elements Per Thread**: each thread processes multiple elements; reduces overhead; 20-50% improvement

**Inclusive vs Exclusive:**
- **Inclusive**: simpler implementation; natural for cumulative sums; output[i] includes input[i]
- **Exclusive**: useful for positions; output[i] = sum of elements before i; radix sort, compaction
- **Conversion**: exclusive = shift(inclusive, 1) with 0 at start; inclusive = exclusive + input; trivial conversion
- **Performance**: same performance; choice based on application needs; exclusive more common in algorithms

**Segmented Scan:**
- **Concept**: scan multiple independent segments; each segment scanned separately; useful for batched operations
- **Implementation**: CUB DeviceSegmentedScan; segment flags or offsets specify boundaries; 300-600 GB/s
- **Use Cases**: per-row scan in matrix, per-group aggregation; graph algorithms; sparse matrix operations
- **Performance**: 300-600 GB/s; 40-60% of peak; depends on segment sizes; small segments have overhead

**Thrust Scan:**
- **API**: thrust::inclusive_scan(d_in.begin(), d_in.end(), d_out.begin()); or thrust::exclusive_scan()
- **Performance**: 400-800 GB/s; 60-80% of hand-tuned; 1 line vs 100-200 for custom implementation
- **Customization**: custom operators supported; thrust::plus<float>(), thrust::maximum<float>(), custom functors
- **Use Cases**: rapid development, general-purpose scan; acceptable 20-40% performance gap

**CUB Scan:**
- **API**: cub::DeviceScan::InclusiveSum(d_temp_storage, temp_storage_bytes, d_in, d_out, N);
- **Performance**: 400-800 GB/s; 70-90% of hand-tuned; 10-30% faster than Thrust; lower-level API
- **Features**: inclusive, exclusive, custom operators; segmented scan; flexible
- **Use Cases**: performance-critical scans; fine-grained control; production systems

**Stream Compaction:**
- **Pattern**: scan predicate flags; use scan result as output positions; write elements to compacted array
- **Code**: flags = predicate(input); positions = exclusive_scan(flags); if (flags[i]) output[positions[i]] = input[i];
- **Performance**: 300-600 GB/s; 40-60% of peak; scan is bottleneck; 2-3× faster than CPU
- **Use Cases**: removing elements, filtering; sparse matrix, graph algorithms; data preprocessing

**Radix Sort Integration:**
- **Histogram**: count elements per bin; 300-600 GB/s
- **Scan**: exclusive scan of histogram; computes output positions; 400-800 GB/s
- **Scatter**: write elements to sorted positions; 200-300 GB/s
- **Performance**: scan is 20-40% of radix sort time; critical for overall performance

**Work Distribution:**
- **Pattern**: scan work counts; use scan result to assign work to threads; load balancing
- **Code**: work_counts = compute_work(input); positions = exclusive_scan(work_counts); assign work based on positions
- **Performance**: 300-600 GB/s for scan; enables balanced work distribution; 20-50% improvement in irregular algorithms
- **Use Cases**: irregular parallelism, dynamic work assignment; graph algorithms; sparse operations

**Hierarchical Patterns:**
- **Three-Level**: warp → block → grid; optimal at each level; 400-800 GB/s
- **Warp Level**: __shfl_up_sync(); 500-1000 GB/s; no shared memory; no sync
- **Block Level**: shared memory for inter-warp; 300-600 GB/s; minimal shared memory usage
- **Grid Level**: multi-pass or cooperative groups; 400-800 GB/s; depends on array size

**Performance Profiling:**
- **Nsight Compute**: shows memory bandwidth, warp efficiency, occupancy; identifies bottlenecks
- **Metrics**: achieved bandwidth / peak bandwidth; target 50-70% for scans; memory-bound
- **Bottlenecks**: uncoalesced access, bank conflicts, excessive synchronization; optimize patterns
- **Tuning**: adjust block size, elements per thread, padding; profile to find optimal

**Common Pitfalls:**
- **Shared Memory Only**: not using warp primitives; 2-5× slower; always use __shfl_up for intra-warp
- **Bank Conflicts**: unpadded shared memory; 2-10× slowdown; add padding to avoid conflicts
- **Uncoalesced Access**: stride access patterns; 10-100× slowdown; ensure coalesced reads/writes
- **Too Many Syncs**: excessive __syncthreads(); 10-30% overhead each; minimize synchronization

**Best Practices:**
- **Warp Primitives**: always use __shfl_up for intra-warp scan; 2-5× faster than shared memory
- **Hierarchical**: warp → block → grid; optimal at each level; 400-800 GB/s
- **Pad Shared Memory**: avoid bank conflicts; 1-2 elements padding; 10-30% improvement
- **Coalesced Access**: ensure coalesced memory reads/writes; 128-byte aligned; achieves 100% bandwidth
- **Profile**: measure actual bandwidth; compare with peak; optimize only if bottleneck

**Performance Targets:**
- **Warp Scan**: 500-1000 GB/s; 60-80% of peak; 2-5× faster than shared memory
- **Block Scan**: 300-600 GB/s; 40-60% of peak; optimal for 256-512 threads
- **Grid Scan**: 400-800 GB/s; 50-70% of peak; for large arrays (>1M elements)
- **Overall**: 400-800 GB/s for large arrays; 50-70% of peak bandwidth; memory-bound

**Real-World Applications:**
- **Stream Compaction**: removing invalid elements; 300-600 GB/s; 40-60% of peak; used in rendering, physics
- **Radix Sort**: computing output positions; 400-800 GB/s; 20-40% of sort time; critical for performance
- **Sparse Matrix**: CSR format construction; 300-600 GB/s; 30-50% of construction time
- **Work Distribution**: load balancing irregular work; 300-600 GB/s; 20-50% improvement in irregular algorithms

GPU Scan (Prefix Sum) represents **the essential parallel primitive for position computation** — by using hierarchical implementation with warp primitives for intra-warp (500-1000 GB/s), shared memory for inter-warp (300-600 GB/s), and multi-pass algorithms for large arrays, developers achieve 400-800 GB/s throughput (50-70% of peak bandwidth) and enable algorithms like stream compaction, radix sort, and sparse matrix operations where scan is fundamental building block and proper implementation using warp-level primitives determines whether applications achieve 100 GB/s or 800 GB/s throughput.');

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/gpu-scan-prefix-sum) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
