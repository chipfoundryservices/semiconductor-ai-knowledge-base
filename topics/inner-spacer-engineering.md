# Inner Spacer Engineering

**Keywords**: inner spacer engineering,gaa inner spacer,spacer between nanosheets,dielectric spacer gaa,spacer parasitic capacitance

---

**Inner Spacer Engineering** is **the critical process technology that forms low-k dielectric spacers between vertically stacked nanosheets in GAA transistors** — reducing parasitic capacitance between gate and source/drain by 30-50%, improving switching speed by 15-25%, and enabling aggressive nanosheet pitch scaling (15-25nm) at 3nm and 2nm nodes by preventing gate-to-S/D shorts while minimizing capacitive coupling, where spacer thickness (3-8nm), material (SiN, SiOCN, air gaps), and formation process determine the performance-reliability trade-off.

**Inner Spacer Function and Requirements:**
- **Electrical Isolation**: prevents gate metal from contacting source/drain epitaxy; avoids shorts; must withstand 0.7-0.9V operating voltage; breakdown field >5 MV/cm
- **Capacitance Reduction**: low-k dielectric (k=4-6) reduces gate-to-S/D capacitance; 30-50% reduction vs no spacer; improves AC performance and reduces power
- **Mechanical Support**: provides structural support between nanosheets; prevents collapse during S/D epitaxy; must withstand 600-800°C growth temperature
- **Thickness Optimization**: 3-8nm typical; thicker reduces capacitance but increases S/D resistance; thinner increases capacitance but reduces resistance; trade-off

**Inner Spacer Formation Process:**
- **SiGe Recess Etch**: after dummy gate formation, selectively etch SiGe sacrificial layers from sides; creates cavities between Si nanosheets; etch depth 5-15nm; HCl or CF₄-based chemistry
- **Spacer Deposition**: atomic layer deposition (ALD) of low-k dielectric; conformal coating; fills cavities between sheets; typical materials: SiN (k=7), SiOCN (k=4-5), SiBCN (k=4-5)
- **Spacer Etch**: anisotropic etch removes spacer from horizontal surfaces; leaves spacer in cavities between sheets; critical dimension control ±1nm
- **S/D Epitaxy**: selective epitaxial growth of SiGe (pMOS) or Si:P (nMOS); grows from exposed Si nanosheet edges; fills space around inner spacers; in-situ doping

**Spacer Material Selection:**
- **Silicon Nitride (SiN)**: most common; k=7; good mechanical strength; thermal stability >1000°C; mature ALD process; but higher k than alternatives
- **Silicon Oxycarbonitride (SiOCN)**: lower k=4-5; reduces capacitance by 30-40% vs SiN; but lower mechanical strength; requires careful process optimization
- **Silicon Borocarbonitride (SiBCN)**: k=4-5; good mechanical strength; thermal stability; emerging material; less mature than SiOCN
- **Air Gaps**: ultimate low-k (k=1); formed by controlled void creation; 50-60% capacitance reduction vs SiN; but reliability concerns; research phase

**Capacitance Impact:**
- **Gate-to-S/D Capacitance**: inner spacer reduces Cgd and Cgs by 30-50%; critical for high-frequency operation; enables 10-20% higher fmax
- **Total Gate Capacitance**: Cgg = Cgs + Cgd + Cgb; inner spacer reduces Cgg by 15-25%; improves switching speed and reduces dynamic power
- **Parasitic Delay**: τ = RC delay; capacitance reduction improves delay by 15-25%; enables higher frequency or lower power at same frequency
- **Miller Capacitance**: Cgd (Miller capacitance) most critical; inner spacer reduces Cgd by 40-60%; improves gain-bandwidth product in analog circuits

**Thickness Optimization:**
- **Thin Spacers (3-5nm)**: lower S/D resistance; shorter distance for epitaxy to grow; but higher capacitance; preferred for low-frequency, high-current applications
- **Thick Spacers (6-8nm)**: lower capacitance; better isolation; but higher S/D resistance; longer epitaxy growth distance; preferred for high-frequency applications
- **Trade-off Analysis**: optimal thickness depends on application; high-performance logic: 5-7nm; low-power logic: 4-6nm; SRAM: 3-5nm
- **Variation Tolerance**: ±1-2nm thickness variation across wafer; affects capacitance and resistance; requires tight process control

**Integration Challenges:**
- **Conformal Deposition**: ALD must conformally coat narrow cavities (3-8nm wide, 5-15nm deep); aspect ratio 1:1 to 3:1; requires excellent step coverage
- **Void-Free Fill**: voids in spacer cause reliability issues; pinch-off at cavity entrance creates voids; requires optimized ALD conditions
- **Selective Etch**: spacer etch must be selective to Si nanosheets; avoid damaging channel; selectivity >20:1 required; plasma damage control
- **Epitaxy Compatibility**: spacer must withstand S/D epitaxy conditions (600-800°C, H₂ ambient); no degradation or delamination; interface stability

