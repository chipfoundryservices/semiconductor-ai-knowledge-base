# cuBLAS and cuDNN Optimization

**Keywords**: cublas cudnn optimization,gpu math libraries,tensor operations gpu,cublas performance tuning,cudnn convolution optimization

---

**cuBLAS and cuDNN Optimization** is **the systematic tuning of NVIDIA's highly-optimized math libraries to achieve 80-95% of theoretical peak performance** — where cuBLAS (CUDA Basic Linear Algebra Subroutines) delivers 10-20 TFLOPS for matrix multiplication on A100 (80-95% of 19.5 TFLOPS peak) and 60-80 TFLOPS with Tensor Cores (80-95% of 312 TFLOPS FP16 peak), while cuDNN (CUDA Deep Neural Network library) provides optimized convolution (15-30 TFLOPS), batch normalization, activation functions, and RNN operations that are 10-100× faster than naive implementations, making proper library usage and tuning essential for deep learning where cuBLAS/cuDNN handle 80-95% of compute and optimization techniques like algorithm selection, workspace tuning, tensor core enablement, and batching can improve performance by 2-10× over default settings.


**cuBLAS Fundamentals:**
- **GEMM**: cublasGemmEx() for matrix multiplication; supports FP32, FP16, INT8, TF32; 10-20 TFLOPS FP32, 60-80 TFLOPS FP16 with Tensor Cores on A100
- **GEMV**: matrix-vector multiplication; 500-1000 GB/s; memory-bound; 80-95% of peak bandwidth
- **Batched Operations**: cublasGemmStridedBatchedEx() for multiple matrices; amortizes overhead; 90-95% efficiency vs single GEMM
- **Math Modes**: CUBLAS_DEFAULT_MATH (CUDA cores), CUBLAS_TENSOR_OP_MATH (Tensor Cores), CUBLAS_TF32_TENSOR_OP_MATH (TF32); explicit control

**cuBLAS Optimization:**
- **Tensor Cores**: cublasSetMathMode(handle, CUBLAS_TENSOR_OP_MATH); enables Tensor Cores; 10-20× speedup for FP16; 312 TFLOPS on A100
- **TF32**: automatic on A100; 8× faster than FP32 (156 TFLOPS vs 19.5 TFLOPS); no code changes; maintains FP32 range
- **Algorithm Selection**: cublasGemmAlgo_t specifies algorithm; CUBLAS_GEMM_DEFAULT_TENSOR_OP for Tensor Cores; auto-tuning available
- **Workspace**: provide workspace buffer; enables better algorithms; 10-30% speedup; typical size 32-256MB

**cuDNN Fundamentals:**
- **Convolution**: cudnnConvolutionForward(); supports 2D, 3D; multiple algorithms; 15-30 TFLOPS on A100; 80-95% of peak with Tensor Cores
- **Batch Normalization**: cudnnBatchNormalizationForwardTraining(); fused operations; 2-5× faster than separate kernels
- **Activation**: cudnnActivationForward(); ReLU, sigmoid, tanh; fused with convolution; 20-40% speedup
- **Pooling**: cudnnPoolingForward(); max, average pooling; 500-1000 GB/s; memory-bound

**cuDNN Optimization:**
- **Algorithm Selection**: cudnnGetConvolutionForwardAlgorithm(); finds best algorithm; CUDNN_CONVOLUTION_FWD_ALGO_IMPLICIT_PRECOMP_GEMM often fastest
- **Workspace Tuning**: cudnnGetConvolutionForwardWorkspaceSize(); larger workspace enables faster algorithms; 10-50% speedup; typical 100MB-2GB
- **Tensor Cores**: cudnnSetConvolutionMathType(CUDNN_TENSOR_OP_MATH); enables Tensor Cores; 10-20× speedup for FP16
- **Auto-Tuning**: cudnnFindConvolutionForwardAlgorithm(); benchmarks all algorithms; finds optimal; 20-50% speedup over default

