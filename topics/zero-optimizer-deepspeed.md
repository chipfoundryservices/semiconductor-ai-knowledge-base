# ZeRO (Zero Redundancy Optimizer)

**Keywords**: zero optimizer deepspeed,zero redundancy optimizer,distributed training memory,zero stage 1 2 3,memory efficient distributed training

---

**ZeRO (Zero Redundancy Optimizer)** is **the memory optimization technique for distributed training that partitions optimizer states, gradients, and parameters across data-parallel processes** — eliminating memory redundancy to enable training models 100-1000× larger than possible with standard data parallelism, achieving linear scaling to thousands of GPUs while maintaining training efficiency and convergence properties.

**Memory Redundancy in Data Parallelism:**
- **Standard Data Parallelism**: each GPU stores complete copy of model parameters, gradients, and optimizer states; for Adam optimizer with model size M: each GPU stores M (parameters) + M (gradients) + 2M (momentum, variance) = 4M memory
- **Redundancy Problem**: for 8 GPUs, total memory 32M but only M unique parameters; 31M wasted on redundant copies; limits model size to what fits on single GPU; inefficient memory utilization
- **Example**: GPT-3 175B parameters in FP16: 350GB parameters + 350GB gradients + 700GB optimizer states = 1.4TB per GPU; impossible on 80GB A100; ZeRO partitions across GPUs
- **Communication**: standard data parallelism requires all-reduce of gradients; communication volume scales with model size; ZeRO adds communication for parameter gathering but reduces memory dramatically

**ZeRO Stages:**
- **ZeRO Stage 1 (Optimizer State Partitioning)**: partition optimizer states across GPUs; each GPU stores 1/N of optimizer states for N GPUs; reduces optimizer memory by N×; parameters and gradients still replicated; 4× memory reduction for Adam
- **ZeRO Stage 2 (Gradient Partitioning)**: partition gradients in addition to optimizer states; each GPU stores 1/N of gradients; reduces gradient memory by N×; parameters still replicated; 8× memory reduction total
- **ZeRO Stage 3 (Parameter Partitioning)**: partition parameters across GPUs; each GPU stores 1/N of parameters; gather parameters just-in-time for forward/backward; maximum memory reduction; 64× reduction for Adam with 8 GPUs
- **Stage Selection**: Stage 1 for moderate models (1-10B); Stage 2 for large models (10-100B); Stage 3 for extreme models (100B-1T); trade-off between memory and communication

**ZeRO Stage 3 Deep Dive:**
- **Parameter Gathering**: before computing layer, all-gather parameters from all GPUs; each GPU broadcasts its 1/N partition; reconstructs full layer; computes forward pass; discards parameters after use
- **Gradient Computation**: backward pass gathers parameters again; computes gradients; reduces gradients to owner GPU; each GPU receives 1/N of gradients; updates its 1/N of parameters
- **Communication Pattern**: all-gather for forward (gather parameters), reduce-scatter for backward (distribute gradients); communication volume same as standard data parallelism; but enables N× larger models
- **Overlapping**: overlap communication with computation; prefetch next layer parameters while computing current layer; hide communication latency; maintains training efficiency

**Memory Savings:**
- **Model States**: ZeRO-3 reduces per-GPU memory from 4M to 4M/N + communication buffers; for 8 GPUs: 8× reduction; for 64 GPUs: 64× reduction; enables models 10-100× larger
- **Activation Memory**: ZeRO doesn't reduce activation memory; combine with gradient checkpointing for activation savings; multiplicative benefits; enables 100-1000× larger models
- **Example Calculation**: 175B parameter model, Adam optimizer, 8 GPUs: Standard DP = 1.4TB per GPU (impossible); ZeRO-3 = 175GB per GPU (feasible on 8×A100 80GB)
- **Scaling**: memory per GPU decreases linearly with GPU count; enables training arbitrarily large models with enough GPUs; practical limit from communication overhead

**Communication Overhead:**
- **Bandwidth Requirements**: ZeRO-3 requires 2× communication vs standard data parallelism (all-gather + reduce-scatter vs all-reduce); but enables models that don't fit otherwise
- **Latency Sensitivity**: small models or fast GPUs may see slowdown from communication; ZeRO-3 beneficial when model size > 1B parameters; smaller models use Stage 1 or 2
- **Network Topology**: requires high-bandwidth interconnect (NVLink, InfiniBand); 100-400 Gb/s per GPU; slower networks (Ethernet) see larger overhead; topology-aware optimization helps
- **Scaling Efficiency**: maintains 80-95% scaling efficiency to 64-128 GPUs; degrades to 60-80% at 512-1024 GPUs; still enables training impossible otherwise

**DeepSpeed Integration:**
- **DeepSpeed Library**: Microsoft's implementation of ZeRO; production-ready; used for training GPT-3, Megatron-Turing NLG, Bloom; extensive optimization and tuning
- **Configuration**: simple JSON config to enable ZeRO stages; zero_optimization: {stage: 3}; automatic partitioning and communication; minimal code changes
- **ZeRO-Offload**: offload optimizer states and gradients to CPU memory; further reduces GPU memory; trades PCIe bandwidth for memory; enables training on consumer GPUs
- **ZeRO-Infinity**: offload to NVMe SSD; enables training models larger than total system memory; extreme memory savings at cost of I/O latency; for models 1T+ parameters

**Combining with Other Techniques:**
- **ZeRO + Gradient Checkpointing**: multiplicative memory savings; ZeRO reduces model state memory, checkpointing reduces activation memory; enables 100-1000× larger models
- **ZeRO + Mixed Precision**: FP16/BF16 training reduces memory 2×; combined with ZeRO gives 128× reduction (64× from ZeRO-3, 2× from mixed precision)
- **ZeRO + Model Parallelism**: ZeRO for data parallelism, pipeline/tensor parallelism for model parallelism; hybrid approach for extreme scale; used in Megatron-DeepSpeed
- **ZeRO + LoRA**: ZeRO enables fine-tuning large models; LoRA reduces trainable parameters; combination enables fine-tuning 100B+ models on modest hardware

**Production Deployment:**
- **Training Stability**: ZeRO maintains same convergence as standard training; no hyperparameter changes needed; extensively validated on large models
- **Fault Tolerance**: checkpoint/resume works with ZeRO; each GPU saves its partition; restore from checkpoint seamlessly; critical for long training runs
- **Monitoring**: DeepSpeed provides memory and communication profiling; identifies bottlenecks; helps optimize configuration; essential for large-scale training
- **Multi-Node Scaling**: ZeRO scales to thousands of GPUs across hundreds of nodes; used for training largest models (Bloom 176B, Megatron-Turing 530B); production-proven

**Best Practices:**
- **Stage Selection**: use Stage 1 for models <10B, Stage 2 for 10-100B, Stage 3 for >100B; measure memory and speed; choose based on bottleneck
- **Batch Size**: increase batch size with saved memory; improves training stability and convergence; typical increase 4-16× vs standard data parallelism
- **Communication Optimization**: use NVLink for intra-node, InfiniBand for inter-node; enable NCCL optimizations; topology-aware placement; critical for efficiency
- **Profiling**: profile memory and communication; identify bottlenecks; adjust configuration; iterate to optimal settings; essential for large-scale training

ZeRO is **the breakthrough that made training 100B+ parameter models practical** — by eliminating memory redundancy in distributed training, it enables models 100-1000× larger than possible with standard approaches, democratizing large-scale AI research and enabling the frontier models that define the current state of artificial intelligence.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/zero-optimizer-deepspeed) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
