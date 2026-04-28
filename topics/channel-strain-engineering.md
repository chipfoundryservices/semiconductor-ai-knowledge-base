# Channel Strain Engineering

**Keywords**: channel strain engineering,strained silicon mobility,strain techniques transistor,stress engineering cmos,mobility enhancement strain

---

**Channel Strain Engineering** is **the technique of introducing controlled mechanical stress into the transistor channel to modify the silicon crystal lattice and enhance carrier mobility** — achieving 20-80% mobility improvement for electrons (nMOS) and 30-100% for holes (pMOS) through tensile or compressive strain, enabling 15-40% higher drive current at same gate length, and utilizing stress sources including strained epitaxial source/drain (eSi:C for nMOS, eSiGe for pMOS), stress liners (tensile SiN for nMOS, compressive SiN for pMOS), and substrate engineering to maintain performance scaling as transistors shrink below 10nm gate length.

**Strain Fundamentals:**
- **Mobility Enhancement**: strain modifies band structure; reduces effective mass; increases carrier mobility; tensile strain benefits electrons (nMOS); compressive strain benefits holes (pMOS)
- **Strain Types**: tensile strain (lattice stretched) increases electron mobility by 20-80%; compressive strain (lattice compressed) increases hole mobility by 30-100%
- **Strain Magnitude**: typical strain 0.5-2.0 GPa (0.5-2% lattice deformation); higher strain gives more mobility improvement; but reliability concerns above 2 GPa
- **Strain Direction**: uniaxial strain (along channel) most effective; biaxial strain (in-plane) also beneficial; triaxial strain (3D) less common

**Strained Source/Drain Epitaxy:**
- **SiGe for pMOS**: epitaxial Si₁₋ₓGeₓ with x=0.25-0.50 (25-50% Ge); larger Ge atoms create compressive strain in channel; 30-100% hole mobility improvement
- **Si:C for nMOS**: epitaxial Si with 0.5-2.0% carbon substitutional doping; smaller C atoms create tensile strain in channel; 20-50% electron mobility improvement
- **Growth Process**: selective epitaxial growth at 600-800°C; in-situ doping with B (pMOS) or P (nMOS); thickness 20-60nm; strain transfer to channel
- **Strain Transfer**: strain from S/D epitaxy transfers to channel through silicon lattice; effectiveness depends on S/D proximity to channel (5-20nm spacing)

**Stress Liner Technology:**
- **Tensile SiN for nMOS**: silicon nitride film with tensile stress (1-2 GPa); deposited over nMOS transistors; creates tensile strain in channel; 10-30% electron mobility improvement
- **Compressive SiN for pMOS**: silicon nitride film with compressive stress (1-2 GPa); deposited over pMOS transistors; creates compressive strain in channel; 15-40% hole mobility improvement
- **Dual Stress Liner (DSL)**: separate liners for nMOS and pMOS; requires additional mask; optimizes strain for both transistor types
- **Contact Etch Stop Layer (CESL)**: stress liner also serves as etch stop during contact formation; dual function; thickness 20-80nm

**Strain Mechanisms:**
- **Lattice Mismatch**: SiGe has 4% larger lattice constant than Si; creates compressive strain when grown on Si; Si:C has smaller lattice; creates tensile strain
- **Stress Transfer**: stress from S/D epitaxy or liner transfers to channel; magnitude depends on geometry, distance, and material properties
- **Band Structure Modification**: strain splits degenerate valleys in Si conduction band (nMOS) or valence band (pMOS); reduces effective mass; increases mobility
- **Scattering Reduction**: strain reduces phonon scattering; increases mean free path; further enhances mobility

**Mobility Enhancement:**
- **nMOS Electron Mobility**: unstrained Si: 400-500 cm²/V·s; with Si:C S/D: 500-700 cm²/V·s (25-40% improvement); with tensile liner: 550-750 cm²/V·s (30-50% improvement)
- **pMOS Hole Mobility**: unstrained Si: 150-200 cm²/V·s; with SiGe S/D: 250-400 cm²/V·s (60-100% improvement); with compressive liner: 200-300 cm²/V·s (30-50% improvement)
- **Combined Effect**: S/D strain + liner strain can be additive; total mobility improvement 50-150% possible; but diminishing returns above certain strain level
- **Saturation Effects**: mobility improvement saturates at high strain (>2 GPa) or high electric field; practical limit to strain engineering

**Process Integration:**
- **S/D Recess Etch**: etch Si in S/D regions; depth 20-60nm; creates cavity for epitaxial growth; critical dimension control ±2nm
- **Selective Epitaxy**: grow SiGe (pMOS) or Si:C (nMOS) in recessed regions; selective to Si; no growth on dielectric; temperature 600-800°C; growth rate 1-5 nm/min
- **Stress Liner Deposition**: plasma-enhanced CVD (PECVD) of SiN; control stress by deposition conditions (temperature, pressure, gas flow); thickness 20-80nm
- **Dual Liner Process**: deposit tensile liner; mask pMOS; etch nMOS liner; deposit compressive liner; mask nMOS; etch pMOS liner; 2 additional masks

**Performance Impact:**
- **Drive Current**: 15-40% higher Ion due to mobility enhancement; enables higher frequency or lower voltage at same performance
- **Transconductance**: 20-50% higher gm; improves analog circuit performance; better gain and bandwidth
- **Saturation Velocity**: strain increases saturation velocity by 10-20%; benefits short-channel devices; improves high-frequency performance
- **Threshold Voltage**: strain can shift Vt by ±20-50mV; must be compensated by work function or doping adjustment

