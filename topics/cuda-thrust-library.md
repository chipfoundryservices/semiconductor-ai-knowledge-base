# CUDA Thrust Library

**Keywords**: cuda thrust library,thrust parallel algorithms,thrust stl gpu,cuda high level library,thrust performance

---

**CUDA Thrust Library** is **the high-level C++ template library providing STL-like parallel algorithms and data structures for CUDA** — offering 40+ algorithms including sort (100-300 GB/s), reduce (500-1000 GB/s), scan (400-800 GB/s), transform, and unique that automatically handle memory management, kernel launches, and optimization, achieving 60-90% of hand-optimized CUDA performance while reducing development time by 5-10× through expressive syntax like thrust::reduce(data.begin(), data.end()) that replaces 50-100 lines of custom CUDA code, making Thrust essential for rapid prototyping and production deployment where developer productivity matters and the 10-40% performance gap versus hand-tuned kernels is acceptable trade-off for 90% reduction in code complexity and maintenance burden.


**Thrust Architecture:**
- **Execution Policies**: thrust::device (GPU), thrust::host (CPU), thrust::omp (OpenMP); explicit control over execution location
- **Containers**: thrust::device_vector<T>, thrust::host_vector<T>; automatic memory management; RAII semantics; seamless CPU-GPU transfers
- **Iterators**: random access, constant, counting, transform, zip iterators; composable; enable complex operations without temporary storage
- **Algorithms**: 40+ parallel algorithms; sort, reduce, scan, transform, copy, unique, partition; optimized implementations; 60-90% of hand-tuned performance

**Core Algorithms:**
- **Sort**: thrust::sort(), thrust::stable_sort(); radix sort for integers, merge sort for general; 100-300 GB/s; 60-80% of CUB performance
- **Reduce**: thrust::reduce(), thrust::reduce_by_key(); sum, max, min, custom operators; 500-1000 GB/s; 70-90% of hand-tuned
- **Scan**: thrust::inclusive_scan(), thrust::exclusive_scan(); prefix sum; 400-800 GB/s; 60-80% of hand-tuned
- **Transform**: thrust::transform(); element-wise operations; 1-2 TB/s; near-optimal for memory-bound operations

**Device Vectors:**
- **Allocation**: thrust::device_vector<float> d_vec(N); automatic cudaMalloc; RAII cleanup; exception-safe
- **Access**: d_vec[i] for individual elements (slow); d_vec.data() for raw pointer; thrust::copy for bulk transfer
- **Resize**: d_vec.resize(new_size); automatic reallocation; preserves existing data; amortized O(1) for growth
- **Performance**: same as manual cudaMalloc; no overhead; automatic memory management eliminates leaks

**Iterators:**
- **Counting Iterator**: thrust::counting_iterator<int>(0); generates sequence 0, 1, 2, ...; no storage; useful for indices
- **Transform Iterator**: thrust::make_transform_iterator(iter, func); applies function on-the-fly; no temporary storage; 2-5× memory savings
- **Zip Iterator**: thrust::make_zip_iterator(thrust::make_tuple(iter1, iter2)); combines multiple sequences; enables multi-array operations
- **Constant Iterator**: thrust::constant_iterator<T>(value); infinite sequence of same value; no storage; useful for fills

**Functional Programming:**
- **Functors**: thrust::plus<T>(), thrust::multiplies<T>(), thrust::maximum<T>(); predefined operators; custom functors supported
- **Lambda Expressions**: C++11 lambdas work with Thrust; [=] __device__ (int x) { return x * x; }; concise custom operations
- **Composition**: combine iterators and functors; complex operations without temporaries; 2-10× memory savings
- **Type Safety**: compile-time type checking; catches errors early; safer than raw CUDA

**Performance Characteristics:**
- **Sort**: 100-300 GB/s; 60-80% of CUB; 80-95% of hand-tuned; acceptable for most applications
- **Reduce**: 500-1000 GB/s; 70-90% of hand-tuned; near-optimal for large arrays (>1M elements)
- **Scan**: 400-800 GB/s; 60-80% of hand-tuned; good for large arrays; overhead for small arrays (<10K elements)
- **Transform**: 1-2 TB/s; 90-100% of hand-tuned; memory-bound operations achieve peak bandwidth

**Memory Management:**
- **Automatic**: device_vector handles allocation, deallocation; no manual cudaMalloc/cudaFree; eliminates memory leaks
- **RAII**: resource acquisition is initialization; exception-safe; automatic cleanup on scope exit
- **Transfers**: thrust::copy(h_vec.begin(), h_vec.end(), d_vec.begin()); automatic cudaMemcpy; type-safe
- **Pinned Memory**: thrust::system::cuda::experimental::pinned_allocator; 2-10× faster transfers; limited resource

**Common Patterns:**
- **Reduction**: float sum = thrust::reduce(d_vec.begin(), d_vec.end(), 0.0f, thrust::plus<float>()); 500-1000 GB/s; 5-10 lines vs 50-100 for custom
- **Transform**: thrust::transform(d_in.begin(), d_in.end(), d_out.begin(), thrust::negate<float>()); 1-2 TB/s; 1 line vs 20-30 for custom
- **Sort**: thrust::sort(d_vec.begin(), d_vec.end()); 100-300 GB/s; 1 line vs 100-200 for custom radix sort
- **Scan**: thrust::exclusive_scan(d_in.begin(), d_in.end(), d_out.begin()); 400-800 GB/s; 1 line vs 50-100 for custom

