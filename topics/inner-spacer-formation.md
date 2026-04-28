# Inner Spacer Formation

**Keywords**: inner spacer formation,inner spacer gaa,spacer dielectric deposition,inner spacer etch selectivity,spacer parasitic capacitance

---

**Inner Spacer Formation** is **the critical GAA transistor process module that deposits and patterns a low-k dielectric spacer between the nanosheet channel edges and the source/drain epitaxial regions — preventing gate-to-S/D capacitance and leakage while maintaining sub-5nm dimensions, requiring atomic-level control of conformal deposition, selective etching, and material engineering to achieve <1 fF/μm parasitic capacitance without compromising device reliability**.

**Inner Spacer Requirements:**
- **Dimensional Constraints**: thickness 3-5nm (thinner reduces S/D resistance, thicker reduces capacitance); length 5-8nm (distance from nanosheet edge to S/D); must fit in 10-15nm vertical gap between nanosheets; aspect ratio >2:1 for conformal filling
- **Dielectric Constant**: low-k material (k=4-5) preferred over SiN (k=7) or SiO₂ (k=3.9); 30-40% capacitance reduction with SiOCN (k=4.5) vs SiN; gate-to-S/D capacitance target <0.8 fF/μm for 3nm node
- **Etch Selectivity**: must survive SiGe release etch (selectivity to HCl vapor >1000:1); must survive gate stack etch and cleans; chemical stability in HF, H₂O₂, and organic solvents; thermal stability to 1000°C for dopant activation anneals
- **Mechanical Properties**: sufficient hardness to support suspended nanosheets during SiGe release; stress <500 MPa (tensile or compressive) to avoid nanosheet bending or cracking; adhesion to Si >1 J/m² to prevent delamination

**Deposition Processes:**
- **Plasma-Enhanced ALD (PEALD)**: SiOCN deposition using BTBAS (bis-tertiarybutylaminosilane) or BDEAS precursor + O₂ or N₂O plasma at 300-400°C; 0.1-0.15nm per cycle; 30-40 cycles for 4nm thickness; plasma power 50-200W; conformality >90% in 10nm gaps
- **Thermal ALD**: SiCO or SiOC deposition using DMDMOS (dimethyldimethoxysilane) + O₃ at 250-350°C; slower deposition (0.08nm/cycle) but better conformality (>95%); lower plasma damage to Si surfaces; preferred for sub-3nm nodes
- **CVD Alternatives**: PECVD SiOCN at 400-500°C using TEOS + NH₃ + CO₂; faster deposition (5-10nm/min) but poorer conformality (70-80%); step coverage inadequate for <5nm gaps; used only for relaxed-pitch designs
- **Composition Tuning**: C content 10-20% reduces k from 5.5 (SiON) to 4.5 (SiOCN); O:N ratio adjusted for etch selectivity (higher O improves HCl resistance); H content <5% for thermal stability; refractive index 1.6-1.8 indicates proper composition

**Patterning and Etch:**
- **Anisotropic Etch**: after conformal deposition, spacer material covers all surfaces; anisotropic plasma etch (CF₄/CHF₃/Ar chemistry) removes horizontal surfaces while preserving vertical spacers; etch selectivity to Si >10:1; endpoint detection by optical emission spectroscopy (OES)
- **Selective Removal**: spacer must be removed from nanosheet top/bottom surfaces and S/D regions while remaining between nanosheet edges and future S/D; etch stop on Si with <0.5nm Si loss; over-etch time <10% of main etch to prevent spacer thinning
- **Recess Control**: spacer recess (distance from nanosheet edge) controlled by etch time; target 5-8nm recess; ±1nm variation acceptable; excessive recess increases S/D resistance; insufficient recess increases gate-S/D capacitance and leakage
- **Damage Mitigation**: plasma etch creates surface damage (broken bonds, implanted ions) on Si nanosheets; post-etch clean (dilute HF + SC1) removes damage; H₂ anneal at 800°C for 60s passivates dangling bonds; interface trap density <5×10¹⁰ cm⁻²eV⁻¹ after repair

**Integration Challenges:**
- **Gap Fill**: 10nm vertical gap between nanosheets with 4nm spacer on each side leaves 2nm opening; precursor diffusion limited in narrow gaps; long purge times (5-10s vs 1s for planar) required; deposition rate decreases with depth (loading effect)
- **Pinch-Off Prevention**: if spacer deposits too quickly, gap entrance closes before interior fills (bread-loafing); creates voids that trap etchants and cause reliability failures; pulsed deposition (deposit 0.5nm, etch 0.2nm, repeat) prevents pinch-off
- **Uniformity**: spacer thickness variation <10% (3σ) across wafer and within die; non-uniformity causes Vt variation (thinner spacer → higher gate-S/D capacitance → slower switching); temperature uniformity <±2°C and pressure uniformity <±1% in ALD chamber required
- **SiGe Etch Compatibility**: inner spacer exposed during SiGe release; HCl vapor at 700°C attacks SiOCN slowly (0.1-0.2nm/min); 60s SiGe etch removes <10nm spacer thickness; densification anneal (900°C, N₂, 30s) before SiGe etch improves resistance

**Material Alternatives:**
- **SiOCN (Standard)**: k=4.5, good etch selectivity, moderate stress; most widely used; C incorporation reduces k but increases etch rate in HCl; optimal composition Si₃₂O₄₀C₁₅N₁₃
- **SiCO (Low-k)**: k=4.0-4.3, excellent capacitance reduction; lower etch selectivity to HCl (requires thicker initial deposition); higher stress (600-800 MPa tensile); used in performance-critical designs
- **SiN (High-k)**: k=7.0, excellent etch selectivity and thermal stability; 50% higher capacitance than SiOCN; used only when process simplicity outweighs performance (mature nodes, cost-sensitive products)
- **Air Gap (Ultimate Low-k)**: k=1.0, eliminate spacer material entirely; nanosheets suspended in air with only thin support posts; extreme fragility; requires protective encapsulation before subsequent processing; research stage for 1nm node

**Parasitic Capacitance Analysis:**
- **Capacitance Components**: gate-to-S/D overlap capacitance C_ov = ε₀·k·A/t where A is overlap area, t is spacer thickness; fringe capacitance C_fringe from field lines curving around spacer edges; total C_par = C_ov + C_fringe ≈ 0.6-0.8 fF/μm for optimized spacer
- **Impact on Performance**: parasitic capacitance adds to gate capacitance; increases CV²f dynamic power; slows switching speed (RC delay); 0.1 fF/μm capacitance reduction → 3-5% frequency improvement for logic circuits
- **Scaling Trends**: as nanosheet dimensions shrink, spacer thickness must scale proportionally; 2nm node targets 2-3nm spacer thickness with k<4; atomic layer precision required; alternative architectures (air gap, vacuum gap) under investigation
- **Measurement**: capacitance-voltage (CV) measurements on test structures; split-CV method separates intrinsic gate capacitance from parasitic; TEM cross-sections verify spacer dimensions and gap fill quality; STEM-EELS (electron energy loss spectroscopy) maps composition

Inner spacer formation is **the most challenging dielectric integration step in GAA transistor manufacturing — requiring the deposition of ultra-thin, low-k films in high-aspect-ratio nanoscale gaps with atomic-level precision, where even 1nm dimensional variation or 0.5 unit k-value change significantly impacts device performance, pushing ALD technology and materials science to their fundamental limits**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/inner-spacer-formation) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
