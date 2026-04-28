# Mixed Precision Training

**Keywords**: mixed precision training,fp16 training,bfloat16 bf16,automatic mixed precision amp,loss scaling gradient

---

**Mixed Precision Training** is **the technique of using lower-precision floating-point formats (FP16 or BF16) for most computations while maintaining FP32 precision for critical operations — leveraging Tensor Cores to achieve 2-4× training speedup and 50% memory reduction, while preserving model accuracy through careful loss scaling, master weight copies, and selective FP32 operations, making it the standard practice for training large neural networks on modern GPUs**.

**Precision Formats:**
- **FP32 (Float32)**: 1 sign bit, 8 exponent bits, 23 mantissa bits; range: ±3.4×10³⁸; precision: ~7 decimal digits; standard precision for deep learning; no special hardware acceleration
- **FP16 (Float16/Half)**: 1 sign bit, 5 exponent bits, 10 mantissa bits; range: ±6.5×10⁴; precision: ~3 decimal digits; 2× memory savings, 8-16× Tensor Core speedup; prone to overflow/underflow
- **BF16 (BFloat16)**: 1 sign bit, 8 exponent bits, 7 mantissa bits; range: ±3.4×10³⁸ (same as FP32); precision: ~2 decimal digits; same range as FP32 eliminates overflow issues; preferred on Ampere/Hopper
- **TF32 (TensorFloat-32)**: 1 sign bit, 8 exponent bits, 10 mantissa bits; internal format for Tensor Cores on Ampere+; FP32 range with reduced precision; automatic (no code changes); 8× speedup over FP32

**Mixed Precision Components:**
- **FP16/BF16 Activations and Weights**: forward pass uses FP16/BF16; backward pass computes gradients in FP16/BF16; 50% memory reduction for activations and gradients; 2× memory bandwidth efficiency
- **FP32 Master Weights**: optimizer maintains FP32 copy of weights; updates computed in FP32; updated weights cast to FP16/BF16 for next iteration; prevents accumulation of rounding errors in weight updates
- **FP32 Accumulation**: matrix multiplication uses FP16/BF16 inputs but FP32 accumulation; Tensor Cores perform D = A×B + C with A,B in FP16/BF16 and C,D in FP32; maintains numerical stability
- **Loss Scaling (FP16 only)**: multiply loss by scale factor (1024-65536) before backward pass; scales gradients to prevent underflow; unscale before optimizer step; not needed for BF16 (wider range)

**Automatic Mixed Precision (AMP):**
- **PyTorch AMP**: from torch.cuda.amp import autocast, GradScaler; with autocast(): output = model(input); loss = criterion(output, target); scaler.scale(loss).backward(); scaler.step(optimizer); scaler.update()
- **Automatic Casting**: autocast() automatically casts operations to FP16/BF16 or FP32 based on operation type; matrix multiplies → FP16; reductions → FP32; softmax → FP32; no manual casting required
- **Dynamic Loss Scaling**: GradScaler automatically adjusts loss scale; increases scale if no overflow; decreases scale if overflow detected; finds optimal scale without manual tuning
- **TensorFlow AMP**: policy = tf.keras.mixed_precision.Policy('mixed_float16'); tf.keras.mixed_precision.set_global_policy(policy); automatic casting and loss scaling; integrated with Keras API

**Loss Scaling for FP16:**
- **Gradient Underflow**: small gradients (<2⁻²⁴ ≈ 6×10⁻⁸) underflow to zero in FP16; common in later training stages; causes convergence stagnation
- **Scaling Mechanism**: multiply loss by scale S (typically 1024-65536); gradients scaled by S; prevents underflow; unscale before optimizer step: gradient_unscaled = gradient_scaled / S
- **Overflow Detection**: if any gradient overflows (>65504 in FP16), skip optimizer step; reduce scale by 2×; retry next iteration; prevents NaN propagation
- **Dynamic Scaling**: start with scale=65536; if no overflow for N steps (N=2000), increase scale by 2×; if overflow, decrease scale by 2×; converges to optimal scale automatically

