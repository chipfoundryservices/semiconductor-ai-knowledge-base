# Pipeline Parallelism

**Keywords**: pipeline parallelism training,pipeline model parallelism,gpipe pipedream,pipeline scheduling strategies,micro batch pipeline

---

**Pipeline Parallelism** is **the model parallelism technique that partitions neural network layers across multiple devices and processes multiple micro-batches concurrently in a pipeline fashion — enabling training of models too large for a single GPU by distributing consecutive layers to different devices while maintaining high GPU utilization through careful scheduling of forward and backward passes across overlapping micro-batches**.

**Pipeline Parallelism Fundamentals:**
- **Layer Partitioning**: divides model into stages (consecutive layer groups); stage 0 on GPU 0, stage 1 on GPU 1, etc.; each stage processes its layers then passes activations to next stage
- **Sequential Dependency**: forward pass flows stage 0 → 1 → 2 → ...; backward pass flows in reverse; creates inherent sequential bottleneck
- **Naive Pipeline Problem**: without micro-batching, only one GPU is active at a time; GPU utilization = 1/num_stages; completely impractical for more than 2-3 stages
- **Micro-Batching Solution**: splits mini-batch into smaller micro-batches; processes multiple micro-batches in flight simultaneously; overlaps computation across stages

**GPipe (Google):**
- **Synchronous Pipeline**: processes all micro-batches of a mini-batch before updating weights; maintains synchronous SGD semantics; gradient accumulation across micro-batches
- **Forward-Then-Backward Schedule**: completes all forward passes for all micro-batches, then all backward passes; simple but high memory usage (stores all activations)
- **Pipeline Bubble**: idle time during pipeline fill (ramp-up) and drain (ramp-down); bubble_time = (num_stages - 1) × micro_batch_time; efficiency = 1 - bubble_time / total_time
- **Activation Checkpointing**: recomputes activations during backward pass to reduce memory; essential for deep pipelines; trades 33% more computation for 90% less activation memory

**PipeDream (Microsoft):**
- **Asynchronous Pipeline**: doesn't wait for all micro-batches to complete; uses weight versioning to handle concurrent forward/backward passes with different weight versions
- **1F1B Schedule (One-Forward-One-Backward)**: alternates forward and backward micro-batches after initial warm-up; reduces memory usage (stores fewer activations) compared to GPipe
- **Weight Stashing**: maintains multiple weight versions for different in-flight micro-batches; ensures gradient consistency; memory overhead for storing weight versions
- **Vertical Sync**: periodically synchronizes weights across all stages; balances staleness and consistency; configurable sync frequency

**Pipeline Scheduling Strategies:**
- **Fill-Drain (GPipe)**: fill pipeline with forward passes, drain with backward passes; high memory (stores all activations), simple implementation
- **1F1B (PipeDream, Megatron)**: after warm-up, alternates 1 forward and 1 backward; steady-state memory usage (constant number of stored activations); most common in practice
- **Interleaved 1F1B**: each device handles multiple non-consecutive stages; device 0: stages [0, 4, 8], device 1: stages [1, 5, 9]; reduces bubble size by increasing scheduling flexibility
- **Chimera**: combines synchronous and asynchronous execution; synchronous within groups, asynchronous across groups; balances consistency and efficiency

**Memory Management:**
- **Activation Memory**: forward pass stores activations for backward pass; memory = num_micro_batches_in_flight × activation_size_per_micro_batch; 1F1B reduces this compared to fill-drain
- **Activation Checkpointing**: stores only subset of activations (e.g., every Nth layer); recomputes others during backward; selective checkpointing balances memory and computation
- **Gradient Accumulation**: accumulates gradients across micro-batches; single weight update per mini-batch; maintains effective batch size = num_micro_batches × micro_batch_size
- **Weight Versioning (PipeDream)**: stores multiple weight versions for asynchronous execution; memory overhead = num_stages × weight_size; limits scalability to 10-20 stages

