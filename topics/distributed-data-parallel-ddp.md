# Distributed Data Parallel (DDP)

**Keywords**: distributed data parallel ddp,pytorch ddp training,gradient synchronization ddp,ddp communication overlap,multi gpu data parallel

---

**Distributed Data Parallel (DDP)** is **the PyTorch framework for synchronous multi-GPU and multi-node training where each process maintains a full model replica and processes a different data subset — automatically synchronizing gradients via all-reduce after backward pass, overlapping communication with computation through gradient bucketing, and achieving 85-95% scaling efficiency to hundreds of GPUs by minimizing synchronization overhead and maximizing hardware utilization through careful engineering of the training loop**.

**DDP Architecture:**
- **Process Group**: each GPU runs independent Python process; processes communicate via NCCL (GPU) or Gloo (CPU); torch.distributed.init_process_group(backend='nccl', init_method='env://', world_size=N, rank=i)
- **Model Replication**: each process has full model copy; model = DDP(model, device_ids=[local_rank]); parameters synchronized at initialization; ensures all replicas start identically
- **Data Partitioning**: DistributedSampler partitions dataset across processes; each process sees different data subset; sampler = DistributedSampler(dataset, num_replicas=world_size, rank=rank); ensures no data duplication
- **Gradient Synchronization**: after backward(), DDP all-reduces gradients across processes; each process receives averaged gradient; optimizer.step() updates local model copy with synchronized gradients

**Gradient Bucketing:**
- **Bucket Formation**: DDP groups parameters into buckets (~25 MB each); parameters in same bucket all-reduced together; reduces communication overhead from N all-reduces (N parameters) to B all-reduces (B buckets)
- **Reverse Order**: buckets formed in reverse parameter order; first bucket contains last layers; enables overlap of backward pass with all-reduce; as soon as bucket's gradients ready, all-reduce starts
- **Overlap**: while backward pass computes gradients for layer i, all-reduce synchronizes gradients for layer i+1; achieves 50-80% overlap; reduces communication time from 20-30% to 5-15% of iteration time
- **Bucket Size Tuning**: DDP(model, bucket_cap_mb=25); larger buckets → more overlap, higher latency; smaller buckets → less overlap, lower latency; 25 MB default optimal for most models

**Communication Overlap:**
- **Backward Hook**: DDP registers hooks on each parameter; hook fires when gradient ready; triggers all-reduce for parameter's bucket; enables asynchronous communication
- **Computation-Communication Overlap**: GPU computes gradients for layer i while NCCL all-reduces gradients for layer i+1; both operations use different hardware resources (SMs vs copy engines); achieves true parallelism
- **Synchronization Point**: optimizer.step() waits for all all-reduces to complete; ensures all gradients synchronized before weight update; maintains training correctness
- **Efficiency**: well-overlapped DDP adds <10% overhead vs single-GPU; poorly overlapped (small model, slow network) adds 50-100% overhead

**Initialization and Setup:**
- **Environment Variables**: MASTER_ADDR, MASTER_PORT, WORLD_SIZE, RANK set by launcher (torchrun, mpirun); init_process_group() reads these; establishes communication
- **Local Rank**: GPU index on current node; local_rank = int(os.environ['LOCAL_RANK']); used for device placement: model.to(local_rank)
- **Torchrun**: torchrun --nproc_per_node=8 train.py; launches 8 processes on single node; handles environment variable setup; simplifies multi-GPU training
- **Multi-Node**: torchrun --nnodes=4 --nproc_per_node=8 --master_addr=node0 --master_port=29500 train.py; launches 32 processes across 4 nodes; requires network connectivity

**Gradient Accumulation with DDP:**
- **No-Sync Context**: with model.no_sync(): loss.backward(); — disables gradient synchronization; gradients accumulate locally; use for all but last accumulation step
- **Final Step**: loss.backward(); — without no_sync, triggers all-reduce; synchronizes accumulated gradients; optimizer.step() updates weights
- **Implementation**: for i in range(accumulation_steps): with model.no_sync() if i < accumulation_steps-1 else nullcontext(): loss = model(data[i]); loss.backward(); optimizer.step()
- **Efficiency**: reduces all-reduce frequency by K× (K=accumulation steps); reduces communication overhead; improves scaling efficiency for small models

**Performance Optimization:**
- **Batch Size**: larger per-GPU batch size improves GPU utilization; reduces communication-to-computation ratio; target >32 samples per GPU; use gradient accumulation if memory limited
- **Model Size**: larger models have more computation per all-reduce; better overlap; small models (<100M parameters) have poor scaling; consider model parallelism instead
- **Network Bandwidth**: NVLink (600 GB/s) enables near-perfect scaling; InfiniBand (200 Gb/s) enables 85-95% scaling; Ethernet (10-100 Gb/s) limits scaling to 50-80%
- **Gradient Compression**: DDP supports FP16 gradient all-reduce; 2× bandwidth reduction; minimal accuracy impact; enable with autocast()

**Comparison with DataParallel:**
- **DataParallel (DP)**: single-process, multi-thread; GIL limits parallelism; broadcasts model every iteration; collects gradients on one GPU; 50-70% scaling efficiency; deprecated
- **DDP**: multi-process; no GIL; model replicated once; gradients all-reduced; 85-95% scaling efficiency; recommended for all multi-GPU training
- **Migration**: replace DataParallel(model) with DDP(model, device_ids=[local_rank]); add init_process_group() and DistributedSampler; 2-3× speedup on 8 GPUs

**Debugging DDP:**
- **Hang Detection**: TORCH_DISTRIBUTED_DEBUG=DETAIL enables verbose logging; identifies communication deadlocks; shows which rank is stuck
- **Gradient Mismatch**: set_detect_anomaly(True) detects NaN/Inf; compare gradients across ranks; mismatch indicates non-deterministic operations (dropout without seed)
- **Performance Profiling**: torch.profiler shows communication time; nsight systems visualizes overlap; identify communication bottlenecks
- **Rank-Specific Logging**: if rank == 0: print(...); prevents duplicate logging; only master rank logs; reduces log clutter

**Advanced Features:**
- **Gradient as Bucket View**: DDP(model, gradient_as_bucket_view=True); gradients stored in contiguous bucket memory; reduces memory copies; 5-10% speedup
- **Static Graph**: DDP(model, static_graph=True); assumes model graph doesn't change; enables optimizations; use for models without dynamic control flow
- **Find Unused Parameters**: DDP(model, find_unused_parameters=True); handles models with conditional branches; adds overhead; only use when necessary (e.g., mixture of experts)
- **Broadcast Buffers**: DDP(model, broadcast_buffers=True); synchronizes batch norm running statistics; ensures consistent inference across ranks

**Scaling Efficiency:**
- **Strong Scaling**: fixed total batch size, increase GPUs; efficiency = T₁/(N×Tₙ); DDP achieves 85-95% for large models; 50-70% for small models
- **Weak Scaling**: batch size scales with GPUs; efficiency = T₁/Tₙ; DDP achieves 90-98%; near-linear scaling; preferred for training large models
- **Bottlenecks**: small models → communication dominates; slow network → synchronization overhead; small batch size → poor GPU utilization

Distributed Data Parallel is **the workhorse of multi-GPU training — by carefully engineering gradient synchronization, communication overlap, and efficient bucketing, DDP achieves 85-95% scaling efficiency with minimal code changes, making it the default choice for training models from ResNet-50 to GPT-3 and enabling researchers to leverage hundreds of GPUs for faster iteration and larger-scale experiments**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/distributed-data-parallel-ddp) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
