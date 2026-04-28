# Silicide Formation

**Keywords**: silicide formation process,nickel silicide nisi,salicide self aligned,silicide phase control,contact silicide optimization

---

**Silicide Formation** is **the self-aligned process of reacting metal with silicon to form low-resistivity metal silicide contacts on source/drain and gate regions — using nickel silicide (NiSi) as the standard material at sub-90nm nodes to achieve contact resistance <100Ω per contact, sheet resistance <10 Ω/sq, and reliable electrical connection between transistors and metal interconnects while managing phase stability, morphology, and narrow-line effects**.

**Salicide Process Flow:**
- **Self-Aligned Silicide (Salicide)**: metal deposited blanket over wafer; reacts with exposed silicon (S/D, gate) but not with dielectric (spacers, STI); unreacted metal selectively removed; silicide forms only where needed
- **Nickel Deposition**: 5-15nm Ni deposited by physical vapor deposition (PVD) at room temperature; thickness determines final silicide thickness (NiSi consumes 1.84:1 Si:Ni ratio)
- **First Anneal**: rapid thermal anneal (RTA) at 280-350°C for 30-60 seconds forms Ni₂Si phase; low-resistivity phase (20-30 μΩ·cm) but not the final target phase
- **Selective Etch**: unreacted Ni removed by wet etch (H₂SO₄/H₂O₂ or HCl/H₂O₂); Ni₂Si remains; critical selectivity >100:1 to avoid silicide attack

**Phase Transformation:**
- **Second Anneal**: RTA at 450-550°C for 30-60 seconds converts Ni₂Si to NiSi; NiSi has lower resistivity (14-20 μΩ·cm) and is the stable phase for device operation
- **Phase Sequence**: Ni + Si → Ni₂Si (280-350°C) → NiSi (400-550°C) → NiSi₂ (>750°C); must stop at NiSi phase; NiSi₂ has high resistivity (50-70 μΩ·cm) and is undesirable
- **Temperature Window**: NiSi stable from 400-750°C; narrow window compared to CoSi₂ (stable to 900°C); requires careful thermal budget management in subsequent processing
- **Kinetics**: Ni₂Si formation is Ni-diffusion limited; NiSi formation is Si-diffusion limited; different kinetics affect uniformity and morphology

**Platinum Addition:**
- **Ni(Pt) Alloy**: 5-10 atomic % Pt in Ni stabilizes NiSi phase to 800°C; prevents transformation to NiSi₂ during subsequent thermal processing
- **Deposition**: co-sputter Ni and Pt, or deposit Ni/Pt bilayer (Ni 10nm, Pt 1nm); Pt intermixes during first anneal
- **Phase Stability**: Ni₀.₉Pt₀.₁Si stable to 800°C; enables compatibility with higher thermal budgets (contact anneals, backend processing)
- **Resistivity**: Ni(Pt)Si has slightly higher resistivity (18-25 μΩ·cm) than pure NiSi but acceptable for improved thermal stability

**Narrow Line Effects:**
- **Agglomeration**: NiSi on narrow poly gates (<50nm width) agglomerates (breaks into islands) during anneal; causes high resistance and device failure
- **Driving Force**: surface energy minimization drives agglomeration; narrower lines have higher surface-to-volume ratio and agglomerate more readily
- **Pt Suppression**: Pt addition significantly suppresses agglomeration; enables reliable NiSi formation on 30-40nm wide lines
- **Alternative Solutions**: thinner Ni deposition, lower anneal temperature, or alternative metals (Co, Ti) for narrow gates

**Contact Resistance:**
- **Specific Contact Resistivity**: ρc = 1-5×10⁻⁸ Ω·cm² for NiSi on heavily-doped silicon (>10²⁰ cm⁻³); depends on doping concentration and silicide quality
- **Doping Dependence**: ρc decreases exponentially with doping; 10²⁰ cm⁻³ gives ρc ≈ 2×10⁻⁸ Ω·cm²; 10²¹ cm⁻³ gives ρc ≈ 5×10⁻⁹ Ω·cm²
- **Contact Resistance**: Rc = ρc/Area + spreading resistance; for 40nm×40nm contact, Rc ≈ 150-250Ω; scales inversely with contact area
- **Optimization**: maximizing S/D doping at contact interface (>2×10²⁰ cm⁻³) and ensuring uniform, thick NiSi (20-30nm) minimizes contact resistance

