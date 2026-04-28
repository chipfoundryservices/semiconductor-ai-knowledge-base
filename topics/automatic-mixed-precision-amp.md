# Automatic Mixed Precision (AMP)

**Keywords**: automatic mixed precision amp,amp pytorch tensorflow,gradient scaler amp,autocast mixed precision,amp performance optimization

---

**Automatic Mixed Precision (AMP)** is **the framework-integrated system that automatically converts operations to optimal precision (FP16/BF16 or FP32) based on operation type and numerical sensitivity — eliminating manual casting, providing dynamic loss scaling, and enabling mixed precision training with 3-5 lines of code, achieving 2-4× speedup and 50% memory reduction while maintaining model accuracy through intelligent operation-level precision selection and automatic gradient scaling**.

**AMP Architecture:**
- **Autocast Context**: with torch.cuda.amp.autocast(): automatically casts operations within context; matrix multiplies → FP16/BF16; reductions → FP32; softmax → FP32; no manual .half() or .float() calls required
- **Operation Whitelist**: GEMM, convolution, attention use FP16/BF16 (Tensor Core operations); benefit from hardware acceleration; constitute 80-95% of training compute
- **Operation Blacklist**: softmax, log_softmax, cross_entropy, layer_norm, batch_norm use FP32; numerically sensitive; require higher precision for stability
- **Operation Graylist**: element-wise operations (add, multiply, ReLU) match input precision; if inputs are FP16, output is FP16; if mixed, promote to FP32; flexible precision based on context

**PyTorch AMP Implementation:**
- **Basic Pattern**: scaler = GradScaler(); with autocast(): output = model(input); loss = criterion(output, target); scaler.scale(loss).backward(); scaler.step(optimizer); scaler.update()
- **GradScaler**: manages loss scaling automatically; scales loss before backward(); unscales gradients before optimizer step; skips step if overflow detected; adjusts scale dynamically
- **Gradient Clipping**: scaler.unscale_(optimizer); torch.nn.utils.clip_grad_norm_(model.parameters(), max_norm); scaler.step(optimizer); — unscale before clipping for correct norm calculation
- **Multiple Optimizers**: separate GradScaler for each optimizer; or single scaler with multiple unscale/step calls; enables complex training loops (GANs, multi-task learning)

**TensorFlow AMP Implementation:**
- **Policy Setup**: policy = tf.keras.mixed_precision.Policy('mixed_float16'); tf.keras.mixed_precision.set_global_policy(policy); applies to all layers and operations
- **Loss Scaling**: optimizer = tf.keras.optimizers.Adam(); optimizer = tf.keras.mixed_precision.LossScaleOptimizer(optimizer); wraps optimizer with automatic loss scaling
- **Custom Training Loop**: with tf.GradientTape() as tape: predictions = model(inputs, training=True); loss = loss_fn(labels, predictions); scaled_loss = optimizer.get_scaled_loss(loss); scaled_gradients = tape.gradient(scaled_loss, model.trainable_variables); gradients = optimizer.get_unscaled_gradients(scaled_gradients); optimizer.apply_gradients(zip(gradients, model.trainable_variables))
- **Keras Integration**: model.compile(optimizer=optimizer, loss=loss, metrics=metrics); model.fit(dataset); — AMP automatic with LossScaleOptimizer; no changes to training loop

**Dynamic Loss Scaling:**
- **Initial Scale**: starts at 2¹⁶ = 65536 (PyTorch) or 32768 (TensorFlow); high enough to prevent most underflow; low enough to avoid immediate overflow
- **Growth**: if no overflow for growth_interval steps (default 2000), scale *= growth_factor (default 2); gradually increases scale to maximize gradient precision
- **Backoff**: if overflow detected (gradient contains Inf/NaN), scale /= backoff_factor (default 2); skip optimizer step; prevents NaN propagation; retries next iteration with lower scale
- **Convergence**: scale converges to optimal value (typically 1024-8192); balances underflow prevention with overflow avoidance; adapts to model and training stage

**Precision Selection Logic:**
- **Compute-Intensive Ops**: operations with O(n³) or O(n²) complexity use FP16/BF16; matrix multiply, convolution, attention; maximize Tensor Core utilization
- **Memory-Intensive Ops**: element-wise operations (O(n)) use input precision; add, multiply, ReLU; precision determined by inputs; minimal compute, precision less critical
- **Numerically Sensitive Ops**: operations with exponentials, logarithms, divisions use FP32; softmax, layer_norm, loss functions; prevent overflow/underflow and maintain accuracy
- **Custom Precision**: @torch.cuda.amp.custom_fwd and @torch.cuda.amp.custom_bwd decorators override default precision; enables fine-grained control for custom operations