**Micro-Batch Size Selection:**
- **Trade-offs**: smaller micro-batches → more parallelism, less bubble, but more communication overhead; larger micro-batches → less overhead, but more bubble
- **Optimal Size**: typically 1-4 samples per micro-batch; depends on model size, stage count, and hardware; profile to find sweet spot
- **Bubble Analysis**: bubble_fraction = (num_stages - 1) / num_micro_batches; want bubble < 10-20%; requires num_micro_batches >> num_stages
- **Memory Constraint**: micro_batch_size limited by per-stage memory; smaller stages can use larger micro-batches; non-uniform micro-batch sizes possible but complex

**Communication Optimization:**
- **Point-to-Point Communication**: stage i sends activations to stage i+1; uses NCCL send/recv or MPI; bandwidth requirements = activation_size × num_micro_batches / time
- **Activation Compression**: compress activations before sending; FP16 instead of FP32 (2× reduction); lossy compression possible but affects accuracy
- **Communication Overlap**: overlaps communication with computation; sends next micro-batch while computing current; requires careful scheduling and buffering
- **Gradient Communication**: backward pass sends gradients to previous stage; same volume as forward activations; can overlap with computation

**Combining with Other Parallelism:**
- **Pipeline + Data Parallelism**: replicate entire pipeline across multiple groups; each group processes different data; scales to arbitrary GPU count
- **Pipeline + Tensor Parallelism**: each pipeline stage uses tensor parallelism; enables larger models per stage; Megatron-LM uses this combination
- **3D Parallelism**: data × tensor × pipeline; example: 512 GPUs = 8 DP × 8 TP × 8 PP; matches parallelism to hardware topology (TP within node, PP across nodes)
- **Optimal Configuration**: depends on model size, hardware, and batch size; automated search (Alpa) or manual tuning based on profiling

**Framework Implementations:**
- **Megatron-LM**: 1F1B schedule with interleaving; combines with tensor parallelism; highly optimized for NVIDIA GPUs; used for GPT, BERT, T5 training
- **DeepSpeed**: pipeline parallelism with ZeRO optimizer; supports various schedules; integrates with PyTorch; extensive documentation and examples
- **Fairscale**: PyTorch-native pipeline parallelism; modular design; easier integration than DeepSpeed; used by Meta for large model training
- **GPipe (TensorFlow/JAX)**: original implementation; synchronous pipeline with activation checkpointing; less commonly used now (Megatron/DeepSpeed preferred)

**Practical Considerations:**
- **Load Balancing**: stages should have similar computation time; unbalanced stages create bottlenecks; use profiling to guide layer partitioning
- **Stage Granularity**: more stages → better load balance but more bubble; fewer stages → less bubble but harder to balance; 4-16 stages typical
- **Batch Size Requirements**: pipeline parallelism requires large batch sizes (num_micro_batches × micro_batch_size); may need gradient accumulation to achieve effective batch size
- **Debugging Complexity**: pipeline failures are hard to debug; use smaller configurations for initial debugging; comprehensive logging essential

**Performance Analysis:**
- **Efficiency Metric**: efficiency = ideal_time / actual_time where ideal_time assumes perfect parallelism; accounts for bubble and communication overhead
- **Bubble Overhead**: bubble_time = (num_stages - 1) × (forward_time + backward_time) / num_micro_batches; minimize by increasing num_micro_batches
- **Communication Overhead**: depends on activation size and bandwidth; high-bandwidth interconnect (NVLink, InfiniBand) critical; measure with profiling tools
- **Memory Efficiency**: pipeline enables training models that don't fit on single GPU; memory per GPU = model_size / num_stages + activation_memory

Pipeline parallelism is **the essential technique for training models that exceed single-GPU memory capacity — enabling the distribution of massive models across multiple devices while maintaining reasonable training efficiency through sophisticated scheduling and micro-batching strategies that minimize idle time and maximize hardware utilization**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/pipeline-parallelism-training) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
