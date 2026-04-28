# Tensor Core Programming

**Keywords**: tensor core programming,cuda tensor cores,wmma api,mma instructions,tensor core optimization

---

**Tensor Core Programming** is **the utilization of specialized matrix multiplication hardware on NVIDIA GPUs to achieve 10-20× higher throughput than CUDA cores** — where Tensor Cores perform mixed-precision matrix operations (FP16/BF16 input, FP32 accumulation) at 312 TFLOPS on A100 and 989 TFLOPS on H100 compared to 19.5 TFLOPS and 67 TFLOPS for CUDA cores, accessed through WMMA (Warp Matrix Multiply-Accumulate) API or cuBLAS/cuDNN libraries that automatically utilize Tensor Cores, requiring specific matrix dimensions (multiples of 8 for FP16, 16 for INT8) and memory layouts (row-major or column-major with proper alignment) to achieve peak performance, enabling 5-15× faster training of large language models and 10-30× faster inference through INT8 quantization, making Tensor Core programming essential for AI workloads where matrix multiplication dominates (60-90% of compute) and proper utilization can reduce training time from weeks to days.

**Tensor Core Capabilities:**
- **A100**: 312 TFLOPS (FP16), 624 TFLOPS (BF16), 1248 TOPS (INT8), 2496 TOPS (INT4); 16× faster than CUDA cores
- **H100**: 989 TFLOPS (FP16), 1979 TFLOPS (BF16), 3958 TOPS (INT8); 3× faster than A100; FP8 support
- **V100**: 125 TFLOPS (FP16); first generation Tensor Cores; 8× faster than CUDA cores
- **Supported Types**: FP16, BF16, TF32, FP8 (H100), INT8, INT4; mixed precision with FP32 accumulation

**WMMA API:**
- **Fragment Types**: matrix_a, matrix_b, accumulator; represent 16×16 matrix tiles; stored in registers
- **Load**: wmma::load_matrix_sync(); loads tile from memory to fragment; requires aligned access
- **Multiply-Accumulate**: wmma::mma_sync(); performs D = A × B + C; single instruction; 16×16×16 operation
- **Store**: wmma::store_matrix_sync(); stores result to memory; coalesced access required

**Matrix Dimensions:**
- **Tile Sizes**: 16×16×16 (FP16/BF16), 8×8×32 (INT8), 8×8×128 (INT4); fixed by hardware
- **Matrix Sizes**: must be multiples of tile size; pad if necessary; M×K × K×N = M×N
- **Alignment**: 128-byte alignment for optimal performance; use cudaMalloc for automatic alignment
- **Layouts**: row-major or column-major; specify in load/store; affects memory access patterns

**Programming Model:**
- **Warp-Level**: Tensor Cores operate at warp level; all 32 threads cooperate; implicit synchronization
- **Fragment Distribution**: matrix fragments distributed across warp; each thread holds portion
- **Accumulation**: accumulator fragment accumulates results; FP32 for precision; multiple MMA operations
- **Synchronization**: implicit in wmma operations; no explicit __syncthreads() needed within warp

**Matrix Multiplication Example:**
```cuda
// Declare fragments
wmma::fragment<wmma::matrix_a, 16, 16, 16, half, wmma::row_major> a_frag;
wmma::fragment<wmma::matrix_b, 16, 16, 16, half, wmma::col_major> b_frag;
wmma::fragment<wmma::accumulator, 16, 16, 16, float> c_frag;

// Initialize accumulator
wmma::fill_fragment(c_frag, 0.0f);

// Loop over K dimension
for (int k = 0; k < K; k += 16) {
    wmma::load_matrix_sync(a_frag, A + k, K);
    wmma::load_matrix_sync(b_frag, B + k * N, N);
    wmma::mma_sync(c_frag, a_frag, b_frag, c_frag);
}

// Store result
wmma::store_matrix_sync(C, c_frag, N, wmma::mem_row_major);
```

**Performance Optimization:**
- **Tile Size**: use largest supported tile (16×16×16); maximizes Tensor Core utilization
- **Loop Unrolling**: unroll K-dimension loop; reduces overhead; 10-20% speedup
- **Shared Memory**: stage data in shared memory; reduces global memory accesses; 2-5× speedup
- **Multiple Accumulators**: use multiple accumulator fragments; increases ILP; 20-40% speedup

**Mixed Precision:**
- **FP16 Input**: half-precision input; 2× memory bandwidth vs FP32; 312 TFLOPS on A100
- **FP32 Accumulation**: full-precision accumulation; maintains accuracy; prevents overflow
- **BF16**: bfloat16 format; same exponent range as FP32; better for training; 624 TFLOPS on A100
- **TF32**: TensorFloat-32; automatic on A100; 156 TFLOPS; no code changes; 8× faster than FP32

**cuBLAS Integration:**
- **Automatic**: cuBLAS automatically uses Tensor Cores; no code changes; cublasGemmEx() for mixed precision
- **Performance**: 80-95% of peak Tensor Core performance; highly optimized; 10-20 TFLOPS on A100
- **Batched**: cublasGemmStridedBatchedEx() for multiple matrices; amortizes overhead; 90-95% efficiency
- **Tuning**: use cublasSetMathMode(CUBLAS_TENSOR_OP_MATH); enables Tensor Cores explicitly

