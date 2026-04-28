# GPU Profiling and Debugging

**Keywords**: gpu profiling debugging,nsight compute profiling,nsight systems timeline,cuda profiling tools,gpu performance analysis

---

**GPU Profiling and Debugging** is **the systematic analysis of GPU application performance and correctness using specialized tools that provide detailed metrics, timeline visualization, and error detection** — where NVIDIA Nsight Compute delivers kernel-level analysis with 1000+ metrics covering memory bandwidth (achieved vs peak 1.5-3 TB/s), compute throughput (achieved vs peak 20-80 TFLOPS), occupancy (50-100%), and warp efficiency (target >90%), while Nsight Systems provides system-wide timeline showing CPU-GPU interaction, kernel launches, memory transfers, and API calls, enabling developers to identify bottlenecks (memory-bound, compute-bound, latency-bound), optimize resource utilization, and achieve 2-10× performance improvement through data-driven optimization, making profiling essential for GPU development where intuition often misleads and measurement is the only path to understanding actual performance characteristics.


**Nsight Compute (Kernel Profiling):**
- **Purpose**: detailed single-kernel analysis; 1000+ metrics; memory, compute, occupancy, warp efficiency; identifies kernel bottlenecks
- **Launch**: ncu ./app or ncu --set full ./app; GUI: ncu-ui; command-line or graphical interface
- **Metrics**: Memory Throughput (GB/s), Compute Throughput (TFLOPS), SM Efficiency (%), Occupancy (%), Warp Execution Efficiency (%), Branch Efficiency (%)
- **Sections**: Memory Workload Analysis, Compute Workload Analysis, Launch Statistics, Occupancy, Scheduler Statistics, Warp State Statistics

**Nsight Systems (System Profiling):**
- **Purpose**: system-wide timeline; CPU-GPU interaction; kernel launches, memory transfers, API calls; identifies system-level bottlenecks
- **Launch**: nsys profile ./app or nsys profile --trace=cuda,nvtx ./app; generates .qdrep file; open in nsys-ui
- **Timeline View**: visualizes all GPU activity; shows overlaps, gaps, synchronization points; identifies idle time
- **Use Cases**: multi-GPU profiling, stream concurrency, CPU-GPU overlap, kernel launch overhead, memory transfer analysis

**Memory Profiling:**
- **Memory Throughput**: achieved bandwidth / peak bandwidth; target 80-100% for memory-bound kernels; A100: 1.5-2 TB/s, H100: 2-3 TB/s
- **Memory Replay**: indicates uncoalesced access; replay >1.5 means poor coalescing; restructure data layout
- **L1/L2 Hit Rate**: cache effectiveness; high hit rate (>80%) good for reused data; low hit rate indicates streaming access
- **Global Load/Store Efficiency**: percentage of useful bytes loaded; low efficiency (<50%) indicates wasted bandwidth; improve coalescing
- **Bank Conflicts**: shared memory bank conflicts; high conflicts (>10%) cause serialization; add padding or change access pattern

**Compute Profiling:**
- **Compute Throughput**: achieved TFLOPS / peak TFLOPS; target 50-80% for compute-bound kernels; A100: 19.5 TFLOPS FP32, 312 TFLOPS FP16
- **SM Efficiency**: percentage of time SMs are active; target 80-100%; low efficiency indicates insufficient work or poor scheduling
- **Tensor Core Utilization**: percentage of time Tensor Cores active; target 50-80% for matrix operations; 312 TFLOPS on A100
- **IPC (Instructions Per Cycle)**: instructions executed per cycle; higher is better; target 2-4 for well-optimized kernels

**Occupancy Analysis:**
- **Achieved Occupancy**: percentage of maximum warps active; target 50-100%; higher occupancy hides latency
- **Theoretical Occupancy**: maximum possible based on resource usage; limited by registers, shared memory, block size
- **Occupancy Limiter**: identifies limiting factor (registers, shared memory, block size); guides optimization
- **Occupancy Calculator**: CUDA Occupancy Calculator spreadsheet; predicts occupancy from resource usage; useful for tuning

