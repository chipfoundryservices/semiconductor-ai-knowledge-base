# Fully Sharded Data Parallel (FSDP)

**Keywords**: fully sharded data parallel fsdp,zero optimizer deepspeed,sharded optimizer state,fsdp memory efficiency,zero redundancy optimizer

---

**Fully Sharded Data Parallel (FSDP)** is **the advanced distributed training technique that shards model parameters, gradients, and optimizer states across GPUs — each GPU stores only 1/N of the model (N=number of GPUs), gathering required parameters on-demand during forward/backward passes and immediately discarding them, reducing per-GPU memory from O(model_size) to O(model_size/N), enabling training of 100B+ parameter models on 8×40GB GPUs that would otherwise require 400GB+ per GPU, achieving 80-90% scaling efficiency despite increased communication overhead**.

**FSDP Sharding Strategy:**
- **Parameter Sharding**: each GPU stores 1/N of model parameters; during forward pass, all-gather collects full parameters for current layer; after computation, parameters discarded; only local shard retained
- **Gradient Sharding**: during backward pass, all-gather collects parameters; compute gradients; reduce-scatter distributes gradient shards; each GPU stores 1/N of gradients
- **Optimizer State Sharding**: each GPU's optimizer only maintains state (momentum, variance) for its 1/N parameter shard; optimizer.step() updates local shard; reduces optimizer memory from O(model_size) to O(model_size/N)
- **Memory Savings**: DDP: model + gradients + optimizer state = 4× model size (FP32) or 2× (FP16); FSDP: (model + gradients + optimizer state)/N + activations; 8 GPUs: 8× memory reduction

**ZeRO Stages (DeepSpeed):**
- **ZeRO Stage 1**: shard optimizer states only; each GPU stores full model and gradients but 1/N of optimizer state; 4× memory reduction for optimizer; minimal communication overhead
- **ZeRO Stage 2**: shard optimizer states and gradients; each GPU stores full model, 1/N gradients, 1/N optimizer state; 8× memory reduction; moderate communication (reduce-scatter gradients)
- **ZeRO Stage 3**: shard everything (parameters, gradients, optimizer states); equivalent to FSDP; maximum memory reduction; highest communication overhead; enables largest models
- **Stage Selection**: Stage 1 for models <10B parameters; Stage 2 for 10-50B; Stage 3 for 50B+; balance memory savings vs communication cost

**FSDP Implementation (PyTorch):**
- **Wrapping**: from torch.distributed.fsdp import FullyShardedDataParallel as FSDP; model = FSDP(model, sharding_strategy=ShardingStrategy.FULL_SHARD); wraps model for sharding
- **Auto Wrap Policy**: auto_wrap_policy=transformer_auto_wrap_policy; automatically wraps transformer blocks; each block independently sharded; enables fine-grained memory management
- **Mixed Precision**: FSDP(model, mixed_precision=MixedPrecision(param_dtype=torch.bfloat16, reduce_dtype=torch.float32)); parameters in BF16, reductions in FP32; combines FSDP with AMP
- **CPU Offload**: FSDP(model, cpu_offload=CPUOffload(offload_params=True)); offloads parameters to CPU when not in use; further reduces GPU memory; 2-3× slower due to PCIe transfers

**Communication Patterns:**
- **Forward Pass**: all-gather parameters for layer i; compute forward; discard parameters; repeat for each layer; sequential all-gathers (one per layer); latency = num_layers × all_gather_time
- **Backward Pass**: all-gather parameters for layer i; compute gradients; reduce-scatter gradients; discard parameters; repeat in reverse order; overlaps reduce-scatter with next layer's all-gather
- **Optimizer Step**: each GPU updates its local parameter shard; no communication required; parameters remain sharded; next forward pass all-gathers updated parameters
- **Communication Volume**: 2× model size per forward pass (all-gather); 2× model size per backward pass (all-gather + reduce-scatter); 4× total vs 2× for DDP (all-reduce gradients only)

