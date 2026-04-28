# Speculative Decoding

**Keywords**: speculative decoding llm,draft model verification,parallel token generation,speculative sampling inference,assisted generation

---

**Speculative Decoding** is **the inference acceleration technique that uses a small draft model to generate multiple candidate tokens in parallel, then verifies them with the target model in a single forward pass** — achieving 2-3× speedup for autoregressive generation while producing identical outputs to standard decoding, making it the most practical lossless inference optimization for large language models deployed in production.

**Core Algorithm:**
- **Draft Generation**: small fast model (100M-1B parameters) generates K candidate tokens (typically K=4-8) autoregressively; draft model runs K times faster than target model due to size; candidates may be incorrect but provide speculation targets
- **Parallel Verification**: target model processes all K candidates in single forward pass using batched computation; computes logits for positions 1 through K; verifies each candidate against target model distribution
- **Acceptance Criterion**: for each position i, accept draft token if it appears in top-p or top-k of target distribution; or accept with probability min(1, p_target(token)/p_draft(token)) for exact distribution matching; reject remaining tokens after first rejection
- **Fallback Sampling**: if all K tokens accepted, sample K+1-th token from target model; if rejection at position j, sample new token from modified distribution that accounts for draft model bias; ensures output distribution matches standard autoregressive sampling

**Mathematical Guarantees:**
- **Distribution Preservation**: speculative decoding produces identical token distribution to standard sampling; proven through rejection sampling theory; no quality degradation or hallucination increase
- **Expected Speedup**: E[tokens_per_step] = Σ(i=1 to K) α^i + α^K where α is per-token acceptance rate; at α=0.6, K=4: expect 1.9 tokens/step; at α=0.8, K=8: expect 4.0 tokens/step
- **Worst Case**: if draft model always wrong (α=0), generates 1 token per step like standard decoding; no slowdown, only overhead of draft model computation (typically <10% of target model cost)
- **Best Case**: if draft model perfect (α=1), generates K tokens per step; K× speedup limited only by draft model speed and verification overhead

**Draft Model Selection:**
- **Distilled Models**: train small model to mimic target model; 10-20× smaller (7B → 700M, 70B → 3B); achieves α=0.6-0.8 on in-domain text; requires distillation training but highest acceptance rates
- **Earlier Checkpoints**: use intermediate checkpoint from target model training; no additional training; α=0.5-0.7; works well when target model is fine-tuned version (use base model as draft)
- **Smaller Model Family**: use smaller model from same family (Llama 2 7B drafts for 70B); α=0.4-0.6; no training needed; readily available; lower acceptance but still 1.5-2× speedup
- **Prompt Lookup**: for tasks with repetitive patterns, use n-gram matching in prompt as draft; zero-parameter approach; α=0.3-0.5 for code completion, documentation; fails for creative generation

**Implementation Optimizations:**
- **Batched Verification**: process all K positions in single forward pass; requires attention mask that allows position i to attend to positions 0..i; increases memory by K× but reduces latency by K×
- **KV Cache Reuse**: draft model and target model share KV cache for accepted tokens; reduces memory; requires compatible architectures (same hidden size, attention structure)
- **Adaptive K**: adjust speculation depth based on acceptance rate; increase K when α high, decrease when α low; typical range K=2-10; improves average-case performance
- **Tree-Based Speculation**: generate multiple candidate sequences in tree structure; verify all branches in parallel; increases acceptance probability; used in Medusa, EAGLE methods; 3-4× speedup vs linear speculation

**Performance Characteristics:**
- **Latency Reduction**: 2-3× faster time-to-completion for typical workloads; 1.5× for creative writing (low α), 3-4× for code completion (high α); benefits increase with longer generations
- **Throughput Impact**: single-request latency improves but throughput may decrease due to increased memory usage; optimal for latency-sensitive applications (chatbots, interactive tools) rather than batch processing
- **Memory Overhead**: requires loading draft model (1-3GB) plus K× larger KV cache during verification; total memory increase 20-40%; acceptable trade-off for 2-3× latency improvement
- **Hardware Utilization**: better GPU utilization during verification (batched computation) vs standard decoding (sequential); increases arithmetic intensity; reduces memory-bound bottleneck

**Production Deployment:**
- **Framework Support**: implemented in Hugging Face Transformers (generate with assistant_model), vLLM, TensorRT-LLM, llama.cpp; easy integration with existing inference pipelines
- **Model Compatibility**: requires draft and target models with same tokenizer and vocabulary; compatible architectures preferred but not required; works across different model families with tokenizer alignment
- **Quality Validation**: extensive testing shows no quality degradation on benchmarks (MMLU, HumanEval, TruthfulQA); user studies confirm identical outputs; safe for production deployment
- **Cost-Benefit**: 2-3× latency reduction with 20-40% memory increase; favorable trade-off for user-facing applications where latency matters; reduces infrastructure cost per request by 40-60%

**Advanced Variants:**
- **Medusa**: adds multiple decoding heads to target model; generates tree of candidates; verifies all paths in parallel; 2.2-3.6× speedup; requires model modification and training
- **EAGLE**: uses auto-regression head on draft model features; higher acceptance rates (α=0.7-0.9); 3-4× speedup; requires training draft model with special objective
- **Lookahead Decoding**: generates multiple tokens per position; uses n-gram matching and Jacobi iteration; no draft model needed; 1.5-2× speedup; works for any model without modification
- **REST (Retrieval-Based Speculative Decoding)**: retrieves similar completions from database; uses as draft candidates; effective for repetitive domains (code, legal documents); α=0.6-0.8 with zero training

Speculative Decoding is **the rare optimization that provides substantial speedup without any quality trade-off** — by exploiting the gap between small fast models and large accurate models through parallel verification, it has become the standard technique for reducing LLM inference latency in production systems where response time directly impacts user experience.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/speculative-decoding-llm) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
