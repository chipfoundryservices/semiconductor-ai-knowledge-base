# Redistribution Layer (RDL)

**Keywords**: redistribution layer rdl,fan out rdl,rdl fabrication process,rdl metal stack,rdl dielectric materials

---

**Redistribution Layer (RDL)** is **the thin-film metal interconnect structure fabricated on wafer or package substrates that reroutes I/O connections from fine-pitch die pads (40-100μm) to coarser-pitch package balls (400-800μm) — enabling fan-out packaging, area array I/O, and heterogeneous integration with 2-10μm line/space lithography, 2-5 metal layers, and resistance <50 mΩ per connection**.

**RDL Structure:**
- **Metal Layers**: Cu traces 2-10μm thick, 2-20μm wide; 2-5 metal levels depending on routing complexity; M1 connects to die pads, top metal connects to solder balls or bumps; via diameter 5-20μm connects metal layers
- **Dielectric Layers**: polymer (polyimide, BCB, PBO) or inorganic (SiO₂, SiN) dielectric 2-15μm thick between metal layers; provides electrical isolation, mechanical support, and stress buffer; dielectric constant 2.5-4.0 for polymers, 3.9-7.0 for inorganics
- **Under-Bump Metallization (UBM)**: Ti/Cu or Ni/Au (5/500nm or 5μm electroless Ni / 0.05μm immersion Au) on top metal; provides solder-wettable surface and diffusion barrier; patterned by photolithography or through-mask plating
- **Passivation**: final polyimide or solder resist layer (5-20μm) protects RDL; openings for UBM and solder balls; provides environmental protection and electrical isolation

**Fabrication Process (Wafer-Level):**
- **Passivation Opening**: plasma etch or laser ablation opens die passivation to expose Al pads; opening diameter 30-80μm; Tokyo Electron Tactras or 3D-Micromac microSTRUCT laser
- **Seed Layer Deposition**: PVD Ti/Cu (50/500nm) sputtered on wafer; Ti provides adhesion to polyimide and Al pads; Cu provides seed for electroplating; Applied Materials Endura or Singulus TIMARIS
- **Photoresist Patterning**: thick photoresist (5-20μm) spin-coated and patterned; defines RDL traces and vias; Tokyo Electron CLEAN TRACK or SUSS MicroTec ACS200; 2-10μm line/space capability
- **Cu Electroplating**: Cu plated in photoresist openings; acid Cu sulfate bath; current density 10-30 mA/cm²; plating time 20-60 minutes for 2-10μm thickness; Lam Research SABRE or Applied Materials Raider

**Dielectric Materials:**
- **Polyimide (PI)**: HD MicroSystems PI-2600 series; spin-coated 2-15μm per layer; soft bake 90-150°C, cure 300-350°C in N₂; dielectric constant 3.2-3.5; CTE 30-50 ppm/K; excellent planarization over topography
- **Polybenzoxazole (PBO)**: HD MicroSystems Durimide; lower moisture absorption than PI (<0.5% vs 2-3%); cure temperature 300-400°C; dielectric constant 2.8-3.0; better dimensional stability; higher cost than PI
- **Benzocyclobutene (BCB)**: Dow Cyclotene; low dielectric constant (2.65); cure temperature 200-250°C; excellent electrical properties for RF applications; poor adhesion requires adhesion promoter (AP3000)
- **Inorganic Dielectrics**: PECVD SiO₂ or SiN; deposited 0.5-2μm per layer; temperature 200-400°C; dielectric constant 3.9 (SiO₂) or 7.0 (SiN); better moisture barrier than polymers but higher stress and cost

**Fan-Out RDL:**
- **eWLB (embedded Wafer-Level Ball Grid Array)**: dies placed face-down on temporary carrier; molded with epoxy mold compound (EMC); carrier removed; RDL fabricated on reconstituted wafer; enables fan-out I/O beyond die footprint
- **InFO (Integrated Fan-Out)**: TSMC technology; multiple dies and passives embedded in mold compound; RDL connects dies and routes to package balls; used in Apple A-series processors; 2μm line/space, 4-5 metal layers
- **FOWLP (Fan-Out Wafer-Level Package)**: generic term for fan-out technologies; RDL pitch 2-10μm enables high I/O count (>1000 balls); package thickness 200-600μm thinner than flip-chip BGA
- **Advantages**: low cost (wafer-level processing), thin profile, excellent electrical performance (short interconnects), scalable to large die sizes; challenges: warpage control, die shift during molding, RDL yield

