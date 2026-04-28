# Peer-to-Peer GPU Communication

**Keywords**: peer to peer gpu communication,nvlink bandwidth,gpu direct rdma,p2p memory access,multi gpu data transfer

---

**Peer-to-Peer GPU Communication** is **the capability for GPUs to directly access each other's memory without routing through the CPU or host memory — utilizing high-bandwidth interconnects like NVLink (300-900 GB/s) or PCIe peer-to-peer (16-32 GB/s) to enable efficient multi-GPU algorithms, achieving 5-20× faster inter-GPU transfers compared to host-mediated copies and enabling tightly-coupled multi-GPU workloads like model parallelism and distributed training**.

**P2P Capabilities:**
- **Direct Memory Access**: GPU 0 can directly read/write GPU 1's memory using device pointers; cudaMemcpyPeer(dst, dstDevice, src, srcDevice, size); transfers data directly between GPUs; bypasses host memory and CPU
- **Unified Virtual Addressing (UVA)**: all GPUs and host share single virtual address space; device pointer from GPU 0 is valid on GPU 1; enables transparent peer access without address translation
- **P2P Enablement**: cudaDeviceCanAccessPeer(&canAccess, device0, device1); checks if P2P possible; cudaDeviceEnablePeerAccess(peerDevice, 0); enables direct access; required once per device pair
- **Automatic P2P**: unified memory with cudaMemAdviseSetAccessedBy automatically uses P2P when available; simplifies multi-GPU programming; achieves optimal performance without explicit P2P management

**NVLink Architecture:**
- **Bandwidth**: NVLink 2.0 (V100): 300 GB/s bidirectional; NVLink 3.0 (A100): 600 GB/s; NVLink 4.0 (H100): 900 GB/s; 10-30× faster than PCIe 4.0 (32 GB/s); enables tightly-coupled multi-GPU algorithms
- **Topology**: DGX A100: all-to-all NVLink (every GPU connected to every other); DGX H100: NVSwitch provides full bisection bandwidth; consumer GPUs: 2-4 NVLink connections per GPU (partial connectivity)
- **Latency**: NVLink latency ~1-2 μs; PCIe latency ~5-10 μs; lower latency enables fine-grained communication patterns; critical for model parallelism with frequent small transfers
- **Coherence**: NVLink supports cache coherence protocols; enables atomic operations across GPUs; unified memory coherence maintained automatically; simplifies multi-GPU synchronization

**PCIe Peer-to-Peer:**
- **Bandwidth**: PCIe 3.0 x16: 16 GB/s; PCIe 4.0 x16: 32 GB/s; PCIe 5.0 x16: 64 GB/s; sufficient for coarse-grained data parallelism; insufficient for fine-grained model parallelism
- **Topology Constraints**: P2P requires GPUs on same PCIe root complex; GPUs on different CPU sockets may not support P2P; check topology with nvidia-smi topo -m; NUMA effects impact performance
- **CPU Affinity**: bind CPU threads to socket nearest to GPU; reduces PCIe latency; improves P2P bandwidth by 10-30%; use numactl or taskset for CPU pinning
- **Switch Limitations**: PCIe switches may limit P2P bandwidth; multiple GPUs sharing switch compete for bandwidth; measure actual bandwidth with p2pBandwidthLatencyTest

**GPUDirect RDMA:**
- **Direct Network Access**: GPUs directly access network adapters (InfiniBand, RoCE) without CPU involvement; eliminates host memory staging; reduces latency from ~10 μs to ~2 μs
- **NCCL Integration**: NCCL (NVIDIA Collective Communications Library) automatically uses GPUDirect RDMA when available; enables efficient multi-node multi-GPU communication; critical for distributed training
- **Bandwidth**: InfiniBand HDR: 200 Gb/s (25 GB/s) per port; 8-port switch provides 1.6 Tb/s aggregate; enables scaling to hundreds of GPUs with minimal communication overhead
- **Requirements**: requires MLNX_OFED drivers, GPUDirect-capable network adapter, and kernel module; check with nvidia-smi and ibstat; widely supported on HPC and cloud infrastructure

