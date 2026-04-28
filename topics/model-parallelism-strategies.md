# Model Parallelism Strategies

**Keywords**: model parallelism strategies,distributed model training,tensor parallelism model,pipeline parallelism training,3d parallelism

---

**Model Parallelism Strategies** are **the techniques for distributing a single neural network across multiple GPUs or nodes when the model is too large to fit on a single device — including tensor parallelism (splitting individual layers), pipeline parallelism (distributing layers across devices), and sequence parallelism (partitioning sequence dimension), enabling training and inference of models with hundreds of billions of parameters**.

**Tensor Parallelism:**
- **Layer Splitting**: splits weight matrices of individual layers across GPUs; for linear layer Y = XW with W of size [d_in, d_out], split W column-wise across N GPUs; each GPU computes partial output, then all-gather to combine results
- **Megatron-LM Approach**: splits attention and MLP layers in Transformers; attention: split Q, K, V projections column-wise, output projection row-wise; MLP: split first linear column-wise, second linear row-wise; minimizes communication (2 all-reduce per layer)
- **Communication Overhead**: requires all-reduce or all-gather after each split layer; communication volume = batch_size × sequence_length × hidden_dim; high-bandwidth interconnect (NVLink, InfiniBand) essential for efficiency
- **Scaling Efficiency**: near-linear scaling up to 8 GPUs per node (NVLink); efficiency drops with inter-node communication; typically combined with data parallelism for larger scale

**Pipeline Parallelism:**
- **Layer Distribution**: assigns consecutive layers to different GPUs; GPU 0: layers 0-7, GPU 1: layers 8-15, etc.; forward pass flows through pipeline, backward pass flows in reverse
- **Naive Pipeline Problem**: GPU 0 processes batch, sends to GPU 1, then idles while GPU 1 processes; severe underutilization (1/N efficiency for N GPUs)
- **Micro-Batching (GPipe)**: splits batch into micro-batches; GPU 0 processes micro-batch 1, sends to GPU 1, then processes micro-batch 2; overlaps computation across GPUs; achieves ~80-90% efficiency
- **Pipeline Bubble**: idle time at pipeline start (filling) and end (draining); bubble size = (num_stages - 1) × micro_batch_time; smaller micro-batches reduce bubble but increase communication overhead

**Advanced Pipeline Techniques:**
- **1F1B (One-Forward-One-Backward)**: alternates forward and backward micro-batches; reduces memory usage compared to GPipe (stores fewer activations); PipeDream and Megatron use this schedule
- **Interleaved Pipeline**: each GPU handles multiple non-consecutive stages; GPU 0: layers [0-3, 12-15], GPU 1: layers [4-7, 16-19]; reduces bubble size by enabling more overlapping
- **Virtual Pipeline Stages**: splits each GPU's layers into multiple virtual stages; increases scheduling flexibility; further reduces bubble at cost of more communication
- **Asynchronous Pipeline**: doesn't wait for all micro-batches to complete; uses stale gradients for some updates; trades consistency for throughput; requires careful learning rate tuning

**Sequence Parallelism:**
- **Sequence Dimension Splitting**: partitions sequence length across GPUs; each GPU processes subset of tokens; used in addition to tensor/pipeline parallelism for very long sequences
- **Communication Pattern**: requires all-gather for attention (each token attends to all tokens); all-reduce for gradients; communication volume proportional to sequence length
- **Megatron Sequence Parallelism**: splits sequence dimension for LayerNorm and Dropout (operations outside attention/MLP); reduces activation memory without additional communication
- **Ring Attention**: processes attention in chunks using ring all-reduce; enables extremely long sequences (millions of tokens); communication overlapped with computation

**3D Parallelism:**
- **Combining Strategies**: data parallelism (DP) × tensor parallelism (TP) × pipeline parallelism (PP); example: 1024 GPUs = 8 DP × 8 TP × 16 PP
- **Dimension Selection**: TP within nodes (high bandwidth), PP across nodes (lower bandwidth), DP for remaining GPUs; matches parallelism strategy to hardware topology
- **Megatron-DeepSpeed**: combines Megatron's tensor/pipeline parallelism with DeepSpeed's ZeRO optimizer; enables training trillion-parameter models
- **Optimal Configuration Search**: profile different DP/TP/PP combinations; consider model size, batch size, hardware topology; automated tools (Alpa) search configuration space

**Memory Optimization:**
- **Activation Checkpointing**: recomputes activations during backward pass instead of storing; trades computation for memory; enables 2-4× larger models; selective checkpointing (checkpoint every N layers) balances trade-off
- **ZeRO (Zero Redundancy Optimizer)**: partitions optimizer states, gradients, and parameters across data parallel ranks; ZeRO-1 (optimizer states), ZeRO-2 (+gradients), ZeRO-3 (+parameters); reduces memory by DP factor
- **Offloading**: stores optimizer states or parameters in CPU memory; loads on-demand during computation; ZeRO-Offload, ZeRO-Infinity enable training models larger than total GPU memory
- **Mixed Precision**: uses FP16/BF16 for activations and gradients, FP32 for optimizer states; reduces memory by 50% for activations; requires loss scaling (FP16) or is numerically stable (BF16)

**Communication Optimization:**
- **Gradient Accumulation**: accumulates gradients over multiple micro-batches before communication; reduces communication frequency; effective batch size = micro_batch_size × accumulation_steps × DP_size
- **Communication Overlap**: overlaps gradient all-reduce with backward computation; starts communication as soon as layer gradients are ready; requires careful scheduling
- **Compression**: compresses gradients before communication; FP16 instead of FP32 (2× reduction), or quantization to INT8 (4× reduction); trades accuracy for bandwidth
- **Hierarchical Communication**: all-reduce within nodes (fast NVLink), then across nodes (slower InfiniBand); reduces cross-node traffic; NCCL automatically optimizes communication topology

**Framework Support:**
- **Megatron-LM (NVIDIA)**: tensor and pipeline parallelism for Transformers; highly optimized for NVIDIA GPUs; used for training GPT, BERT, T5 at scale
- **DeepSpeed (Microsoft)**: ZeRO optimizer, pipeline parallelism, and 3D parallelism; supports PyTorch; extensive optimization for large-scale training
- **Alpa (UC Berkeley)**: automatic parallelization; searches for optimal DP/TP/PP configuration; compiler-based approach; supports JAX
- **Fairscale (Meta)**: modular parallelism components for PyTorch; FSDP (Fully Sharded Data Parallel) similar to ZeRO-3; easier integration than DeepSpeed

**Practical Considerations:**
- **Batch Size Scaling**: larger parallelism requires larger batch sizes for efficiency; global_batch_size = micro_batch_size × gradient_accumulation × DP_size; very large batches may hurt convergence
- **Learning Rate Tuning**: linear scaling rule (LR ∝ batch_size) often works; warmup critical for large batches; may need to tune for specific model/dataset
- **Debugging Complexity**: distributed training failures are hard to debug; use smaller scale for initial debugging; comprehensive logging and monitoring essential
- **Cost-Performance Trade-off**: more GPUs = faster training but higher cost; find sweet spot where training time is acceptable and cost is reasonable; consider spot instances for cost savings

Model parallelism strategies are **the enabling technology for frontier AI models — without tensor, pipeline, and sequence parallelism, training GPT-4, Llama 3, and other hundred-billion-parameter models would be impossible, making these techniques essential for pushing the boundaries of AI capability**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/model-parallelism-strategies) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
