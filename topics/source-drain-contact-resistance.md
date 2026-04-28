# Source/Drain Contact Resistance

**Keywords**: source drain contact resistance,sd contact resistance,contact resistivity reduction,metal semiconductor contact,silicide contact resistance

---

**Source/Drain Contact Resistance** is **the electrical resistance at the interface between metal contacts and the heavily doped source/drain regions of transistors** — representing 30-50% of total transistor on-resistance at advanced nodes (3nm, 2nm), limiting drive current by 20-40% compared to ideal devices, and requiring aggressive contact area scaling, silicide engineering, and novel contact metals (Ni, Co, Ru, W) to achieve target contact resistivity <1×10⁻⁹ Ω·cm² while maintaining reliability and manufacturability at contact dimensions below 20nm.

**Contact Resistance Fundamentals:**
- **Definition**: Rc = ρc/Ac where ρc is contact resistivity (Ω·cm²) and Ac is contact area (cm²); total resistance includes spreading resistance and bulk resistance
- **Scaling Challenge**: as contact area shrinks (20nm × 20nm = 400nm² at 3nm node), resistance increases inversely; Rc ∝ 1/Ac; becomes dominant resistance component
- **Target Resistivity**: <1×10⁻⁹ Ω·cm² for high-performance logic; <5×10⁻⁹ Ω·cm² for low-power logic; <1×10⁻⁸ Ω·cm² for SRAM; challenging at high doping
- **Resistance Budget**: S/D contact resistance should be <30% of total Ron; at 3nm node, Rc target <50-100 Ω per contact; requires aggressive optimization

**Contact Resistance Components:**
- **Interface Resistivity (ρc)**: resistance at metal-semiconductor interface; depends on Schottky barrier height, doping concentration, and interface quality; dominant component
- **Spreading Resistance**: resistance in semiconductor as current spreads from small contact to larger S/D region; depends on contact size and doping profile
- **Bulk Resistance**: resistance in metal contact plug and S/D region; usually small compared to interface resistance; but significant for narrow contacts
- **Total Resistance**: Rc,total = Rc,interface + Rc,spreading + Rc,bulk; interface resistance dominates for contacts <30nm diameter

**Silicide Engineering:**
- **Nickel Silicide (NiSi)**: most common; low resistivity (10-20 μΩ·cm); low Schottky barrier (0.4-0.6 eV for n-type Si); forms at 300-500°C; mature process
- **Cobalt Silicide (CoSi₂)**: alternative to NiSi; resistivity 15-25 μΩ·cm; good thermal stability; higher formation temperature (500-700°C); used at some fabs
- **Titanium Silicide (TiSi₂)**: older technology; resistivity 15-20 μΩ·cm; higher barrier than NiSi; less common at advanced nodes
- **Silicide Thickness**: 5-15nm typical; thicker reduces resistance but consumes more Si; trade-off between resistance and junction depth

**Advanced Contact Metals:**
- **Ruthenium (Ru)**: emerging contact metal; low resistivity (7-15 μΩ·cm); excellent gap fill; enables smaller contacts; higher cost than W or Cu
- **Tungsten (W)**: traditional contact metal; resistivity 5-10 μΩ·cm; excellent gap fill; thermal stability >1000°C; mature process; but higher resistivity than Cu
- **Copper (Cu)**: lowest resistivity (1.7 μΩ·cm); but diffuses into Si; requires thick barriers; challenging for small contacts; used with barriers
- **Molybdenum (Mo)**: alternative to W; resistivity 5-8 μΩ·cm; good thermal stability; less mature process; emerging for advanced nodes