**Warp Efficiency:**
- **Warp Execution Efficiency**: percentage of active threads in executed warps; target >90%; low efficiency indicates divergence
- **Branch Efficiency**: percentage of branches without divergence; target >90%; divergent branches cause serialization
- **Predication Efficiency**: percentage of instructions not predicated off; target >90%; high predication indicates divergence
- **Optimization**: minimize divergence; use ballot/shuffle for divergent code; restructure algorithms

**Roofline Model:**
- **Concept**: plots achieved performance vs arithmetic intensity; shows whether memory-bound or compute-bound
- **Memory Roof**: horizontal line at peak memory bandwidth; memory-bound kernels hit this ceiling
- **Compute Roof**: diagonal line at peak compute throughput; compute-bound kernels hit this ceiling
- **Optimization**: move toward upper-right (higher intensity, higher performance); tiling increases intensity

**Timeline Analysis:**
- **Kernel Gaps**: idle time between kernels; indicates launch overhead or synchronization; use streams to overlap
- **Memory Transfer Gaps**: idle time during transfers; use async transfers and streams; overlap with compute
- **CPU-GPU Sync**: cudaDeviceSynchronize() causes gaps; minimize synchronization; use events for fine-grained control
- **Multi-GPU**: visualize cross-GPU communication; identify load imbalance; optimize data distribution

**NVTX Markers:**
- **Purpose**: annotate code regions; shows in Nsight Systems timeline; helps identify bottlenecks in application logic
- **API**: nvtxRangePush("label"), nvtxRangePop(); marks code regions; nvtxMark("event") for single events
- **Use Cases**: mark training iterations, data loading, preprocessing, inference; correlate with GPU activity
- **Best Practice**: annotate all major code sections; hierarchical markers; color-code by category

**Debugging Tools:**
- **cuda-memcheck**: detects memory errors; out-of-bounds access, race conditions, uninitialized memory; run with cuda-memcheck ./app
- **Compute Sanitizer**: newer tool replacing cuda-memcheck; more features; memcheck, racecheck, initcheck, synccheck modes
- **CUDA_LAUNCH_BLOCKING=1**: serializes all operations; easier debugging; disables async; use only for debugging
- **cuda-gdb**: command-line debugger; breakpoints, watchpoints, inspect variables; cuda-gdb ./app

**Performance Metrics:**
- **Achieved Bandwidth**: GB/s of memory traffic; compare to peak (1.5-3 TB/s); target 80-100% for memory-bound
- **Achieved TFLOPS**: floating-point operations per second; compare to peak (20-80 TFLOPS); target 50-80% for compute-bound
- **Kernel Time**: total kernel execution time; identify slow kernels; focus optimization efforts
- **Launch Overhead**: time between kernel launches; target <1% of total time; use CUDA Graphs to reduce

**Bottleneck Identification:**
- **Memory-Bound**: <50% compute throughput, high memory throughput; optimize memory access patterns, use shared memory, reduce accesses
- **Compute-Bound**: <50% memory throughput, high compute throughput; use Tensor Cores, increase ILP, reduce divergence
- **Latency-Bound**: low occupancy, low throughput; increase occupancy, reduce register usage, increase block size
- **Instruction-Bound**: high instruction overhead; reduce branches, use warp primitives, optimize control flow

**Optimization Workflow:**
- **Profile**: run Nsight Compute and Nsight Systems; identify bottleneck (memory, compute, latency)
- **Analyze**: examine relevant metrics; memory throughput, compute throughput, occupancy, warp efficiency
- **Optimize**: apply targeted optimizations; memory coalescing, shared memory, occupancy tuning, divergence reduction
- **Measure**: re-profile; verify improvement; compare metrics before and after
- **Iterate**: repeat for next bottleneck; diminishing returns after 3-5 iterations; 2-10× total speedup typical