**BF16 Advantages:**
- **No Loss Scaling**: BF16 has same exponent range as FP32; gradient underflow extremely rare; eliminates loss scaling complexity and overhead
- **Simpler Implementation**: no GradScaler needed; direct casting to BF16 sufficient; fewer failure modes (no overflow/underflow issues)
- **Better Stability**: training stability comparable to FP32; FP16 occasionally diverges even with loss scaling; BF16 rarely diverges
- **Hardware Support**: Ampere (A100, RTX 30xx), Hopper (H100), AMD MI200+ support BF16 Tensor Cores; older GPUs (Volta, Turing) only support FP16

**Performance Gains:**
- **Tensor Core Speedup**: A100 FP16 Tensor Cores: 312 TFLOPS vs 19.5 TFLOPS FP32 CUDA Cores — 16× speedup; H100 FP8: 1000+ TFLOPS — 20× speedup
- **Memory Bandwidth**: FP16/BF16 activations and gradients use 50% memory; 2× effective bandwidth; enables larger batch sizes or models
- **Training Time**: typical speedup 1.5-3× for large models (BERT, GPT, ResNet); speedup higher for models with large matrix multiplications; minimal speedup for small models (overhead dominates)
- **Memory Savings**: 30-50% total memory reduction; enables 1.5-2× larger batch sizes; critical for training large models (70B+ parameters)

**Operation-Specific Precision:**
- **FP16/BF16 Operations**: matrix multiplication (GEMM), convolution, attention; benefit from Tensor Cores; majority of compute time
- **FP32 Operations**: softmax, layer norm, batch norm, loss functions; numerically sensitive; require higher precision for stability
- **FP32 Reductions**: sum, mean, variance; accumulation in FP16 causes rounding errors; FP32 accumulation maintains accuracy
- **Mixed Operations**: attention = softmax(Q×K/√d) × V; Q×K in FP16, softmax in FP32, result×V in FP16; automatic in AMP

**Numerical Stability Techniques:**
- **Gradient Clipping**: clip gradients to maximum norm; prevents exploding gradients; more important in mixed precision; clip before unscaling (PyTorch) or after (TensorFlow)
- **Epsilon in Denominators**: use larger epsilon (1e-5 instead of 1e-8) in layer norm, batch norm; prevents division by near-zero in FP16
- **Attention Scaling**: scale attention logits by 1/√d before softmax; prevents overflow in FP16; standard practice in Transformers
- **Residual Connections**: add residuals in FP32 when possible; prevents accumulation of rounding errors; critical for very deep networks (100+ layers)

**Debugging Mixed Precision Issues:**
- **NaN/Inf Detection**: check for NaN/Inf in activations and gradients; torch.isnan(tensor).any(); indicates numerical instability
- **Loss Divergence**: loss suddenly jumps to NaN or infinity; caused by overflow or underflow; reduce learning rate or adjust loss scale
- **Accuracy Degradation**: mixed precision accuracy <FP32 accuracy; increase FP32 operations (disable autocast for sensitive layers); use BF16 instead of FP16
- **Profiling**: nsight compute shows Tensor Core utilization; target >80%; low utilization indicates insufficient mixed precision usage or small batch sizes

**Best Practices:**
- **Use BF16 on Ampere+**: simpler, more stable, same performance as FP16; FP16 only for Volta/Turing GPUs
- **Enable TF32**: torch.backends.cuda.matmul.allow_tf32 = True; automatic 8× speedup for FP32 code on Ampere+; no code changes
- **Gradient Accumulation**: compatible with mixed precision; scale loss by accumulation_steps and loss_scale; reduces memory further
- **Large Batch Sizes**: mixed precision memory savings enable larger batches; larger batches improve GPU utilization; balance with convergence requirements

Mixed precision training is **the foundational optimization for modern deep learning — by leveraging specialized Tensor Core hardware and careful numerical techniques, it achieves 2-4× training speedup and 50% memory reduction with minimal accuracy impact, making it essential for training large models efficiently and the default training mode for all production deep learning workloads**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/mixed-precision-training) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