**Mixed Precision:**
- **FP16 Compute**: use FP16 for matrix operations; 2× memory bandwidth, 10-20× compute (Tensor Cores); 312 TFLOPS on A100
- **FP32 Accumulation**: accumulate in FP32; maintains accuracy; prevents overflow; standard practice
- **Automatic Mixed Precision (AMP)**: PyTorch/TensorFlow automatic FP16; 2-3× training speedup; minimal code changes
- **TF32**: automatic on A100; 8× speedup vs FP32; no code changes; maintains FP32 range; 156 TFLOPS

**Batching Strategies:**
- **Batched GEMM**: cublasGemmStridedBatchedEx(); processes multiple matrices; 90-95% efficiency; amortizes overhead
- **Batch Size**: larger batches improve efficiency; 32-256 typical; 80-95% efficiency; limited by memory
- **Micro-Batching**: split large batch into micro-batches; fits in cache; 10-30% speedup for some workloads
- **Dynamic Batching**: combine multiple requests; improves throughput; 2-4× for inference serving

**Algorithm Selection:**
- **cuBLAS Algorithms**: CUBLAS_GEMM_DEFAULT, CUBLAS_GEMM_ALGO0-23; different tile sizes, strategies; auto-tune for workload
- **cuDNN Algorithms**: IMPLICIT_GEMM, IMPLICIT_PRECOMP_GEMM, GEMM, DIRECT, FFT, WINOGRAD; different trade-offs; auto-tune critical
- **Heuristics**: cudnnGetConvolutionForwardAlgorithm_v7(); uses heuristics; fast but may not be optimal; benchmark for critical paths
- **Benchmarking**: cudnnFindConvolutionForwardAlgorithm(); benchmarks all; finds optimal; 20-50% speedup; cache results

**Workspace Management:**
- **Purpose**: temporary storage for algorithms; larger workspace enables faster algorithms; trade memory for speed
- **Size**: query with cudnnGetConvolutionForwardWorkspaceSize(); typical 100MB-2GB; depends on algorithm and tensor size
- **Allocation**: pre-allocate workspace; reuse across operations; eliminates allocation overhead (5-50ms)
- **Tuning**: try different workspace limits; find optimal trade-off; 10-50% speedup with larger workspace

**Fusion Opportunities:**
- **Convolution + Activation**: cudnnConvolutionBiasActivationForward(); fuses conv, bias, activation; 20-40% speedup; reduces memory traffic
- **Batch Norm + Activation**: fused batch normalization; 2-5× faster than separate; reduces kernel launches
- **GEMM + Bias**: cublasGemmEx() with bias; fused operation; 10-20% speedup; reduces memory accesses
- **Custom Fusion**: use cuDNN fusion API; define custom fusions; 20-60% speedup for complex patterns

**Performance Profiling:**
- **Nsight Compute**: profiles cuBLAS/cuDNN kernels; shows Tensor Core utilization, memory bandwidth, compute throughput
- **Metrics**: achieved TFLOPS / peak TFLOPS; target 80-95%; memory bandwidth utilization; target 80-100%
- **Bottlenecks**: memory-bound (small matrices), compute-bound (large matrices), launch overhead (many small operations)
- **Optimization**: increase batch size, use Tensor Cores, fuse operations, tune workspace

**Tensor Core Utilization:**
- **Requirements**: matrix dimensions multiples of 8 (FP16), 16 (INT8); proper alignment; FP16/BF16 data types
- **Verification**: Nsight Compute shows Tensor Core utilization; target 50-80%; low utilization indicates dimension mismatch
- **Padding**: pad matrices to multiples of 8/16; enables Tensor Cores; 10-20× speedup outweighs padding overhead
- **Performance**: 312 TFLOPS FP16 on A100, 989 TFLOPS on H100; 10-20× faster than CUDA cores

**Memory Optimization:**
- **Data Layout**: NCHW (channels first) vs NHWC (channels last); NHWC often faster on Tensor Cores; 10-30% speedup
- **Alignment**: 128-byte alignment for optimal performance; cudaMalloc provides automatic alignment
- **Pinned Memory**: use pinned memory for CPU-GPU transfers; 2-10× faster; cudaMallocHost()
- **Prefetching**: overlap data transfer with computation; async transfers; 20-50% throughput improvement

