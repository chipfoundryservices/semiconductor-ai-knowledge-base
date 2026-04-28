# Buried Power Rails (BPR)

**Keywords**: buried power rails,bpr technology,power rail in cell,subtractive bpr,additive bpr

---

**Buried Power Rails (BPR)** is **the advanced standard cell architecture that embeds VDD and VSS power rails within the transistor active region below the gate level** — reducing standard cell height by 15-30%, improving area scaling by 1.2-1.4×, and enabling continued logic density improvement at 5nm, 3nm, and 2nm nodes by eliminating the need for dedicated metal tracks for power delivery within the cell, where power rails are formed in shallow trenches in silicon or in the middle-of-line (MOL) dielectric.

**BPR Architecture:**
- **Rail Location**: power rails buried in shallow trenches (50-150nm deep) in silicon substrate or in MOL dielectric layers; located below M0 (local interconnect) layer; VDD and VSS rails run horizontally across cell
- **Rail Dimensions**: width 20-50nm; thickness 30-80nm; pitch 100-200nm; resistance 1-5 Ω/μm; must carry cell current without excessive IR drop
- **Cell Height Reduction**: eliminates M1 power rails; reduces cell height from 6-7 tracks to 4-5 tracks; 15-30% height reduction; enables smaller standard cells
- **Connection Method**: transistor source/drain regions connect to buried rails through contacts; short vertical connection; low resistance; simplified routing

**Fabrication Approaches:**
- **Subtractive BPR**: etch trenches in silicon substrate; deposit barrier/liner (TiN, 2-5nm); fill with metal (tungsten, ruthenium, or molybdenum); CMP to planarize; metal remains in trenches
- **Additive BPR**: deposit metal layer on silicon; pattern metal lines; deposit dielectric around metal; CMP to planarize; metal sits on silicon surface, not in trenches
- **MOL BPR**: form power rails in middle-of-line dielectric layers; above transistors but below M0; uses standard copper damascene process; easier integration than substrate BPR
- **Hybrid Approaches**: combine substrate and MOL rails; VDD in substrate, VSS in MOL (or vice versa); optimizes for different current requirements

**Key Advantages:**
- **Area Scaling**: 1.2-1.4× logic density improvement vs conventional cells; 15-30% smaller cell height; more transistors per mm²; critical for continued Moore's Law
- **Routing Resources**: M1 layer freed for signal routing; 20-30% more routing tracks available; reduces congestion; enables higher utilization
- **Parasitic Reduction**: shorter connections from transistor to power rail; lower resistance and capacitance; improves performance and reduces power
- **Design Flexibility**: enables new cell architectures; supports forksheet and CFET transistors; foundation for future scaling

**Subtractive BPR Process:**
- **Trench Formation**: shallow trench isolation (STI) process adapted for power rails; etch 50-150nm deep trenches in silicon; width 20-50nm; pitch 100-200nm
- **Barrier Deposition**: atomic layer deposition (ALD) of TiN or TaN barrier; thickness 2-5nm; conformal coating; prevents metal diffusion into silicon
- **Metal Fill**: chemical vapor deposition (CVD) of tungsten, ruthenium, or molybdenum; void-free fill critical; resistivity 10-30 μΩ·cm (higher than copper but acceptable for short rails)
- **CMP Planarization**: remove excess metal; planarize surface; dishing and erosion control critical; surface roughness <1nm
- **Contact Formation**: etch contacts through dielectric to buried rails; fill with tungsten or copper; connect transistor S/D to power rails

**Additive BPR Process:**
- **Metal Deposition**: deposit ruthenium, cobalt, or copper on silicon surface; thickness 30-80nm; blanket deposition or selective deposition
- **Patterning**: lithography and etch to define power rail lines; width 20-50nm; pitch 100-200nm; critical dimension control ±2nm
- **Dielectric Fill**: deposit oxide or low-k dielectric around metal rails; gap fill process; void-free fill between narrow rails; CMP to planarize
- **Integration**: subsequent transistor and contact formation; metal rails must survive high-temperature processing (>400°C)

**Material Selection:**
- **Tungsten (W)**: most common for subtractive BPR; resistivity 5-10 μΩ·cm; excellent gap fill; thermal stability >1000°C; mature process
- **Ruthenium (Ru)**: emerging material; resistivity 7-15 μΩ·cm; better electromigration than tungsten; enables thinner barriers; higher cost
- **Molybdenum (Mo)**: alternative to tungsten; resistivity 5-8 μΩ·cm; good thermal stability; less mature process
- **Copper (Cu)**: lowest resistivity (1.7 μΩ·cm) but diffuses into silicon; requires thick barriers; challenging for narrow trenches; used in MOL BPR

