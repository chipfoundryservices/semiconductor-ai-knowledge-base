# Negative Capacitance FET (NCFET)

**Keywords**: negative capacitance fet ncfet,ferroelectric transistor,ncfet steep slope,ferroelectric hfo2,ncfet voltage amplification

---

**Negative Capacitance FET (NCFET)** is **the steep-slope transistor concept that integrates a ferroelectric material in series with the gate dielectric to achieve voltage amplification through negative capacitance — enabling subthreshold slopes below 60 mV/decade and 30-50% reduction in operating voltage while maintaining MOSFET-like drive current, using ferroelectric HfO₂ or Hf₀.₅Zr₀.₅O₂ deposited by ALD and integrated with conventional CMOS processes for potential deployment at 3nm node and beyond**.

**Negative Capacitance Principle:**
- **Ferroelectric Capacitance**: ferroelectric materials exhibit S-shaped polarization-voltage (P-V) curve with negative slope region (dP/dV < 0); in this region, capacitance C = dQ/dV = A × dP/dV is negative; negative capacitance amplifies voltage across the underlying gate dielectric
- **Voltage Amplification**: ferroelectric (C_FE < 0) in series with gate dielectric (C_ox > 0); total capacitance C_total = (C_FE × C_ox) / (C_FE + C_ox); when |C_FE| < C_ox, voltage across oxide V_ox > V_gate; amplification factor A = 1 + C_ox/|C_FE| = 1.5-3×
- **Subthreshold Slope**: S = (kT/q) × ln(10) × (1 + C_dep/C_total); voltage amplification increases C_total, reducing S; theoretical S < 60 mV/decade achievable; experimental demonstrations show S = 20-40 mV/decade over 3-4 decades of current
- **Hysteresis Concern**: ferroelectric materials typically show hysteresis (memory effect); for NCFET logic, hysteresis must be eliminated; achieved by operating in capacitance-matching regime where C_FE + C_ox ≈ 0 (stabilized negative capacitance state)

**Ferroelectric Materials:**
- **Ferroelectric HfO₂**: doped HfO₂ (with Si, Zr, Al, or Y) exhibits ferroelectricity in orthorhombic phase; discovered 2011; CMOS-compatible (unlike PZT or BaTiO₃); remnant polarization P_r = 10-30 μC/cm²; coercive field E_c = 1-2 MV/cm
- **Hf₀.₅Zr₀.₅O₂ (HZO)**: most widely studied composition; 50% Hf, 50% Zr provides optimal ferroelectric properties; P_r = 20-35 μC/cm²; E_c = 1.0-1.5 MV/cm; orthorhombic phase stable after 400-600°C anneal; thickness 5-15nm typical
- **Si-Doped HfO₂**: 3-6% Si doping stabilizes orthorhombic phase; P_r = 15-25 μC/cm²; compatible with existing HfO₂ ALD processes; Si incorporation during deposition or by ion implantation; annealing at 500-700°C crystallizes ferroelectric phase
- **Al-Doped HfO₂**: 2-5% Al doping; P_r = 10-20 μC/cm²; lower coercive field (0.8-1.2 MV/cm) than HZO; easier switching; used in ferroelectric memory (FeRAM); suitable for NCFET with proper thickness optimization

**NCFET Structures:**
- **MFIS (Metal-Ferroelectric-Insulator-Semiconductor)**: ferroelectric layer on top of conventional gate dielectric (SiO₂ or HfO₂); most common structure; ferroelectric thickness 5-10nm, dielectric thickness 1-2nm; capacitance matching condition: t_FE/ε_FE ≈ t_ox/ε_ox
- **MFMIS (Metal-Ferroelectric-Metal-Insulator-Semiconductor)**: internal metal gate between ferroelectric and dielectric; decouples ferroelectric switching from channel; reduces hysteresis; enables independent optimization of ferroelectric and dielectric; adds process complexity
- **MFS (Metal-Ferroelectric-Semiconductor)**: ferroelectric directly on Si channel; no intermediate dielectric; maximum voltage amplification; interface quality critical; high interface trap density degrades performance; requires surface passivation
- **Baseline Subtraction**: ferroelectric layer in series with baseline transistor; baseline provides conventional MOSFET behavior; ferroelectric adds negative capacitance boost; enables direct comparison of NC effect; used in research demonstrations

