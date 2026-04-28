# GPU Sorting Algorithms

**Keywords**: gpu sorting algorithms,cuda radix sort,parallel sorting gpu,gpu sort performance,cuda sort optimization

---

**GPU Sorting Algorithms** are **the parallel implementations of sorting that leverage thousands of GPU threads to achieve 100-300 GB/s throughput** — where radix sort (optimal for integers and fixed-point) achieves 200-300 GB/s by processing multiple bits per pass and exploiting warp-level primitives, merge sort (optimal for general comparisons) achieves 100-200 GB/s through hierarchical merging, and bitonic sort (optimal for power-of-2 sizes) achieves 150-250 GB/s with fixed communication patterns, making GPU sorting 10-50× faster than CPU sorting (5-20 GB/s) and essential for applications like database operations, graph algorithms, and data preprocessing where sorting is bottleneck (20-60% of runtime) and proper algorithm selection based on data characteristics (integer vs float, key-only vs key-value, size) determines whether applications achieve 40% or 90% of theoretical peak bandwidth.


**Radix Sort:**
- **Algorithm**: sorts by processing k bits per pass; typically k=4-8 bits; requires ceil(32/k) passes for 32-bit integers; stable sort
- **Performance**: 200-300 GB/s on A100; 60-80% of peak memory bandwidth; optimal for integers, fixed-point; 10-50× faster than CPU
- **Implementation**: histogram per block → prefix sum → scatter; uses shared memory for local histogram; warp primitives for reduction
- **Use Cases**: integer keys, fixed-point values; uniform distribution; large arrays (>1M elements); 80-95% of peak bandwidth

**Merge Sort:**
- **Algorithm**: hierarchical merging; bottom-up or top-down; log2(N) passes; stable sort; comparison-based
- **Performance**: 100-200 GB/s on A100; 40-60% of peak bandwidth; optimal for general comparisons, small arrays
- **Implementation**: warp-level merge → block-level merge → global merge; uses shared memory for local merging
- **Use Cases**: general comparisons, custom comparators; small-medium arrays (10K-1M elements); stable sort required

**Bitonic Sort:**
- **Algorithm**: comparison network; fixed communication pattern; log2(N) × (log2(N)+1) / 2 comparisons; not stable
- **Performance**: 150-250 GB/s on A100; 50-70% of peak bandwidth; optimal for power-of-2 sizes; predictable performance
- **Implementation**: warp-level bitonic → block-level bitonic → global bitonic; uses shuffle for warp-level, shared memory for block-level
- **Use Cases**: power-of-2 sizes, predictable latency; small-medium arrays; GPU-friendly communication pattern

**Thrust Sort:**
- **API**: thrust::sort(d_vec.begin(), d_vec.end()); automatic algorithm selection; radix sort for integers, merge sort for general
- **Performance**: 100-300 GB/s; 60-80% of hand-tuned; 1 line of code vs 100-200 for custom implementation
- **Customization**: thrust::sort(d_vec.begin(), d_vec.end(), thrust::greater<int>()); custom comparators supported
- **Use Cases**: rapid development, general-purpose sorting; acceptable 10-30% performance gap vs hand-tuned

**CUB Sort:**
- **API**: cub::DeviceRadixSort::SortKeys(d_temp_storage, temp_storage_bytes, d_keys, d_sorted, N); explicit control
- **Performance**: 200-300 GB/s; 70-90% of peak bandwidth; 10-30% faster than Thrust; lower-level API
- **Features**: key-only or key-value pairs; ascending or descending; segmented sort; double-buffer for in-place
- **Use Cases**: performance-critical sorting; fine-grained control; production systems

**Key-Value Sorting:**
- **Radix Sort**: cub::DeviceRadixSort::SortPairs(); sorts keys, reorders values; 150-250 GB/s; 2× slower than key-only
- **Merge Sort**: stable sort preserves value order; 80-150 GB/s; 40-60% of peak bandwidth
- **Performance**: 2-3× slower than key-only; memory bandwidth limited; 2× data movement
- **Use Cases**: sorting with associated data; database operations; graph algorithms

**Segmented Sort:**
- **Concept**: sort multiple independent segments; each segment sorted separately; useful for batched operations
- **Implementation**: cub::DeviceSegmentedRadixSort::SortKeys(); segment offsets specify boundaries
- **Performance**: 150-250 GB/s; 50-70% of peak; depends on segment sizes; small segments have overhead
- **Use Cases**: batched sorting, per-group sorting; graph algorithms; database operations

**Optimization Techniques:**
- **Warp-Level Primitives**: use __shfl for warp-level sorting; 2-5× faster than shared memory; 500-1000 GB/s for small arrays
- **Shared Memory**: use for block-level sorting; 100× faster than global memory; 164KB per SM on A100
- **Coalesced Access**: ensure coalesced memory access; 128-byte aligned; achieves 100% bandwidth; stride-1 access optimal
- **Occupancy**: balance shared memory usage and occupancy; 50-100% occupancy typical; 256 threads per block optimal

**Radix Sort Optimization:**
- **Bits Per Pass**: 4-8 bits typical; more bits = fewer passes but larger histogram; 4 bits optimal for most cases
- **Histogram**: per-block histogram in shared memory; warp primitives for reduction; 300-600 GB/s
- **Prefix Sum**: exclusive scan of histogram; 400-800 GB/s; CUB provides optimized implementation
- **Scatter**: write sorted elements to output; coalesced writes; 200-300 GB/s; double-buffering eliminates copies

