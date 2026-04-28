# Tensor Parallelism

**Keywords**: tensor parallelism distributed,megatron tensor parallelism,column row parallelism,tensor model parallelism,attention parallelism

---

**Tensor Parallelism** is **the model parallelism technique that splits individual weight matrices and tensors across multiple GPUs, with each GPU computing a portion of each layer's output — enabling models with layers too large for single-GPU memory by distributing matrix multiplications column-wise or row-wise and synchronizing results through collective communication operations like all-reduce and all-gather**.

**Tensor Parallelism Fundamentals:**
- **Matrix Partitioning**: for matrix multiplication Y = XW, split weight matrix W across GPUs; column-wise split: each GPU computes Y_i = X·W_i (partial output); row-wise split: each GPU computes Y = X_i·W (partial input)
- **Communication Patterns**: column-wise split requires all-gather to combine partial outputs; row-wise split requires all-reduce to sum partial results; communication volume = batch_size × sequence_length × hidden_dim
- **Intra-Layer Parallelism**: unlike pipeline parallelism (distributes layers), tensor parallelism distributes computation within each layer; all GPUs process same batch simultaneously
- **Scaling Characteristics**: near-linear scaling within a node (8 GPUs with NVLink); efficiency drops with inter-node communication; typically limited to 8-16 GPUs per tensor parallel group

**Megatron-LM Tensor Parallelism:**
- **Attention Layer Splitting**: Q, K, V projections split column-wise across GPUs; each GPU computes attention for subset of heads; output projection split row-wise; requires 2 all-reduce operations per attention layer
- **MLP Layer Splitting**: first linear layer (hidden → intermediate) split column-wise; activation function applied independently; second linear layer (intermediate → hidden) split row-wise; 2 all-reduce operations per MLP
- **Communication Minimization**: careful splitting strategy minimizes communication; only 2 all-reduce per Transformer block (attention + MLP); communication overlapped with computation where possible
- **Identity Operators**: inserts identity operators in forward pass that become all-reduce in backward pass (and vice versa); elegant implementation using autograd

**Column-Wise Parallelism:**
- **Operation**: Y = X·W where W is split column-wise; W = [W_1, W_2, ..., W_N] across N GPUs; each GPU computes Y_i = X·W_i
- **Output Combination**: concatenate partial outputs [Y_1, Y_2, ..., Y_N] to form full output Y; requires all-gather communication
- **Use Cases**: first layer of MLP, Q/K/V projections in attention; enables independent computation of output dimensions
- **Memory Distribution**: each GPU stores 1/N of weights; activation memory not reduced (all GPUs process full batch)

**Row-Wise Parallelism:**
- **Operation**: Y = X·W where W is split row-wise; W = [W_1; W_2; ...; W_N] (stacked vertically); input X also split; each GPU computes Y_i = X_i·W_i
- **Output Combination**: sum partial outputs Y = Σ Y_i; requires all-reduce communication
- **Use Cases**: second layer of MLP, output projection in attention; follows column-wise split to minimize communication
- **Input Splitting**: requires input X to be split across GPUs; typically X is already split from previous column-wise layer

**Communication Optimization:**
- **All-Reduce Fusion**: fuses multiple all-reduce operations into single communication; reduces latency overhead; NCCL automatically fuses small all-reduces
- **Communication Overlap**: starts all-reduce as soon as partial results are ready; overlaps with computation of next layer; requires careful scheduling
- **Gradient All-Reduce**: backward pass requires all-reduce for gradients; same communication volume as forward pass; can overlap with backward computation
- **High-Bandwidth Interconnect**: NVLink (300-600 GB/s within node) essential for efficiency; InfiniBand (200-400 Gb/s across nodes) for multi-node; communication-bound without fast interconnect

