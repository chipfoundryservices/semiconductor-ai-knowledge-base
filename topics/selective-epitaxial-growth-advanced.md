# Selective Epitaxial Growth (Advanced)

**Keywords**: selective epitaxial growth advanced,selective epi source drain,epi growth selectivity,facet engineering epitaxy,defect free epitaxy

---

**Selective Epitaxial Growth (Advanced)** is **the precision crystal growth technique that deposits single-crystal semiconductor material only on exposed crystalline surfaces while preventing deposition on dielectric surfaces — enabling the formation of raised source/drain regions, channel strain engineering, and heterogeneous material integration with atomic-layer control, facet engineering, and defect densities below 10⁴ cm⁻² required for sub-3nm CMOS nodes**.

**Growth Fundamentals:**
- **Selectivity Mechanism**: precursor molecules (SiH₄, Si₂H₆, GeH₄) decompose on Si surfaces catalyzed by dangling bonds; dielectric surfaces (SiO₂, SiN) lack dangling bonds and remain passivated; HCl or Cl₂ etchant added to gas mixture preferentially etches polycrystalline nuclei on dielectrics while leaving single-crystal growth intact
- **Growth Window**: temperature and pressure range where selectivity is maintained; typical window 550-750°C, 10-100 Torr for Si epitaxy; outside window, either no growth (too low T) or loss of selectivity (too high T); HCl:precursor ratio 0.1-10 tunes selectivity vs growth rate
- **Facet Formation**: epitaxial growth on patterned surfaces forms crystallographic facets; {111} and {311} facets dominate for <100> Si substrates; facet angles determined by surface energy minimization; diamond-shaped S/D profile results from {111} facet formation
- **Growth Rate**: 0.5-5 nm/min for selective Si; 1-10 nm/min for SiGe; faster growth increases throughput but reduces selectivity and increases defects; multi-step growth (fast nucleation, slow bulk growth) optimizes quality and throughput

**Source/Drain Epitaxy:**
- **NMOS S/D (SiP)**: Si epitaxy with in-situ P doping using PH₃; growth temperature 650-700°C; P concentration 1-3×10²¹ cm⁻³ (solid solubility limit ~2×10²¹ cm⁻³); higher doping reduces contact resistance but increases junction leakage; growth rate 2-4 nm/min
- **PMOS S/D (SiGe:B)**: SiGe epitaxy with in-situ B doping using B₂H₆; growth temperature 550-600°C (lower than NMOS to prevent B out-diffusion); Ge content 30-40% for strain; B concentration 1-2×10²¹ cm⁻³; growth rate 1-3 nm/min; Ge composition uniformity <2% required
- **Raised S/D Structure**: epitaxial S/D grows 20-50nm above original Si surface; reduces S/D series resistance by increasing cross-sectional area; enables larger contact area without increasing junction capacitance; critical for sub-20nm gate length devices
- **Merge and Facet Control**: adjacent S/D regions merge between fins or nanosheets; facet angle controls merge height and S/D resistance; {111} facets (54.7° angle) preferred for low resistance; growth conditions (temperature, pressure, HCl ratio) tune facet angles ±5°

**Strain Engineering:**
- **Compressive Strain (PMOS)**: SiGe S/D with 30-40% Ge has 1.5-2% larger lattice constant than Si channel; induces compressive strain in channel; increases hole mobility by 30-50% through valence band warping; strain magnitude proportional to Ge content and S/D volume
- **Tensile Strain (NMOS)**: SiP S/D with 1-2% P has slightly smaller lattice constant; induces weak tensile strain (~0.2%); additional tensile strain from contact etch stop layer (CESL) or stress memorization technique (SMT); electron mobility enhancement 10-20%
- **Strain Relaxation**: strain relaxes through misfit dislocation formation if critical thickness exceeded; critical thickness for Si₀.₇Ge₀.₃ on Si is ~15nm; thicker S/D requires graded buffer layers or strain-compensating structures; defect density <10⁴ cm⁻² required for yield
- **Strain Measurement**: Raman spectroscopy measures Si phonon peak shift (520 cm⁻¹ unstrained); 1 cm⁻¹ shift ≈ 0.25 GPa stress; nano-beam electron diffraction (NBED) in TEM provides nanometer-scale strain maps; X-ray diffraction (XRD) measures average strain across wafer

