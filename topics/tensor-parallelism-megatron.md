# Tensor Parallelism

**Keywords**: tensor parallelism megatron,model parallelism layer,intra layer parallelism,tensor model parallel,column row parallelism

---

**Tensor Parallelism** is **the model parallelism technique that partitions individual layers across multiple devices by splitting weight matrices along specific dimensions** — enabling training of models with layers too large for single GPU memory by distributing computation within each layer, achieving near-linear scaling with minimal communication overhead when devices are connected via high-bandwidth interconnects like NVLink.

**Tensor Parallelism Fundamentals:**
- **Matrix Partitioning**: split weight matrix W ∈ R^(m×n) across P devices; column-wise: each device stores W_i ∈ R^(m×n/P); row-wise: each device stores W_i ∈ R^(m/P×n); reduces memory by P×
- **Computation Distribution**: for Y = XW, column partition: each device computes Y_i = XW_i; concatenate results; row partition: each device computes partial Y_i = XW_i; sum results via all-reduce
- **Communication Patterns**: column partition requires all-gather after computation; row partition requires all-reduce; communication volume = hidden_size × sequence_length × batch_size; independent of model size
- **Transformer Application**: apply to attention (Q, K, V, O projections) and FFN (up, down projections); 6 weight matrices per layer; each partitioned across P devices; reduces per-device memory by P×

**Megatron-LM Tensor Parallelism:**
- **Attention Partitioning**: split Q, K, V, O matrices column-wise; each device computes subset of attention heads; head_per_device = total_heads / P; independent attention computation; no communication during attention
- **FFN Partitioning**: split first linear (up projection) column-wise, second linear (down projection) row-wise; first layer: Y = XW1, each device computes Y_i = XW1_i; second layer: Z = YW2, all-reduce after computation
- **Communication Placement**: all-gather after attention output projection; all-reduce after FFN down projection; 2 communications per transformer block; overlapped with computation
- **Identity Operators**: insert identity in forward (all-gather/all-reduce), gradient in backward; enables automatic differentiation; elegant implementation; mathematically equivalent to single-device

**Memory and Communication:**
- **Memory Reduction**: parameters reduced by P×; activations reduced by P× for partitioned dimensions; total memory reduction ~P× for large models; enables models P× larger
- **Communication Volume**: 2 × hidden_size × sequence_length × batch_size per layer; independent of model size; scales with sequence length and batch size; not with parameters
- **Bandwidth Requirements**: requires high-bandwidth interconnect; NVLink (900 GB/s per GPU) ideal; InfiniBand (200-400 Gb/s) acceptable; Ethernet too slow; intra-node preferred
- **Latency Sensitivity**: communication latency critical; sub-microsecond latency needed for efficiency; NVLink provides <1μs; InfiniBand 1-2μs; limits scaling beyond single node

**Scaling Efficiency:**
- **Intra-Node Scaling**: near-linear scaling within node (2-8 GPUs); NVLink provides sufficient bandwidth; 95-98% efficiency typical; communication fully overlapped with computation
- **Inter-Node Scaling**: efficiency degrades with InfiniBand; 80-90% efficiency for 2-4 nodes; 60-80% for 8+ nodes; communication becomes bottleneck; prefer pipeline parallelism for inter-node
- **Optimal Parallelism Degree**: P=2-8 for tensor parallelism; beyond 8, communication overhead dominates; combine with pipeline parallelism for larger scale; hybrid approach optimal
- **Sequence Length Impact**: longer sequences increase communication volume; reduces efficiency; FlashAttention helps by reducing activation size; critical for long-context models

**Implementation Details:**
- **Megatron-LM**: NVIDIA's reference implementation; highly optimized; supports tensor, pipeline, data parallelism; used for training GPT-3, Megatron-Turing NLG; production-ready
- **Parallelism Mapping**: tensor parallelism within node (NVLink), pipeline across nodes (InfiniBand), data parallelism across pipeline replicas; matches parallelism to hardware topology
- **Sequence Parallelism**: extends tensor parallelism to non-partitioned dimensions; reduces activation memory further; enables longer sequences; used in Megatron-LM for extreme contexts
- **Selective Activation Recomputation**: recompute activations during backward; reduces memory; combined with tensor parallelism for maximum memory efficiency; enables very large models

**Comparison with Pipeline Parallelism:**
- **Granularity**: tensor parallelism partitions within layers; pipeline partitions across layers; tensor has finer granularity; better load balance
- **Communication**: tensor requires all-gather/all-reduce per layer; pipeline requires point-to-point between stages; tensor needs higher bandwidth; pipeline more flexible
- **Efficiency**: tensor achieves 95%+ efficiency with NVLink; pipeline achieves 60-80% with micro-batching; tensor better for intra-node; pipeline better for inter-node
- **Memory**: both reduce memory by parallelism degree; tensor reduces per-layer memory; pipeline reduces total model memory; complementary approaches

**Advanced Techniques:**
- **Sequence Parallelism**: partition sequence dimension in addition to model dimensions; reduces activation memory; enables 2-4× longer sequences; critical for long-context models
- **Expert Parallelism**: for Mixture of Experts models, partition experts across devices; combines with tensor parallelism for non-expert layers; enables trillion-parameter MoE models
- **Tensor-Pipeline Hybrid**: use tensor parallelism within pipeline stages; reduces per-stage memory; enables larger models; used in Megatron-DeepSpeed for 530B parameters
- **Automatic Partitioning**: tools like Alpa automatically determine optimal partitioning strategy; considers hardware topology and model architecture; simplifies deployment

**Use Cases:**
- **Large Language Models**: GPT-3 175B uses tensor parallelism within nodes; Megatron-Turing 530B uses tensor + pipeline + data; essential for models >10B parameters
- **Vision Transformers**: ViT-Huge, ViT-Giant benefit from tensor parallelism; enables training on high-resolution images; reduces per-device memory for large models
- **Multi-Modal Models**: CLIP, Flamingo use tensor parallelism for large encoders; enables training on large batch sizes; critical for contrastive learning
- **Long-Context Models**: models with 32K-100K context use tensor + sequence parallelism; enables training on long sequences; critical for document understanding

**Best Practices:**
- **Parallelism Degree**: use P=2-8 for tensor parallelism; match to NVLink topology (8 GPUs per node); beyond 8, use pipeline parallelism; measure efficiency
- **Hardware Topology**: use tensor parallelism within NVLink domain; pipeline across InfiniBand; data parallelism for replicas; match parallelism to hardware
- **Batch Size**: increase batch size with saved memory; improves efficiency; typical increase 2-8× vs single GPU; balance memory and efficiency
- **Profiling**: profile communication and computation; ensure communication overlapped; identify bottlenecks; optimize based on measurements

Tensor Parallelism is **the technique that enables training models with layers too large for single GPU** — by partitioning weight matrices and distributing computation within layers, it achieves near-linear scaling on high-bandwidth interconnects, forming the foundation of the parallelism strategies that enable training of the largest language models in existence.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/tensor-parallelism-megatron) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
