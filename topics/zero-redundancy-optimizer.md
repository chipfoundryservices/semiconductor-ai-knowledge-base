# Zero Redundancy Optimizer (ZeRO)

**Keywords**: zero redundancy optimizer,zero deepspeed,memory efficient optimizer,optimizer state partitioning,fsdp fully sharded

---

**Zero Redundancy Optimizer (ZeRO)** is **the memory optimization technique that eliminates redundant storage of optimizer states, gradients, and parameters across data parallel processes — partitioning these memory components across GPUs so each device stores only 1/N of the total, enabling training of models N× larger than single-GPU capacity while maintaining data parallelism's computational efficiency and ease of implementation**.

**Memory Breakdown in Distributed Training:**
- **Model States**: parameters (fp32 + fp16 = 6 bytes/param), gradients (2 bytes/param), optimizer states (Adam: momentum + variance = 8 bytes/param); total 16 bytes/param
- **Activations**: intermediate layer outputs stored for backward pass; memory = batch_size × sequence_length × hidden_dim × num_layers × dtype_size
- **Redundancy in Data Parallelism**: each GPU stores complete copy of model states; for 8 GPUs, 8× redundant storage; only gradients are communicated (all-reduce)
- **Memory Bottleneck**: 175B parameter model requires 2.8TB for model states (16 bytes × 175B); exceeds memory of any GPU cluster without optimization

**ZeRO Stage 1 (Optimizer State Partitioning):**
- **Partitioning**: divides optimizer states across data parallel ranks; each GPU stores 1/N of optimizer states (momentum, variance for Adam)
- **Memory Savings**: reduces optimizer state memory from 8 bytes/param to 8/N bytes/param; for N=8, saves 7/16 = 43.75% of total memory
- **Communication**: all-gather updated parameters after optimizer step; communication volume = model_size; happens once per training step
- **Implementation**: minimal code changes; compatible with existing training code; DeepSpeed ZeRO-1 is drop-in replacement

**ZeRO Stage 2 (+ Gradient Partitioning):**
- **Partitioning**: additionally partitions gradients across ranks; each GPU stores gradients for its 1/N of parameters
- **Memory Savings**: reduces gradient memory from 2 bytes/param to 2/N bytes/param; total savings = (8+2)/16 = 62.5% for N=8
- **Communication**: reduce-scatter during backward pass (each GPU receives gradients for its partition); all-gather parameters after optimizer step
- **Gradient Accumulation**: compatible with gradient accumulation; accumulates local gradients before reduce-scatter; enables large effective batch sizes

**ZeRO Stage 3 (+ Parameter Partitioning):**
- **Partitioning**: partitions parameters themselves; each GPU stores only 1/N of model parameters; parameters all-gathered on-demand during forward/backward
- **Memory Savings**: reduces all model state memory by N×; total memory = 16/N bytes/param; enables N× larger models
- **Communication**: all-gather parameters before each layer's forward/backward; communication volume = 2 × model_size per training step (forward + backward)
- **Trade-off**: maximum memory savings but highest communication overhead; requires high-bandwidth interconnect for efficiency

**ZeRO-Offload:**
- **CPU Offloading**: offloads optimizer states and gradients to CPU memory; GPU stores only parameters and activations
- **Computation**: optimizer step runs on CPU; slower but enables training models that don't fit in GPU memory
- **Communication**: CPU-GPU data transfer via PCIe; overlaps with GPU computation where possible
- **Use Case**: training large models on limited GPU memory; trades speed for capacity; 10-50× slower optimizer step but enables otherwise impossible training

**ZeRO-Infinity:**
- **NVMe Offloading**: extends offloading to NVMe SSD; enables training models larger than CPU + GPU memory combined
- **Bandwidth Hierarchy**: GPU memory (1-2 TB/s) > CPU memory (100-200 GB/s) > NVMe (5-10 GB/s); careful data movement scheduling critical
- **Infinity Offload Engine**: manages data movement across memory hierarchy; prefetches data to hide latency; overlaps communication with computation
- **Extreme Scale**: enables training trillion-parameter models on modest GPU clusters; demonstrated 32T parameter model training

