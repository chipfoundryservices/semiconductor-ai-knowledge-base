# Absorbing State Diffusion

**Keywords**: absorbing state diffusion, generative models

---

**Absorbing State Diffusion** for text is a diffusion approach where **tokens gradually transition toward a special mask token (absorbing state)** — providing a natural discrete diffusion process where the forward process masks tokens with increasing probability and the reverse process learns to unmask, connecting diffusion models to masked language modeling like BERT.

**What Is Absorbing State Diffusion?**

- **Definition**: Diffusion process where tokens transition to [MASK] token (absorbing state).
- **Forward**: Tokens randomly replaced with [MASK] with increasing probability over time.
- **Reverse**: Model learns to predict original tokens from partially masked sequences.
- **Key Insight**: Masking is natural discrete corruption process.

**Why Absorbing State Diffusion?**

- **Natural for Discrete Data**: Masking is intuitive corruption for text.
- **Connection to BERT**: Leverages masked language modeling insights.
- **Simpler Than Continuous**: No embedding/projection complications.
- **Interpretable**: Easy to understand forward and reverse processes.
- **Effective**: Competitive with other discrete diffusion approaches.

**How It Works**

**Forward Process (Masking)**:
- **Start**: Clean text sequence x_0 = [token_1, token_2, ..., token_n].
- **Step t**: Each token has probability q(t) of being [MASK].
- **Schedule**: q(t) increases from 0 to 1 as t goes from 0 to T.
- **End**: x_T is fully masked [MASK, MASK, ..., MASK].

**Transition Probabilities**:
```
P(x_t = [MASK] | x_{t-1} = token) = β_t
P(x_t = token | x_{t-1} = token) = 1 - β_t
P(x_t = token | x_{t-1} = [MASK]) = 0  (absorbing!)
```
- **Absorbing**: Once masked, stays masked (can't unmask in forward process).
- **Schedule**: β_t defines masking rate at each step.

**Reverse Process (Unmasking)**:
- **Start**: Fully masked sequence x_T.
- **Model**: Transformer predicts original tokens from masked sequence.
- **Input**: Partially masked sequence + timestep t.
- **Output**: Probability distribution over tokens for each [MASK] position.
- **Sampling**: Sample tokens from predicted distribution, gradually unmask.

**Connection to BERT**

**Similarities**:
- **Masking**: Both use [MASK] token as corruption.
- **Prediction**: Both predict original tokens from masked context.
- **Bidirectional**: Both use bidirectional context for prediction.

**Differences**:
- **BERT**: Single masking level (15% typically), single prediction step.
- **Diffusion**: Multiple masking levels, iterative unmasking over T steps.
- **BERT**: Trained for representation learning.
- **Diffusion**: Trained for generation.

**Insight**: Absorbing state diffusion generalizes BERT to iterative generation.

**Training**

**Objective**:
- **Loss**: Cross-entropy between predicted and true tokens at masked positions.
- **Sampling**: Sample timestep t, mask according to schedule, predict original.
- **Optimization**: Standard supervised learning, no adversarial training.

**Training Algorithm**:
```
1. Sample clean sequence x_0 from dataset
2. Sample timestep t ~ Uniform(1, T)
3. Mask tokens according to schedule q(t)
4. Model predicts original tokens from masked sequence
5. Compute cross-entropy loss on masked positions
6. Backpropagate and update model
```

**Masking Schedule**:
- **Linear**: q(t) = t/T (uniform masking rate increase).
- **Cosine**: q(t) = cos²(πt/2T) (slower at start, faster at end).
- **Tuning**: Schedule affects generation quality, requires tuning.

**Generation (Sampling)**

**Iterative Unmasking**:
```
1. Start with fully masked sequence x_T = [MASK, ..., MASK]
2. For t = T down to 1:
   a. Model predicts token probabilities for each [MASK]
   b. Sample tokens from predicted distributions
   c. Unmask some positions (according to schedule)
   d. Keep other positions masked for next iteration
3. Final x_0 is generated text
```

**Unmasking Strategy**:
- **Confidence-Based**: Unmask positions with highest prediction confidence.
- **Random**: Randomly select positions to unmask.
- **Scheduled**: Unmask fixed fraction at each step.

**Temperature**:
- **Sampling**: Use temperature to control randomness.
- **Low Temperature**: More deterministic, higher quality.
- **High Temperature**: More diverse, more creative.

**Advantages**

**Natural Discrete Process**:
- **No Embedding**: No need to embed to continuous space.
- **No Projection**: No projection back to discrete tokens.
- **Interpretable**: Masking and unmasking are intuitive.

**Leverages BERT Insights**:
- **Pretrained Models**: Can initialize from BERT-like models.
- **Masked LM**: Builds on well-understood masked language modeling.
- **Transfer Learning**: Leverage existing masked LM research.

**Flexible Generation**:
- **Infilling**: Naturally handles filling masked spans.
- **Partial Generation**: Can fix some tokens, generate others.
- **Iterative Refinement**: Multiple passes improve quality.

**Controllable**:
- **Guidance**: Easy to apply constraints during unmasking.
- **Conditional**: Condition on various signals.
- **Editing**: Modify specific parts while keeping others.

**Limitations**

**Multiple Steps Required**:
- **Slow**: Requires T forward passes (typically T=50-1000).
- **Latency**: Higher latency than single autoregressive pass.
- **Trade-Off**: Quality vs. speed.

**Unmasking Order**:
- **Challenge**: Optimal unmasking order unclear.
- **Heuristics**: Confidence-based works but not optimal.
- **Impact**: Order affects generation quality.

**Long-Range Dependencies**:
- **Challenge**: Iterative unmasking may struggle with long-range coherence.
- **Autoregressive Advantage**: Left-to-right maintains coherence naturally.
- **Mitigation**: Careful schedule, more steps.

**Examples & Implementations**

**D3PM (Discrete Denoising Diffusion Probabilistic Models)**:
- **Approach**: Absorbing state diffusion for discrete data.
- **Application**: Text, images, graphs.
- **Performance**: Competitive with autoregressive on some tasks.

**MDLM (Masked Diffusion Language Model)**:
- **Approach**: Absorbing state diffusion specifically for language.
- **Connection**: Explicit connection to masked language modeling.
- **Performance**: Strong results on text generation benchmarks.

**Applications**

**Text Infilling**:
- **Task**: Fill in missing parts of text.
- **Advantage**: Naturally handles arbitrary masked spans.
- **Use Case**: Document completion, story writing.

**Controlled Generation**:
- **Task**: Generate text with constraints.
- **Advantage**: Easy to fix certain tokens, generate others.
- **Use Case**: Template filling, constrained generation.

**Text Editing**:
- **Task**: Modify specific parts of text.
- **Advantage**: Mask regions to edit, unmask with new content.
- **Use Case**: Paraphrasing, style transfer, improvement.

**Tools & Resources**

- **Research Papers**: D3PM, MDLM papers and code.
- **Implementations**: PyTorch/JAX implementations on GitHub.
- **Experimental**: Not yet in production frameworks.

Absorbing State Diffusion is **a promising approach for discrete diffusion** — by using masking as the corruption process, it provides a natural, interpretable way to apply diffusion to text that connects to successful masked language modeling, offering advantages in infilling, editing, and controllable generation while remaining simpler than continuous embedding approaches.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/absorbing-state-diffusion) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