**Fabrication Process:**
- **ALD Deposition**: HZO deposited by thermal ALD using TEMAH (tetrakis(ethylmethylamino)hafnium) + TDMAZ (tetrakis(dimethylamino)zirconium) + H₂O at 250-300°C; 0.1nm per cycle; 50-100 cycles for 5-10nm thickness; composition controlled by precursor pulse ratio
- **Crystallization Anneal**: as-deposited HZO is amorphous; rapid thermal anneal (RTA) at 400-600°C for 20-60s in N₂ crystallizes orthorhombic ferroelectric phase; anneal temperature and time critical (too low: incomplete crystallization, too high: monoclinic phase forms)
- **Capping Layer**: TiN or TaN capping layer (5-10nm) deposited before anneal; prevents oxygen loss during anneal; stabilizes orthorhombic phase; serves as top electrode; capping layer material and thickness affect ferroelectric properties
- **Integration with CMOS**: ferroelectric layer added to gate stack; compatible with replacement metal gate (RMG) process; ferroelectric deposited after dummy gate removal; standard CMOS process for S/D, contacts, and interconnects; no major process changes required

**Performance and Characteristics:**
- **Subthreshold Slope**: experimental NCFETs demonstrate S = 20-40 mV/decade over 3-4 decades of current; point slope (minimum S) as low as 5-10 mV/decade; average slope over full subthreshold region 30-50 mV/decade; 30-50% improvement vs conventional MOSFET
- **Drive Current**: maintains MOSFET-like on-current (500-1000 μA/μm) unlike TFET; voltage amplification boosts gate overdrive; enables same performance at 30-50% lower Vdd (0.4-0.5V vs 0.7V); key advantage over other steep-slope devices
- **Hysteresis**: hysteresis-free operation demonstrated in capacitance-matched regime; memory window <10 mV for logic applications; larger hysteresis (>100 mV) for memory applications (ferroelectric FET memory); thickness ratio tuning eliminates hysteresis
- **Reliability**: ferroelectric wake-up effect (P_r increases with cycling); fatigue (P_r decreases after 10⁶-10⁹ cycles); imprint (preferred polarization state develops); breakdown field 4-6 MV/cm; 10-year lifetime requires <3 MV/cm operating field

**Challenges and Solutions:**
- **Thickness Scaling**: ferroelectric thickness must scale with gate dielectric for capacitance matching; sub-5nm HZO shows reduced P_r and increased coercive field; limits scaling to 3nm node; alternative materials (BaTiO₃, PZT) have better properties but CMOS-incompatible
- **Variability**: ferroelectric grain size (5-20nm) comparable to device dimensions; grain-to-grain P_r variation causes Vt variation; σVt = 30-50mV for 10nm gate length; larger than conventional MOSFET; requires design margins
- **Temperature Dependence**: ferroelectric properties degrade at high temperature; Curie temperature T_c = 400-600°C for HZO; operating temperature must be <150°C; limits applications in automotive (125°C) and industrial (85°C) environments
- **Process Integration**: ferroelectric anneal (400-600°C) must occur after S/D formation (>1000°C); requires gate-last process; compatible with RMG but not gate-first; adds process complexity and cost

**Applications and Roadmap:**
- **Low-Power Logic**: 30-50% power reduction through voltage scaling; maintains performance unlike TFET; suitable for mobile SoCs, IoT, and edge AI accelerators; potential deployment at 3nm or 2nm node if reliability and variability solved
- **Steep-Slope SRAM**: NCFET-based SRAM operates at lower Vmin (0.4V vs 0.6V); improves retention time; reduces leakage power by 5-10×; enables aggressive voltage scaling for ultra-low-power applications
- **Ferroelectric Memory**: same ferroelectric material used for non-volatile memory (FeRAM); NCFET logic + FeRAM memory on same chip; enables normally-off computing (zero standby power); instant-on operation
- **Commercialization Status**: no NCFET in production as of 2024; Intel, TSMC, Samsung investigating for future nodes; reliability and variability remain concerns; may appear in niche products (ultra-low-power IoT) before mainstream logic

Negative capacitance FET is **the most promising steep-slope transistor technology — achieving sub-60 mV/decade operation while maintaining MOSFET-like drive current through ferroelectric voltage amplification, using CMOS-compatible HfO₂-based materials that could enable 30-50% power reduction at 3nm node and beyond if the challenges of reliability, variability, and process integration can be overcome in the next 3-5 years**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/negative-capacitance-fet-ncfet-13710) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