**FSDP (Fully Sharded Data Parallel):**
- **PyTorch Native**: PyTorch's implementation of ZeRO-3 concepts; integrated into PyTorch core; easier to use than DeepSpeed for PyTorch users
- **Sharding Strategy**: shards parameters, gradients, and optimizer states; similar to ZeRO-3 but with PyTorch-native APIs
- **Mixed Precision**: supports BF16/FP16 training with FP32 master weights; automatic loss scaling for FP16
- **Activation Checkpointing**: integrates with PyTorch's activation checkpointing; combined memory savings enable very large models

**Communication Patterns:**
- **All-Gather**: gathers sharded parameters before computation; volume = parameter_size; happens before each layer's forward/backward
- **Reduce-Scatter**: reduces gradients and scatters to owners; volume = gradient_size; happens during backward pass
- **All-Reduce (Baseline)**: standard data parallelism; volume = gradient_size; ZeRO replaces with reduce-scatter + all-gather
- **Communication Volume**: ZeRO-3 has 1.5× communication of standard data parallelism; acceptable trade-off for memory savings

**Optimization Techniques:**
- **Communication Overlap**: overlaps all-gather with computation; prefetches parameters for next layer while computing current layer
- **Gradient Bucketing**: groups small gradients into buckets for efficient reduce-scatter; reduces communication overhead
- **Hierarchical Communication**: all-gather within nodes (NVLink), reduce-scatter across nodes (InfiniBand); matches communication to hardware topology
- **Parameter Prefetching**: prefetches parameters for upcoming layers; hides all-gather latency behind computation

**Combining with Other Parallelism:**
- **ZeRO + Tensor Parallelism**: ZeRO for data parallelism, tensor parallelism within groups; example: 64 GPUs = 8 DP (ZeRO) × 8 TP
- **ZeRO + Pipeline Parallelism**: ZeRO within pipeline stages; each stage uses ZeRO for its layers; enables very large models with deep pipelines
- **3D Parallelism + ZeRO**: DP (ZeRO) × TP × PP; maximum flexibility; used for training largest models (GPT-3, Megatron-Turing NLG)
- **Optimal Configuration**: depends on model size, GPU memory, and interconnect bandwidth; automated tools (DeepSpeed Autotuning) search configuration space

**Performance Characteristics:**
- **Memory Efficiency**: ZeRO-3 enables N× larger models for N GPUs; 8 GPUs with 80GB each can train 640GB model (8 × 80GB)
- **Communication Overhead**: ZeRO-1/2 have minimal overhead (<5%); ZeRO-3 has 10-30% overhead depending on model size and bandwidth
- **Scaling Efficiency**: 85-95% efficiency for ZeRO-1/2; 70-85% for ZeRO-3; improves with larger models (communication amortized over more computation)
- **Throughput**: ZeRO-3 throughput = 0.7-0.9× standard data parallelism; acceptable trade-off for enabling much larger models

**Practical Guidelines:**
- **Stage Selection**: ZeRO-1 for models that fit in GPU memory (minimal overhead); ZeRO-2 for moderate memory pressure; ZeRO-3 for models that don't fit
- **Batch Size**: larger batches amortize communication overhead; ZeRO-3 benefits from batch_size × sequence_length > 1M tokens
- **Activation Checkpointing**: combine with ZeRO for maximum memory savings; enables 2-4× larger models at 30% speed cost
- **Monitoring**: track memory usage, communication time, and throughput; identify bottlenecks; tune configuration based on profiling

**Framework Support:**
- **DeepSpeed**: original ZeRO implementation; ZeRO-1/2/3, ZeRO-Offload, ZeRO-Infinity; comprehensive optimization toolkit
- **PyTorch FSDP**: PyTorch-native ZeRO-3; easier integration; good performance; recommended for PyTorch users
- **Fairscale**: Meta's implementation; modular components; used in production at Meta
- **Megatron-DeepSpeed**: combines Megatron's tensor/pipeline parallelism with DeepSpeed's ZeRO; used for training largest models

Zero Redundancy Optimizer is **the breakthrough that democratized large-scale model training — by eliminating redundant memory storage across data parallel processes, it enables researchers and practitioners to train models orders of magnitude larger than previously possible on the same hardware, making frontier AI research accessible beyond the largest tech companies**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/zero-redundancy-optimizer) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
