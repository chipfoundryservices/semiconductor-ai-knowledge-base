# Expert Parallelism

**Keywords**: expert parallelism moe,mixture of experts distributed,moe training parallelism,expert model parallel,switch transformer training

---

**Expert Parallelism** is **the parallelism strategy for Mixture of Experts models that distributes expert networks across devices while routing tokens to appropriate experts** — enabling training of models with hundreds to thousands of experts (trillions of parameters) by partitioning experts while maintaining efficient all-to-all communication for token routing, achieving 10-100× parameter scaling vs dense models.

**Expert Parallelism Fundamentals:**
- **Expert Distribution**: for N experts across P devices, each device stores N/P experts; experts partitioned by expert ID; device i stores experts i×(N/P) to (i+1)×(N/P)-1
- **Token Routing**: router assigns each token to k experts (typically k=1-2); tokens routed to devices holding assigned experts; requires all-to-all communication to exchange tokens
- **Computation**: each device processes tokens routed to its experts; experts compute independently; no communication during expert computation; results gathered back to original devices
- **Communication Pattern**: all-to-all scatter (distribute tokens to experts), compute on experts, all-to-all gather (collect results); 2 all-to-all operations per MoE layer

**All-to-All Communication:**
- **Token Exchange**: before expert computation, all-to-all exchanges tokens between devices; each device sends tokens to devices holding assigned experts; receives tokens for its experts
- **Communication Volume**: total tokens × hidden_size × 2 (send and receive); independent of expert count; scales with batch size and sequence length
- **Load Balancing**: unbalanced routing causes communication imbalance; some devices send/receive more tokens; auxiliary loss encourages balanced routing; critical for efficiency
- **Bandwidth Requirements**: requires high-bandwidth interconnect; InfiniBand (200-400 Gb/s) or NVLink (900 GB/s); all-to-all is bandwidth-intensive; network can be bottleneck

**Combining with Other Parallelism:**
- **Expert + Data Parallelism**: replicate MoE model across data-parallel groups; each group has expert parallelism internally; scales to large clusters; standard approach
- **Expert + Tensor Parallelism**: apply tensor parallelism to each expert; reduces per-expert memory; enables larger experts; used in GLaM, Switch Transformer
- **Expert + Pipeline Parallelism**: MoE layers in pipeline stages; expert parallelism within stages; complex but enables extreme scale; used in trillion-parameter models
- **Hierarchical Expert Parallelism**: group experts hierarchically; intra-node expert parallelism (NVLink), inter-node data parallelism (InfiniBand); matches parallelism to hardware topology

**Load Balancing Challenges:**
- **Routing Imbalance**: router may assign most tokens to few experts; causes compute imbalance; some devices idle while others overloaded; reduces efficiency
- **Auxiliary Loss**: L_aux = α × Σ(f_i × P_i) encourages uniform expert utilization; f_i is fraction of tokens to expert i, P_i is router probability; typical α=0.01-0.1
- **Expert Capacity**: limit tokens per expert to capacity C; tokens exceeding capacity dropped or routed to next-best expert; prevents extreme imbalance; typical C=1.0-1.25× average
- **Dynamic Capacity**: adjust capacity based on actual routing; increases capacity for popular experts; reduces for unpopular; improves efficiency; requires dynamic memory allocation

**Memory Management:**
- **Expert Memory**: each device stores N/P experts; for Switch Transformer with 2048 experts, 8 devices: 256 experts per device; reduces per-device memory 8×
- **Token Buffers**: must allocate buffers for incoming tokens; buffer size = capacity × num_local_experts × hidden_size; can be large for high capacity factors
- **Activation Memory**: activations for tokens processed by local experts; memory = num_tokens_received × hidden_size × expert_layers; varies with routing
- **Total Memory**: expert parameters + token buffers + activations; expert parameters dominate for large models; buffers can be significant for high capacity

**Scaling Efficiency:**
- **Computation Scaling**: near-linear scaling if load balanced; each device processes 1/P of experts; total computation same as single device
- **Communication Overhead**: all-to-all communication overhead 10-30% depending on network; higher for smaller batch sizes; lower for larger batches
- **Load Imbalance Impact**: 20% imbalance reduces efficiency by 20%; auxiliary loss critical for maintaining balance; monitoring per-expert utilization essential
- **Optimal Expert Count**: N=64-256 for most models; beyond 256, diminishing returns; communication overhead increases; load balancing harder

**Implementation Frameworks:**
- **Megatron-LM**: supports expert parallelism for MoE models; integrates with tensor and pipeline parallelism; production-tested; used for large MoE models
- **DeepSpeed-MoE**: Microsoft's MoE implementation; optimized all-to-all communication; supports ZeRO for expert parameters; enables trillion-parameter models
- **FairScale**: Meta's MoE implementation; modular design; easy integration with PyTorch; good for research; less optimized than Megatron/DeepSpeed
- **GShard**: Google's MoE framework for TensorFlow; used for training GLaM, Switch Transformer; supports TPU and GPU; production-ready

**Training Stability:**
- **Router Collapse**: router may route all tokens to few experts early in training; other experts never trained; solution: higher router learning rate, router z-loss, expert dropout
- **Expert Specialization**: experts specialize to different input patterns; desirable behavior; but can cause instability if specialization too extreme; monitor expert utilization
- **Gradient Scaling**: gradients for popular experts larger than unpopular; can cause training instability; gradient clipping per expert helps; normalize by expert utilization
- **Checkpoint/Resume**: must save expert assignments and router state; ensure deterministic routing on resume; critical for long training runs

**Use Cases:**
- **Large Language Models**: Switch Transformer (1.6T parameters, 2048 experts), GLaM (1.2T, 64 experts), GPT-4 (rumored MoE); enables trillion-parameter models
- **Multi-Task Learning**: different experts specialize to different tasks; natural fit for MoE; enables single model for many tasks; used in multi-task transformers
- **Multi-Lingual Models**: experts specialize to different languages; improves quality vs dense model; used in multi-lingual translation models
- **Multi-Modal Models**: experts for different modalities (vision, language, audio); enables efficient multi-modal processing; active research area

**Best Practices:**
- **Expert Count**: start with N=64-128; increase if model capacity needed; diminishing returns beyond 256; balance capacity and efficiency
- **Capacity Factor**: C=1.0-1.25 typical; higher C reduces token dropping but increases memory; lower C saves memory but drops more tokens
- **Load Balancing**: monitor expert utilization; adjust auxiliary loss weight; aim for >80% utilization on all experts; critical for efficiency
- **Communication Optimization**: use high-bandwidth interconnect; optimize all-to-all implementation; consider hierarchical expert parallelism for multi-node

Expert Parallelism is **the technique that enables training of trillion-parameter models** — by distributing experts across devices and efficiently routing tokens through all-to-all communication, it achieves 10-100× parameter scaling vs dense models, enabling the sparse models that define the frontier of language model capabilities.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/expert-parallelism-moe-13771) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
