# Sequence Parallelism

**Keywords**: sequence parallelism transformers,long sequence parallelism,ring attention mechanism,sequence dimension splitting,ulysses sequence parallel

---

**Sequence Parallelism** is **the parallelism technique that partitions the sequence length dimension across multiple GPUs to handle extremely long sequences that exceed single-GPU memory capacity — distributing tokens across devices while maintaining the ability to compute global attention through ring-based communication patterns or hierarchical attention schemes that enable processing of million-token contexts**.

**Sequence Parallelism Fundamentals:**
- **Sequence Dimension Splitting**: divides sequence of length N into chunks across P GPUs; each GPU processes N/P tokens; reduces per-GPU memory from O(N) to O(N/P)
- **Attention Challenge**: self-attention requires each token to attend to all tokens; naive splitting breaks attention computation; requires communication to gather all tokens or clever algorithmic modifications
- **Memory Bottleneck**: for long sequences, activation memory dominates; sequence length 100K with hidden_dim 4096 requires ~40GB just for activations; sequence parallelism addresses this bottleneck
- **Complementary to Tensor Parallelism**: tensor parallelism splits hidden dimension, sequence parallelism splits sequence dimension; can be combined for maximum memory reduction

**Megatron Sequence Parallelism:**
- **LayerNorm and Dropout Splitting**: splits sequence dimension for operations outside attention/MLP (LayerNorm, Dropout); these operations are sequence-independent and easily parallelizable
- **Communication Pattern**: all-gather before attention (gather all tokens), all-reduce after attention (reduce across sequence dimension); communication volume = sequence_length × hidden_dim
- **Memory Savings**: reduces activation memory for LayerNorm/Dropout by P×; attention activations still replicated; effective for moderate sequence lengths (8K-32K)
- **Integration with Tensor Parallelism**: naturally combines with tensor parallelism; sequence parallel group can be same as or different from tensor parallel group

**Ring Attention:**
- **Block-Wise Attention**: divides sequence into blocks; computes attention block-by-block using ring communication; each GPU maintains local block and receives remote blocks in sequence
- **Ring Communication**: GPUs arranged in ring topology; each step, GPU i sends its block to GPU i+1 and receives from GPU i-1; P steps to process all blocks
- **Memory Efficiency**: only stores 2 blocks at a time (local + received); memory = O(N/P) instead of O(N); enables extremely long sequences (millions of tokens)
- **Computation**: for each received block, computes attention between local queries and received keys/values; accumulates attention outputs; mathematically equivalent to full attention

**Ulysses Sequence Parallelism:**
- **All-to-All Communication**: uses all-to-all collective to redistribute tokens; transforms sequence-parallel layout to head-parallel layout and back
- **Attention Computation**: after all-to-all, each GPU has all tokens for subset of attention heads; computes full attention for its heads; another all-to-all to restore sequence-parallel layout
- **Communication Volume**: 2 all-to-all operations per attention layer; volume = sequence_length × hidden_dim; higher bandwidth requirement than ring but simpler implementation
- **Scaling**: efficient for moderate sequence parallelism (2-8 GPUs); communication overhead increases with more GPUs; works well with high-bandwidth interconnect

**DeepSpeed-Ulysses:**
- **Hybrid Approach**: combines sequence parallelism with tensor parallelism; sequence parallel within groups, tensor parallel across groups
- **Communication Optimization**: overlaps all-to-all communication with computation; uses NCCL for efficient collective operations
- **Memory Efficiency**: reduces activation memory by sequence_parallel_size × tensor_parallel_size; enables very long sequences with large models
- **Implementation**: integrated into DeepSpeed; supports various Transformer architectures; production-ready with extensive testing

**Hierarchical Attention:**
- **Local + Global Attention**: local attention within sequence chunks (no communication), global attention across chunk representatives (with communication)
- **Chunk Representatives**: each chunk produces summary token(s); global attention computed on summaries; results broadcast back to chunks
- **Memory Savings**: local attention is O(N/P) per GPU; global attention is O(P) (number of chunks); total memory O(N/P + P) << O(N)
- **Approximation**: not exact attention; trades accuracy for efficiency; quality depends on chunk size and representative selection