**Common Profiling Patterns:**
- **Baseline**: profile unoptimized code; establish baseline metrics; identify major bottlenecks
- **Incremental**: optimize one aspect at a time; measure impact; easier to attribute improvements
- **Comparison**: compare against reference implementation (cuBLAS, cuDNN); identify gaps; target 80-95% of library performance
- **Regression**: profile after code changes; detect performance regressions; maintain performance over time

**Multi-GPU Profiling:**
- **Nsight Systems**: visualizes all GPUs simultaneously; shows cross-GPU communication; identifies load imbalance
- **NCCL Profiling**: NCCL_DEBUG=INFO shows communication details; bandwidth, latency, algorithm selection
- **Per-GPU Metrics**: profile each GPU separately; identify stragglers; optimize slowest GPU first
- **Scaling Analysis**: measure scaling efficiency; compare 1 GPU vs N GPUs; target 80-95% efficiency

**Advanced Profiling:**
- **Sampling**: sample-based profiling for long-running applications; lower overhead; nsys profile --sample=cpu,cuda
- **Metrics Collection**: collect specific metrics; ncu --metrics sm__throughput.avg.pct_of_peak_sustained_elapsed; reduces overhead
- **Kernel Replay**: replay kernel with different configurations; find optimal launch parameters; ncu --launch-count 1 --replay-mode kernel
- **Source Correlation**: correlate metrics with source code; identify hot spots; ncu --source-level-analysis

**Performance Targets:**
- **Memory Bandwidth**: 80-100% of peak (1.5-3 TB/s); coalesced access, minimal bank conflicts
- **Compute Throughput**: 50-80% of peak (20-80 TFLOPS); use Tensor Cores, high ILP, minimal divergence
- **Occupancy**: 50-100%; balance register and shared memory usage; 256 threads per block typical
- **Warp Efficiency**: >90%; minimize divergence; uniform control flow
- **Kernel Time**: <1ms for small kernels, <100ms for large; longer kernels risk timeout; split if necessary

**Best Practices:**
- **Profile Early**: profile from the start; avoid premature optimization but measure early; establish baseline
- **Profile Often**: profile after each optimization; verify improvement; catch regressions
- **Use Both Tools**: Nsight Compute for kernel details, Nsight Systems for system view; complementary insights
- **Focus on Bottlenecks**: optimize slowest kernels first; 80/20 rule applies; 20% of kernels often account for 80% of time
- **Measure, Don't Guess**: intuition often wrong; always measure; data-driven optimization

**Common Mistakes:**
- **Optimizing Wrong Thing**: optimizing fast kernels instead of slow ones; profile to identify bottlenecks
- **Ignoring Occupancy**: assuming higher occupancy always better; balance with resource usage; profile to find optimal
- **Over-Optimizing**: diminishing returns after 3-5 iterations; 2-10× total speedup typical; know when to stop
- **Not Profiling**: relying on intuition; guessing bottlenecks; always measure actual performance

**Real-World Impact:**
- **Matrix Multiplication**: profiling reveals 20% of peak; optimization achieves 80-95% of peak; 4-5× speedup
- **Reduction**: profiling shows bank conflicts; optimization eliminates conflicts; 2-3× speedup; 60-80% of peak
- **Convolution**: profiling reveals memory-bound; shared memory tiling achieves 70-90% of peak; 5-10× speedup
- **Custom Kernels**: profiling guides optimization; 2-10× improvement typical; achieves 50-80% of peak

GPU Profiling and Debugging represent **the essential tools for GPU performance optimization** — by providing detailed metrics, timeline visualization, and error detection through Nsight Compute and Nsight Systems, developers identify bottlenecks, optimize resource utilization, and achieve 2-10× performance improvement through data-driven optimization, making profiling the difference between GPU code that achieves 10% or 80% of theoretical peak performance where measurement is the only path to understanding actual performance characteristics and intuition often misleads.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/gpu-profiling-debugging) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