**Multi-GPU Communication Patterns:**
- **Broadcast**: one GPU sends data to all others; NVLink enables simultaneous broadcast to all peers; PCIe requires sequential sends or tree-based broadcast; NCCL provides optimized broadcast
- **Reduce**: all GPUs send data to one GPU for aggregation; reverse of broadcast; used for gradient accumulation in distributed training; NCCL uses tree or ring algorithms for optimal bandwidth
- **All-Reduce**: every GPU receives reduction of all GPUs' data; most common operation in data-parallel training; NCCL ring all-reduce achieves optimal bandwidth utilization (2×(N-1)/N efficiency for N GPUs)
- **All-to-All**: every GPU sends unique data to every other GPU; highest bandwidth requirement; used in model parallelism and tensor parallelism; requires full bisection bandwidth (NVLink or NVSwitch)

**Performance Optimization:**
- **Batch Transfers**: combine multiple small transfers into large transfer; amortizes latency overhead; 1 MB transfer: 90% efficiency; 1 KB transfer: 10% efficiency; target >1 MB per transfer
- **Asynchronous Transfers**: cudaMemcpyPeerAsync(dst, dstDev, src, srcDev, size, stream); overlaps transfer with compute; use streams to pipeline communication and computation
- **Bidirectional Bandwidth**: NVLink supports simultaneous send and receive; achieve 2× bandwidth by overlapping transfers in both directions; use separate streams for each direction
- **Topology-Aware Placement**: place communicating GPUs on same NVLink domain; avoid cross-socket PCIe transfers; use nvidia-smi topo -m to understand topology; assign work based on connectivity

**NCCL (NVIDIA Collective Communications Library):**
- **Collective Operations**: ncclAllReduce, ncclBroadcast, ncclReduce, ncclAllGather, ncclReduceScatter; optimized for GPU topology; automatically selects best algorithm (ring, tree, double-binary-tree)
- **Multi-Node Support**: NCCL handles both intra-node (NVLink/PCIe) and inter-node (InfiniBand/Ethernet) communication; unified API for single-node and multi-node; scales to thousands of GPUs
- **Performance**: achieves 90-95% of hardware bandwidth for large messages (>1 MB); 50-70% for small messages (<64 KB); outperforms MPI by 2-5× for GPU-to-GPU communication
- **Integration**: PyTorch DistributedDataParallel, TensorFlow MultiWorkerMirroredStrategy, Horovod all use NCCL; transparent to application code; optimal performance without manual tuning

**Profiling and Debugging:**
- **Bandwidth Measurement**: p2pBandwidthLatencyTest (CUDA samples) measures P2P bandwidth and latency; compare to theoretical maximum; identify topology bottlenecks
- **Nsight Systems**: visualizes P2P transfers on timeline; shows overlap with compute; identifies communication bottlenecks; essential for optimizing multi-GPU applications
- **NCCL_DEBUG=INFO**: enables NCCL logging; shows selected algorithms, detected topology, and performance warnings; useful for debugging communication issues
- **nvidia-smi topo -m**: displays GPU topology matrix; shows NVLink connections, PCIe paths, and NUMA affinity; essential for understanding communication capabilities

**Use Cases:**
- **Data Parallelism**: broadcast model parameters, all-reduce gradients; coarse-grained communication (every few milliseconds); PCIe P2P sufficient; NVLink provides 2-3× speedup
- **Model Parallelism**: split model across GPUs; fine-grained communication (every layer); requires NVLink for acceptable performance; PCIe causes 5-10× slowdown
- **Pipeline Parallelism**: pass activations between GPUs; medium-grained communication (every micro-batch); NVLink preferred; PCIe acceptable with large micro-batches
- **Tensor Parallelism**: split individual tensors across GPUs; very fine-grained communication (every operation); requires NVLink or NVSwitch; impossible with PCIe alone

Peer-to-peer GPU communication is **the enabling technology for multi-GPU deep learning and HPC — by providing direct, high-bandwidth, low-latency GPU-to-GPU data transfer through NVLink and GPUDirect, P2P enables scaling from single-GPU to multi-node clusters with 80-95% efficiency, making it the foundation of all large-scale distributed training and the key to training frontier AI models**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/peer-to-peer-gpu-communication) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
