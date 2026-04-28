# Positional Encoding Absolute vs Relative

**Keywords**: positional encoding,absolute vs relative position,transformer position embedding,sequence position modeling

---

**Positional Encoding Absolute vs Relative** compares **fundamental mechanisms for incorporating sequence position information into transformer models — absolute positional embeddings adding position-dependent vectors to inputs while relative encodings embed position differences in attention operations, each enabling different context length generalizations and architectural properties**.

**Absolute Positional Embedding:**
- **Mechanism**: learning position-specific embedding vectors e_pos ∈ ℝ^d_model for each position p ∈ [0, context_length)
- **Addition**: adding position embedding to token embedding: x_p = token_embed(w_p) + pos_embed(p)
- **Learnable Approach**: treating position embeddings as learnable parameters trained with rest of model
- **Formula**: position embedding vectors learned during training, identical across all training examples — shared across batch
- **Context Length Limit**: embeddings only defined for positions seen during training — inference limited to training context length

**Absolute Embedding Characteristics:**
- **Vocabulary**: typically 2048-32768 position embeddings stored in embedding table (similar to word embeddings)
- **Parameter Count**: position embeddings contribute d_model×max_position parameters — non-trivial memory overhead
- **Training Stability**: requires careful initialization; often smaller learning rates for position embeddings vs word embeddings
- **Pre-trained Models**: BERT, GPT-2, early transformers use absolute embeddings; position embeddings not transferable to longer sequences

**Sinusoidal Positional Encoding:**
- **Motivation**: non-learnable encoding providing position information without learnable parameters
- **Formula**: PE(pos, 2i) = sin(pos / 10000^(2i/D)); PE(pos, 2i+1) = cos(pos / 10000^(2i/D))
- **Wavelengths**: varying frequency per dimension (low frequencies capture position globally, high frequencies locally)
- **Mathematical Properties**: designed for relative position perception (transformer can learn relative differences)
- **Extrapolation**: non-learnable periodic pattern enables some extrapolation beyond training length (limited effectiveness)

**Sinusoidal Encoding Advantages:**
- **Explicit Formula**: no learnable parameters, deterministic computation enables efficient position encoding
- **Theoretical Grounding**: designed based on attention mechanics and relative position assumptions
- **Wavelength Separation**: different dimensions encode different time scales enabling multi-scale position representation
- **Parameter Efficiency**: zero parameters for position encoding vs d_model×context_length for learned embeddings

**Relative Positional Encoding:**
- **Core Idea**: encoding relative position differences (j-i) rather than absolute positions
- **Attention Modification**: modifying attention computation to incorporate relative position bias
- **Distance Dependence**: attention score incorporates both content-based similarity and relative position distance
- **Generalization**: relative encodings enable extrapolation to longer sequences not seen during training

**Relative Position Implementation (T5, DeBERTa):**
- **Bias Addition**: adding position-based biases to attention logits before softmax: Attention(Q,K,V) = softmax(QK^T/√d_k + relative_bias) × V
- **Relative Bias Computation**: computing bias matrix of shape [seq_len, seq_len] encoding relative distances
- **Bucket-Based Encoding**: grouping large relative distances into buckets; "within 32 tokens" uses fine-grained distances, ">32 tokens" uses coarse buckets
- **Parameter Efficiency**: relative biases typically 100-200 parameters vs thousands for absolute embeddings

**ALiBi (Attention with Linear Biases):**
- **Formula**: adding linear bias to attention scores proportional to distance: bias(i,j) = -α × |i-j| where α is head-specific
- **Head-Specific Scaling**: different attention heads use different α values (0.25, 0.5, 0.75, etc.) enabling multi-scale distance modeling
- **Zero Parameters**: no position embeddings required — pure linear bias on distances
- **Extrapolation**: theoretically unlimited extrapolation (distances computed dynamically based on actual sequence length)

**ALiBi Performance:**
- **RoPE Comparison**: ALiBi achieves comparable performance to RoPE with simpler mechanism
- **Length Generalization**: training on 512 tokens enables inference on 2048+ with minimal accuracy loss (<1%)
- **Parameter Reduction**: no position embeddings saves d_model×max_context parameters — 16M saved for 32K context
- **Adoption**: BLOOM, MPT models use ALiBi; becoming standard for length-generalization

**Relative Position vs Absolute Trade-offs:**
- **Generalization**: relative position better for length extrapolation (infer on 2K after training on 512)
- **Expressiveness**: absolute embedding theoretically more expressive (dedicated embedding per position)
- **Interpretability**: relative encoding more interpretable (distance-based attention clear); absolute embedding opacity
- **Computational Cost**: relative encoding adds per-token computation (bias addition); absolute embedding constant (already added to input)

**Rotary Position Embedding (RoPE):**
- **Mechanism**: rotating query/key vectors based on position angle — multiplicative rather than additive
- **Formula**: applying 2D rotation to consecutive dimension pairs with angle m·θ where m is position
- **Relative Position Property**: attention score depends on relative position: (Q_m)^T·(K_n) ∝ cos(θ(m-n))
- **Extrapolation**: enabling extrapolation to longer contexts through frequency scaling — base frequency adjusted dynamically
- **Adoption**: Llama, Qwen, modern models standard — becoming dominant positional encoding

**RoPE Advantages:**
- **Explicit Relative Position**: mathematically guarantees relative position focus through rotation mechanics
- **Length Scaling**: enabling context window extension (2K→32K) through simple frequency adjustment without retraining
- **Efficiency**: multiplicative operation enables efficient GPU computation — integrated into attention kernels
- **Interpolation**: linear position interpolation enables fine-grained context extension with <1% accuracy loss

**Empirical Position Encoding Comparison:**
- **Absolute Embeddings**: BERT-base achieves 92.3% on SuperGLUE; training limited to 512 context
- **Sinusoidal**: original Transformer achieves 88.2% on BLEU (machine translation); enables unlimited context theoretically
- **T5 Relative**: achieving 94.5% on SuperGLUE with 512 context; relative encoding improves downstream tasks
- **ALiBi**: BloombergGPT 50B achieves comparable performance to RoPE with simpler mechanism
- **RoPE**: Llama 70B achieves 85.2% on MMLU with 4K context, 32K extended context with interpolation

**Position Encoding in Different Contexts:**
- **Encoder-Only Models**: BERT uses absolute embeddings; T5 uses relative biases; newer models use ALiBi
- **Decoder-Only Models**: GPT-2/3 use absolute embeddings; Llama/Falcon use RoPE; Bloom uses ALiBi
- **Long-Context Models**: length extrapolation critical; RoPE with interpolation standard; ALiBi effective alternative
- **Efficient Models**: mobile/edge models use ALiBi reducing parameter count

**Positional Encoding Absolute vs Relative highlights fundamental design trade-offs — absolute embeddings providing simplicity and parameter expressiveness while relative/multiplicative encodings enabling length extrapolation and modern efficient mechanisms like RoPE and ALiBi.**

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/positional-encoding) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