**Panel-Level RDL:**
- **Large Substrates**: RDL fabricated on 510×515mm or 600×600mm glass or organic panels; 4-9× area vs 300mm wafers; economies of scale reduce cost per unit
- **Equipment**: modified PCB equipment for large panels; Shibaura Mechatronics panel plating, Nikon or Canon panel lithography, Toray or Ajinomoto dielectric coating
- **Challenges**: panel bow and warpage (>500μm across 600mm); non-uniform plating and lithography; handling and transport of large panels; yield learning ongoing
- **Status**: pilot production by ASE, Deca Technologies, and Nepes; cost benefits projected 20-40% vs wafer-level for large die and high-volume applications

**Electrical Performance:**
- **Resistance**: Cu trace resistance 17 mΩ/sq for 1μm thickness; typical RDL trace 2-5mm length, 5-10μm width, 3-5μm thickness → 10-50 mΩ resistance; via resistance 1-5 mΩ depending on diameter and aspect ratio
- **Capacitance**: trace-to-trace capacitance 0.1-0.5 pF/mm for 10μm spacing in polyimide (ε=3.3); trace-to-ground capacitance 0.5-2 pF/mm² for 5μm dielectric thickness
- **Inductance**: RDL trace inductance 0.5-2 nH/mm depending on width and ground plane proximity; lower than wire bonds (1-5 nH per bond) enabling higher frequency operation
- **Signal Integrity**: 2-5μm line/space RDL supports >10 GHz signaling; impedance control ±10% achieved through width and spacing design; ground planes in multi-layer RDL reduce crosstalk

**Reliability:**
- **Thermal Cycling**: JEDEC JESD22-A104 (-40°C to 125°C, 1000 cycles); failure mechanism: Cu trace cracking or delamination at dielectric interface; CTE mismatch between Cu (16.5 ppm/K), polyimide (30-50 ppm/K), and Si (2.6 ppm/K)
- **Moisture Resistance**: JEDEC JESD22-A120 (85°C/85% RH, 1000 hours); polyimide absorbs 2-3% moisture causing swelling and delamination; PBO and BCB have better moisture resistance (<0.5% absorption)
- **Electromigration**: Cu trace electromigration at high current density (>10⁵ A/cm²); mean time to failure (MTTF) = A·j⁻²·exp(Ea/kT) where Ea≈0.9 eV for Cu; design rule: current density <5×10⁴ A/cm² for 10-year lifetime
- **Stress-Induced Voiding**: voids form in Cu traces due to thermal stress; accelerated by moisture and high temperature; proper annealing (200-400°C, 30-60 min) after plating reduces voiding

**Inspection and Metrology:**
- **Optical Inspection**: automated optical inspection (AOI) checks line width, spacing, and defects; KLA 8 series or Camtek Falcon; resolution 0.5-1μm; detects opens, shorts, and dimensional defects
- **Electrical Test**: 4-wire Kelvin measurement of trace resistance; typical specification 10-50 mΩ; >100 mΩ indicates high resistance or open circuit; daisy-chain test structures enable continuity testing
- **Cross-Section Analysis**: FIB-SEM cross-sections verify layer thickness, via fill quality, and interface adhesion; Thermo Fisher Helios or Zeiss Crossbeam; destructive test on sample units
- **Warpage Measurement**: shadow moiré or laser profilometry measures package warpage; specification typically <100μm across package; excessive warpage causes assembly issues and reliability failures

Redistribution layers are **the flexible interconnect fabric that enables modern advanced packaging — providing the routing density and electrical performance to connect fine-pitch die I/O to package-level interconnects while enabling fan-out architectures, heterogeneous integration, and system-in-package solutions that define the post-Moore's Law era of semiconductor scaling**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/redistribution-layer-rdl) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
