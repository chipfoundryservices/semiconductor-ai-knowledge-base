# Selective Deposition Techniques

**Keywords**: selective deposition techniques,area selective deposition,self aligned deposition,bottom up fill,selective cvd

---

**Selective Deposition Techniques** are **the processes that deposit material only on specific surfaces or regions while preventing deposition on others** — enabling self-aligned fabrication, bottom-up fill of high aspect ratio features, and elimination of lithography/etch steps, reducing process complexity by 30-50% and improving alignment by 2-5nm for applications including spacer formation, contact metallization, and interconnect fabrication at 5nm, 3nm nodes.

**Selectivity Mechanisms:**
- **Surface Chemistry Selectivity**: exploit different surface reactivity; deposit on metal but not dielectric, or vice versa; based on chemical affinity of precursor to surface; typical selectivity 10:1 to >100:1
- **Inhibitor-Based Selectivity**: apply self-assembled monolayer (SAM) inhibitor to non-growth surface; blocks precursor adsorption; deposit on uninhibited surface; remove inhibitor after deposition; enables arbitrary pattern selectivity
- **Kinetic Selectivity**: control temperature, pressure, precursor flux to favor deposition on one surface; metastable selectivity; requires careful process control; selectivity 5:1 to 20:1 typical
- **Topography-Based Selectivity**: preferential deposition in recessed features vs field; bottom-up fill; driven by precursor diffusion and surface area; used for via/trench fill

**Selective CVD Processes:**
- **Selective Tungsten (W)**: deposit W on TiN barrier but not on SiO₂; WF₆ + H₂ chemistry; nucleation delay on oxide (50-100 cycles); selectivity >50:1; used for contact plug fill
- **Selective Cobalt (Co)**: deposit Co on metal (Cu, Co) but not on dielectric; Co(CO)₃NO precursor; thermal CVD at 150-200°C; selectivity >20:1; used for via bottom liner, contact metallization
- **Selective Silicon (Si)**: deposit Si on Si but not on SiO₂ or SiN; SiH₄ or Si₂H₆ precursor; epitaxial growth on Si; selectivity >100:1; used for source/drain epitaxy, channel formation
- **Selective SiN**: deposit SiN on Si but not on SiO₂; PEALD or thermal ALD; used for self-aligned spacer formation; selectivity 10:1 to 30:1

**Area Selective ALD (AS-ALD):**
- **SAM Inhibitor Approach**: deposit SAM (e.g., octadecyltrichlorosilane) on SiO₂; blocks ALD precursor; deposit metal (Pt, Ru, Co) on uninhibited metal surface; remove SAM with O₂ plasma or UV/ozone
- **Small Molecule Inhibitor**: use small molecules (acetylacetone, aniline) as inhibitors; co-dose with ALD precursor; preferentially adsorb on non-growth surface; enables selectivity without SAM patterning
- **Inherent Selectivity**: exploit different surface reactivity in ALD; TiO₂ deposits on OH-terminated surfaces but not on H-terminated; pattern surface termination for selectivity
- **Selectivity Window**: number of ALD cycles maintaining selectivity; typical 20-100 cycles (2-10nm thickness); limited by defects and nucleation on non-growth surface

**Bottom-Up Fill Applications:**
- **Via Fill**: selective metal deposition fills via from bottom up; eliminates voids; superior to top-down PVD; used for W, Co, Ru vias at 5nm/3nm nodes
- **Trench Fill**: selective deposition in trenches; conformal sidewall coverage; void-free fill; critical for high aspect ratio (>10:1) features
- **Gap Fill**: selective oxide or nitride deposition fills narrow gaps (<10nm); prevents pinch-off; used for shallow trench isolation (STI), inter-layer dielectric (ILD)
- **Contact Metallization**: selective Co or Ru deposition on contact bottom; reduces contact resistance; eliminates barrier/liner in some cases; 30-50% resistance reduction

**Self-Aligned Processes:**
- **Self-Aligned Contact (SAC)**: selective deposition on source/drain but not on gate; eliminates contact-to-gate alignment margin; enables aggressive scaling; 5-10nm area reduction per contact
- **Self-Aligned Via (SAV)**: selective via fill on lower metal but not on dielectric; eliminates via-to-metal alignment; reduces via resistance; critical for advanced interconnects
- **Self-Aligned Spacer**: selective SiN deposition on Si sidewall but not on gate; eliminates spacer etch; improves uniformity; reduces process steps by 2-3
- **Alignment Benefit**: self-aligned processes eliminate lithography alignment error (±2-3nm); improve device density 10-20%; reduce design rules

**Process Integration Challenges:**
- **Selectivity Loss**: defects, contamination cause nucleation on non-growth surface; selectivity degrades with thickness; typical limit 50-100 ALD cycles or 5-10nm CVD
- **Surface Preparation**: requires pristine surface; native oxide, contamination prevent selectivity; pre-clean critical; <0.1nm oxide thickness required
- **Thermal Budget**: many selective processes require 200-400°C; limits integration with temperature-sensitive materials; low-temperature alternatives under development
- **Uniformity**: selective deposition can have non-uniform thickness; loading effects in high aspect ratio features; optimization required for each application

**Equipment and Tools:**
- **Applied Materials Selectra**: dedicated platform for selective deposition and etch; integrated pre-clean, deposition, post-treatment; optimized for AS-ALD
- **Lam Research Striker**: selective Co deposition tool; CVD and ALD capability; production-proven for contact metallization
- **Tokyo Electron**: selective W, Co deposition tools; integrated with etch for self-aligned processes
- **ASM**: ALD tools with AS-ALD capability; research and development focus; exploring new chemistries

**Metrology and Process Control:**
- **Selectivity Measurement**: deposit on patterned wafer; measure thickness on growth vs non-growth surface; SEM cross-section, TEM for verification
- **Defect Inspection**: optical inspection for macro defects; SEM for micro defects; defect density <0.1/cm² required for production
- **Thickness Uniformity**: ellipsometry, XRF for thickness measurement; ±5% uniformity (3σ) target; challenging due to selectivity variations
- **Composition Analysis**: XPS, SIMS verify material purity; contamination from inhibitor or precursor decomposition; <1% impurity target

**Cost and Productivity:**
- **Process Simplification**: eliminates 2-4 lithography/etch steps per self-aligned process; 30-50% cost reduction for affected layers
- **Throughput**: selective ALD 20-40 wafers/hour; selective CVD 40-80 wafers/hour; comparable to conventional deposition
- **Yield Improvement**: self-alignment reduces defects from misalignment; 2-5% yield improvement typical; justifies adoption despite process complexity
- **Equipment Cost**: selective deposition tools $5-10M; similar to conventional deposition; integration complexity adds cost

**Industry Adoption and Future:**
- **Logic**: Intel, TSMC, Samsung adopt selective Co for contacts at 7nm/5nm; selective W for vias; self-aligned contacts in development
- **DRAM**: selective deposition for capacitor formation, contact plugs; 18nm DRAM and below; critical for scaling
- **3D NAND**: selective oxide deposition for gap fill; selective metal for word line; high aspect ratio challenges
- **Future Directions**: expand material portfolio (Ru, Mo, Ir); improve selectivity (>100:1, >100 cycles); lower temperature (<200°C); enable more self-aligned processes

Selective Deposition Techniques are **the enabler of self-aligned manufacturing** — by depositing material only where needed, these processes eliminate lithography steps, improve alignment, and enable bottom-up fill of challenging features, reducing process complexity and cost while improving device performance and yield at advanced nodes where conventional approaches reach fundamental limits.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/selective-deposition-techniques) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
