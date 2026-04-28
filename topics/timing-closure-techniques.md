# Timing Closure

**Keywords**: timing closure techniques,setup hold fixing,timing optimization methods,useful skew timing,timing margin recovery

---

**Timing Closure** is **the iterative physical design process of achieving timing slack ≥ 0 for all paths across all operating corners and modes — employing a combination of logic restructuring, gate sizing, buffer insertion, placement optimization, and clock skew scheduling to meet the target frequency while managing power and area trade-offs**.

**Timing Fundamentals:**
- **Setup Timing**: data must arrive at the capture flip-flop before the clock edge minus setup time; setup slack = T_clk - (T_launch_clk + T_logic + T_setup - T_capture_clk); negative slack means the path is too slow and violates timing at the target frequency
- **Hold Timing**: data must remain stable for hold time after the clock edge; hold slack = (T_launch_clk + T_logic) - (T_capture_clk + T_hold); hold violations occur when data changes too quickly, typically at fast corners; cannot be fixed by frequency reduction
- **Critical Path**: the path with the worst (most negative) slack; determines maximum achievable frequency; timing closure focuses on fixing critical and near-critical paths (slack < 100ps) first
- **Timing Corners**: designs must meet timing across all PVT corners (slow-slow for setup, fast-fast for hold, typical for power); multi-corner optimization simultaneously considers all corners; advanced nodes require 10-20 corner analysis

**Gate-Level Optimization:**
- **Gate Sizing (Upsizing)**: replace cells with higher drive strength versions to reduce delay; increases area and power but improves timing; automated sizing tools (Synopsys Design Compiler, Cadence Genus) upsize gates on critical paths while downsizing non-critical paths to recover area
- **Threshold Voltage (Vt) Swapping**: use low-Vt cells (faster, higher leakage) on critical paths and high-Vt cells (slower, lower leakage) on non-critical paths; multi-Vt optimization balances performance and leakage power; typical mix: 10-20% LVT, 60-70% RVT, 10-20% HVT
- **Buffer Insertion**: add buffers to split long wires and reduce RC delay; buffers also restore signal slew; optimal buffer insertion considers wire delay, buffer delay, and downstream load; Synopsys and Cadence tools use dynamic programming for optimal buffer placement
- **Logic Restructuring**: resynthesize critical paths with different logic structures; transform carry chains, rebalance trees, clone high-fanout gates; typically done early in physical synthesis before placement is locked

**Physical Optimization:**
- **Placement Optimization**: move cells closer to reduce wire length and delay; critical path cells placed in proximity with minimal detours; incremental placement refinement after each timing iteration; modern tools use analytical placement with timing-driven objectives
- **Routing Optimization**: minimize wire length on critical nets; use wider wires (lower resistance) or upper metal layers (lower RC) for critical paths; non-default routing rules (NDR) specify wider/spaced routing for specific nets
- **Useful Skew**: intentionally delay clock arrival to launching flip-flops relative to capturing flip-flops on critical paths; borrows time from the next cycle; can recover 5-15% frequency; Cadence Innovus and Synopsys ICC2 support automated useful skew optimization
- **Pipelining**: insert flip-flops to break long combinational paths into multiple shorter paths; increases latency but improves throughput and maximum frequency; requires RTL changes but provides the largest timing improvement (2-3× frequency possible)

**Hold Fixing:**
- **Delay Cell Insertion**: add delay buffers or delay cells on paths with hold violations; increases delay without affecting setup timing (if done carefully); hold fixing typically adds 2-5% area overhead
- **Clock Skew Adjustment**: reduce clock skew or reverse skew direction to fix hold violations; must ensure setup timing is not degraded; useful skew optimization considers both setup and hold simultaneously
- **Minimum Delay Paths**: hold violations often occur on short paths between nearby flip-flops; fixing requires adding delay without impacting critical setup paths; automated hold fixing tools insert minimum-sized delay cells
- **Fast Corner Challenges**: hold violations worsen at fast-fast corner (low Vt, high voltage, low temperature); must ensure hold slack ≥ 0 at fast corner while maintaining setup slack ≥ 0 at slow corner; conflicting requirements make hold fixing challenging

**Advanced Techniques:**
- **Concurrent Clock and Data (CCD) Optimization**: co-optimizes clock tree and data paths simultaneously; adjusts clock arrival times and data path delays together for optimal timing; more effective than sequential CTS followed by timing optimization
- **Path-Based Analysis (PBA)**: analyzes each path individually with path-specific slew and delay values rather than using pessimistic worst-case values; recovers 50-200ps of timing margin by removing pessimism; essential for timing closure at advanced nodes
- **Physically Aware Synthesis**: performs logic synthesis with physical information (estimated placement and routing); reduces the gap between synthesis and physical implementation; Synopsys Fusion Compiler and Cadence Genus iSpatial provide physical synthesis
- **Machine Learning Timing Prediction**: ML models predict post-route timing from early design stages; enables faster design space exploration and reduces timing closure iterations; emerging capability in commercial EDA tools

**Timing Closure Metrics:**
- **Worst Negative Slack (WNS)**: the most negative slack across all paths; primary metric for timing closure; target is WNS ≥ 0 with margin (typically +50ps to +100ps for design margin and variation)
- **Total Negative Slack (TNS)**: sum of all negative slacks; indicates the overall timing health; TNS = 0 means all paths meet timing; large TNS indicates many failing paths requiring extensive optimization
- **Number of Violating Paths**: count of paths with negative slack; complements TNS by showing how widespread timing issues are; 1000 paths with -10ps each is easier to fix than 10 paths with -1000ps each
- **Timing Margin**: positive slack beyond zero; provides margin for process variation, aging, and design changes; typical target margin is 5-10% of clock period (50-100ps at 1GHz)

Timing closure is **the most time-consuming and iterative phase of physical design — consuming 40-60% of implementation schedule at advanced nodes, requiring deep expertise in timing analysis, optimization techniques, and tool flows to achieve the target frequency while meeting power and area constraints**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/timing-closure-techniques) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
