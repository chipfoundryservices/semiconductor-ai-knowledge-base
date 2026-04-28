# PagedAttention

**Keywords**: pagedattention vllm,virtual memory kv cache,paged memory management,kv cache blocks,memory efficient serving

---

**PagedAttention** is **the attention mechanism that manages KV cache using virtual memory techniques with fixed-size blocks (pages)** — eliminating memory fragmentation and enabling near-optimal memory utilization (90-95% vs 20-40% for naive allocation), allowing 2-4× larger batch sizes or longer contexts in LLM serving, forming the foundation of high-throughput inference systems like vLLM.

**Memory Fragmentation Problem:**
- **Naive Allocation**: pre-allocate contiguous memory for maximum sequence length; wastes memory for shorter sequences; example: allocate for 2048 tokens, use 100 tokens, waste 95% memory
- **Fragmentation**: variable-length sequences create fragmentation; cannot pack sequences efficiently; memory utilization 20-40% typical; limits batch size and throughput
- **Dynamic Growth**: sequences grow token-by-token during generation; hard to predict final length; over-allocation wastes memory; under-allocation requires reallocation
- **Example**: 32 sequences, max length 2048, average length 200; naive allocation: 32×2048 = 65K tokens; actual usage: 32×200 = 6.4K tokens; 90% waste

**PagedAttention Design:**
- **Block-Based Storage**: divide KV cache into fixed-size blocks (pages); typical block size 16-64 tokens; allocate blocks on-demand as sequence grows
- **Virtual Memory Mapping**: each sequence has virtual address space; maps to physical blocks; non-contiguous physical storage; transparent to attention computation
- **Block Table**: maintain mapping from virtual blocks to physical blocks; similar to OS page table; enables efficient address translation
- **On-Demand Allocation**: allocate blocks only when needed; deallocate when sequence completes; eliminates waste from over-allocation; achieves 90-95% utilization

**Attention Computation:**
- **Block-Wise Attention**: compute attention block-by-block; gather physical blocks for sequence; compute attention as if contiguous; mathematically equivalent to standard attention
- **Address Translation**: translate virtual block IDs to physical block IDs; load physical blocks from memory; compute attention; store results
- **Kernel Optimization**: custom CUDA kernels for block-wise attention; optimized memory access patterns; fused operations; achieves near-native performance
- **Performance**: 5-10% overhead vs contiguous memory; acceptable trade-off for 2-4× memory efficiency; overhead decreases with larger blocks

**Copy-on-Write Sharing:**
- **Prefix Sharing**: sequences with common prefix (system prompt, few-shot examples) share physical blocks; only copy when sequences diverge
- **Reference Counting**: track references to each block; deallocate when reference count reaches zero; enables safe sharing
- **Divergence Handling**: when sequence modifies shared block, copy block before modification; update block table; other sequences unaffected
- **Use Cases**: multi-turn conversations (share conversation history), beam search (share prefix), parallel sampling (share prompt); major memory savings

**Memory Management:**
- **Block Allocation**: maintain free list of available blocks; allocate from free list on-demand; deallocate to free list when sequence completes
- **Eviction Policy**: when memory full, evict blocks from low-priority sequences; LRU or priority-based eviction; enables oversubscription
- **Swapping**: swap blocks to CPU memory or disk; enables serving more sequences than GPU memory; trades latency for capacity
- **Defragmentation**: not needed due to block-based design; major advantage over contiguous allocation; simplifies memory management

**Performance Impact:**
- **Memory Utilization**: 90-95% vs 20-40% for naive allocation; 2-4× improvement; directly enables larger batch sizes
- **Batch Size**: 2-4× larger batches in same memory; improves throughput proportionally; critical for serving efficiency
- **Throughput**: combined with continuous batching, achieves 10-20× throughput vs naive serving; major cost savings
- **Latency**: minimal overhead (5-10%) from block-based access; acceptable for massive memory savings; user-imperceptible

**Implementation Details:**
- **Block Size Selection**: 16-64 tokens typical; smaller blocks reduce internal fragmentation but increase metadata overhead; 32 tokens balances trade-offs
- **Metadata Overhead**: block table size = num_sequences × max_blocks_per_sequence × 4 bytes; typically <1% of total memory; negligible
- **CUDA Kernels**: custom kernels for block-wise attention; optimized for coalesced memory access; fused operations; critical for performance
- **Multi-GPU**: each GPU has independent block allocator; sequences can span GPUs with tensor parallelism; requires coordination

**vLLM Integration:**
- **Core Component**: PagedAttention is foundation of vLLM; enables high-throughput serving; production-tested at scale
- **Continuous Batching**: PagedAttention enables efficient continuous batching; dynamic memory allocation critical for variable batch sizes
- **Prefix Caching**: automatic prefix sharing; transparent to user; major performance improvement for repetitive prompts
- **Monitoring**: vLLM provides memory utilization metrics; block allocation statistics; helps optimize configuration

**Comparison with Alternatives:**
- **vs Naive Allocation**: 2-4× better memory utilization; enables larger batches; major throughput improvement
- **vs Reallocation**: no reallocation overhead; predictable performance; simpler implementation
- **vs Compression**: orthogonal to compression; can combine PagedAttention with quantization; multiplicative benefits
- **vs Offloading**: PagedAttention reduces need for offloading; but can combine for extreme oversubscription

**Advanced Features:**
- **Prefix Caching**: automatically cache and share common prefixes; reduces computation; improves throughput for repetitive prompts
- **Sliding Window**: for models with sliding window attention (Mistral), only cache recent blocks; reduces memory; enables unbounded generation
- **Multi-LoRA**: serve multiple LoRA adapters with shared base model KV cache; different adapters per sequence; enables multi-tenant serving
- **Speculative Decoding**: PagedAttention compatible with speculative decoding; manage draft and target model caches efficiently

**Use Cases:**
- **High-Throughput Serving**: production API endpoints; chatbots; code completion; any high-request-rate application; 10-20× throughput improvement
- **Long-Context Serving**: enables serving longer contexts by reducing memory waste; 2-4× longer contexts in same memory
- **Multi-Tenant Serving**: efficient memory sharing across tenants; prefix caching for common prompts; cost-effective multi-tenancy
- **Beam Search**: efficient memory management for multiple beams; prefix sharing reduces memory; enables larger beam widths

**Best Practices:**
- **Block Size**: use 32-64 tokens for most applications; smaller for memory-constrained scenarios; larger for simplicity
- **Memory Reservation**: reserve 10-20% memory for incoming requests; prevents out-of-memory errors; maintains headroom
- **Monitoring**: track block utilization, fragmentation, sharing efficiency; optimize based on metrics; critical for production
- **Tuning**: adjust block size, reservation based on workload; profile and iterate; workload-dependent optimization

PagedAttention is **the innovation that made high-throughput LLM serving practical** — by applying virtual memory techniques to KV cache management, it eliminates fragmentation and achieves near-optimal memory utilization, enabling the 10-20× throughput improvements that make large-scale LLM deployment economically viable.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/pagedattention-vllm) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
