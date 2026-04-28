# Hierarchical Collective Communication

**Keywords**: collective communication hierarchical, two level allreduce, node leader collectives, multi tier communication

---

**Hierarchical Collective Communication** is **the multi-tier communication strategy that exploits the bandwidth and latency asymmetry of modern clusters by performing separate collective operations at each level of the system hierarchy (intra-node, intra-rack, inter-rack) — using fast shared memory or NVLink for local communication and slower InfiniBand or Ethernet for remote communication, reducing cross-tier traffic by 8-64× and enabling efficient scaling to thousands of nodes**.

**System Hierarchy Levels:**
- **Intra-Node (L1)**: GPUs within a single node communicate via NVLink (900 GB/s), PCIe (64 GB/s), or shared memory (100+ GB/s); 8-16 GPUs per node; sub-microsecond latency; highest bandwidth tier
- **Intra-Rack (L2)**: nodes within a rack communicate via top-of-rack switch; typically 8-32 nodes per rack; InfiniBand or high-speed Ethernet; 100-400 Gb/s per node; 1-5μs latency
- **Inter-Rack (L3)**: racks communicate via spine switches; 10-100 racks; may have oversubscription (4:1 or 8:1); 25-100 Gb/s effective per node; 5-20μs latency
- **Bandwidth Asymmetry**: L1:L2:L3 bandwidth ratio typically 10:5:1; L1 latency 10-100× lower than L3; hierarchical algorithms exploit this asymmetry

**Two-Level Hierarchical All-Reduce:**
- **Intra-Node Reduction**: each node performs local all-reduce among its GPUs using NVLink/shared memory; completes in microseconds; produces one reduced result per node
- **Inter-Node All-Reduce**: node leaders (one GPU per node) perform all-reduce across nodes using InfiniBand; transfers 1/N_gpus_per_node data compared to flat all-reduce; completes in milliseconds
- **Intra-Node Broadcast**: node leaders broadcast inter-node result to local GPUs; completes in microseconds; all GPUs now have complete all-reduce result
- **Traffic Reduction**: inter-node traffic reduced by N_gpus_per_node (typically 8×); critical when inter-node bandwidth is bottleneck

**Algorithm Selection Per Level:**
- **L1 (Intra-Node)**: shared memory copies or NVLink direct transfers; no network protocol overhead; simple memcpy or GPU-to-GPU cudaMemcpy; 8 GPUs complete in <100μs
- **L2 (Intra-Rack)**: ring or tree all-reduce over InfiniBand; low latency within rack; 32 nodes complete in <1ms
- **L3 (Inter-Rack)**: ring all-reduce for bandwidth efficiency; tree if latency-critical; may use compression to reduce cross-rack traffic
- **Hybrid Algorithms**: NCCL automatically detects hierarchy and selects optimal algorithm per level; ring for L3, tree for L2, direct copy for L1

**Multi-Tier Hierarchical Collectives:**
- **Three-Level All-Reduce**: intra-node → intra-rack → inter-rack → intra-rack broadcast → intra-node broadcast; five phases total; each phase uses algorithm optimized for that tier
- **Recursive Hierarchy**: generalize to arbitrary depth; each level performs local all-reduce, one representative per group participates in next level; logarithmic reduction in traffic at each level
- **Topology-Aware Grouping**: group processes by physical proximity; SLURM topology plugin provides hierarchical node grouping; MPI communicator splitting creates sub-communicators per level
- **Dynamic Hierarchy**: adapt hierarchy to current network conditions; if inter-rack links congested, increase intra-rack batch size to reduce cross-rack frequency

**Node Leader Selection:**
- **Fixed Leader**: designate GPU 0 on each node as leader; simple but may create hotspot if leader also performs computation
- **Round-Robin**: rotate leader role across GPUs; balances load but adds complexity
- **Least-Loaded**: select GPU with least work as leader; requires load monitoring; optimal for heterogeneous workloads
- **Dedicated Communication GPU**: reserve one GPU per node for communication; maximizes compute GPU utilization but wastes 12.5% of GPUs (1/8)

**Performance Benefits:**
- **Bandwidth Savings**: 8-GPU nodes reduce inter-node traffic by 8×; 1000 GPUs (125 nodes) transfer 125× less data across inter-node network than flat all-reduce
- **Latency Reduction**: local all-reduce completes in microseconds; only inter-node phase contributes milliseconds; total latency dominated by slowest tier, not sum of all tiers
- **Scalability**: hierarchical all-reduce scales to 10,000+ GPUs; flat all-reduce becomes communication-bound beyond 1000 GPUs; hierarchy maintains <20% communication overhead at scale
- **Fault Isolation**: failures within a node don't affect inter-node communication; hierarchical structure contains fault impact; improves overall system reliability

**Implementation Challenges:**
- **Synchronization**: all GPUs in a node must reach intra-node all-reduce before node leader proceeds to inter-node; requires barriers or careful dependency tracking
- **Load Imbalance**: if nodes have different computation times, fast nodes wait at inter-node barrier; hierarchical structure amplifies load imbalance effects
- **Memory Management**: node leader must buffer data from local GPUs; requires additional memory allocation; can cause out-of-memory if not carefully managed
- **Complexity**: three-level hierarchy requires coordinating three separate collective operations; debugging and optimization more difficult than flat collectives

**NCCL Hierarchical Implementation:**
- **Automatic Detection**: NCCL detects NVLink topology, PCIe topology, and network topology; builds hierarchical communication plan automatically
- **Collnet Protocol**: NCCL protocol for hierarchical collectives; uses node leaders for inter-node communication; optimized for InfiniBand with SHARP (in-network reduction)
- **Tuning Parameters**: NCCL_CROSS_NIC controls inter-node communication; NCCL_COLLNET_ENABLE enables hierarchical collectives; NCCL_TOPO_FILE specifies custom topology
- **Performance**: NCCL hierarchical all-reduce achieves 90%+ of theoretical bandwidth; 2-3× faster than flat all-reduce at 1000+ GPU scale

**Use Cases:**
- **Large-Scale Training**: 1000+ GPU training runs; inter-node bandwidth becomes bottleneck; hierarchical collectives essential for scaling
- **Cloud Environments**: cloud instances have high intra-node bandwidth (NVLink) but limited inter-node bandwidth (25-100 Gb/s); hierarchy exploits this asymmetry
- **Heterogeneous Networks**: mix of fast local interconnect and slower wide-area network; hierarchical approach adapts to heterogeneity
- **Cost Optimization**: oversubscribed inter-rack links (8:1) reduce network cost; hierarchical collectives maintain performance despite oversubscription

Hierarchical collective communication is **the essential technique for scaling distributed training beyond single nodes — by exploiting the natural hierarchy of modern clusters and reducing cross-tier traffic by orders of magnitude, hierarchical collectives enable efficient training at scales where flat collectives would be completely communication-bound**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/collective-communication-hierarchical) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
