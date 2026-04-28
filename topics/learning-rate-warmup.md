# Learning Rate Warmup and Cosine Scheduling

**Keywords**: learning rate warmup,cosine annealing schedule,training schedule,optimization convergence,temperature scheduling

---

**Learning Rate Warmup and Cosine Scheduling** are **complementary techniques that strategically adjust learning rates during training — gradually increasing learning rate in warmup phase prevents gradient shock and poor weight initialization, while cosine annealing smoothly reduces learning rate to enable fine-grained optimization enabling both faster convergence and better final performance**.

**Learning Rate Warmup Phase:**
- **Linear Warmup**: increasing learning rate from 0 to target_lr over warmup_steps (typically 1000-10000 steps) — linear_lr(t) = target_lr × (t / warmup_steps)
- **Initialization Impact**: with random weight initialization, early gradients large and noisy — warmup prevents large updates that destabilize training
- **Adam Optimizer Interaction**: warmup especially important for Adam; without it, early adaptive learning rates become too aggressive
- **Warmup Duration**: typically 10% of training steps for smaller models, 5% for large models — shorter warmup for well-initialized models
- **BERT Standard**: using 10K warmup steps over 100K total steps (10% ratio) — consistent across BERT variants

**Mathematical Formulation:**
- **Linear Warmup**: lr(t) = min(t/warmup_steps, 1) × base_lr for t ≤ warmup_steps
- **Learning Rate at Step t**: combines warmup with base schedule (e.g., cosine) applied to warmup-scaled values
- **Gradient Impact**: with warmup, gradient magnitudes typically 0.1-0.5 in early steps, increasing to 1.0-2.0 by warmup end
- **Loss Curvature**: warmup allows model to move into low-loss regions before aggressive optimization

**Cosine Annealing Schedule:**
- **Formula**: lr(t) = base_lr × (1 + cos(π·t/T))/2 where t is current step, T is total steps — smooth decay from base_lr to ≈0
- **Characteristics**: slow initial decay, faster mid-training, asymptotic approach to zero — natural optimization progression
- **Restart Schedules**: periodic resets (warm restarts) enable escape from local minima — "SGDR" schedule with periodic restarts
- **Cosine vs Linear**: cosine provides smoother gradients, avoiding sudden learning rate drops that cause optimization disruption

**Training Curve Behavior:**
- **Warmup Phase (0-10K steps)**: loss decreases slowly (2-5% improvement per 1K steps), highly variable
- **Main Training (10K-90K steps)**: rapid loss decrease (10-20% per 10K steps), smooth convergence trajectory
- **Annealing Phase (90K-100K steps)**: fine-grained optimization, loss improvements <1% per step
- **Final Performance**: cosine annealing achieves 1-2% better validation accuracy than linear decay over same epoch count

**Practical Examples and Benchmarks:**
- **BERT-Base Training**: 1M steps total, 10K linear warmup, then cosine decay to near-zero — 97.0% accuracy on GLUE (SuperGLUE benchmark)
- **GPT-2 Training**: 500K steps, 500 warmup steps (0.1%), then cosine decay — loss 2.4 on WikiText-103 (SOTA at publication)
- **Llama 2 Training**: 2M steps, linear warmup 0.2%, cosine decay — achieves consistent performance across model scales (7B to 70B)
- **T5 Training**: 1M steps, warmup 10K, cosine decay with minimum learning rate (0.1 × base) — prevents learning rate from decaying to zero

**Advanced Scheduling Variants:**
- **Warmup and Polynomial Decay**: lr = base_lr × max(0, 1 - t/total_steps)^p where p ∈ [0.5, 2.0] — alternative to cosine
- **Step-Based Decay**: reducing learning rate by factor (e.g., 0.1×) at specific steps — enables coarse-grained control
- **Exponential Decay**: lr(t) = base_lr × decay_rate^t — smooth exponential decrease
- **Inverse Square Root**: lr(t) = c / √t — used in original Transformer paper, enables adaptive scaling to batch size

**Interaction with Batch Size:**
- **Large Batch Training**: larger batch sizes benefit from higher learning rates during warmup — enables faster convergence
- **Scaling Rule**: lr_new = lr_old × √(batch_size_new / batch_size_old) — LARS optimizer implements this
- **Warmup Adjustment**: warmup steps scale with effective batch size — warmup_steps_new = warmup_steps × (batch_size_new / batch_size_old)
- **Linear Scaling Hypothesis**: loss-batch size relationship enables proportional learning rate scaling

**Optimizer-Specific Considerations:**
- **SGD Warmup**: less critical than Adam, but still helpful for stability — simple learning rate schedule often sufficient
- **Adam Warmup**: essential due to adaptive learning rate behavior — without warmup, early adaptive rates too aggressive
- **LAMB Optimizer**: layer-wise adaptation enables larger batch sizes — reduces warmup importance but still beneficial
- **AdamW (Decoupled Weight Decay)**: improved optimizer enabling larger learning rates — warmup remains important for stability

**Multi-Phase Training Strategies:**
- **Pre-training then Fine-tuning**: pre-training uses full warmup and cosine schedule over millions of steps; fine-tuning uses short warmup (500-1000 steps) with aggressive cosine decay
- **Progressive Warmup**: gradual increase of batch size combined with learning rate warmup — enables stable large-batch training
- **Cyclic Learning Rates**: combining warmup with periodic restarts — enables exploration of different loss regions
- **Curriculum Learning Integration**: warmup enables starting with easy examples, then annealing to harder distribution — improves sample efficiency

**Empirical Tuning Guidelines:**
- **Warmup Fraction**: 5-10% of total training steps (10K out of 100K-200K typical) — longer for larger models or harder tasks
- **Cosine Minimum**: setting minimum learning rate (e.g., 0.1 × base) prevents decay to exactly zero — maintains gradient signal
- **Base Learning Rate**: determined separately through grid search; typically 1e-4 to 5e-4 for fine-tuning, 1e-3 for pre-training
- **Total Steps**: estimated based on epochs × steps_per_epoch; commonly 1-3M steps for pre-training, 10K-100K for fine-tuning

**Distributed Training Considerations:**
- **Synchronization**: warmup and annealing affect gradient updates across devices — consistent schedules important for reproducibility
- **Effective Batch Size**: total batch size (per-GPU × num_GPUs) determines learning rate scaling — warmup duration should scale proportionally
- **Checkpointing and Resumption**: maintaining consistent learning rate schedule across checkpoint restarts — track step count globally

**Learning Rate Warmup and Cosine Scheduling are fundamental optimization techniques — enabling stable training of deep networks through strategic learning rate management that combines initialization protection (warmup) with smooth convergence (cosine annealing).**

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/learning-rate-warmup) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