**Doping Optimization:**
- **High Doping Concentration**: >1×10²⁰ cm⁻³ required for low contact resistance; enables tunneling through Schottky barrier; reduces barrier width
- **Activation Annealing**: laser annealing or flash annealing at 1000-1300°C for <1ms; activates dopants without excessive diffusion; achieves >80% activation
- **Doping Profile**: box-like profile preferred; uniform high doping in contact region; minimizes spreading resistance; challenging to achieve
- **Dopant Species**: phosphorus (P) or arsenic (As) for n-type; boron (B) for p-type; solid solubility limits maximum concentration

**Contact Area Scaling:**
- **7nm Node**: contact diameter 25-30nm; area 500-700nm²; Rc target <100 Ω; achievable with NiSi and high doping
- **5nm Node**: contact diameter 20-25nm; area 300-500nm²; Rc target <150 Ω; requires optimized silicide and doping
- **3nm Node**: contact diameter 15-20nm; area 200-300nm²; Rc target <200 Ω; challenging; requires advanced metals (Ru) or novel approaches
- **2nm Node**: contact diameter 12-18nm; area 150-250nm²; Rc target <250 Ω; extremely challenging; may require alternative contact schemes

**Novel Contact Approaches:**
- **Selective Metal Deposition**: deposit contact metal only on S/D regions; eliminates etch step; reduces damage; improves contact resistance by 20-30%
- **Dopant Segregation**: segregate dopants (As, Sb) at metal-Si interface; reduces Schottky barrier; improves contact resistivity by 2-5×; requires precise control
- **Graphene Interlayer**: insert graphene layer between metal and Si; reduces barrier; improves contact resistivity; research phase; integration challenges
- **Semimetal Contacts**: use semimetals (Bi, Sb) as contact material; lower barrier than conventional metals; research phase; manufacturability unknown

**Measurement Techniques:**
- **Transfer Length Method (TLM)**: standard technique; measures resistance vs contact spacing; extracts contact resistivity and sheet resistance; requires test structures
- **Cross-Bridge Kelvin Resistor (CBKR)**: four-point measurement; eliminates lead resistance; more accurate than TLM; requires larger test structures
- **Transmission Line Model (TLM)**: variant of TLM; accounts for current crowding; more accurate for small contacts; widely used
- **Conductive AFM**: atomic force microscopy with conductive tip; measures local contact resistance; nanoscale resolution; research tool

**Impact on Transistor Performance:**
- **Drive Current Reduction**: high contact resistance reduces Ion by 20-40% vs ideal device; limits frequency and performance
- **On-Resistance**: Rc contributes 30-50% of total Ron at 3nm node; becomes dominant resistance component; must be minimized
- **Delay Impact**: increased Ron increases RC delay; 10-20% delay penalty from contact resistance; affects timing closure
- **Power Impact**: higher resistance increases I²R power loss; 5-10% power penalty; affects power budget and thermal design

**Reliability Considerations:**
- **Electromigration**: high current density (1-5 MA/cm²) in small contacts; metal migration risk; requires lifetime testing; target >10 years
- **Stress Migration**: thermal cycling causes stress; void formation at contact interface; affects reliability; stress management critical
- **Contact Spiking**: metal diffusion into Si junction; causes leakage or shorts; barrier layers prevent spiking; TiN or TaN barriers 2-5nm thick
- **Time-Dependent Breakdown**: high electric field at contact interface; dielectric breakdown risk; affects long-term reliability

**Process Integration:**
- **Contact Etch**: anisotropic etch through dielectric to S/D; high aspect ratio (3:1 to 5:1); critical dimension control ±2nm; avoid Si damage
- **Cleaning**: remove etch residue and native oxide; HF dip or plasma clean; critical for low contact resistance; surface preparation
- **Barrier/Liner**: deposit TiN or TaN barrier (2-5nm); prevents metal diffusion; ALD for conformal coating; must not increase total resistance
- **Metal Fill**: CVD or electroplating of W, Cu, or Ru; void-free fill critical; overfill and CMP; planarization for subsequent layers