**Strain in FinFET:**
- **Fin Strain**: strain in narrow fins (5-10nm width) differs from planar; quantum confinement affects strain; requires 3D strain modeling
- **S/D Epitaxy**: SiGe or Si:C grown on fin sidewalls; strain transfer to fin channel; effectiveness depends on fin width and height
- **Stress Liner**: liner wraps around fin; 3D stress distribution; more complex than planar; but still effective
- **Strain Relaxation**: narrow fins may partially relax strain; reduces effectiveness; requires optimization of fin geometry

**Strain in GAA/Nanosheet:**
- **Nanosheet Strain**: strain in suspended nanosheets (5-8nm thick, 20-40nm wide); different from bulk or fin; requires careful engineering
- **S/D Epitaxy**: SiGe or Si:C grown around nanosheet stack; strain transfer through nanosheet edges; effectiveness depends on sheet dimensions
- **Strain Uniformity**: achieving uniform strain across multiple stacked sheets challenging; top and bottom sheets may have different strain
- **Inner Spacer Impact**: inner spacers between sheets affect strain transfer; must be considered in strain engineering

**Reliability Considerations:**
- **Defect Generation**: high strain (>2 GPa) can generate dislocations or defects; reduces reliability; limits maximum strain
- **Strain Relaxation**: strain may relax over time at operating temperature; reduces mobility benefit; must be stable for 10 years
- **Electromigration**: strain affects electromigration in S/D and contacts; can improve or degrade depending on strain type; requires testing
- **Hot Carrier Injection (HCI)**: strain affects HCI; higher mobility increases carrier energy; may degrade HCI reliability; trade-off

**Design Implications:**
- **Mobility Models**: SPICE models must include strain effects; mobility as function of strain; affects timing and power analysis
- **Vt Compensation**: strain-induced Vt shift must be compensated; work function or doping adjustment; maintains target Vt
- **Layout Optimization**: strain effectiveness depends on layout; S/D proximity, liner coverage; layout-dependent effects (LDE)
- **Analog Design**: higher gm from strain benefits analog circuits; better gain, bandwidth, and noise; enables lower power analog

**Industry Implementation:**
- **Intel**: pioneered strain engineering at 90nm node (2003); continued through 14nm, 10nm, 7nm; SiGe S/D for pMOS, Si:C for nMOS, dual stress liners
- **TSMC**: implemented strain at 65nm node; optimized for each node; N5 and N3 use advanced strain techniques; SiGe with 40-50% Ge content
- **Samsung**: similar strain techniques; 3nm GAA uses strain in nanosheet channels; optimized S/D epitaxy and stress liners
- **imec**: researching advanced strain techniques for future nodes; exploring alternative materials and geometries

**Cost and Economics:**
- **Process Cost**: strain engineering adds 5-10 mask layers; epitaxy, liner deposition, additional lithography; +10-15% wafer processing cost
- **Performance Benefit**: 15-40% drive current improvement justifies cost; enables frequency targets or power reduction
- **Yield Impact**: epitaxy defects and strain-induced defects can reduce yield; requires mature process; target >98% yield
- **Alternative**: without strain, would need smaller gate length for same performance; strain enables performance at larger gate length; reduces cost

**Scaling Trends:**
- **28nm-14nm Nodes**: strain engineering mature; SiGe S/D with 25-35% Ge; dual stress liners; 30-60% mobility improvement
- **10nm-7nm Nodes**: increased Ge content (35-45%); optimized liner stress; 40-80% mobility improvement; critical for FinFET performance
- **5nm-3nm Nodes**: further optimization; 40-50% Ge; advanced liner techniques; strain in GAA nanosheets; 50-100% mobility improvement
- **Future Nodes**: approaching limits of strain engineering; >50% Ge difficult; alternative channel materials (Ge, III-V) may replace strained Si

**Comparison with Alternative Approaches:**
- **vs Channel Material Change**: strain is cheaper and more manufacturable than Ge or III-V channels; but lower mobility improvement; strain is near-term solution
- **vs Gate Length Scaling**: strain provides performance without gate length scaling; reduces short-channel effects; complementary to scaling
- **vs Voltage Scaling**: strain enables performance at lower voltage; reduces power; complementary to voltage scaling
- **vs Multi-Vt**: strain improves performance for all Vt options; complementary to multi-Vt design; both used together

**Advanced Strain Techniques:**
- **Embedded SiGe Stressors**: SiGe regions embedded in S/D; higher Ge content (60-80%); larger strain; but integration challenges
- **Strain-Relaxed Buffer (SRB)**: grow relaxed SiGe layer; then grow strained Si on top; biaxial strain; used in some SOI processes
- **Ge-on-Si**: grow Ge channel on Si substrate; high hole mobility (1900 cm²/V·s); but high defect density; research phase
- **III-V on Si**: grow InGaAs or GaAs on Si; ultra-high electron mobility (>2000 cm²/V·s); but integration challenges; research phase

**Future Outlook:**
- **Continued Optimization**: strain engineering will continue at 2nm and 1nm nodes; incremental improvements; approaching fundamental limits
- **Material Transition**: beyond 1nm, may transition to Ge or III-V channels; strain engineering in new materials; different techniques required
- **Heterogeneous Integration**: combine strained Si (logic) with Ge (pMOS) and III-V (nMOS) on same chip; ultimate performance; integration challenges
- **Quantum Effects**: at <5nm dimensions, quantum confinement affects strain; requires quantum mechanical modeling; new physics

Channel Strain Engineering is **the most successful mobility enhancement technique in CMOS history** — by introducing controlled tensile or compressive stress through epitaxial source/drain and stress liners, strain engineering achieves 20-100% mobility improvement and 15-40% higher drive current, enabling continued performance scaling from 90nm to 3nm nodes and beyond while providing a manufacturable and cost-effective alternative to exotic channel materials, making it an indispensable tool for maintaining Moore's Law in the face of fundamental scaling limits.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/channel-strain-engineering) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