**cuDNN Integration:**
- **Convolution**: cudnnConvolutionForward() uses Tensor Cores; 10-20× faster than CUDA cores
- **RNN**: cudnnRNNForward() uses Tensor Cores for matrix operations; 5-15× speedup
- **Attention**: cudnnMultiHeadAttnForward() optimized for Tensor Cores; 10-30× faster
- **Automatic**: cuDNN automatically selects Tensor Core algorithms; no code changes

**INT8 Quantization:**
- **Throughput**: 1248 TOPS on A100; 4× faster than FP16; 2496 TOPS on H100
- **Accuracy**: 1-2% accuracy loss typical; acceptable for inference; calibration required
- **Quantization**: convert FP32 weights to INT8; scale factors for each layer; TensorRT automates
- **Deployment**: 10-30× faster inference; 4× less memory; enables larger batch sizes

**FP8 (H100):**
- **E4M3**: 4-bit exponent, 3-bit mantissa; for forward pass; 1979 TFLOPS on H100
- **E5M2**: 5-bit exponent, 2-bit mantissa; for gradients; wider range; 1979 TFLOPS
- **Transformer Engine**: automatic FP8 training; maintains FP16 accuracy; 2× faster than FP16
- **Scaling**: per-tensor or per-channel scaling; maintains accuracy; automatic in frameworks

**Memory Considerations:**
- **Bandwidth**: Tensor Cores consume 2-4× more bandwidth than CUDA cores; memory-bound at small sizes
- **Tiling**: use shared memory tiling; reduces global memory accesses; 5-20× speedup
- **Prefetching**: overlap memory transfers with compute; async copy; 20-50% speedup
- **Alignment**: 128-byte alignment critical; misalignment causes 2-10× slowdown

**Occupancy:**
- **Register Usage**: WMMA uses 256-512 registers per warp; limits occupancy; 50-75% typical
- **Shared Memory**: tiling requires 32-64KB per block; limits occupancy; balance with registers
- **Block Size**: 128-256 threads optimal; 4-8 warps per block; maximizes Tensor Core utilization
- **SM Utilization**: 80-100% SM utilization achievable; proper launch configuration critical

**Performance Metrics:**
- **TFLOPS**: measure achieved TFLOPS; compare to peak (312 on A100, 989 on H100); target 50-80%
- **Memory Bandwidth**: measure bandwidth utilization; 80-100% for large matrices; memory-bound for small
- **Occupancy**: 50-75% typical; limited by register usage; acceptable for Tensor Core workloads
- **Efficiency**: TFLOPS / peak TFLOPS; 50-80% achievable with optimization; 80-95% with cuBLAS

**Common Pitfalls:**
- **Wrong Dimensions**: matrix dimensions not multiples of tile size; pad matrices; 10-50% overhead
- **Misalignment**: unaligned memory access; 2-10× slowdown; use cudaMalloc or align manually
- **Wrong Layout**: row-major vs column-major mismatch; incorrect results or slowdown; specify correctly
- **Insufficient Occupancy**: too many registers; limits active warps; reduce register usage or increase block size

**Frameworks Integration:**
- **PyTorch**: automatic Tensor Core usage with torch.cuda.amp; mixed precision training; 2-3× speedup
- **TensorFlow**: automatic mixed precision with tf.keras.mixed_precision; 2-3× speedup
- **JAX**: automatic with jax.default_matmul_precision('high'); 2-3× speedup
- **TensorRT**: automatic INT8 quantization; 10-30× inference speedup; calibration required

**Use Cases:**
- **Training**: large language models, vision transformers; 5-15× faster with Tensor Cores; weeks to days
- **Inference**: real-time inference with INT8; 10-30× faster; enables larger batch sizes
- **Scientific Computing**: matrix-heavy workloads; molecular dynamics, climate modeling; 10-20× speedup
- **Recommendation Systems**: embedding lookups and matrix operations; 5-15× speedup

**Best Practices:**
- **Use Libraries**: cuBLAS, cuDNN, TensorRT; 80-95% of peak; highly optimized; easier than custom kernels
- **Mixed Precision**: FP16/BF16 for compute, FP32 for accumulation; 2× speedup; maintains accuracy
- **Proper Dimensions**: ensure matrix dimensions are multiples of tile size; pad if necessary
- **Profile**: use Nsight Compute; verify Tensor Core utilization; target 50-80% of peak

Tensor Core Programming represents **the key to AI performance on NVIDIA GPUs** — by utilizing specialized matrix multiplication hardware through WMMA API or cuBLAS/cuDNN libraries, developers achieve 10-20× higher throughput (312 TFLOPS on A100, 989 TFLOPS on H100) compared to CUDA cores, enabling 5-15× faster training and 10-30× faster inference through INT8 quantization, making Tensor Core programming essential for AI workloads where proper utilization can reduce training time from weeks to days.');

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/tensor-core-programming) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
