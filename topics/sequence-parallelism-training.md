# Sequence Parallelism

**Keywords**: sequence parallelism training,long sequence distributed,context parallelism,sequence dimension partition,ulysses sequence parallel

---

**Sequence Parallelism** is **the parallelism technique that partitions the sequence dimension across multiple devices to reduce activation memory for long-context training** — enabling training on sequences 4-16× longer than possible on single GPU by distributing activations along sequence length, achieving near-linear scaling when combined with tensor parallelism for models with 32K-100K+ token contexts.

**Sequence Parallelism Motivation:**
- **Activation Memory Bottleneck**: for sequence length L, batch size B, hidden dimension H, num layers N: activation memory = O(B×L×H×N); grows linearly with sequence length; limits context to 2K-8K tokens on single GPU
- **Tensor Parallelism Limitation**: tensor parallelism partitions hidden dimension but not sequence dimension; activations still O(B×L×H/P) per device; sequence length remains bottleneck for long contexts
- **Memory Scaling**: doubling sequence length doubles activation memory; 32K context requires 8× memory vs 4K; sequence parallelism enables linear scaling with device count
- **Example**: Llama 2 70B with 32K context requires 120GB activation memory; exceeds single A100 80GB; sequence parallelism across 2 GPUs reduces to 60GB per GPU

**Sequence Parallelism Strategies:**
- **Megatron Sequence Parallelism**: partitions sequence dimension in non-tensor-parallel regions (layer norm, dropout, residual); combined with tensor parallelism for attention/FFN; reduces activation memory by P× where P is tensor parallel size
- **Ulysses (All-to-All Sequence Parallelism)**: partitions sequence across devices; uses all-to-all communication to gather full sequence for attention; each device computes attention on full sequence, different heads; enables arbitrary sequence lengths
- **Ring Attention**: partitions sequence and KV cache; computes attention in blocks using ring communication; enables training on sequences longer than total GPU memory; extreme memory efficiency
- **DeepSpeed-Ulysses**: combines sequence and tensor parallelism; optimizes communication patterns; achieves 2.5× speedup vs Megatron for long sequences; production-ready implementation

**Megatron Sequence Parallelism Details:**
- **Partitioning Strategy**: partition sequence in layer norm, dropout, residual connections; these operations are sequence-independent; no communication needed during computation
- **Communication Points**: all-gather before tensor-parallel regions (attention, FFN); reduce-scatter after tensor-parallel regions; 2 communications per layer; same as tensor parallelism
- **Memory Reduction**: reduces activation memory by P× in non-tensor-parallel regions; combined with tensor parallelism, total reduction ~P× for activations; enables P× longer sequences
- **Implementation**: requires minimal code changes; integrated in Megatron-LM; automatic when tensor parallelism enabled; transparent to user

**Ulysses Sequence Parallelism:**
- **All-to-All Communication**: before attention, all-to-all scatter-gather exchanges sequence chunks for head chunks; each device gets full sequence, subset of heads; computes attention independently
- **Attention Computation**: each device computes attention for its assigned heads on full sequence; no further communication during attention; results all-to-all gathered after attention
- **Communication Volume**: 2 × B × L × H per layer (all-to-all before and after attention); same as tensor parallelism; but enables longer sequences
- **Scaling**: near-linear scaling to 8-16 devices; communication overhead 10-20%; enables 8-16× longer sequences; practical for 32K-128K contexts

**Ring Attention:**
- **Block-Wise Computation**: divides sequence into blocks; each device stores subset of blocks; computes attention using ring communication to access other blocks
- **Ring Communication**: devices arranged in ring; pass KV blocks around ring; each device computes attention with local Q and remote KV; accumulates results
- **Memory Efficiency**: each device stores only L/P tokens; enables sequences longer than total GPU memory; extreme memory reduction; enables million-token contexts
- **Computation Overhead**: each block accessed P times (once per device); P× computation vs standard attention; trade computation for memory; practical for P=4-8

**Performance Characteristics:**
- **Memory Reduction**: Megatron SP: P× reduction in non-tensor-parallel activations; Ulysses: enables P× longer sequences; Ring: enables sequences > total memory
- **Communication Overhead**: Megatron SP: no additional communication vs tensor parallelism; Ulysses: 2 all-to-all per layer; Ring: ring communication per attention block
- **Scaling Efficiency**: Megatron SP: 95%+ efficiency (same as tensor parallelism); Ulysses: 80-90% efficiency; Ring: 50-70% efficiency (high computation overhead)
- **Sequence Length**: Megatron SP: 2-4× longer; Ulysses: 8-16× longer; Ring: 100-1000× longer (limited by computation, not memory)

**Combining with Other Parallelism:**
- **Sequence + Tensor Parallelism**: natural combination; sequence parallelism in non-tensor regions, tensor in attention/FFN; multiplicative memory savings; standard in Megatron-LM
- **Sequence + Pipeline Parallelism**: sequence parallelism within pipeline stages; reduces per-stage activation memory; enables longer sequences in pipeline training
- **Sequence + Data Parallelism**: replicate sequence-parallel model across data-parallel groups; scales to large clusters; enables large batch sizes on long sequences
- **3D + Sequence Parallelism**: combines tensor, pipeline, data, and sequence parallelism; optimal for extreme scale (1000+ GPUs, 100K+ contexts); complex but achieves best efficiency

**Use Cases:**
- **Long-Context LLMs**: training models with 32K-100K context windows; Llama 2 Long (32K), Code Llama (100K) use sequence parallelism; enables document-level understanding
- **Retrieval-Augmented Generation**: processing long retrieved documents; 10K-50K token contexts common; sequence parallelism enables efficient training
- **Code Generation**: repository-level code understanding requires 50K-200K tokens; sequence parallelism critical for training on full repositories
- **Scientific Text**: processing long papers, books, legal documents; 20K-100K tokens typical; sequence parallelism enables training on full documents

**Implementation and Tools:**
- **Megatron-LM**: built-in sequence parallelism; automatic when tensor parallelism enabled; production-tested; used for training Llama 2, Code Llama
- **DeepSpeed-Ulysses**: Ulysses implementation in DeepSpeed; optimized all-to-all communication; supports hybrid parallelism; easy integration
- **Ring Attention**: research implementation available; not yet production-ready; enables extreme sequence lengths; active development
- **Framework Support**: PyTorch FSDP exploring sequence parallelism; JAX supports custom parallelism strategies; TensorFlow less mature

**Best Practices:**
- **Choose Strategy**: Megatron SP for moderate sequences (8K-32K); Ulysses for long sequences (32K-128K); Ring for extreme sequences (>128K)
- **Combine with Tensor Parallelism**: always use sequence + tensor parallelism together; multiplicative benefits; standard practice
- **Batch Size**: increase batch size with saved memory; improves training stability; typical increase 2-4× vs without sequence parallelism
- **Profile Communication**: measure all-to-all overhead; ensure high-bandwidth interconnect (NVLink, InfiniBand); optimize communication patterns

Sequence Parallelism is **the technique that breaks the sequence length barrier in transformer training** — by partitioning the sequence dimension across devices, it enables training on contexts 4-16× longer than possible on single GPU, unlocking the long-context capabilities that define the next generation of language models.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/sequence-parallelism-training) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