**Defect Control:**
- **Threading Dislocations**: originate from lattice mismatch or surface contamination; propagate vertically through epitaxial layer; cause junction leakage (>10× increase per dislocation); density must be <10⁴ cm⁻² for acceptable yield; pre-epi clean (HF dip + H₂ bake) critical
- **Stacking Faults**: planar defects on {111} planes; caused by growth interruptions or contamination; increase junction leakage and reduce mobility; eliminated by continuous growth without interruption and ultra-clean chamber (<10¹⁰ atoms/cm³ O₂, H₂O)
- **Facet Defects**: {111} facets are slow-growing and accumulate impurities; can form micro-twins or stacking faults; Ge surfactant (0.1-1% Ge in Si growth) passivates {111} surfaces and reduces defects; growth temperature optimization minimizes facet defect density
- **Pattern Loading Effect**: growth rate varies with pattern density; dense patterns grow slower due to precursor depletion; causes S/D height non-uniformity across die; compensated by pressure adjustment or multi-zone heating in reactor

**Advanced Techniques:**
- **Cyclic Deposition-Etch (CDE)**: alternate growth and etch cycles; each cycle deposits 1-5nm then etches 0.5-2nm; improves selectivity by removing polycrystalline nuclei on dielectrics; enables growth on smaller features (<10nm) where conventional selective epi fails
- **Low-Temperature Epitaxy**: 400-500°C growth using Si₂H₆ or higher silanes (Si₃H₈); enables epitaxy after low-thermal-budget processes (metal gates, low-k dielectrics); growth rate 0.1-1 nm/min; higher defect density than high-temperature epi but acceptable for some applications
- **Ge Condensation**: grow SiGe layer; thermal oxidation consumes Si preferentially (Si:Ge oxidation ratio 10:1); Ge concentration increases in remaining layer; creates high-Ge-content (>50%) or pure Ge layers for PMOS channel or III-V integration
- **Heteroepitaxy**: grow III-V (InGaAs, InP) or Ge on Si for high-mobility channels; large lattice mismatch (4-8%) requires buffer layers; aspect ratio trapping (ART) confines dislocations to trench sidewalls; enables defect-free III-V regions for NMOS or photonics

**Process Integration:**
- **Pre-Epi Clean**: remove native oxide and contaminants; HF dip (1-2% HF, 30-60s) removes oxide; DI water rinse; H₂ bake in epi reactor (800-850°C, 1-2 min) desorbs residual oxygen and carbon; surface must be atomically clean (<0.01 ML contamination)
- **In-Situ Doping**: dopant precursor (PH₃, B₂H₆, AsH₃) mixed with Si precursor; doping concentration controlled by gas flow ratio; uniform doping throughout epitaxial layer; eliminates need for ion implantation and activation anneal; reduces thermal budget
- **Multi-Layer Structures**: grade Ge composition (0→30% over 10nm) to reduce strain and defects; cap high-Ge layer with Si (2-5nm) for better contact properties; alternating Si/SiGe layers for band engineering; each layer requires precise thickness and composition control
- **Post-Epi Anneal**: 900-1000°C spike anneal for 5-30 seconds; activates dopants (>80% activation); repairs crystal damage; redistributes dopants for abrupt junctions; must not cause excessive dopant diffusion (<2nm lateral diffusion)

**Characterization:**
- **TEM Cross-Section**: verifies S/D height, facet angles, merge quality, and defect density; STEM-EDS (energy dispersive spectroscopy) maps Ge and dopant distribution; sample preparation by FIB (focused ion beam) milling
- **SIMS (Secondary Ion Mass Spectrometry)**: measures dopant concentration profiles; depth resolution 2-5nm; detection limit 10¹⁶-10¹⁸ cm⁻³; verifies in-situ doping uniformity and abruptness
- **Electrical Testing**: S/D resistance measured by four-point probe or transmission line method (TLM); contact resistance extracted from TLM structures; junction leakage measured by I-V on diode test structures; target S/D resistance <100 Ω·μm
- **Defect Inspection**: optical microscopy detects large defects (>1μm); SEM inspection finds smaller defects (>50nm); defect density <0.1 cm⁻² for production-worthy process; defect review by TEM identifies root cause

Selective epitaxial growth is **the cornerstone of advanced CMOS S/D engineering — enabling the precise deposition of strain-inducing, heavily-doped crystalline materials with complex 3D geometries and atomic-level control, where the interplay of thermodynamics, kinetics, and surface chemistry must be mastered to achieve the defect-free, high-performance S/D structures that define modern nanometer-scale transistors**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/selective-epitaxial-growth-advanced) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