**Multi-GPU Scaling:**
- **Data Parallelism**: replicate model, split data; cuBLAS/cuDNN on each GPU; 85-95% scaling efficiency on 8 GPUs
- **Model Parallelism**: split model across GPUs; requires careful orchestration; 70-85% efficiency
- **Gradient Synchronization**: NCCL all-reduce after backward; 5-20ms for 1GB on 8 GPUs with NVLink
- **Load Balancing**: ensure equal work per GPU; monitor utilization; 10-30% improvement with proper balancing

**Framework Integration:**
- **PyTorch**: automatic cuBLAS/cuDNN usage; torch.backends.cudnn.benchmark=True for auto-tuning; 20-50% speedup
- **TensorFlow**: automatic library usage; XLA compilation for fusion; 30-60% speedup
- **JAX**: automatic with jax.default_matmul_precision('high'); 2-3× speedup
- **ONNX Runtime**: cuDNN/cuBLAS backend; TensorRT for optimization; 2-5× inference speedup

**Best Practices:**
- **Enable Tensor Cores**: use FP16/BF16, set math mode, ensure proper dimensions; 10-20× speedup
- **Auto-Tune**: use cudnnFindConvolutionForwardAlgorithm(); benchmark algorithms; 20-50% speedup; cache results
- **Batch Operations**: use batched APIs; amortizes overhead; 90-95% efficiency
- **Fuse Operations**: use fused APIs; reduces memory traffic and kernel launches; 20-60% speedup
- **Profile**: use Nsight Compute; verify Tensor Core utilization, memory bandwidth; optimize based on data

**Performance Targets:**
- **cuBLAS GEMM**: 10-20 TFLOPS FP32, 60-80 TFLOPS FP16 (Tensor Cores) on A100; 80-95% of peak
- **cuDNN Convolution**: 15-30 TFLOPS on A100; 80-95% of peak with Tensor Cores; 70-85% without
- **Memory Bandwidth**: 80-100% of peak (1.5-2 TB/s on A100); for memory-bound operations
- **Tensor Core Utilization**: 50-80%; indicates proper usage; low utilization means dimension mismatch

**Common Pitfalls:**
- **Wrong Dimensions**: matrix dimensions not multiples of 8/16; Tensor Cores disabled; 10-20× slowdown
- **Default Settings**: not enabling Tensor Cores; using default algorithms; 2-10× slower than optimal
- **Small Batches**: batch size too small; low efficiency; increase batch size for 2-5× improvement
- **No Auto-Tuning**: using default algorithm; 20-50% slower than optimal; always auto-tune critical paths

**Real-World Performance:**
- **ResNet-50 Training**: 15-25 TFLOPS on A100; 80-90% of peak; cuDNN convolution dominates; 2-3× speedup with optimization
- **BERT Training**: 20-30 TFLOPS on A100; 85-95% of peak; cuBLAS GEMM dominates; 2-4× speedup with Tensor Cores
- **GPT-3 Inference**: 30-50 TFLOPS on A100; 80-90% of peak; cuBLAS batched GEMM; 3-5× speedup with batching and Tensor Cores
- **Stable Diffusion**: 10-20 TFLOPS on A100; 70-85% of peak; cuDNN convolution; 2-3× speedup with optimization

cuBLAS and cuDNN Optimization represent **the foundation of high-performance deep learning** — by properly configuring these highly-optimized libraries to enable Tensor Cores, auto-tune algorithms, batch operations, and fuse computations, developers achieve 80-95% of theoretical peak performance (10-20 TFLOPS FP32, 60-80 TFLOPS FP16 on A100) and 2-10× speedup over default settings, making library optimization essential for deep learning where cuBLAS/cuDNN handle 80-95% of compute and proper tuning determines whether training takes days or weeks and whether inference meets latency requirements.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/cublas-cudnn-optimization) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