**Performance Optimization:**
- **Tensor Core Utilization**: ensure matrix dimensions are multiples of 8 (FP16) or 16 (INT8); non-aligned dimensions reduce Tensor Core efficiency; pad if necessary
- **Batch Size**: larger batches improve Tensor Core utilization; AMP memory savings enable 1.5-2× larger batches; larger batches → better GPU utilization → higher speedup
- **Model Size**: AMP speedup increases with model size; small models (<10M parameters): 1.2-1.5× speedup; large models (>1B parameters): 2-4× speedup; overhead amortized over more compute
- **Operation Fusion**: fused operations (fused_adam, fused_layer_norm) maintain FP16/BF16 throughout; avoid FP16→FP32→FP16 conversions; 10-20% additional speedup

**BF16 vs FP16 in AMP:**
- **BF16 Advantages**: no loss scaling needed; simpler code (no GradScaler); fewer failure modes; same performance as FP16 on Ampere+
- **BF16 Usage**: with torch.cuda.amp.autocast(dtype=torch.bfloat16): — uses BF16 instead of FP16; no other changes; recommended for Ampere/Hopper GPUs
- **FP16 Usage**: default on Volta/Turing; requires GradScaler; more complex but necessary on older hardware
- **Mixed BF16/FP16**: some operations use BF16, others FP16; framework selects based on hardware support; transparent to user

**Debugging AMP Issues:**
- **Overflow Detection**: scaler.get_scale() returns current scale; if scale decreases to <100, frequent overflow; reduce learning rate or use BF16
- **Underflow Detection**: if loss stops decreasing but gradients are non-zero, possible underflow; increase loss scale manually or use BF16
- **Accuracy Regression**: compare AMP vs FP32 accuracy; if AMP <FP32, disable autocast for specific layers; use custom_fwd/custom_bwd to force FP32
- **NaN Debugging**: torch.autograd.set_detect_anomaly(True); identifies operation producing NaN; check for division by zero, log of negative, overflow in exponentials

**Integration with Other Techniques:**
- **Gradient Accumulation**: scaler.scale(loss / accumulation_steps).backward(); accumulate gradients; scaler.step() only after accumulation complete; compatible with AMP
- **Distributed Training**: AMP works with DDP, FSDP, DeepSpeed; each rank has independent GradScaler; all-reduce operates on scaled gradients; unscale before optimizer step
- **Gradient Checkpointing**: checkpoint saves FP16/BF16 activations; recomputation uses autocast; 50% memory savings from AMP + 50% from checkpointing = 75% total savings
- **ZeRO Optimizer**: AMP reduces activation memory; ZeRO reduces optimizer state memory; combined enable training 10× larger models

**Profiling AMP Performance:**
- **Tensor Core Utilization**: nsight compute reports sm_efficiency and tensor_precision_fu_utilization; target >80% for compute-bound kernels
- **Memory Bandwidth**: AMP reduces memory traffic by 50%; measure achieved bandwidth; should be 1.5-2× higher than FP32
- **Speedup Measurement**: compare wall-clock time per epoch; AMP vs FP32; typical speedup 1.5-3× for large models; <1.5× indicates insufficient Tensor Core usage
- **Memory Usage**: nvidia-smi shows memory consumption; AMP should reduce by 30-50%; enables larger batch sizes or models

**Best Practices:**
- **Always Use AMP**: on Volta+ GPUs, AMP provides free speedup; no accuracy loss for most models; 3-5 lines of code; no reason not to use
- **Prefer BF16 on Ampere+**: simpler, more stable, same performance; FP16 only for Volta/Turing
- **Combine with Other Optimizations**: AMP + gradient accumulation + checkpointing + FSDP enables training 100B+ models on 8×40GB GPUs
- **Monitor Scale**: if scale <1000 or >100000, investigate; optimal scale typically 1024-8192; extreme values indicate numerical issues

Automatic Mixed Precision is **the productivity breakthrough that makes mixed precision training accessible to all developers — by automating precision selection, loss scaling, and gradient management, AMP delivers 2-4× training speedup and 50% memory reduction with minimal code changes, making it the default training mode for modern deep learning and the foundation for training large-scale models efficiently**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/automatic-mixed-precision-amp) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