**Advanced Spacer Architectures:**
- **Dual-Layer Spacers**: inner layer (low-k SiOCN) for capacitance reduction, outer layer (SiN) for mechanical strength; combines benefits of both materials
- **Graded Composition**: composition varies through thickness; optimizes k and mechanical properties; requires advanced ALD process
- **Air Gap Spacers**: intentional void creation for ultra-low k; formed by controlled pinch-off during deposition; 50-60% capacitance reduction; reliability challenges
- **Hybrid Spacers**: different materials for different nanosheet gaps; top gaps use low-k, bottom gaps use high-strength; complex process

**Performance Impact:**
- **Frequency Improvement**: 10-20% higher fmax with optimized inner spacers vs no spacers; critical for high-performance processors
- **Power Reduction**: 15-25% lower dynamic power due to reduced capacitance; significant for mobile and datacenter applications
- **Delay Reduction**: 15-25% lower gate delay; enables faster logic paths; improves timing closure
- **Analog Performance**: higher fT and fmax; better gain-bandwidth product; critical for RF and mixed-signal circuits

**Reliability Considerations:**
- **Dielectric Breakdown**: spacer must withstand operating voltage for 10 years; breakdown field >5 MV/cm; TDDB testing required
- **Thermal Cycling**: spacer must survive thermal cycling without cracking; CTE mismatch with Si causes stress; stress management critical
- **Moisture Absorption**: low-k materials may absorb moisture; degrades dielectric constant and reliability; hermetic sealing required
- **Interface Stability**: spacer-Si interface must be stable; no delamination or void formation; affects long-term reliability

**Design Implications:**
- **Parasitic Extraction**: accurate inner spacer capacitance models required; affects timing and power analysis; 3D field solver for extraction
- **Library Characterization**: standard cells characterized with inner spacer parasitics; different spacer thickness options may require separate libraries
- **Timing Closure**: reduced capacitance improves timing; may enable higher frequency targets; affects design optimization
- **Power Analysis**: reduced dynamic power from lower capacitance; affects power budget and thermal design

**Industry Implementation:**
- **Samsung**: implemented inner spacers in 3nm GAA (2022); SiOCN material; 5-7nm thickness; production-proven
- **TSMC**: inner spacers in N3 and N2 nodes; optimized for performance and reliability; conservative material choice (SiN)
- **Intel**: inner spacers in Intel 20A and 18A; exploring air gap spacers for future nodes; aggressive roadmap
- **imec**: pioneered inner spacer research; demonstrated various materials and architectures; industry collaboration

**Cost and Yield:**
- **Process Cost**: inner spacer adds 3-5 mask layers; ALD deposition, etch, metrology; +5-10% wafer processing cost
- **Yield Impact**: void formation and etch damage are yield detractors; requires mature process; target >98% yield for inner spacer steps
- **Metrology**: TEM cross-sections for thickness and void inspection; inline metrology challenging; affects cycle time and cost
- **Rework**: inner spacer defects often not reworkable; scrap wafer if critical defects found; emphasizes need for process control

**Comparison with FinFET:**
- **FinFET Spacers**: only outer spacers on fin sidewalls; no inner spacers needed; simpler process
- **GAA Advantage**: inner spacers enable aggressive nanosheet pitch scaling; FinFET limited by fin pitch; GAA provides better density
- **Capacitance**: GAA with inner spacers has 20-30% lower gate capacitance than FinFET at same performance; GAA advantage
- **Complexity**: GAA inner spacers add process complexity; but performance benefit justifies cost; necessary for GAA viability

**Future Trends:**
- **Thinner Spacers**: future nodes may use 2-4nm spacers; requires advanced ALD; challenges for conformal deposition
- **Lower-k Materials**: exploring k<4 materials; porous dielectrics, air gaps; 60-70% capacitance reduction potential
- **Selective Deposition**: area-selective ALD to deposit spacer only in cavities; eliminates etch step; simplifies process; research phase
- **Forksheet and CFET**: inner spacer technology extends to future architectures; critical for vertical stacking; enables continued scaling

Inner Spacer Engineering is **the enabling technology for high-performance GAA transistors** — by forming low-k dielectric spacers between nanosheets, inner spacers reduce parasitic capacitance by 30-50% and improve switching speed by 15-25%, making them essential for achieving the performance targets of 3nm and 2nm nodes while enabling aggressive pitch scaling that would otherwise be limited by gate-to-source/drain shorts and excessive capacitive coupling.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/inner-spacer-engineering) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
