# Hardware Performance Monitoring: PMU Access and Analysis — performance counter instrumentation revealing CPU behavior (cache, branch prediction, instruction-level parallelism) guiding optimization

**Keywords**: hardware performance counter monitoring,perf linux profiling,vtune profiler intel,papi performance api,performance monitoring unit pmu

---

**Hardware Performance Monitoring: PMU Access and Analysis — performance counter instrumentation revealing CPU behavior (cache, branch prediction, instruction-level parallelism) guiding optimization**

**CPU Performance Counters**
- **Cycle Count**: clock cycles elapsed (basic metric, used to normalize other counters)
- **Instruction Count**: total instructions executed, IPC = instructions/cycles (>1 indicates parallelism, <1 indicates stalls)
- **Cache Misses**: L1/L2/L3 cache misses per 1000 instructions, high misses indicate memory bottleneck
- **Branch Mispredictions**: incorrect branch predictions, stall pipeline (15-20 cycle penalty typical)
- **Specialized**: floating-point ops, vector operations, SIMD utilization, page faults

**Top-Down Microarchitecture Analysis (TMA)**
- **Frontend/Backend Stalls**: categorize cycles where CPU stalled (frontend: fetch not available, backend: execution blocked)
- **Bad Speculation**: cycles wasted on mispredicted branches or speculative execution
- **Retiring**: cycles spent on useful work (committed instructions)
- **Implication**: identifies where optimization effort should focus (frontend vs backend vs speculation)

**Linux perf Tool**
- **perf stat**: measure counters for single run (``perf stat ./program'), output avg/total counts
- **perf record**: record counter data during execution (``perf record -e cycles,cache-misses ./program'), generates data.perf
- **perf report**: analyze recorded data (``perf report'), flame graph shows hot functions
- **CPU Event Selection**: vendor-specific (Intel: UOPS_ISSUED, AMD: DISPATCH0_STALLS), requires knowledge of ISA

**PAPI (Performance Application Programming Interface)**
- **Portable API**: abstract performance counter names (PAPI_L1_DCM = L1 data cache miss, works on Intel/AMD/ARM)
- **C Library**: ``#include <papi.h>', call PAPI_start_counters(), PAPI_read_counters(), PAPI_stop_counters()
- **Preset Events**: pre-defined events (PAPI_FP_OPS floating-point ops), user-friendly vs raw PMU events
- **Group Recording**: measure multiple counters simultaneously (hardware limit: typically 4-8 concurrent counters)

**Intel VTune Profiler**
- **GUI Interface**: graphical analysis (vs CLI perf), intuitive timeline visualization
- **Multiple Modes**: sampling (record every N cycles), tracing (record all events), metrics (compute derived metrics)
- **Hotspot Analysis**: identifies functions consuming most time, drill-down to lines of code
- **System-Wide**: profile entire system (all processes), identify unexpected CPU utilization
- **License**: commercial (Intel, part of oneAPI toolkit), free for limited academic use

**AMD uProf**
- **AMD Equivalent**: similar to Intel VTune, optimized for AMD EPYC/Ryzen
- **Features**: instruction-based sampling, memory analysis (cache coherency, interconnect)
- **Integration**: Linux perf compatibility (can import perf data)
- **Cost**: free for AMD customers

**NVIDIA Nsight (GPU Profiling)**
- **GPU Performance**: kernel occupancy (how many thread blocks executing), memory throughput (coalescing)
- **Warp Divergence**: GPU threads (in same warp) diverge (take different branches), serializes execution
- **Memory Analysis**: global memory coalescing (contiguous access efficient), local memory usage
- **Timeline**: GPU timeline synchronized with CPU timeline (overall system view)

**PMU (Performance Monitoring Unit) Programming**
- **Linux Perf Events**: perf_event_open() syscall, configure which counter to measure, attach to process/CPU
- **Counter Multiplexing**: hardware limit (N concurrent counters), OS time-multiplexes if more requested
- **Ring Buffers**: kernel maintains buffer (overflows discard oldest), user-space reads periodically
- **Permissions**: typical users require elevated privileges (sysctl perf_event_paranoid), or system admin grant access

**Performance Baseline and Comparison**
- **Baseline Measurement**: profile unoptimized code (establish starting point), track improvements over iterations
- **A/B Testing**: compare two code variants (perf stat -c program_v1, program_v2), identify faster version
- **Statistical Significance**: multiple runs (10+), report mean/stddev, account for variance from system noise

**Flame Graphs and Visualization**
- **Flame Graph**: horizontal bars represent function call stack (height = stack depth), width = time spent
- **Hot Paths**: wide functions indicate hot spots (candidates for optimization)
- **Color**: typically hue indicates thread, saturation indicates issue type (stalls, cache misses)
- **Tool**: brendangregg/FlameGraph (convert perf output to svg visualization)

**Cache Analysis and Optimization**
- **L1/L2/L3 Miss Rates**: compute miss/hit ratio per level, guide prefetch/memory layout optimization
- **Cache Associativity**: capacity misses (conflict misses) if data patterns don't align with cache structure
- **Working Set**: estimate how much memory actively used (vs cold data), if >cache capacity: memory bottleneck
- **Prefetch Hints**: software hints (PREFETCH instruction) or hardware prefetchers (predictive)

**Branch Prediction and Speculation**
- **Misprediction Rate**: percentage of branches mispredicted, target <2-3% (modern predictors ~98%+ accuracy)
- **Penalty**: misprediction costs 15-25 cycles (pipeline flush), sum mispredictions: significant performance loss
- **Optimization**: reduce branches (loop unrolling, predicated execution), improve prediction (data-dependent branches difficult)

**Scaling to Many Cores**
- **Per-Core Counters**: all cores generate performance data (N cores = N counter streams)
- **Aggregation**: typically average/sum across cores, but per-core analysis useful (load imbalance detection)
- **Storage**: sampling rates ~1000 Hz typical (per core), 1000 cores = 1M events/sec (significant I/O)

**Online vs Offline Analysis**
- **Online**: analyze performance during run (adjust knobs if needed), requires minimal overhead
- **Offline**: post-mortem analysis (full data capture), enables detailed study but too late for adjustment
- **Hybrid**: profile phase (collect data), optimize phase (modify code), repeat

**Future Tools and Emerging Standards**
- **OpenTelemetry**: standard for observability (logs, metrics, traces), HPC adoption emerging
- **eBPF**: kernel event collection (low overhead), emerging alternative to perf (tools like bcc)
- **Machine Learning**: automatic anomaly detection (profiler identifies unexpected behavior, alerts user)

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/hardware-performance-counter-monitoring) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
