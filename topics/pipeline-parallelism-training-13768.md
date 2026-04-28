# Pipeline Parallelism

**Keywords**: pipeline parallelism training,model parallelism pipeline,gpipe training,pipeline bubble,micro batch pipeline

---

**Pipeline Parallelism** is **the model parallelism technique that partitions neural network layers across multiple devices and processes micro-batches in a pipelined fashion** — enabling training of models too large to fit on single GPU by distributing layers while maintaining high device utilization through overlapping computation, achieving 60-80% efficiency compared to single-device training for models with 10-100+ layers.

**Pipeline Parallelism Fundamentals:**
- **Layer Partitioning**: divide model into K stages across K devices; each device stores 1/K of layers; stage 1 has first L/K layers, stage 2 has next L/K layers, etc.; reduces per-device memory by K×
- **Sequential Dependency**: stage i+1 depends on output of stage i; creates pipeline where data flows through stages; forward pass: stage 1 → 2 → ... → K; backward pass: stage K → K-1 → ... → 1
- **Micro-Batching**: split mini-batch into M micro-batches; process micro-batches in pipeline; while stage 2 processes micro-batch 1, stage 1 processes micro-batch 2; overlaps computation across stages
- **Pipeline Bubble**: idle time when stages wait for data; occurs at pipeline fill (start) and drain (end); bubble time = (K-1) × micro-batch time; reduces efficiency; minimized by increasing M

**Pipeline Schedules:**
- **GPipe (Fill-Drain)**: simple schedule; fill pipeline with forward passes, drain with backward passes; bubble time (K-1)/M of total time; for K=4, M=16: 18.75% bubble; easy to implement
- **PipeDream (1F1B)**: interleaves forward and backward; after warmup, each stage alternates 1 forward, 1 backward; reduces bubble to (K-1)/(M+K-1); for K=4, M=16: 15.8% bubble; better efficiency
- **Interleaved Pipeline**: each device holds multiple non-consecutive stages; reduces bubble further; complexity increases; used in Megatron-LM for large models; achieves 5-10% bubble
- **Schedule Comparison**: GPipe simplest but lowest efficiency; 1F1B good balance; interleaved best efficiency but complex; choice depends on model size and hardware

**Memory and Communication:**
- **Activation Memory**: must store activations for all in-flight micro-batches; memory = M × activation_size_per_microbatch; larger M improves efficiency but increases memory; typical M=4-32
- **Gradient Accumulation**: accumulate gradients across M micro-batches; update weights after full mini-batch; equivalent to large batch training; maintains convergence properties
- **Communication Volume**: send activations forward, gradients backward; volume = 2 × hidden_size × sequence_length × M per pipeline stage; bandwidth-intensive; requires fast interconnect
- **Point-to-Point Communication**: stages communicate only with neighbors; stage i sends to i+1, receives from i-1; simpler than all-reduce; works with slower interconnects than data parallelism

**Efficiency Analysis:**
- **Ideal Speedup**: K× speedup for K devices if no bubble; actual speedup K × (1 - bubble_fraction); for K=8, M=32, 1F1B schedule: 8 × 0.82 = 6.6× speedup
- **Scaling Limits**: efficiency decreases as K increases (more bubble); practical limit K=8-16 for typical models; beyond 16, bubble dominates; combine with other parallelism for larger scale
- **Micro-Batch Count**: increasing M reduces bubble but increases memory; optimal M balances efficiency and memory; typical M=4K to 8K for good efficiency
- **Layer Balance**: unbalanced stages (different compute time) reduce efficiency; slowest stage determines throughput; careful partitioning critical; automated tools help

**Implementation Frameworks:**
- **Megatron-LM**: NVIDIA's framework for large language models; supports pipeline, tensor, and data parallelism; interleaved pipeline schedule; production-tested on GPT-3 scale models
- **DeepSpeed**: Microsoft's framework; integrates pipeline parallelism with ZeRO; automatic partitioning; supports various schedules; used for training Turing-NLG, Bloom
- **FairScale**: Meta's library; modular pipeline parallelism; easy integration with PyTorch; supports GPipe and 1F1B schedules; good for research and prototyping
- **PyTorch Native**: torch.distributed.pipeline with PipeRPCWrapper; basic pipeline support; less optimized than specialized frameworks; suitable for simple use cases

**Combining with Other Parallelism:**
- **Pipeline + Data Parallelism**: replicate pipeline across multiple data-parallel groups; each group has K devices for pipeline, N groups for data parallelism; total K×N devices; scales to large clusters
- **Pipeline + Tensor Parallelism**: each pipeline stage uses tensor parallelism; reduces per-device memory further; enables very large models; used in Megatron-DeepSpeed for 530B parameter models
- **3D Parallelism**: combines pipeline, tensor, and data parallelism; optimal for extreme scale (1000+ GPUs); complex but achieves best efficiency; requires careful tuning
- **Hybrid Strategy**: use pipeline for inter-node (slower interconnect), tensor for intra-node (NVLink); matches parallelism to hardware topology; maximizes efficiency

**Challenges and Solutions:**
- **Load Imbalance**: different layers have different compute times; transformer layers uniform but embedding/output layers different; solution: group small layers, split large layers
- **Memory Imbalance**: first/last stages may have different memory (embeddings, output layer); solution: adjust partition boundaries, use tensor parallelism for large layers
- **Gradient Staleness**: in 1F1B, gradients computed on slightly stale activations; generally not a problem; convergence equivalent to standard training; validated on large models
- **Debugging Complexity**: errors propagate through pipeline; harder to debug than single-device; solution: test on small model first, use extensive logging, validate gradients

**Use Cases:**
- **Large Language Models**: GPT-3, PaLM, Bloom use pipeline parallelism; enables training 100B-500B parameter models; combined with tensor and data parallelism for extreme scale
- **Vision Transformers**: ViT-Huge, ViT-Giant benefit from pipeline parallelism; enables training on high-resolution images; reduces per-device memory for large models
- **Multi-Modal Models**: CLIP, Flamingo use pipeline parallelism; vision and language encoders on different stages; natural partitioning for multi-modal architectures
- **Long Sequence Models**: models with many layers benefit most; 48-96 layer transformers ideal for pipeline parallelism; enables training on long sequences with many layers

**Best Practices:**
- **Partition Strategy**: balance compute time across stages; profile layer times; adjust boundaries; automated tools (Megatron-LM) help; manual tuning for optimal performance
- **Micro-Batch Size**: start with M=4K, increase until memory limit; measure efficiency; diminishing returns beyond M=8K; balance efficiency and memory
- **Schedule Selection**: use 1F1B for most cases; interleaved for extreme efficiency; GPipe for simplicity; measure and compare on your model
- **Validation**: verify convergence matches single-device training; check gradient norms; validate on small model first; scale up gradually

Pipeline Parallelism is **the essential technique for training models too large for single GPU** — by distributing layers across devices and overlapping computation through pipelining, it enables training of 100B+ parameter models while maintaining reasonable efficiency, forming a critical component of the parallelism strategies that power frontier AI research.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/pipeline-parallelism-training-13768) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
