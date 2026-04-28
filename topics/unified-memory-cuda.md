# Unified Memory

**Keywords**: unified memory cuda,managed memory allocation,page migration gpu,prefetching unified memory,memory oversubscription

---

**Unified Memory** is **the CUDA programming model that provides a single memory address space accessible from both CPU and GPU — automatically migrating data between host and device on-demand through page faulting, eliminating explicit cudaMemcpy calls and enabling memory oversubscription (using more GPU memory than physically available), simplifying development while achieving 70-95% of manual memory management performance when properly optimized with prefetching and usage hints**.

**Unified Memory Fundamentals:**
- **Allocation**: cudaMallocManaged(&ptr, size); allocates memory accessible from CPU and GPU; returns single pointer valid on both; replaces separate cudaMalloc() + cudaMallocHost() + cudaMemcpy() workflow
- **Automatic Migration**: on first access from CPU or GPU, page fault triggers migration; 4 KB pages transferred on-demand; subsequent accesses to same page are local (no migration); hardware page fault mechanism (Pascal+) or software migration (pre-Pascal)
- **Coherence**: modifications on CPU visible to GPU and vice versa; coherence maintained through migration and invalidation; no explicit synchronization required for correctness (but may be needed for performance)
- **Oversubscription**: allocate more managed memory than GPU capacity; inactive pages reside in host memory; active pages migrate to GPU; enables processing datasets larger than GPU memory without manual chunking

**Page Migration and Faulting:**
- **Hardware Page Faulting (Pascal+)**: GPU generates page fault on access to non-resident page; page migrated from host to device; fault handled transparently; ~10-50 μs latency per fault
- **Fault Granularity**: 4 KB pages (64 KB on some systems); accessing single byte migrates entire page; spatial locality improves efficiency; random access causes excessive faulting
- **Thrashing**: when working set exceeds GPU memory, pages migrate back and forth; severe performance degradation (10-100× slowdown); use prefetching or explicit memory management to avoid
- **Eviction**: when GPU memory full, least-recently-used pages evicted to host; eviction is asynchronous (doesn't block kernel); but subsequent access causes fault and migration

**Prefetching and Hints:**
- **Prefetch API**: cudaMemPrefetchAsync(ptr, size, device, stream); explicitly migrates pages to device before access; eliminates page faults; achieves near-manual-copy performance
- **Prefetch Pattern**: cudaMemPrefetchAsync(data, size, gpuId, stream); kernel<<<..., stream>>>(); — prefetch overlaps with previous kernel; data ready when kernel starts; zero fault overhead
- **CPU Prefetch**: cudaMemPrefetchAsync(ptr, size, cudaCpuDeviceId, stream); migrates data back to CPU; useful before CPU processing phase; avoids faults on CPU access
- **Advice API**: cudaMemAdvise(ptr, size, cudaMemAdviseSetReadMostly, device); hints that data is read-only; enables replication (copies on multiple GPUs) instead of migration; reduces migration overhead for shared read-only data

**Memory Advice Flags:**
- **cudaMemAdviseSetReadMostly**: data is read-only or rarely modified; enables replication across devices; multiple GPUs can access without migration; ideal for model weights, lookup tables
- **cudaMemAdviseSetPreferredLocation**: sets preferred residence (CPU or specific GPU); pages migrate to preferred location when not actively used; reduces migration overhead for data with clear affinity
- **cudaMemAdviseSetAccessedBy**: indicates which devices will access the data; enables direct access over NVLink/PCIe without migration; useful for multi-GPU with high-bandwidth interconnect
- **cudaMemAdviseUnsetReadMostly**: reverts read-mostly behavior; necessary before modifying data; otherwise modifications may not propagate correctly

**Performance Optimization:**
- **Prefetch Everything**: for predictable access patterns, prefetch all data before kernel launch; eliminates page faults entirely; achieves 90-95% of manual cudaMemcpy performance
- **Batch Prefetching**: prefetch multiple allocations in single stream; overlaps migration with compute; cudaMemPrefetchAsync(A, ...); cudaMemPrefetchAsync(B, ...); kernel<<<...>>>(); — both A and B migrate concurrently
- **Read-Only Data**: use cudaMemAdviseSetReadMostly for weights, constants; enables zero-copy access from multiple GPUs; eliminates migration overhead for shared data
- **Structured Access**: access memory in large contiguous chunks; improves page fault batching; random access causes one fault per page; sequential access amortizes fault overhead

**Multi-GPU Unified Memory:**
- **Peer Access**: with NVLink, GPUs can directly access each other's memory; cudaMemAdviseSetAccessedBy enables direct access; avoids migration through host memory; achieves 50-300 GB/s bandwidth (NVLink) vs 16-32 GB/s (PCIe)
- **Replication**: read-only data replicated on all GPUs; each GPU has local copy; zero migration overhead; ideal for model parameters in data-parallel training
- **Concurrent Access**: multiple GPUs can access same managed memory; coherence maintained automatically; enables shared data structures without explicit synchronization
- **Preferred Location**: set preferred location to GPU with highest access frequency; other GPUs access over NVLink; balances migration overhead with access latency

**Limitations and Trade-offs:**
- **Fault Overhead**: page faults cost 10-50 μs each; 1 GB data = 256K pages; without prefetching, 2.5-12 seconds of fault overhead; prefetching is essential for performance
- **Atomics**: atomic operations on managed memory may be slower than device memory; atomics across CPU-GPU require coherence protocol overhead; use device-local atomics when possible
- **Debugging Complexity**: memory errors may manifest as page faults; harder to debug than explicit copy failures; use cuda-memcheck and nsight compute for diagnosis
- **Pascal+ Required**: hardware page faulting requires Pascal or newer; pre-Pascal uses software migration with higher overhead; check compute capability before relying on unified memory

**Use Cases:**
- **Rapid Prototyping**: eliminate explicit memory management during development; add prefetching for production; reduces development time by 30-50%
- **Irregular Access Patterns**: graph algorithms, sparse matrices with unpredictable access; unified memory handles migration automatically; manual management would require complex logic
- **Memory Oversubscription**: process 100 GB dataset on 40 GB GPU; unified memory pages in/out automatically; enables large-scale processing without manual chunking
- **Multi-GPU Sharing**: shared data structures across GPUs; unified memory handles coherence; simplifies multi-GPU programming

**Performance Comparison:**
- **With Prefetching**: 90-95% of manual cudaMemcpy performance; <5% overhead from page table management; acceptable for most applications
- **Without Prefetching**: 10-50% of manual performance; page fault overhead dominates; only acceptable for irregular access patterns where prefetching is impossible
- **Oversubscription**: 5-20% of in-memory performance; depends on working set size and access pattern; acceptable when alternative is out-of-core processing

Unified Memory is **the productivity-enhancing feature that simplifies CUDA programming by eliminating explicit memory management — when combined with strategic prefetching and memory advice, it achieves near-optimal performance while providing automatic data migration, memory oversubscription, and simplified multi-GPU programming, making it the preferred memory model for modern CUDA applications**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/unified-memory-cuda) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