**Merge Sort Optimization:**
- **Warp-Level Merge**: use __shfl for merging within warp; 2-5× faster than shared memory; 500-1000 GB/s
- **Block-Level Merge**: shared memory for merging within block; 100-200 GB/s; minimize global memory accesses
- **Global Merge**: hierarchical merging; multiple passes; 80-150 GB/s; memory-bound
- **Path Decomposition**: parallel merge path algorithm; better load balancing; 20-40% speedup

**Bitonic Sort Optimization:**
- **Warp-Level**: use __shfl_xor for butterfly exchanges; 2-5× faster than shared memory; 500-1000 GB/s
- **Block-Level**: shared memory for larger bitonic networks; 150-250 GB/s; minimize bank conflicts
- **Padding**: add padding to avoid bank conflicts; 1-2 elements typical; 10-30% improvement
- **Unrolling**: unroll inner loops; reduces overhead; 10-20% speedup; compiler often does automatically

**Performance Comparison:**
- **Radix Sort**: 200-300 GB/s; best for integers; 60-80% of peak; 10-50× faster than CPU
- **Merge Sort**: 100-200 GB/s; best for general comparisons; 40-60% of peak; 5-20× faster than CPU
- **Bitonic Sort**: 150-250 GB/s; best for power-of-2 sizes; 50-70% of peak; 10-30× faster than CPU
- **Thrust Sort**: 100-300 GB/s; automatic selection; 60-80% of hand-tuned; easiest to use

**Size Considerations:**
- **Small Arrays (<10K)**: bitonic sort or warp-level sort; 150-250 GB/s; low overhead; predictable latency
- **Medium Arrays (10K-1M)**: merge sort or radix sort; 150-250 GB/s; good balance; algorithm depends on data type
- **Large Arrays (>1M)**: radix sort for integers, merge sort for general; 200-300 GB/s; amortizes overhead; optimal performance
- **Very Large (>100M)**: out-of-core sorting; multiple passes; 100-200 GB/s; memory-limited

**Stability:**
- **Stable**: radix sort, merge sort; preserves relative order of equal elements; required for some applications
- **Unstable**: bitonic sort, quicksort; may reorder equal elements; faster but less predictable
- **Use Cases**: stable required for multi-key sorting, database operations; unstable acceptable for unique keys

**Custom Comparators:**
- **Merge Sort**: supports custom comparators; thrust::sort(d_vec.begin(), d_vec.end(), my_comparator()); flexible
- **Radix Sort**: limited to integer keys; can use custom bit extraction; less flexible but faster
- **Performance**: custom comparators 10-30% slower; function call overhead; inline when possible

**Profiling and Tuning:**
- **Nsight Compute**: shows memory bandwidth, occupancy, warp efficiency; identifies bottlenecks
- **Metrics**: achieved bandwidth / peak bandwidth; target 60-80% for sorting; memory-bound operation
- **Bottlenecks**: uncoalesced access, bank conflicts, low occupancy; optimize access patterns
- **Tuning**: adjust block size, bits per pass, shared memory usage; profile to find optimal

**Best Practices:**
- **Use Libraries**: Thrust or CUB for most cases; 60-90% of hand-tuned; 10-100× less code
- **Algorithm Selection**: radix sort for integers, merge sort for general; bitonic for power-of-2; profile to verify
- **Batch Operations**: sort multiple arrays together; amortizes overhead; 20-40% improvement
- **Profile**: measure actual bandwidth; compare with peak; optimize only if bottleneck
- **Pre-Allocate**: allocate temporary storage once; reuse across sorts; eliminates allocation overhead

**Performance Targets:**
- **Radix Sort**: 200-300 GB/s; 60-80% of peak (1.5-3 TB/s); optimal for integers
- **Merge Sort**: 100-200 GB/s; 40-60% of peak; optimal for general comparisons
- **Bitonic Sort**: 150-250 GB/s; 50-70% of peak; optimal for power-of-2 sizes
- **Key-Value**: 150-250 GB/s; 50-70% of peak; 2× slower than key-only

**Real-World Applications:**
- **Database Operations**: sorting query results; 200-300 GB/s with radix sort; 10-50× faster than CPU
- **Graph Algorithms**: sorting edges by source/destination; 150-250 GB/s; 20-40% of graph processing time
- **Data Preprocessing**: sorting features for ML; 200-300 GB/s; 10-30% of preprocessing time
- **Rendering**: sorting primitives by depth; 150-250 GB/s; 5-20% of rendering time

GPU Sorting Algorithms represent **the essential building block for data-intensive applications** — by leveraging thousands of parallel threads and optimized memory access patterns, GPU sorting achieves 100-300 GB/s throughput (10-50× faster than CPU) through algorithms like radix sort for integers (200-300 GB/s), merge sort for general comparisons (100-200 GB/s), and bitonic sort for power-of-2 sizes (150-250 GB/s), making GPU sorting critical for applications where sorting is bottleneck and proper algorithm selection based on data characteristics determines whether applications achieve 40% or 90% of theoretical peak bandwidth.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/gpu-sorting-algorithms) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
