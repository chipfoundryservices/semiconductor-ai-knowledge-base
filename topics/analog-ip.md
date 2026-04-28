# Analog IP Design (Op-Amp)

**Keywords**: analog ip,op-amp,two stage op amp,gain bandwidth,miller compensation,cmrr psrr

---

**Analog IP Design (Op-Amp)** is the **design of operational amplifiers — multi-stage designs optimized for gain, bandwidth, power, area — enabling precision sensing, signal processing, and power management across analog and mixed-signal systems**. Op-amps are fundamental analog building blocks.

**Two-Stage Miller-Compensated Op-Amp**
Standard op-amp architecture: (1) differential input stage (pair of transistors, high impedance input, low noise), (2) second stage (common-source amplifier, high gain), (3) output stage (rail-to-rail buffer, high current drive). Two-stage design balances: (1) simplicity (fewer stages, smaller area, lower power), (2) gain (two stages provide reasonable gain, >100 V/V typical), (3) bandwidth (2-stage can achieve >1 MHz bandwidth). Miller compensation uses capacitor C_c in negative feedback from second stage output to first stage output, creating dominant pole at first stage. Benefits: (1) stabilizes feedback loop (introduces phase margin), (2) lowers closed-loop bandwidth (limited by dominant pole, ~f_p = GBW / DC_gain), enabling stability.

**Gain Calculation and Design**
DC gain is product of stage gains: A_v = gm1×Ro1 × gm2×Ro2, where gm = transconductance (input-output current gain), Ro = output impedance. Gain is high (~100-1000 V/V, 40-60 dB) but limited by: (1) technology — higher Vt, lower gm; (2) power budget — higher gm requires more bias current, more power; (3) load — higher load capacitance reduces Ro, reduces gain. Design goal: achieve target gain with minimum power (lowest bias current). Trade-off: lower bias current reduces gm and gain; higher bias current improves gain but increases power consumption.

**Gain-Bandwidth Product (GBW)**
GBW = DC_gain × bandwidth, a figure-of-merit. GBW is set by compensation capacitor C_c: GBW ≈ gm1 / (2π × C_c). Higher GBW requires: (1) larger gm1 (higher bias), or (2) smaller C_c (less compensation, risk of instability). Typical GBW: 1-10 MHz (precision op-amps), 100 MHz-1 GHz (fast op-amps). GBW is fundamental limit: cannot increase gain and bandwidth simultaneously (higher gain means lower bandwidth, and vice-versa). Design specifies GBW, then optimization minimizes power for given GBW.

**Phase Margin and Stability**
Phase margin is phase difference between gain and -180° at unity-gain frequency. Phase margin >60° ensures stability (low ringing, no oscillation). Miller compensation creates dominant pole at low frequency (stabilizing), leading to -20 dB/decade rolloff, reaching unity gain at frequency f_UG = GBW. Phase margin at f_UG depends on second pole location: lower second pole (higher bandwidth) causes earlier phase drop (lower margin, risk of instability). Design goal: phase margin >60°, achieved by placing second pole above 10-100x f_UG (frequency separation).

**CMRR and PSRR**
CMRR (common-mode rejection ratio): ratio of differential gain to common-mode gain. Common-mode signal (same signal on both inputs) should have zero output; finite CMRR means slight output ripple. Causes: (1) mismatch in input pair (W/L, Vth), (2) tail current variation (input-stage tail is biased, not infinite impedance). CMRR target >80 dB (gain error <0.01 V/V for common-mode input). PSRR (power supply rejection ratio): ratio of open-loop gain to supply-induced output change. When Vdd varies, output shifts slightly (PSRR finite). Causes: (1) Early effect (Vdd variation shifts bias points, changes gm/Ro), (2) substrate coupling. PSRR target >60-70 dB (similar to CMRR). High CMRR and PSRR require: (1) layout symmetry (matched transistors, common-centroid), (2) high impedance bias (cascodes, current mirrors), (3) noise filtering (substrate isolation, guard rings).

**Input-Referred Noise**
Op-amp input-referred noise is the equivalent input voltage that produces observed output noise: V_n,in = V_n,out / A_v. Noise originates from: (1) thermal noise in transistors (kT/C, ~0.1-10 µV over signal bandwidth), (2) flicker noise (1/f noise, low frequency, ~100-1000 µV at 1 Hz, decreases at higher frequency). Input-referred noise improves (decreases) with: (1) higher gm (larger input transistor, lower thermal noise), (2) higher bias current (more thermal noise absolute, but lower relative to signal), (3) larger input transistor W/L (more gm, lower noise). Noise specification: typical ~10-100 nV/√Hz (thermal, white noise), ~1 µV/√f (flicker, 1/f). Trade-off: reducing noise requires larger transistors (larger area, more power).

**Systematic Offset**
Offset voltage (Vos) is non-ideal output voltage when inputs are tied together (should be zero). Systematic offset (due to design intent): (1) biased input for stable bias, (2) resistor mismatch in bias chain. Random offset (due to mismatch, covered in Monte Carlo analysis): expected from Pelgrom's law, ~5-50 mV for typical-sized op-amp. Design minimizes systematic offset via: (1) careful resistor matching (same thermal history, common-centroid layout), (2) symmetric bias networks. Worst-case offset (6-sigma mismatch): ~50-100 mV for precision op-amps, specified as max offset spec for worst silicon. Offset trim circuits (switchable resistor networks) can reduce offset post-manufacture (at test).

**Op-Amp Layout (Current Mirror Matching, Guard Rings)**
Op-amp layout is critical: (1) input pair — matched transistors, common-centroid layout (reduce random mismatch), (2) current mirror — matched transistor pair, high-impedance node (substrate taps, guard rings to isolate from noise), (3) power rails — wide buses (low resistance, supply noise reduction), (4) signal routing — short paths (low parasitic L, reduced coupling), (5) guard rings — surround sensitive analog blocks (substrate noise isolation). Layout directly impacts: (1) mismatch (determines Vos distribution), (2) noise (substrate coupling, supply noise), (3) gain (parasitic capacitance at nodes, reduces impedance). Layout optimization often requires hand-layout (not automated), targeting >1000 μm² typical area, down to ~100 μm² for power-constrained designs.

**Folded-Cascode Op-Amp and Variants**
Folded-cascode is alternative architecture: (1) cascode connected in feedback path (folded configuration), (2) two gain stages in parallel (higher speed, ~2-3x faster than 2-stage for same GBW), (3) lower output swing (cascode limits swing, not rail-to-rail). Folded-cascode trades speed for swing; suitable for low-voltage designs (<5 V supplies). Rail-to-rail output stage (p-MOSFET + n-MOSFET in parallel) enables swing from 0 to Vdd, important for battery-powered and low-voltage systems. Rail-to-rail requires careful biasing (transition between p and n dominance at mid-range).

**Summary**
Op-amp design is a mature discipline, balancing gain, bandwidth, power, and noise for diverse applications. Continued advances in low-voltage design, noise reduction, and integration enable analog IP across modern system-on-chip platforms.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/analog-ip) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
