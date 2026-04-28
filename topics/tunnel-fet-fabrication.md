# Tunnel FET (TFET) Fabrication

**Keywords**: tunnel fet fabrication,tfet band to band tunneling,tfet steep slope,tfet heterojunction,tfet low power operation

---

**Tunnel FET (TFET) Fabrication** is **the process technology for creating transistors that operate by quantum mechanical band-to-band tunneling (BTBT) rather than thermionic emission — achieving subthreshold slopes below the 60 mV/decade Boltzmann limit through abrupt P⁺-I-N⁺ junctions, heterojunction engineering (Si/Ge, III-V), and optimized gate alignment, enabling ultra-low-power operation at sub-0.3V supply voltages for IoT and energy-harvesting applications despite 10-100× lower drive current than conventional MOSFETs**.

**TFET Operating Principle:**
- **Band-to-Band Tunneling**: electrons tunnel from valence band of P⁺ source through narrow bandgap barrier into conduction band of intrinsic channel; tunneling probability T ∝ exp(-4√(2m*) × E_g^(3/2) / (3qℏE)) where E_g is bandgap, E is electric field; requires ultra-high field (>1 MV/cm) and thin barrier (<5nm)
- **Steep Subthreshold Slope**: not limited by Boltzmann distribution; subthreshold swing S = 60 × (kT/q) × ln(10) × (1 + C_dep/C_ox) for MOSFETs; TFETs achieve S = 20-40 mV/decade through tunneling mechanism; enables lower Vt and lower Vdd (0.2-0.3V vs 0.5-0.7V for MOSFETs)
- **P-I-N Structure**: P⁺ source (B doping >10²⁰ cm⁻³), intrinsic channel (doping <10¹⁶ cm⁻³), N⁺ drain (P or As doping >10²⁰ cm⁻³); gate modulates tunneling barrier at source-channel junction; drain is passive (unlike MOSFET where drain creates channel field)
- **Ambipolar Behavior**: tunneling can occur at both source and drain junctions; causes ambipolar conduction (current flows for both positive and negative Vgs); suppressed by asymmetric doping or heterostructures; limits logic applications

**Homojunction Si TFET:**
- **Abrupt Junction Formation**: ultra-abrupt P⁺-I junction (<2nm/decade doping gradient) required for high tunneling current; ion implantation with low energy (0.5-2 keV) and rapid thermal anneal (1000-1050°C, <1s); or in-situ doped selective epitaxy with abrupt doping transition
- **Gate Alignment**: gate must overlap source-channel junction by 5-10nm for optimal tunneling field; misalignment degrades performance exponentially; requires <2nm overlay accuracy; self-aligned gate process (gate-first or replacement gate) preferred
- **Channel Engineering**: thin SOI (5-10nm) or nanowire (diameter 5-10nm) increases gate control; improves subthreshold slope; reduces ambipolar current; GAA geometry provides best electrostatics (S = 25-35 mV/decade demonstrated)
- **Performance Limitations**: Si bandgap (1.12 eV) limits tunneling current; on-current 1-10 μA/μm at Vdd=0.5V; 10-100× lower than MOSFET; insufficient for high-performance logic; suitable only for ultra-low-power applications (<1 MHz operation)

**Heterojunction TFET:**
- **SiGe Source**: Ge content 50-80% reduces effective bandgap at source-channel interface; increases tunneling probability by 10-100×; on-current 10-50 μA/μm at Vdd=0.5V; SiGe grown by selective epitaxy at 550-650°C; abrupt Si/SiGe interface (<1nm) critical
- **Ge-on-Si TFET**: pure Ge source (E_g = 0.66 eV) on Si channel; 100× higher tunneling current than Si; Ge epitaxy on Si requires buffer layer to accommodate 4% lattice mismatch; threading dislocation density <10⁶ cm⁻² required; aspect ratio trapping (ART) confines defects
- **III-V Heterojunction**: InGaAs/GaAsSb or InAs/GaSb heterojunctions with broken-gap alignment (valence band of one material above conduction band of other); enables direct tunneling without barrier; on-current >100 μA/μm; requires III-V epitaxy on Si (challenging integration)
- **2D Material TFET**: MoS₂/WSe₂ or graphene/MoS₂ heterojunctions; atomically sharp interfaces; tunable bandgap; demonstrated S < 10 mV/decade; on-current limited by contact resistance; research stage (not manufacturable)