**Advanced Algorithms:**
- **Reduce By Key**: thrust::reduce_by_key(keys.begin(), keys.end(), values.begin(), ...); groups by key; 300-600 GB/s; useful for histograms
- **Unique**: thrust::unique(d_vec.begin(), d_vec.end()); removes duplicates; 200-400 GB/s; requires sorted input
- **Partition**: thrust::partition(d_vec.begin(), d_vec.end(), predicate); splits by condition; 300-600 GB/s; stable variant available
- **Merge**: thrust::merge(d_vec1.begin(), d_vec1.end(), d_vec2.begin(), d_vec2.end(), d_out.begin()); 200-400 GB/s

**Custom Operations:**
- **Custom Functors**: struct my_op { __device__ float operator()(float x) { return x * x; } }; use with transform, reduce
- **Binary Operations**: struct my_binary_op { __device__ float operator()(float a, float b) { return a + b * b; } }; use with reduce, transform
- **Predicates**: struct is_positive { __device__ bool operator()(float x) { return x > 0; } }; use with partition, remove_if
- **Performance**: custom functors inline; no overhead vs hand-written; full optimization

**Integration with CUDA:**
- **Raw Pointers**: thrust::device_ptr<float> d_ptr = thrust::device_pointer_cast(raw_ptr); wraps raw CUDA pointers
- **Custom Kernels**: mix Thrust algorithms with custom kernels; use d_vec.data() for raw pointer; seamless integration
- **Streams**: thrust::cuda::par.on(stream); execute Thrust algorithms on specific stream; enables concurrency
- **Memory**: Thrust and CUDA share same memory; no copies; interoperable

**Comparison with Alternatives:**
- **vs CUB**: CUB 10-40% faster; lower-level; more complex API; Thrust easier to use; acceptable performance gap
- **vs cuBLAS/cuDNN**: specialized libraries faster for their domains; Thrust more general; use specialized libraries when available
- **vs Hand-Tuned**: hand-tuned 10-40% faster; 5-10× more code; Thrust faster development; use hand-tuned for critical kernels only
- **vs STL**: Thrust 10-100× faster than CPU STL; same API; easy migration; parallel by default

**Development Productivity:**
- **Code Reduction**: 5-10× less code than custom CUDA; 1-10 lines vs 50-200 for complex algorithms
- **Maintainability**: high-level abstractions; easier to understand; fewer bugs; 50-80% reduction in debugging time
- **Portability**: same code runs on CPU (thrust::host) or GPU (thrust::device); easy testing; gradual migration
- **Learning Curve**: familiar STL-like API; easier than raw CUDA; 2-4 weeks to proficiency vs 2-3 months for CUDA

**Performance Optimization:**
- **Execution Policy**: thrust::device for GPU, thrust::host for CPU; explicit control; choose based on data size
- **Fused Operations**: combine operations to reduce kernel launches; thrust::transform_reduce(); 20-40% faster than separate
- **Iterator Composition**: use transform iterators to avoid temporaries; 2-10× memory savings; 20-50% faster
- **Custom Allocators**: thrust::device_malloc_allocator, custom allocators; control memory management; 10-30% improvement

**Profiling Thrust:**
- **Nsight Systems**: shows Thrust kernel launches; identifies bottlenecks; visualizes execution timeline
- **Nsight Compute**: profiles individual Thrust kernels; memory, compute metrics; guides optimization
- **Overhead**: Thrust overhead <5% for large arrays (>1M elements); 10-30% for small arrays (<10K elements)
- **Optimization**: profile to identify slow operations; replace with custom kernels if needed; 90% of code uses Thrust, 10% custom

**Best Practices:**
- **Use Thrust First**: start with Thrust; optimize only if profiling shows bottleneck; 90% of code doesn't need hand-tuning
- **Fuse Operations**: combine operations to reduce launches; transform_reduce, transform_scan; 20-40% faster
- **Iterator Composition**: avoid temporaries with transform iterators; 2-10× memory savings; 20-50% faster
- **Profile**: measure actual performance; compare with requirements; hand-tune only critical 10%
- **Mix with CUDA**: use Thrust for general algorithms, custom kernels for specialized operations; best of both worlds

**Performance Targets:**
- **Sort**: 100-300 GB/s; 60-80% of hand-tuned; acceptable for most applications; 1 line of code
- **Reduce**: 500-1000 GB/s; 70-90% of hand-tuned; near-optimal for large arrays; 1 line of code
- **Scan**: 400-800 GB/s; 60-80% of hand-tuned; good for large arrays; 1 line of code
- **Transform**: 1-2 TB/s; 90-100% of hand-tuned; memory-bound operations achieve peak; 1 line of code

**Real-World Usage:**
- **Data Processing**: sort, filter, transform pipelines; 5-10× faster development; 60-90% of hand-tuned performance
- **Scientific Computing**: reductions, scans, transformations; 80-95% of hand-tuned; 90% less code
- **Machine Learning**: data preprocessing, feature extraction; 70-90% of hand-tuned; rapid prototyping
- **Graph Algorithms**: sorting edges, reducing vertices; 60-80% of hand-tuned; 5-10× less code

CUDA Thrust Library represents **the productivity revolution in GPU programming** — by providing STL-like parallel algorithms that achieve 60-90% of hand-optimized performance while reducing code by 5-10× and development time by similar factors, Thrust makes GPU programming accessible to developers without deep CUDA expertise and enables rapid prototyping and production deployment where the 10-40% performance gap versus hand-tuned kernels is acceptable trade-off for 90% reduction in code complexity, making Thrust the default choice for GPU algorithms where developer time is more valuable than the last 10-40% of performance.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/cuda-thrust-library) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