**Design Implications:**
- **Contact Sizing**: larger contacts reduce resistance but increase area; trade-off between performance and density; design rules specify minimum size
- **Contact Redundancy**: multiple contacts per S/D reduce resistance and improve reliability; but increase area; used for critical paths
- **Layout Optimization**: contact placement affects resistance and parasitic capacitance; EDA tools optimize contact layout for timing
- **Resistance Modeling**: accurate contact resistance models in SPICE; affects timing and power analysis; extraction from test structures

**Industry Approaches:**
- **TSMC**: NiSi silicide with W contacts at N5 and N3; exploring Ru contacts for N2; conservative approach; proven reliability
- **Samsung**: Co silicide with W contacts at 3nm GAA; optimized doping and annealing; aggressive contact scaling
- **Intel**: NiSi with selective Ru contacts at Intel 4 and Intel 3; exploring dopant segregation for Intel 18A; innovative approaches
- **imec**: researching graphene interlayers, semimetal contacts, and selective deposition; industry collaboration for future nodes

**Cost and Yield:**
- **Process Cost**: contact formation adds 5-10 mask layers; etch, clean, deposition, CMP; +10-15% of total wafer cost
- **Yield Impact**: contact opens (high resistance) and shorts are major yield detractors; requires tight process control; target <1% defect rate
- **Metrology**: electrical test of contact resistance on test structures; inline monitoring; TEM for physical inspection; affects cycle time
- **Rework**: contact defects often not reworkable; scrap wafer if critical defects found; emphasizes need for process control

**Scaling Roadmap:**
- **Current Status (3nm)**: NiSi + W contacts; ρc ≈ 1-2×10⁻⁹ Ω·cm²; contact diameter 15-20nm; Rc ≈ 150-250 Ω
- **Near-Term (2nm)**: Ru contacts or dopant segregation; ρc target <1×10⁻⁹ Ω·cm²; contact diameter 12-18nm; Rc target <250 Ω
- **Long-Term (1nm)**: novel approaches (graphene, semimetals, selective deposition); ρc target <5×10⁻¹⁰ Ω·cm²; contact diameter <15nm
- **Fundamental Limits**: quantum mechanical tunneling limits minimum resistivity; ρc ≈ 1×10⁻¹⁰ Ω·cm² may be fundamental limit

**Comparison with Previous Nodes:**
- **28nm Node**: contact diameter 40-50nm; Rc ≈ 50-100 Ω; contact resistance <20% of total Ron; not a major concern
- **14nm/10nm Nodes**: contact diameter 30-40nm; Rc ≈ 100-150 Ω; contact resistance ≈20-30% of total Ron; becoming significant
- **7nm/5nm Nodes**: contact diameter 20-30nm; Rc ≈ 150-250 Ω; contact resistance ≈30-40% of total Ron; major concern
- **3nm/2nm Nodes**: contact diameter 15-20nm; Rc ≈ 200-350 Ω; contact resistance ≈40-50% of total Ron; dominant resistance component

**Future Outlook:**
- **Material Innovation**: exploring 2D materials (graphene, MoS₂), semimetals, and novel silicides; potential for 2-5× resistivity reduction
- **Process Innovation**: selective deposition, dopant segregation, and interface engineering; 20-50% resistance reduction potential
- **Architecture Changes**: alternative contact schemes (wrap-around contacts, backside contacts); may enable lower resistance
- **Fundamental Limits**: approaching quantum mechanical limits; further reduction beyond 1nm node may require paradigm shift

Source/Drain Contact Resistance is **the dominant resistance bottleneck at advanced nodes** — contributing 30-50% of total transistor on-resistance and limiting drive current by 20-40%, contact resistance requires aggressive optimization through silicide engineering, novel contact metals like ruthenium, dopant segregation, and potentially revolutionary approaches like graphene interlayers to achieve the sub-1×10⁻⁹ Ω·cm² resistivity needed for continued performance scaling at 3nm, 2nm, and beyond.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/source-drain-contact-resistance) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
