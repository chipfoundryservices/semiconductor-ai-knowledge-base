# Tunnel FET (TFET) and Beyond-CMOS

**Keywords**: tunnel fet tfet device,band to band tunneling,tfet steep subthreshold,tunnel junction gate,negative capacitance fet ncfet

---

**Tunnel FET (TFET) and Beyond-CMOS** is the **steep-subthreshold-swing transistor leveraging band-to-band tunneling instead of thermal emission — enabling sub-60 mV/dec switching for ultra-low-voltage computation and power-constrained applications**.

**Band-to-Band Tunneling (BTBT) Mechanism:**
- Tunneling current: quantum mechanical tunneling between valence and conduction bands; electron-hole pair generation
- Energy band diagram: reverse-biased junction with large depletion width; electrons tunnel from VB to CB
- Tunneling probability: exponential dependence on bandgap and electric field; sensitive to field direction
- Gate modulation: gate voltage controls tunneling probability; enables transistor action
- Temperature independence: tunneling rate weakly dependent on temperature (vs thermal emission)

**TFET Device Structure:**
- Source-drain: p-type source and n-type drain (for electron devices); reverse-biased source-drain
- Intrinsic channel: intrinsic or lightly doped channel; gate controls tunneling
- Gate location: gate electrode positioned to control tunneling at source-drain interface
- Band-to-band tunnel junction: gated p-i-n structure; spatially selective tunneling
- Current path: tunneled carriers flow through channel; modulated by gate voltage

**Subthreshold Swing (SS) Performance:**
- Thermal limit: conventional MOSFETs limited to ~60 mV/dec at room temperature (from thermionic theory)
- TFET advantage: tunneling circumvents thermal limit; SS < 60 mV/dec possible
- Measured performance: sub-60 mV/dec demonstrated at low current; degradation at higher current
- Temperature advantage: SS weakly dependent on temperature; constant at different T vs MOSFET increase
- Ultra-low voltage: steep SS enables reduced supply voltage (0.1-0.3 V practical)

**Gate-Induced Drain Leakage (GIDL):**
- Gate tunneling: gate voltage induces band bending; direct tunneling from gate
- Current component: additional leakage path reducing on/off ratio; parasitic to transistor operation
- Minimization: careful gate oxide engineering reduces GIDL; dielectric and thickness selection
- Off-state current: high GIDL degradates off-state performance; limits on-off ratio
- Design trade-off: optimizing on-state tunneling increases GIDL; balance necessary

**Ambipolar Conduction Challenge:**
- Both carrier types: both electrons and holes tunnel; ambipolar device characteristics
- Problem: off-state has both electron and hole conduction paths; high leakage current
- Limit on/off ratio: ambipolar effects reduce on-off ratio vs MOSFET (>10⁶)
- Solutions: asymmetric doping, heterostructure TFETs, workfunction engineering reduce ambipolarity
- Practical limitation: ambipolar TFETs show moderate on-off ratio (10³-10⁴)

**Heterostructure TFET:**
- Material engineering: different bandgaps in source/drain/channel; optimize tunneling
- InAs/Si TFET: narrow bandgap InAs source enables efficient tunneling into wider bandgap Si channel
- Bandgap engineering: source smaller gap → higher tunneling rate; steep subthreshold swing
- Performance: improved on-state current and steeper SS vs homojunction TFET
- Fabrication: heteroepitaxy and monolithic integration challenging; requires advanced processing

**Negative Capacitance FET (NC-FET):**
- Ferroelectric gate: ferroelectric material in gate stack; exhibits negative capacitance at certain bias
- Landau theory: ferroelectric capacitance negative in certain polarization regions
- Internal voltage: ferroelectric provides voltage amplification; reduces gate voltage required for switching
- Subthreshold swing: amplified internal voltage enables SS < 60 mV/dec at room temperature
- Theory vs practice: theoretical promise; practical implementation challenges remain

**Ferroelectric Effect in NC-FET:**
- Polarization: electric polarization in ferroelectric material; creates dipole field
- Hysteresis: polarization vs field hysteresis; nonlinear response
- Negative capacitance: specific polarization regions exhibit dP/dV < 0; capacitance negative
- Instability: ferroelectric with metallic gate potentially unstable; requires proper design
- Material candidates: HfZrO₂, PbZrTiO₃; recent advances enable thin film ferroelectrics

**Ultra-Low Voltage Operation:**
- Supply voltage: TFETs enable efficient operation at 0.1-0.3 V (vs 0.7-1.2 V for MOSFET)
- Power reduction: lower voltage reduces dynamic power (∝ V²) and leakage (exponential in V)
- Energy-efficient circuits: ultra-low voltage circuits dramatically reduce energy consumption
- Speed trade-off: lower voltage reduces speed; acceptable for energy-constrained applications
- Subthreshold operation: intentional operation in subthreshold regime for maximum energy efficiency

**Integration and Circuit Design:**
- Noise margin: lower voltage reduces noise margins; circuit design must account for reduced robustness
- Impedance: higher impedance at lower voltage; affects circuit behavior
- Speed degradation: lower voltage proportionally reduces speed; timing margins critical
- Application scope: ultra-low-voltage TFETs suitable for biomedical, sensor, and edge-AI applications
- System perspective: requires co-design of circuits and devices; holistic approach necessary

**Steep-Slope Device Comparison:**
- Tunnel FET: band-to-band tunneling mechanism; heterojunction enables best performance
- NC-FET: ferroelectric gate enables voltage amplification
- Impact ionization FET: avalanche effect for carrier generation; alternative mechanism
- Electrostatic doping: dynamic workfunction modulation; alternative approach
- Device selection: application requirements drive choice; trade-offs in performance/complexity

**Challenges and Limitations:**
- On-state current: tunneling current lower than thermionic current; ON current smaller than MOSFET
- Drive current: limits circuit speed; applications limited to low-frequency, power-constrained domains
- Reliability: ferroelectric degradation; tunneling-induced damage; long-term reliability questions
- Variability: manufacturing variability in tunneling probability; process control challenging
- Ambipolarity: symmetric tunneling reduces asymmetry; limits beneficial asymmetric device behavior

**Performance Metrics:**
- Subthreshold swing: 15-30 mV/dec demonstrated (vs 60 mV/dec MOSFET theoretical limit)
- On-off ratio: 10³-10⁴ typical (vs >10⁶ for MOSFET); still adequate for many applications
- Tunneling current density: 10⁻¹²-10⁻¹¹ A/μm typical; increases with gate voltage exponentially
- On-state current: 1-100 μA/μm typical; depends on material and design

**Application Scenarios:**
- Biomedical implants: ultra-low voltage enables multi-year battery operation
- IoT sensors: energy-harvesting powered devices; extremely low power budget
- Edge AI accelerators: reduced voltage for inference; improved energy efficiency
- Analog circuits: low-voltage operation enables portable/wearable applications
- Power management: reduced supply voltage dramatically improves efficiency for distributed systems

**Tunnel FETs and NC-FETs offer steep subthreshold swing below 60 mV/dec thermal limit — enabling ultra-low-voltage computation for energy-constrained applications through band-to-band tunneling and ferroelectric voltage amplification.**

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/tunnel-fet-tfet-device) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
