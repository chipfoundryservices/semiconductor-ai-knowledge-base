# Chip-Package Co-Design

**Keywords**: chip package,co-design,chip package co-simulation,solder bump,package resonance,package resonance

---

**Chip-Package Co-Design** is the **simultaneous optimization of chip I/O and package routing — accounting for package parasitic inductance, resonance, and signal integrity — enabling high-speed I/O, power integrity, and cost-effective assembly — critical for high-performance systems at 5 GHz and above**. Chip-package interaction is inseparable in modern design.

**C4 Bump and BGA Ball Assignment**
Die-to-package connection uses: (1) C4 bump (controlled collapse chip connection) — solder bump placed directly on die bond pads, connected to package substrate via solder reflow, (2) wire bond (legacy) — thin wire from die to package lead, (3) BGA ball (ball grid array) — spherical solder ball on package bottom, connects to board via reflow. C4 and BGA assignment involves: (1) signal assignment — high-speed signals placed for short path, low-impedance, (2) power/ground assignment — distributed for low inductance, (3) high-frequency signals (clock, differential pairs) placed for controlled impedance. Assignment directly impacts signal integrity (crosstalk, reflections, ISI).

**Package Parasitic (L, R, C)**
Package interconnect (substrate traces, vias, solder balls, leadframe) has parasitic inductance (L), resistance (R), and capacitance (C). Typical package parasitic: (1) inductance per via ~100 pH (via inductance = 2 nH per 100 µm height), (2) via resistance ~1-10 mΩ, (3) substrate trace inductance ~10-100 pH per mm (depends on spacing and layer). These parasitics dominate high-speed signal paths: loop inductance (signal + return) determines overshoot/ringing. Package parasitic L dominates at GHz frequencies: impedance Z = ωL >> R at high frequency.

**Resonance in Package PDN**
Power delivery network (PDN) combines die-level decaps, package inductance, and board-level capacitors. Multiple L and C create resonances: when ω = 1/√(LC), impedance peaks (anti-resonance). Multiple peaks occur at different frequencies: (1) die-level decap resonance ~100 MHz, (2) package resonance ~300-500 MHz (package L ~1-2 nH + bulk cap C ~10-100 nF), (3) board resonance ~10-50 MHz. Resonance peaks create impedance spikes where PDN cannot source current effectively; simultaneous large current demands at resonance frequency cause voltage droop. Mitigation: (1) flatten PDN impedance across all frequencies (multiple cap types with different resonances), (2) avoid simultaneous switching at resonance frequency (frequency design).

**Co-Simulation (SPICE + S-Parameters)**
Accurate analysis of chip-package interaction requires co-simulation: (1) package is characterized via 3D EM simulation (Ansys HFSS, ADS Momentum) producing S-parameters (frequency-dependent impedance/transmission), (2) S-parameters are converted to SPICE models (rational function models), (3) die and package models connected in SPICE simulation, (4) time-domain simulation predicts signal waveforms (rise time, overshoot, ISI). Co-simulation requires: (1) detailed package geometry (substrate, vias, traces), (2) die model (power distribution, clock tree), (3) board model (decap placement, impedance). Simulation is slow (hours to days for large circuits) but essential for high-speed design.

**Package-Level EM and IR Analysis**
Package-level EM (electromigration) analysis checks current density in package traces and vias: same as chip-level EM, but applied to package. Package traces are often wider than chip metal (~10-50 µm vs 1-5 µm on chip), allowing higher current density. However, solder joints and vias can be current bottlenecks, requiring EM checks. IR analysis calculates voltage drop from power pad to chip bump: package resistance causes ~5-50 mV drop depending on current. Must be accounted for in total voltage margin.

**Die-to-Package Interface (Flip-Chip vs Wire Bond)**
Flip-chip (C4 bumps, die face-down on substrate) is superior to wire bond for high-speed: (1) shorter path (bumps directly on die), (2) lower inductance (L ~0.1-1 nH per path vs 2-5 nH for wire bond), (3) distributed power/ground (multiple bumps reduce impedance). Wire bond (legacy, still used for cost-sensitive products) has longer inductance, unsuitable for GHz. Flip-chip is standard for high-performance (>1 GHz). Cost premium for flip-chip: ~5-20% higher assembly cost, but justified by better performance.

**2.5D and 3D Package Co-Design**
2.5D (multiple dies on interposer) and 3D (stacked dies) packaging introduce additional parasitic. Interposer traces have lower inductance than organic substrate (lower-loss material, sometimes silicon with metal lines), but vias connecting dies add inductance. 3D stacking (dies bonded via micro-bumps or hybrid bonding) requires tight control of micro-bump inductance (~1-10 pH per bump). Co-design of chip, interposer, and 3D stack is essential: (1) placement on die affects bump location, (2) bump location affects interposer routing, (3) interposer routing affects signal integrity. Iterative co-optimization is required.

**High-Speed Signal Integrity**
High-speed signals (5-20 GHz) require: (1) controlled impedance (50 Ω typical for differential pairs), (2) low crosstalk (tight shielding), (3) low skew (matched trace lengths for differential pairs), (4) low insertion loss (minimize resistance/dielectric loss at high frequency). Package routing must maintain impedance control: trace width/spacing must be consistent, vias must be stitched (multiple vias reduce via inductance). Simulation predicts: (1) eye diagram (data signal integrity, margin to timing/threshold), (2) jitter (timing variation, critical for clock recovery), (3) crosstalk (unwanted coupling between signals).

**Why Co-Design Matters**
Chip and package are inseparable: poor chip design (large current transients, low impedance source) overwhelms package (package cannot supply current fast enough, voltage droop). Conversely, well-designed chip with poor package (high inductance, low cap) also fails. Co-design balances: (1) chip minimizes switching noise (timing constraints, gating), (2) package provides low impedance (many bumps, good cap placement), (3) board provides bulk energy (large caps, low-ESR). Integrated approach achieves high-speed, reliable operation.

**Summary**
Chip-package co-design is essential for high-speed systems, requiring joint optimization of die I/O, package routing, and PDN. Continued advances in package materials (lower inductance, lower-loss), simulation (faster, more accurate), and integration techniques (smaller bumps, higher density) enable aggressive performance targets.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/chip-package) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
