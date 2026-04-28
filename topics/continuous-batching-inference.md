# Continuous Batching

**Keywords**: continuous batching inference,dynamic batching llm,iteration level batching,orca batching,vllm continuous batching

---

**Continuous Batching** is **the inference serving technique that dynamically adds and removes sequences from batches at each generation step rather than waiting for all sequences to complete** — improving GPU utilization by 2-10× and reducing average latency by 30-50% compared to static batching, enabling high-throughput LLM serving systems like vLLM and TensorRT-LLM to serve 10-100× more requests per GPU.

**Static Batching Limitations:**
- **Batch Completion Wait**: static batching processes fixed batch of sequences; waits for longest sequence to complete; short sequences finish early but GPU idles; wasted computation
- **Length Variation**: real-world requests have 10-100× length variation (10 tokens to 1000+ tokens); batch completion time determined by longest sequence; average utilization 20-40%
- **Example**: batch of 32 sequences, 31 complete in 50 tokens, 1 requires 500 tokens; GPU idles for 31 sequences while processing last sequence; 97% waste
- **Throughput Impact**: low utilization directly reduces throughput; serving 100 requests/sec with 40% utilization could serve 250 requests/sec at 100% utilization

**Continuous Batching Algorithm:**
- **Iteration-Level Batching**: form new batch at each generation step; add newly arrived requests; remove completed sequences; batch size varies dynamically
- **Sequence Lifecycle**: request arrives → added to batch at next step → generates tokens → completes → removed from batch; no waiting for batch completion
- **Memory Management**: allocate memory for each sequence independently; deallocate when sequence completes; no memory waste from completed sequences
- **Scheduling**: priority queue of waiting requests; add highest-priority requests to batch when space available; fair scheduling or priority-based

**Implementation Details:**
- **KV Cache Management**: each sequence has independent KV cache; caches grow/shrink as sequences added/removed; requires dynamic memory allocation
- **Attention Masking**: variable-length sequences in batch require attention masks; each sequence attends only to its own tokens; padding not needed
- **Batch Size Limits**: maximum batch size limited by memory (KV cache + activations); dynamically adjust based on sequence lengths; longer sequences reduce max batch size
- **Prefill vs Decode**: prefill (first token) processes full prompt; decode (subsequent tokens) processes one token; separate batching for prefill and decode improves efficiency

**Performance Improvements:**
- **GPU Utilization**: increases from 20-40% (static) to 60-80% (continuous); 2-4× improvement; directly translates to throughput increase
- **Throughput**: 2-10× higher requests/second depending on length distribution; larger improvement for higher length variation; typical 3-5× in production
- **Latency**: reduces average latency by 30-50%; short sequences don't wait for long sequences; improves user experience; critical for interactive applications
- **Cost Efficiency**: 3-5× more requests per GPU; reduces infrastructure cost by 60-80%; major cost savings for large-scale deployment

**Memory Management:**
- **PagedAttention**: treats KV cache like virtual memory; allocates in fixed-size blocks (pages); enables efficient memory utilization; used in vLLM
- **Block Allocation**: allocate blocks on-demand as sequence grows; deallocate when sequence completes; eliminates fragmentation; achieves 90-95% memory utilization
- **Copy-on-Write**: sequences with shared prefix (e.g., system prompt) share KV cache blocks; only copy when sequences diverge; critical for multi-turn conversations
- **Memory Limits**: maximum concurrent sequences limited by total KV cache memory; dynamically adjust based on sequence lengths; reject requests when memory full

**Scheduling Strategies:**
- **FCFS (First-Come-First-Served)**: simple fair scheduling; add requests in arrival order; easy to implement; may starve long requests
- **Shortest-Job-First**: prioritize requests with shorter expected length; minimizes average latency; requires length prediction; may starve long requests
- **Priority-Based**: assign priorities to requests; serve high-priority first; useful for multi-tenant systems; requires priority mechanism
- **Fair Scheduling**: ensure all requests make progress; prevent starvation; balance throughput and fairness; used in production systems

**Prefill-Decode Separation:**
- **Prefill Batching**: batch multiple prefill requests together; process full prompts in parallel; high memory usage (full prompt activations); limited batch size
- **Decode Batching**: batch decode steps from multiple sequences; process one token per sequence; low memory usage; large batch sizes possible
- **Separate Queues**: maintain separate queues for prefill and decode; schedule independently; optimize for different characteristics; improves overall efficiency
- **Chunked Prefill**: split long prompts into chunks; process chunks like decode steps; reduces memory spikes; enables larger prefill batches

**Framework Implementations:**
- **vLLM**: pioneering continuous batching implementation; PagedAttention for memory management; achieves 10-20× throughput vs naive serving; open-source, production-ready
- **TensorRT-LLM**: NVIDIA's inference framework; continuous batching with optimized CUDA kernels; in-flight batching; highest performance on NVIDIA GPUs
- **Text Generation Inference (TGI)**: Hugging Face's serving framework; continuous batching support; easy deployment; good for diverse models
- **Ray Serve**: distributed serving with continuous batching; scales to multiple nodes; good for large-scale deployment; integrates with Ray ecosystem

**Production Deployment:**
- **Request Routing**: load balancer distributes requests across replicas; each replica runs continuous batching; scales horizontally; handles high request rates
- **Monitoring**: track batch size, utilization, latency, throughput; identify bottlenecks; adjust configuration; critical for optimization
- **Auto-Scaling**: scale replicas based on request rate and latency; continuous batching improves utilization, reduces scaling needs; cost savings
- **Fault Tolerance**: handle failures gracefully; retry failed requests; checkpoint long-running sequences; critical for production reliability

**Advanced Techniques:**
- **Speculative Decoding Integration**: combine continuous batching with speculative decoding; multiplicative speedup; 5-10× total improvement vs naive serving
- **Multi-LoRA Serving**: serve multiple LoRA adapters in same batch; different adapter per sequence; enables multi-tenant serving; critical for customization
- **Quantization**: INT8/INT4 quantization reduces memory; enables larger batches; combined with continuous batching for maximum throughput
- **Prefix Caching**: cache KV for common prefixes (system prompts); share across requests; reduces computation; improves throughput for repetitive prompts

**Use Cases:**
- **Chatbots**: high request rate, variable response length; continuous batching critical for cost-effective serving; 3-5× cost reduction typical
- **Code Completion**: short prompts, variable completion length; benefits from continuous batching; improves latency and throughput
- **Content Generation**: variable-length outputs (summaries, articles); continuous batching prevents long generations from blocking short ones
- **API Serving**: diverse request patterns; continuous batching handles variation efficiently; critical for production API endpoints

**Best Practices:**
- **Batch Size**: set maximum batch size based on memory; monitor actual batch size; adjust based on request patterns; typical max 32-128 sequences
- **Timeout**: set generation timeout to prevent runaway sequences; release resources from timed-out sequences; critical for stability
- **Memory Reservation**: reserve memory for incoming requests; prevents out-of-memory errors; maintain headroom for request spikes
- **Profiling**: profile end-to-end latency; identify bottlenecks (prefill, decode, scheduling); optimize based on measurements

Continuous Batching is **the technique that transformed LLM serving economics** — by eliminating the waste of static batching and dynamically managing sequences, it achieves 2-10× higher throughput and 30-50% lower latency, making large-scale LLM deployment practical and cost-effective for production applications.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/continuous-batching-inference) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
