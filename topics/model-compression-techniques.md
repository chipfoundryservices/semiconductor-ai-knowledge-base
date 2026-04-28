# Model Compression Techniques

**Keywords**: model compression techniques,neural network pruning,weight pruning structured,magnitude pruning lottery ticket,compression deep learning

---

**Model Compression Techniques** are **the family of methods that reduce neural network size, memory footprint, and computational cost while preserving accuracy — including pruning (removing unnecessary weights or neurons), quantization (reducing precision), knowledge distillation (training smaller models), and architecture search for efficient designs, enabling deployment on resource-constrained devices and reducing inference costs**.

**Magnitude-Based Pruning:**
- **Unstructured Pruning**: removes individual weights with smallest absolute values; prune weights where |w| < threshold or keep top-k% by magnitude; achieves high compression ratios (90-95% sparsity) with minimal accuracy loss but requires sparse matrix operations for speedup; standard dense hardware doesn't accelerate unstructured sparsity
- **Structured Pruning**: removes entire channels, filters, or layers rather than individual weights; maintains dense computation that runs efficiently on standard hardware; typical compression: 30-50% of channels removed with 1-3% accuracy loss; directly reduces FLOPs and memory without specialized kernels
- **Iterative Magnitude Pruning (IMP)**: train → prune lowest magnitude weights → retrain → repeat; gradual pruning over multiple iterations preserves accuracy better than one-shot pruning; Han et al. (2015) achieved 90% sparsity on AlexNet with minimal accuracy loss
- **Pruning Schedule**: pruning rate typically follows cubic schedule: s_t = s_f + (s_i - s_f)(1 - t/T)³ where s_i is initial sparsity, s_f is final sparsity, t is current step, T is total steps; gradual pruning allows the network to adapt to increasing sparsity

**Lottery Ticket Hypothesis:**
- **Core Idea**: dense networks contain sparse subnetworks (winning tickets) that, when trained in isolation from initialization, match the full network's performance; finding these subnetworks enables training sparse models from scratch rather than pruning dense models
- **Winning Ticket Identification**: train dense network, prune to sparsity s, rewind weights to initialization (or early training checkpoint), retrain the sparse mask; the resulting sparse network achieves comparable accuracy to the original dense network
- **Implications**: suggests that much of a network's capacity is redundant; the critical factor is finding the right sparse connectivity pattern, not the final weight values; challenges the necessity of overparameterization for training
- **Practical Limitations**: finding winning tickets requires training the full dense network first (no computational savings during search); works well at moderate sparsity (50-80%) but breaks down at extreme sparsity (>95%); more of a scientific insight than a practical compression method

**Structured Pruning Methods:**
- **Channel Pruning**: removes entire convolutional filters/channels based on importance metrics; importance measured by L1/L2 norm of filter weights, activation statistics, or gradient-based sensitivity; directly reduces FLOPs and memory with no specialized hardware needed
- **Layer Pruning**: removes entire layers from deep networks; surprisingly, many layers can be removed with minimal accuracy loss; BERT can drop 25-50% of layers with <2% accuracy degradation; requires careful selection of which layers to remove (middle layers often more redundant than early/late)
- **Attention Head Pruning**: removes entire attention heads in Transformers; many heads are redundant or attend to similar patterns; pruning 20-40% of heads typically has minimal impact; enables faster attention computation and reduced KV cache memory
- **Width Pruning**: reduces hidden dimensions uniformly across all layers; simpler than selective channel pruning but less efficient (removes capacity uniformly rather than targeting redundant channels)

**Dynamic and Adaptive Pruning:**
- **Dynamic Sparse Training**: maintains constant sparsity throughout training by periodically removing low-magnitude weights and growing new connections; RigL (Rigging the Lottery) grows weights with largest gradient magnitudes; enables training sparse networks from scratch without dense pre-training
- **Gradual Magnitude Pruning (GMP)**: increases sparsity gradually during training following a schedule; used in TensorFlow Model Optimization Toolkit; simpler than iterative pruning (single training run) but typically achieves lower compression ratios
- **Movement Pruning**: prunes weights that move toward zero during training rather than weights with small magnitude; considers weight trajectory, not just current value; achieves better accuracy-sparsity trade-offs for Transformers
- **Soft Pruning**: uses continuous relaxation of binary masks (differentiable pruning); learns pruning masks via gradient descent; L0 regularization encourages sparsity; enables end-to-end pruning without iterative train-prune cycles

**Pruning for Specific Architectures:**
- **Transformer Pruning**: attention heads, FFN intermediate dimensions, and entire layers can be pruned; structured pruning of FFN (removing rows/columns) is most effective; CoFi (Coarse-to-Fine Pruning) achieves 50% compression with <1% accuracy loss on BERT
- **CNN Pruning**: filter pruning is standard; early layers are more sensitive (contain low-level features); later layers are more redundant; pruning ratios typically vary by layer (10-30% early, 50-70% late)
- **LLM Pruning**: SparseGPT enables one-shot pruning of LLMs to 50-60% sparsity with minimal perplexity increase; Wanda (Pruning by Weights and Activations) uses activation statistics to identify important weights; enables running 70B models with 50% fewer parameters

**Combining Compression Techniques:**
- **Pruning + Quantization**: prune to 50% sparsity, then quantize to INT8; achieves 8-10× compression with 1-2% accuracy loss; order matters — typically prune first, then quantize
- **Pruning + Distillation**: prune the teacher model, then distill to a smaller student; combines structural compression (pruning) with capacity transfer (distillation); achieves better accuracy than pruning alone
- **AutoML for Compression**: neural architecture search finds optimal pruning ratios per layer; NetAdapt, AMC (AutoML for Model Compression) automatically determine layer-wise compression policies; achieves better accuracy-efficiency trade-offs than uniform pruning

Model compression techniques are **essential for democratizing AI deployment — enabling state-of-the-art models to run on smartphones, embedded devices, and edge hardware by removing the 50-90% of parameters that contribute minimally to accuracy, making advanced AI accessible beyond datacenter-scale infrastructure**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/model-compression-techniques) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
