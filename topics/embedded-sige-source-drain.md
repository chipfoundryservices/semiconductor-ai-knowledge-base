# Embedded SiGe Source/Drain

**Keywords**: embedded sige source drain,sige epitaxy pmos,sige recess etch,sige stress engineering,selective epitaxial growth

---

**Embedded SiGe Source/Drain** is **the strain engineering technique that replaces silicon in PMOS source/drain regions with epitaxially-grown silicon-germanium alloy — exploiting the 4% larger lattice constant of SiGe to induce compressive stress in the channel when constrained by surrounding silicon, achieving 20-40% hole mobility enhancement and enabling aggressive PMOS performance scaling at 65nm node and beyond**.

**SiGe Epitaxy Process:**
- **Recess Etch**: after gate and spacer formation, anisotropic reactive ion etch (RIE) removes silicon from source/drain regions; etch depth 40-100nm, width defined by spacer; Cl₂/HBr chemistry provides vertical profile with minimal lateral undercut
- **Recess Shape**: sigma-shaped recess (faceted sidewalls) vs rectangular recess; sigma recess provides more SiGe volume and higher stress but requires careful etch control; facet angles typically {111} or {311} planes
- **Cleaning**: post-etch clean removes native oxide and etch residue; dilute HF (DHF 100:1) followed by H₂ bake at 800-850°C in epitaxy chamber provides atomically clean silicon surface
- **Selective Epitaxy**: low-temperature epitaxy (550-700°C) grows SiGe only on exposed silicon, not on oxide or nitride surfaces; SiH₂Cl₂/GeH₄/HCl chemistry; HCl suppresses nucleation on dielectrics

**Germanium Content Optimization:**
- **Ge Concentration**: 20-40% Ge typical; higher Ge provides more stress but increases defect density and process complexity; 25-30% Ge optimal for most processes
- **Stress Generation**: 1% Ge mismatch generates approximately 100MPa compressive stress; 30% Ge produces 800-1200MPa channel stress depending on geometry
- **Lattice Mismatch**: SiGe lattice constant 4.2% larger than Si at 30% Ge; mismatch creates compressive stress when SiGe is constrained by surrounding silicon substrate
- **Critical Thickness**: SiGe films thicker than critical thickness (60-100nm for 30% Ge) relax stress through dislocation formation; recess depth must stay below critical thickness

**In-Situ Doping:**
- **Boron Incorporation**: B₂H₆ added during epitaxy provides in-situ p-type doping; active doping concentration 1-3×10²⁰ cm⁻³ achieves low contact resistance
- **Doping Uniformity**: boron concentration must be uniform throughout SiGe film; concentration gradients cause stress gradients and non-uniform contact resistance
- **Activation**: as-grown SiGe has >90% dopant activation; minimal additional activation anneal required; reduces thermal budget compared to implanted S/D
- **Segregation**: boron segregates to SiGe/Si interface during growth; can create high-doping spike at interface beneficial for contact resistance

**Stress Transfer Mechanism:**
- **Lateral Stress**: SiGe in S/D regions pushes laterally on channel silicon; compressive stress along channel direction (longitudinal) enhances hole mobility
- **Stress Magnitude**: channel stress 800-1200MPa for 30% Ge, 40-80nm recess depth, and 30-50nm gate length; stress increases with Ge content and recess depth
- **Gate Length Dependence**: shorter gates receive more stress; stress ∝ 1/Lgate approximately; 30nm gate has 1.5-2× stress of 60nm gate
- **Width Dependence**: narrow devices (<100nm width) have reduced stress due to STI proximity; stress modeling must account for 2D geometry effects

**Performance Enhancement:**
- **Mobility Improvement**: 30-50% hole mobility enhancement at 30% Ge; mobility improvement saturates above 35% Ge due to alloy scattering in SiGe
- **Drive Current**: 20-35% PMOS drive current improvement at same gate length and Vt; enables PMOS to match NMOS performance (historically PMOS 2-3× weaker)
- **Balanced Performance**: embedded SiGe combined with tensile NMOS stress (from CESL or SMT) provides balanced NMOS/PMOS performance; critical for circuit design
- **Scalability**: SiGe stress effectiveness increases at shorter gate lengths; provides continued benefit through 22nm node before FinFET transition

**Integration Challenges:**
- **Recess Control**: recess depth and profile uniformity critical; ±5nm depth variation causes 10-15mV Vt variation and 3-5% performance variation
- **Facet Formation**: uncontrolled faceting during epitaxy can cause non-uniform SiGe thickness and stress; facet angle control through growth conditions and HCl flow
- **Defect Formation**: threading dislocations from strain relaxation degrade junction leakage and reliability; defect density must be <10⁴ cm⁻² for acceptable yield
- **Gate-to-S/D Spacing**: SiGe must not contact gate; spacer width and lateral epitaxy control prevent SiGe-gate shorts; typical spacing 5-10nm

**Epitaxy Process Optimization:**
- **Temperature**: lower temperature (550-600°C) reduces dopant diffusion and provides better selectivity; higher temperature (650-700°C) improves crystal quality and growth rate
- **Growth Rate**: 5-15nm/min typical; slower growth provides better uniformity and selectivity; faster growth improves throughput
- **HCl Flow**: HCl/SiH₂Cl₂ ratio 0.1-0.5; higher HCl improves selectivity but reduces growth rate; optimization balances selectivity and throughput
- **Pressure**: 10-100 Torr; lower pressure improves uniformity; higher pressure increases growth rate

**Advanced SiGe Techniques:**
- **Graded SiGe**: Ge content graded from 20% at bottom to 40% at top; reduces defect density while maintaining high surface stress
- **SiGe:C**: carbon incorporation (0.2-0.5% C) suppresses boron diffusion and reduces defect density; enables higher Ge content without relaxation
- **Raised SiGe**: SiGe grown above original silicon surface (raised S/D); provides more SiGe volume for higher stress and lower contact resistance
- **Condensation**: grow thick SiGe, oxidize to consume Si and increase Ge concentration; can achieve 50-70% Ge for maximum stress

**Reliability Considerations:**
- **Junction Leakage**: defects in SiGe increase junction leakage; must maintain <1pA/μm leakage for acceptable off-state power
- **Contact Reliability**: NiSi formation on SiGe more complex than on Si; Ge segregation during silicidation affects contact resistance and reliability
- **Stress Relaxation**: high-temperature processing after SiGe formation causes partial stress relaxation; thermal budget management critical
- **Electromigration**: SiGe S/D regions have different electromigration characteristics than Si; contact and via design must account for SiGe properties

Embedded SiGe source/drain is **the most effective PMOS performance booster in planar CMOS history — the combination of significant mobility enhancement (30-50%), excellent scalability, and compatibility with other strain techniques made eSiGe standard in every advanced logic process from 65nm to 14nm, finally achieving balanced NMOS/PMOS performance after decades of PMOS being the weaker device**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/embedded-sige-source-drain) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