**Advanced TFET Structures:**
- **Line Tunneling**: conventional TFET has point tunneling (small tunneling area); line-TFET uses L-shaped gate creating line tunneling along source edge; 5-10× higher on-current; requires precise gate alignment and 3D gate structure
- **Vertical TFET**: source at bottom, drain at top, gate wraps vertical pillar; tunneling occurs at bottom source-channel interface; natural line tunneling geometry; higher current density; fabrication similar to vertical MOSFET
- **Double-Gate TFET**: gates on both sides of thin channel; increases tunneling field by 2×; improves on-current and subthreshold slope; requires aligned double-gate process (challenging for <20nm gate length)
- **Feedback FET (FBFET)**: positive feedback through capacitive coupling between gate and floating body; achieves S < 5 mV/decade; hysteresis in I-V characteristics; suitable for memory applications; not true TFET but related steep-slope device

**Fabrication Challenges:**
- **Abrupt Doping Profile**: <1nm/decade gradient required; ion implantation causes straggle (5-10nm); solid-source diffusion or in-situ doped epitaxy preferred; SIMS verification of doping profile; abruptness directly correlates with on-current
- **Low Thermal Budget**: abrupt junctions degrade with high-temperature processing; limits subsequent thermal steps to <800°C; incompatible with conventional CMOS integration (requires >1000°C for S/D activation); requires process re-architecture
- **Contact Resistance**: P⁺ source contact resistance critical (source supplies tunneling current); requires <1×10⁻⁸ Ω·cm² contact resistivity; silicide formation (NiSi, TiSi) on heavily-doped source; contact resistance often dominates total resistance
- **Ambipolar Suppression**: heterostructure with large valence band offset at drain-channel interface; or thick gate oxide at drain side; or asymmetric gate work function; reduces drain-side tunneling by >100×; essential for logic operation

**Performance Metrics:**
- **Subthreshold Swing**: best Si TFET: S = 30-40 mV/decade; SiGe TFET: S = 20-30 mV/decade; III-V TFET: S = 10-20 mV/decade; point subthreshold swing (minimum S) vs average subthreshold swing (over 3-4 decades of current)
- **On-Current**: Si TFET: 1-10 μA/μm; SiGe TFET: 10-50 μA/μm; III-V TFET: 50-200 μA/μm at Vdd=0.5V; compare to MOSFET: 500-1000 μA/μm at Vdd=0.7V; TFET on-current insufficient for high-performance logic
- **Off-Current**: <1 pA/μm achievable due to steep slope; enables ultra-low standby power; 100-1000× lower than MOSFET at same on-current; key advantage for energy-constrained applications
- **Energy Efficiency**: CV²f energy reduced by 4-9× through voltage scaling (0.3V vs 0.7V); offsets lower on-current for low-frequency applications (<10 MHz); energy-delay product competitive with MOSFET for f < 1 MHz

**Applications and Outlook:**
- **Ultra-Low-Power IoT**: sensor nodes, wearables, implantable devices operating at <1 MHz; energy harvesting from ambient sources (solar, thermal, RF); TFET enables operation at 0.2-0.3V matching harvester output
- **Steep-Slope Logic**: hybrid CMOS-TFET circuits; TFETs for low-activity blocks (sleep transistors, retention logic); MOSFETs for high-performance paths; 30-50% energy reduction for duty-cycled applications
- **Memory Access Transistors**: TFET as access device for DRAM or SRAM; steep slope enables lower Vmin; improves retention time and reduces refresh power; demonstrated in research but not production
- **Commercialization Challenges**: no TFET in production as of 2024; on-current remains 10× below requirements for general logic; heterojunction integration with CMOS too complex; niche applications (ultra-low-power) may adopt in late 2020s if integration challenges solved

Tunnel FET fabrication is **the pursuit of the ultimate low-power transistor — breaking the 60 mV/decade Boltzmann limit through quantum tunneling, enabling sub-0.3V operation for energy-harvesting applications, but facing the fundamental trade-off between steep slope and drive current that has prevented mainstream adoption despite 20 years of research and development**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/tunnel-fet-fabrication) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