**Sheet Resistance:**
- **NiSi Sheet Resistance**: Rsh = ρ/t where ρ is resistivity and t is thickness; 20nm NiSi with ρ=18 μΩ·cm gives Rsh = 9 Ω/sq
- **Thickness Control**: thicker NiSi reduces Rsh but consumes more silicon; typical thickness 15-30nm balances resistance and junction depth
- **Uniformity**: Rsh variation <5% across wafer required; depends on Ni thickness uniformity and anneal temperature uniformity
- **Gate Resistance**: NiSi on poly gate provides low gate resistance; critical for high-frequency circuits where gate RC delay limits performance

**Silicon Consumption:**
- **Consumption Ratio**: NiSi formation consumes 1.84 atoms Si per 1 atom Ni; 10nm Ni consumes 18.4nm Si; must account for in junction depth budget
- **Junction Integrity**: excessive Si consumption can penetrate through shallow junctions (<30nm); causes junction leakage and shorts
- **Thickness Optimization**: Ni thickness chosen to provide adequate silicide thickness (15-25nm) without consuming excessive silicon
- **Raised S/D Benefit**: raised source/drain provides additional Si volume; enables thicker silicide without junction penetration concerns

**Morphology Control:**
- **Grain Structure**: NiSi forms polycrystalline film with grain size 20-50nm; grain boundaries affect resistivity and contact resistance
- **Texture**: preferred (002) or (211) texture provides lower resistivity; anneal conditions affect texture development
- **Roughness**: smooth NiSi surface (<2nm RMS) required for reliable contact formation; rough silicide increases contact resistance variation
- **Voiding**: incomplete silicide formation creates voids; causes high resistance and reliability failures; proper cleaning and anneal conditions prevent voiding

**Integration Challenges:**
- **Dopant Segregation**: dopants (B, P, As) segregate to NiSi/Si interface during silicidation; creates high-doping spike beneficial for contact resistance
- **Stress**: NiSi formation creates stress due to volume change; stress can affect channel mobility and device performance
- **Etch Selectivity**: silicide etch must not attack underlying silicon or adjacent dielectrics; requires careful chemistry selection and endpoint control
- **Thermal Budget**: subsequent processing must stay below NiSi stability limit (750°C for pure NiSi, 800°C for Ni(Pt)Si); limits backend thermal options

**Advanced Silicide Technologies:**
- **Cobalt Silicide (CoSi₂)**: used at 130nm-90nm nodes; higher thermal stability (900°C) but worse narrow-line behavior than NiSi; replaced by NiSi at 65nm
- **Titanium Silicide (TiSi₂)**: used at >250nm nodes; C49 to C54 phase transformation; poor scalability; obsolete for advanced nodes
- **Erbium Silicide (ErSi)**: low Schottky barrier for n-type silicon; research for improved NMOS contact resistance; not in production
- **Nickel-Platinum-Germanium**: for SiGe source/drain contacts; Ge incorporation affects phase formation and properties; requires optimized process

**Characterization Methods:**
- **Four-Point Probe**: measures sheet resistance; standard method for process monitoring; requires large test structures for accuracy
- **Cross-Bridge Kelvin Resistor (CBKR)**: measures specific contact resistivity; separates contact resistance from sheet resistance
- **X-Ray Diffraction (XRD)**: identifies silicide phases; confirms NiSi formation and absence of NiSi₂; monitors phase stability
- **Transmission Electron Microscopy (TEM)**: images silicide thickness, morphology, and interface quality; validates process optimization

**Reliability Considerations:**
- **Electromigration**: NiSi has good electromigration resistance; median time to failure >10⁷ hours at 105°C, 1 MA/cm²
- **Stress Migration**: thermal cycling causes stress-induced voiding; Pt addition improves stress migration resistance
- **Contact Spiking**: silicide penetration through shallow junctions causes shorts; proper thickness control prevents spiking
- **Aging**: NiSi properties stable over device lifetime at operating temperatures (<125°C); no significant resistance drift

Silicide formation is **the critical process that bridges the gap between silicon transistors and metal interconnects — nickel silicide's combination of low resistivity, low silicon consumption, and acceptable narrow-line behavior (with Pt addition) makes it the universal choice for contact formation in all advanced CMOS processes from 90nm to 7nm nodes, enabling the low contact resistance essential for high-performance scaled transistors**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/silicide-formation-process) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
