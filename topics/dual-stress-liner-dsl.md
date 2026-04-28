# Dual Stress Liners (DSL)

**Keywords**: dual stress liner dsl,tensile stress liner nmos,compressive stress liner pmos,stress liner deposition,cesl nitride film

---

**Dual Stress Liners (DSL)** are **the strain engineering technique that applies tensile silicon nitride films over NMOS transistors and compressive nitride films over PMOS transistors — using contact etch stop layers (CESL) with opposite intrinsic stress states to induce beneficial channel strain, achieving 15-30% performance improvement through stress-enhanced mobility without additional lithography layers beyond the block masks**.

**Stress Liner Fundamentals:**
- **Contact Etch Stop Layer (CESL)**: silicon nitride film deposited by plasma-enhanced CVD (PECVD) after silicide formation; serves dual purpose as etch stop during contact formation and stress-inducing layer
- **Intrinsic Film Stress**: as-deposited nitride films have intrinsic stress from 1-2.5GPa depending on deposition conditions; stress arises from atomic-scale mismatch between film and substrate
- **Stress Transfer**: film stress transfers to underlying silicon channel through mechanical coupling; stress magnitude in channel is 20-40% of film stress depending on film thickness, gate length, and geometry
- **Thickness**: CESL thickness 30-80nm; thicker films transfer more stress but increase process complexity and contact aspect ratio; typical thickness 50-60nm balances stress and integration

**Tensile Liner for NMOS:**
- **Deposition Conditions**: high RF power (300-600W), low pressure (2-6 Torr), low temperature (400-500°C), and SiH₄-rich chemistry produce tensile stress; high ion bombardment creates tensile film structure
- **Stress Magnitude**: 1.0-2.0GPa tensile stress in as-deposited film; higher stress provides more performance benefit but increases film cracking risk and integration challenges
- **Channel Stress**: 200-500MPa tensile stress induced in NMOS channel; stress magnitude scales inversely with gate length (shorter gates receive more stress)
- **Mobility Enhancement**: tensile longitudinal stress increases electron mobility 30-60%; 15-25% drive current improvement for NMOS at same gate length and Vt

**Compressive Liner for PMOS:**
- **Deposition Conditions**: low RF power (100-300W), high pressure (4-8 Torr), high NH₃/SiH₄ ratio produce compressive stress; low ion bombardment and high hydrogen content create compressive structure
- **Stress Magnitude**: 1.5-2.5GPa compressive stress; PMOS benefits more from higher stress than NMOS; compressive films more stable than tensile (less cracking)
- **Channel Stress**: 300-700MPa compressive stress in PMOS channel; combined with embedded SiGe S/D (if used), total compressive stress reaches 1.0-1.5GPa
- **Mobility Enhancement**: compressive longitudinal stress increases hole mobility 20-40%; 12-20% drive current improvement for PMOS

**Dual Liner Integration:**
- **Process Flow**: deposit tensile CESL blanket over entire wafer; pattern and etch tensile CESL from PMOS regions using block mask; deposit compressive CESL blanket; pattern and etch compressive CESL from NMOS regions using second block mask
- **Alternative Flow**: deposit compressive CESL first (more stable), remove from NMOS, deposit tensile CESL, remove from PMOS; order depends on film stability and etch selectivity
- **Mask Count**: DSL adds two mask layers (NMOS block and PMOS block); some processes combine with other block masks (Vt adjust, S/D implant) to minimize added masks
- **Etch Selectivity**: nitride etch must have high selectivity to underlying silicide (>20:1) and oxide spacers (>10:1); CHF₃/O₂ or CF₄/O₂ plasma provides required selectivity

**Stress Optimization:**
- **Film Thickness**: thicker CESL transfers more stress but increases contact aspect ratio; optimization typically yields 50-70nm for tensile, 40-60nm for compressive
- **Spacer Width**: wider spacers reduce stress transfer efficiency; stress scales approximately as 1/(spacer width); narrow spacers (8-12nm) maximize stress
- **Gate Length Dependence**: stress transfer efficiency ∝ 1/Lgate; 30nm gate receives 2× stress of 60nm gate from same liner; requires length-dependent modeling
- **Layout Effects**: stress varies with device width, spacing, and proximity to STI; isolated devices receive different stress than dense arrays; stress-aware OPC compensates

**Performance Impact:**
- **Drive Current**: combined NMOS and PMOS improvement averages 15-25% at same off-state leakage; enables 15-20% frequency improvement or equivalent power reduction
- **Variability**: stress-induced performance varies with layout; requires statistical models capturing stress-layout interactions; adds 3-5% performance variability
- **Reliability**: stress affects NBTI and HCI; compressive stress slightly worsens NBTI in PMOS; tensile stress has minimal HCI impact; overall reliability impact manageable
- **Temperature Dependence**: stress relaxation at high temperature reduces benefit; stress effect decreases 10-20% from 25°C to 125°C due to thermal expansion mismatch

**Advanced Techniques:**
- **Graded Stress Liners**: multiple CESL layers with different stress levels; bottom layer high stress for maximum channel impact, top layer lower stress for mechanical stability
- **Selective Stress**: apply high-stress liners only to critical paths; non-critical devices use single-liner or no-liner approach; reduces mask count while optimizing performance
- **Stress Memorization**: combine DSL with stress memorization technique (SMT) for additive stress effects; total stress 1.2-1.5× DSL alone
- **Hybrid Stress**: DSL combined with embedded SiGe (PMOS) and/or substrate strain; multiple stress sources provide 30-50% total performance improvement

**Integration Challenges:**
- **Film Cracking**: high tensile stress (>1.8GPa) causes film cracking, especially at corners and edges; crack propagation creates reliability risks; stress optimization balances performance and mechanical stability
- **Adhesion**: compressive films have poor adhesion to some surfaces; adhesion promoters or thin intermediate layers improve reliability
- **Thermal Budget**: post-CESL thermal processing (contact anneal, backend anneals) causes stress relaxation; 10-30% stress loss depending on thermal budget; requires compensation in initial stress target
- **CMP Interaction**: CESL hardness affects subsequent CMP processes; hard nitride films cause dishing and erosion; CMP recipe optimization required

Dual stress liners represent **the most widely adopted strain engineering technique in CMOS manufacturing — the combination of process simplicity (standard PECVD with different conditions), significant performance benefit (15-25%), and compatibility with other strain techniques makes DSL a standard feature in every advanced logic process from 90nm to 14nm nodes**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/dual-stress-liner-dsl) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