**Flash Attention with Sequence Parallelism:**
- **Tiled Computation**: Flash Attention already tiles attention computation; natural fit for sequence parallelism
- **Ring Flash Attention**: combines ring communication with Flash Attention tiling; each GPU processes tiles of local and remote blocks
- **Memory Efficiency**: O(N/P) memory per GPU with O(N²) computation; enables both long sequences and memory efficiency
- **Performance**: 2-4× faster than naive sequence parallelism; IO-aware algorithm minimizes memory traffic

**Communication Patterns:**
- **All-Gather**: gathers all sequence chunks to each GPU; required before full attention; volume = (P-1)/P × sequence_length × hidden_dim
- **All-Reduce**: reduces attention outputs across GPUs; volume = sequence_length × hidden_dim
- **All-to-All**: redistributes tokens for head-parallel layout; volume = sequence_length × hidden_dim; bidirectional communication
- **Ring Send/Recv**: point-to-point communication in ring topology; P steps with volume = sequence_length/P × hidden_dim per step

**Combining with Other Parallelism:**
- **Sequence + Tensor Parallelism**: sequence parallel for sequence dimension, tensor parallel for hidden dimension; orthogonal dimensions enable independent scaling
- **Sequence + Pipeline Parallelism**: each pipeline stage uses sequence parallelism; enables long sequences with large models
- **4D Parallelism**: data × tensor × pipeline × sequence; example: 1024 GPUs = 4 DP × 8 TP × 8 PP × 4 SP; maximum flexibility for extreme scale
- **Optimal Configuration**: depends on sequence length, model size, and hardware; longer sequences benefit more from sequence parallelism

**Use Cases:**
- **Long Document Processing**: processing entire books (100K+ tokens) or codebases; sequence parallelism enables single-pass processing without chunking
- **High-Resolution Images**: vision transformers with high-resolution inputs (1024×1024 = 1M patches); sequence parallelism handles large patch counts
- **Video Understanding**: video with many frames (1000 frames × 256 patches = 256K tokens); sequence parallelism enables full-video attention
- **Scientific Computing**: protein sequences (10K+ amino acids), genomic sequences (millions of base pairs); sequence parallelism enables analysis of complete sequences

**Implementation Considerations:**
- **Communication Overhead**: sequence parallelism adds communication; requires high-bandwidth interconnect (NVLink, InfiniBand) for efficiency
- **Load Balancing**: uneven sequence lengths cause load imbalance; padding or dynamic load balancing required
- **Gradient Synchronization**: backward pass requires communication for gradients; same patterns as forward pass
- **Numerical Stability**: distributed attention computation must maintain numerical stability; careful handling of softmax normalization

**Performance Analysis:**
- **Memory Scaling**: activation memory reduces by P× (sequence parallel size); enables P× longer sequences
- **Computation Scaling**: computation per GPU reduces by P×; ideal speedup = P×
- **Communication Overhead**: depends on pattern (ring vs all-to-all) and bandwidth; overhead = communication_time / computation_time; want < 20%
- **Scaling Efficiency**: 80-90% efficiency for 2-8 GPUs with high-bandwidth interconnect; diminishing returns beyond 8 GPUs

**Framework Support:**
- **Megatron-LM**: sequence parallelism for LayerNorm/Dropout; integrates with tensor and pipeline parallelism
- **DeepSpeed-Ulysses**: all-to-all based sequence parallelism; supports various Transformer architectures
- **Ring Attention (Research)**: ring-based attention for extreme sequence lengths; reference implementations available
- **Colossal-AI**: supports multiple sequence parallelism strategies; flexible configuration

Sequence parallelism is **the frontier technique for processing extremely long sequences — enabling million-token contexts through clever distribution of the sequence dimension and ring-based communication patterns, making it possible to process entire books, codebases, or high-resolution videos in a single forward pass without truncation or hierarchical chunking**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/sequence-parallelism-transformers) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