**Memory Distribution:**
- **Weight Memory**: each GPU stores 1/N of model weights; enables models N× larger than single GPU capacity
- **Activation Memory**: not reduced by tensor parallelism (all GPUs process full batch); combine with pipeline parallelism or activation checkpointing to reduce activation memory
- **Optimizer State Memory**: each GPU stores optimizer states for its 1/N of weights; total optimizer memory reduced by N×
- **Gradient Memory**: each GPU computes gradients for its 1/N of weights; gradient memory reduced by N×

**Sequence Parallelism Extension:**
- **Motivation**: LayerNorm and Dropout activations not split by standard tensor parallelism; consume significant memory for long sequences
- **Sequence Dimension Splitting**: splits sequence length across GPUs for LayerNorm/Dropout; each GPU processes subset of tokens
- **Communication**: requires all-gather before attention (each token attends to all tokens); all-reduce after attention; additional communication but reduces activation memory
- **Memory Savings**: reduces activation memory by N× for LayerNorm/Dropout; critical for very long sequences (>8K tokens)

**Combining with Other Parallelism:**
- **Tensor + Data Parallelism**: tensor parallelism within groups, data parallelism across groups; example: 64 GPUs = 8 TP × 8 DP
- **Tensor + Pipeline Parallelism**: each pipeline stage uses tensor parallelism; enables very large models; Megatron-LM uses TP within nodes, PP across nodes
- **3D Parallelism**: DP × TP × PP; example: 512 GPUs = 8 DP × 8 TP × 8 PP; matches parallelism to hardware topology
- **Optimal Configuration**: TP within nodes (high bandwidth), PP across nodes (lower bandwidth), DP for remaining GPUs; automated search or manual tuning

**Framework Support:**
- **Megatron-LM (NVIDIA)**: reference implementation of tensor parallelism for Transformers; highly optimized; used for training GPT, BERT, T5 at scale
- **DeepSpeed**: supports tensor parallelism via Megatron integration; combines with ZeRO optimizer; comprehensive parallelism toolkit
- **Fairscale**: PyTorch-native tensor parallelism; modular design; easier integration than Megatron; used by Meta
- **Alpa**: automatic parallelization including tensor parallelism; compiler-based approach; supports JAX

**Implementation Considerations:**
- **Collective Communication**: uses NCCL (NVIDIA) or MPI for all-reduce/all-gather; requires proper initialization and synchronization
- **Determinism**: tensor parallelism is deterministic (same results as single GPU); unlike data parallelism which may have non-deterministic reduction order
- **Gradient Clipping**: must clip gradients after all-reduce; clipping before all-reduce gives incorrect results
- **Batch Normalization**: requires synchronization across tensor parallel group; typically replaced with LayerNorm in Transformers

**Performance Analysis:**
- **Computation Scaling**: each GPU does 1/N of computation; ideal speedup = N×
- **Communication Overhead**: 2 all-reduce per Transformer block; overhead = communication_time / computation_time; want ratio < 10-20%
- **Bandwidth Requirements**: all-reduce volume = 2 × batch_size × sequence_length × hidden_dim per block; requires high bandwidth for efficiency
- **Scaling Efficiency**: 90-95% efficiency within node (NVLink); 70-80% efficiency across nodes (InfiniBand); diminishing returns beyond 16 GPUs

**Practical Guidelines:**
- **When to Use**: model layers don't fit on single GPU; have high-bandwidth interconnect (NVLink); need low-latency parallelism
- **Tensor Parallel Size**: 2-8 GPUs typical; 8 GPUs within node optimal; beyond 8 requires inter-node communication (less efficient)
- **Batch Size**: larger batches amortize communication overhead; batch_size × sequence_length should be large (>1M tokens total)
- **Debugging**: start with TP=2 to verify correctness; scale up gradually; use smaller models for initial debugging

Tensor parallelism is **the fine-grained parallelism technique that enables training of models with individual layers too large for single-GPU memory — by splitting weight matrices and carefully orchestrating collective communication, it achieves near-linear scaling within high-bandwidth GPU clusters, making it essential for frontier models where even a single attention layer exceeds GPU capacity**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/tensor-parallelism-distributed) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