**Electrical Performance:**
- **Resistance**: 1-5 Ω/μm for buried rails; acceptable for cell-level power delivery; IR drop <10-20mV across typical cell
- **Current Capacity**: 0.5-2 mA/μm width; sufficient for standard cell current requirements; electromigration lifetime >10 years at operating conditions
- **Parasitic Capacitance**: 0.1-0.3 fF/μm to substrate; lower than M1 rails due to smaller dimensions; improves switching speed
- **Contact Resistance**: 10-50 Ω per contact to buried rail; must be minimized through barrier optimization and contact area

**Design Implications:**
- **Standard Cell Library**: complete redesign of cell library required; new cell heights (4-5 tracks vs 6-7); new power connection strategy
- **Place and Route**: EDA tools must understand BPR architecture; power planning simplified (no M1 power grid); but new design rules
- **Power Analysis**: IR drop analysis must include buried rails; different resistance model than M1 rails; new extraction methodology
- **Cell Characterization**: timing and power characterization with BPR parasitics; different delay and power models

**Integration Challenges:**
- **Process Complexity**: adds 5-10 mask layers to FEOL; increases process cost by 10-15%; yield risk from narrow trenches and gap fill
- **Thermal Budget**: buried rails must survive subsequent high-temperature processing; limits material choices; metal stability critical
- **Defect Sensitivity**: voids in narrow trenches cause open circuits; stringent defect control required; <0.01 defects/cm² target
- **Alignment**: buried rails must align to transistor active regions; ±10-20nm alignment tolerance; critical for contact formation

**Industry Adoption:**
- **Intel**: demonstrated BPR in 2019; production in Intel 18A (1.8nm) node; part of PowerVia backside PDN strategy
- **Samsung**: announced BPR for 3nm GAA node (2022 production); combined with forksheet transistors at 2nm
- **TSMC**: evaluating BPR for N2 (2nm) node; conservative approach; may adopt for N1 (1nm) or beyond
- **imec**: pioneered BPR research; demonstrated various approaches; industry collaboration for process development

**Cost and Economics:**
- **Process Cost**: +10-15% wafer processing cost; additional lithography, etch, deposition, CMP steps
- **Area Benefit**: 1.2-1.4× density improvement offsets higher process cost; net 10-25% cost reduction per transistor
- **Yield Risk**: narrow trench fill and defect sensitivity add yield loss; requires mature process; target >98% yield for BPR steps
- **Time to Market**: 2-3 years after initial GAA adoption; Samsung first to production (2022); industry adoption 2022-2026

**Comparison with Alternatives:**
- **vs Conventional M1 Rails**: BPR provides 15-30% cell height reduction and 20-30% more M1 routing resources; clear advantage for advanced nodes
- **vs Backside PDN**: complementary technologies; BPR reduces cell height, backside PDN improves global power delivery; can combine both
- **vs Thicker M1 Rails**: thicker M1 reduces resistance but increases capacitance and doesn't save area; BPR is superior
- **vs Multiple M1 Power Tracks**: adding M1 tracks increases cell height; opposite of BPR goal; BPR is better for density

**Reliability Considerations:**
- **Electromigration**: buried rails must meet 10-year lifetime at operating current density; 1-5 mA/μm²; material and geometry optimization
- **Stress Migration**: thermal cycling causes stress in buried metal; void formation risk; requires stress management
- **Time-Dependent Dielectric Breakdown (TDDB)**: dielectric around buried rails must withstand operating voltage; >10 years at 0.7-0.9V
- **Contact Reliability**: contacts to buried rails must be reliable; resistance drift <10% over lifetime; barrier integrity critical

**Future Evolution:**
- **Narrower Rails**: future nodes may use 10-20nm width rails; requires advanced patterning (EUV, SADP); lower resistance per unit width
- **Alternative Materials**: exploring graphene, carbon nanotubes, or 2D materials for ultra-low resistance; research phase
- **3D Integration**: BPR enables power delivery in monolithic 3D structures; power rails for multiple transistor tiers
- **Heterogeneous Integration**: BPR in logic dies combined with backside PDN; optimized power delivery for chiplet architectures

Buried Power Rails represent **the most significant standard cell architecture change in 20 years** — by embedding power rails below the gate level, BPR reduces cell height by 15-30% and enables continued logic density scaling at 3nm, 2nm, and beyond, providing a critical foundation for future transistor architectures like forksheet and CFET while freeing up routing resources for increasingly complex signal interconnects.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/buried-power-rails) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
