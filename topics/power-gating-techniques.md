# Power Gating

**Keywords**: power gating techniques,header footer switches,power domain isolation,power gating control,mtcmos multi threshold

---

**Power Gating** is **the power management technique that completely disconnects the power supply from idle logic blocks using high-Vt header or footer switches — reducing leakage power by 10-100× during sleep mode at the cost of wake-up latency, state retention complexity, and switch area overhead, making it essential for battery-powered devices where standby power dominates total energy consumption**.

**Power Gating Architecture:**
- **Header Switches**: PMOS transistors between VDD and virtual VDD (VVDD); when enabled, VVDD ≈ VDD and logic operates normally; when disabled, VVDD floats and logic loses power; header switches preferred for noise isolation (VVDD can be discharged during shutdown)
- **Footer Switches**: NMOS transistors between virtual VSS (VVSS) and VSS; when enabled, VVSS ≈ VSS; when disabled, VVSS floats; footer switches have better on-resistance (NMOS stronger than PMOS) but worse noise isolation
- **Dual Switches**: both header and footer switches for maximum leakage reduction; more complex control but achieves 100× leakage reduction vs 10× for single switch; used for ultra-low-power applications
- **Switch Sizing**: switches must be large enough to supply peak current without excessive IR drop; typical sizing is 1μm switch width per 10-50μm of logic width; under-sizing causes performance degradation; over-sizing wastes area

**Multi-Threshold CMOS (MTCMOS):**
- **High-Vt Switches**: power switches use high-Vt transistors (Vt = 0.5-0.7V) for low leakage when off; 10-100× lower leakage than low-Vt transistors; slower switching but acceptable for power gating (millisecond wake-up time)
- **Low-Vt Logic**: logic uses low-Vt or regular-Vt transistors for high performance; leakage is high but only matters when powered on; MTCMOS combines the benefits of both Vt options
- **Leakage Reduction**: high-Vt switches in series with low-Vt logic create stack effect; total leakage is dominated by switch leakage (10-100× lower than logic leakage); achieves 10-100× total leakage reduction
- **Retention Flip-Flops**: special flip-flops with always-on retention latch; save state before power-down and restore after power-up; enable stateful power gating without software state save/restore

**Power Gating Control:**
- **Control Signals**: power gating controlled by PMU (power management unit) or software; control signals must be on always-on power domain; typical control sequence: isolate outputs → save state → disable switches → (sleep) → enable switches → restore state → de-isolate outputs
- **Switch Sequencing**: large power domains use multiple switch groups enabled sequentially; reduces inrush current (di/dt) that causes supply bounce; typical sequence is 10-100μs per group with 1-10μs delays between groups
- **Acknowledgment Signals**: power domain provides acknowledgment when fully powered up; prevents premature access to partially-powered logic; critical for reliable operation
- **Retention Control**: separate control for retention flip-flops; retention power remains on during sleep; retention control must be asserted before power switches disable

**Isolation Cells:**
- **Purpose**: prevent unknown logic values from propagating from powered-down domain to active domains; unknown values can cause crowbar current or incorrect logic operation
- **Placement**: isolation cells placed at power domain boundaries on all outputs from the gated domain; inputs to gated domain do not require isolation (powered-down logic does not drive)
- **Isolation Value**: isolation cell clamps output to known value (0 or 1) when domain is powered down; isolation value chosen to minimize power in receiving logic (typically 0 for NAND/NOR, 1 for AND/OR)
- **Timing**: isolation must be enabled before power switches disable and disabled after power switches enable; incorrect sequencing causes glitches or contention

**Wake-Up and Inrush Current:**
- **Wake-Up Latency**: time from enable signal to domain fully operational; includes switch turn-on (1-10μs), voltage ramp (10-100μs), and state restore (1-100μs); total latency 10μs-10ms depending on domain size and retention strategy
- **Inrush Current**: when switches enable, domain capacitance charges rapidly; peak current can be 10-100× normal operating current; causes supply voltage droop and ground bounce
- **Inrush Mitigation**: sequential switch enable (reduces peak current), series resistance in switches (slows charging), or active current limiting (feedback control); trade-off between wake-up time and supply noise
- **Power Grid Impact**: power grid must be sized for inrush current; decoupling capacitors near power switches absorb inrush; inadequate grid causes voltage droop affecting active domains

**Implementation Flow:**
- **Power Intent (UPF/CPF)**: specify power domains, switch cells, isolation cells, and retention cells in Unified Power Format (UPF) or Common Power Format (CPF); power intent drives synthesis, placement, and verification
- **Synthesis**: logic synthesis with power-aware libraries; insert isolation cells, retention flip-flops, and level shifters; optimize for leakage in addition to timing and area
- **Placement**: place power switches in rows near domain boundary; minimize switch-to-logic distance (reduces IR drop); place isolation and level shifter cells at domain boundaries
- **Verification**: simulate power-up/power-down sequences; verify isolation timing, state retention, and inrush current; Cadence Voltus and Synopsys PrimePower provide power-aware verification

**Advanced Power Gating Techniques:**
- **Fine-Grain Power Gating**: gate individual functional units (ALU, multiplier) rather than large blocks; reduces wake-up latency and improves power efficiency; requires more switches and control complexity
- **Adaptive Power Gating**: dynamically adjust power gating thresholds based on workload; machine learning predicts idle periods and triggers power gating; 10-30% additional power savings vs static thresholds
- **Partial Power Gating**: gate only a portion of a domain (e.g., 50% of switches); reduces leakage by 5-10× with faster wake-up; used for short idle periods where full power gating overhead is not justified
- **Distributed Switches**: place switches within logic rather than at domain boundary; reduces IR drop and improves current distribution; complicates layout but improves performance

**Power Gating Metrics:**
- **Leakage Reduction**: ratio of leakage power with and without power gating; typical values are 10-100× depending on switch Vt and logic leakage; measured at worst-case leakage corner (high temperature, high voltage)
- **Area Overhead**: switches, isolation cells, and retention flip-flops add 5-20% area; larger domains have lower overhead (switch area amortized over more logic)
- **Performance Impact**: IR drop across switches reduces effective supply voltage; typical impact is 5-15% frequency degradation; mitigated by adequate switch sizing
- **Break-Even Time**: minimum idle time for power gating to save energy (accounting for wake-up energy cost); typical break-even is 10μs-10ms; shorter idle periods use clock gating instead

**Advanced Node Considerations:**
- **Increased Leakage**: 7nm/5nm nodes have 10-100× higher leakage than 28nm; power gating becomes essential even for performance-oriented designs
- **FinFET Advantages**: FinFET high-Vt devices have 10× lower leakage than planar high-Vt; enables more aggressive power gating with lower switch area
- **Voltage Scaling**: power gating combined with voltage scaling (0.7V sleep, 1.0V active) provides additional power savings; requires level shifters and more complex control
- **3D Integration**: through-silicon vias (TSVs) enable per-die power gating in stacked chips; reduces power delivery challenges and improves granularity

Power gating is **the most effective leakage reduction technique for idle logic — by completely disconnecting power, it achieves orders-of-magnitude leakage reduction that no other technique can match, making it indispensable for mobile and IoT devices where battery life depends on minimizing standby power consumption**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/power-gating-techniques) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