**Performance Optimization:**
- **Activation Checkpointing**: FSDP(model, activation_checkpointing=True); recomputes activations during backward; trades compute for memory; enables 2-4× larger models; essential for FSDP
- **Limit All-Gather**: FSDP(model, limit_all_gathers=True); limits number of concurrent all-gathers; reduces memory spikes; prevents OOM during all-gather operations
- **Forward Prefetch**: FSDP(model, forward_prefetch=True); prefetches next layer's parameters during current layer's computation; overlaps communication with compute; reduces forward pass time by 10-20%
- **Backward Prefetch**: FSDP(model, backward_prefetch=BackwardPrefetch.BACKWARD_PRE); prefetches parameters for next backward layer; overlaps communication with computation; critical for performance

**Hybrid Sharding:**
- **Hybrid Strategy**: FSDP(model, sharding_strategy=ShardingStrategy.HYBRID_SHARD); shards within node, replicates across nodes; reduces inter-node communication; leverages fast intra-node NVLink
- **HSDP (Hierarchical Sharding)**: shard across 8 GPUs per node; replicate across nodes; all-reduce gradients across nodes (DDP-style); all-gather parameters within node (FSDP-style); optimal for multi-node training
- **Performance**: hybrid sharding achieves 90-95% scaling efficiency vs 80-85% for full sharding; reduces inter-node bandwidth requirements; preferred for 100+ GPU training

**Memory Breakdown:**
- **DDP (8 GPUs, 70B model, BF16)**: 140 GB parameters + 140 GB gradients + 280 GB optimizer state (FP32) = 560 GB per GPU; impossible on 40-80 GB GPUs
- **FSDP (8 GPUs, 70B model, BF16)**: (140 + 140 + 280)/8 = 70 GB sharded + 20 GB activations = 90 GB per GPU; fits on 8×80GB A100
- **FSDP + Activation Checkpointing**: 70 GB sharded + 5 GB activations = 75 GB; enables training on 8×80GB with headroom
- **FSDP + CPU Offload**: 10 GB GPU (activations only) + 70 GB CPU (sharded parameters); enables training on 8×16GB GPUs; 3-5× slower

**Comparison with DDP:**
- **Memory**: FSDP uses 1/N memory of DDP; enables N× larger models; critical for 50B+ parameter models
- **Communication**: FSDP has 2× communication volume of DDP; but enables models that don't fit with DDP; acceptable trade-off
- **Speed**: FSDP is 10-30% slower than DDP for same model size; but enables models impossible with DDP; net benefit for large models
- **Complexity**: FSDP requires careful tuning (wrap policy, prefetch, checkpointing); DDP is simpler; use DDP when model fits, FSDP when it doesn't

**Scaling to Extreme Sizes:**
- **100B Parameters**: 8×80GB A100 with FSDP + activation checkpointing + BF16; 85% scaling efficiency; 2-3 days for 1T tokens
- **1T Parameters**: 64×80GB A100 with FSDP + CPU offload + activation checkpointing; 70% scaling efficiency; requires fast interconnect (InfiniBand HDR)
- **Offload Strategies**: parameters to CPU, optimizer states to NVMe SSD; enables training models 10× larger than GPU memory; 5-10× slower but makes impossible possible

**Debugging FSDP:**
- **OOM During All-Gather**: reduce limit_all_gathers; enable activation checkpointing; reduce batch size; indicates insufficient memory for temporary all-gathered parameters
- **Slow Training**: check communication time in profiler; if >30%, reduce model size per GPU or improve network; enable prefetching; use hybrid sharding
- **Gradient Mismatch**: ensure consistent wrap policy across ranks; use auto_wrap_policy; manual wrapping error-prone
- **Checkpoint/Resume**: use FSDP.state_dict_type(model, StateDictType.FULL_STATE_DICT); gathers full model for checkpointing; only on rank 0; avoids saving N sharded checkpoints

Fully Sharded Data Parallel is **the memory-efficiency breakthrough that enables training of models 10-100× larger than GPU memory — by sharding all model state across GPUs and carefully orchestrating communication, FSDP makes training 100B+ parameter models accessible on modest GPU clusters, democratizing large-scale model training and enabling researchers to push the boundaries of model scale without requiring massive infrastructure investments**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/fully-sharded-data-parallel-fsdp) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
