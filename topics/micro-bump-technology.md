# Micro-Bump Technology

**Keywords**: micro bump technology,copper pillar bump,fine pitch bumping,ubm under bump metallization,bump pitch scaling

---

**Micro-Bump Technology** is **the fine-pitch interconnect method using Cu pillars or solder bumps at 20-150μm pitch to connect die in 2.5D/3D packages** — achieving <10mΩ resistance per bump, >10,000 bumps per die, and enabling bandwidth >1 TB/s for HBM-logic connections, die-to-die communication in chiplets, and 3D stacking with applications in AI accelerators, GPUs, and HPC processors where conventional flip-chip bumps (>150μm pitch) lack sufficient density.

**Micro-Bump Structures:**
- **Cu Pillar Bump**: electroplated Cu pillar 20-50μm diameter, 30-80μm height; capped with solder (SnAg); provides mechanical support and electrical connection; most common for <100μm pitch
- **Solder Bump**: pure solder (SnAg, SnAgCu) bump; 30-100μm diameter; used for 100-150μm pitch; simpler than Cu pillar but less reliable at fine pitch
- **Cu-Cu Hybrid Bonding**: direct Cu-to-Cu connection without solder; <10μm pitch capability; discussed separately; next-generation technology
- **Bump Height**: 20-80μm typical; taller bumps accommodate die thickness variation; shorter bumps enable thinner packages; trade-off between compliance and package height

**Fabrication Process:**
- **UBM (Under Bump Metallization)**: sputter Ti/Cu or Ni/Au seed layer on wafer; thickness 0.5-2μm; provides adhesion and diffusion barrier; critical for reliability
- **Photolithography**: coat photoresist; expose and develop to define bump locations; critical dimension control ±2-5μm; overlay ±3-5μm
- **Cu Electroplating**: plate Cu pillar through photoresist openings; height 30-80μm; uniformity ±5μm; plating chemistry and current density optimized for uniformity
- **Solder Capping**: electroplate solder (SnAg 3-10μm thick) on Cu pillar; or deposit solder paste; reflow to form cap; provides wettability for bonding
- **Reflow**: heat to 250-260°C; solder melts and forms spherical cap; Cu pillar remains solid; final bump height 40-100μm after reflow

**Pitch Scaling and Density:**
- **Coarse Pitch**: 100-150μm; used in standard flip-chip; 1000-5000 bumps per die; mature technology; high yield (>99%)
- **Fine Pitch**: 40-100μm; used in 2.5D interposers, advanced FOWLP; 5000-20,000 bumps per die; Cu pillar required; yield 97-99%
- **Ultra-Fine Pitch**: 20-40μm; research and development; >20,000 bumps per die; challenges in lithography, plating uniformity; yield 95-97%
- **Scaling Limit**: <20μm pitch requires hybrid bonding; solder bump technology limited by lithography resolution and reflow process

**Electrical and Thermal Performance:**
- **Resistance**: 5-15mΩ per bump depending on diameter and height; lower than wire bond (50-100mΩ); enables high-current connections
- **Inductance**: 10-50pH per bump; 10-100× lower than wire bond (1-5nH); critical for high-frequency signals; enables multi-Gb/s per bump
- **Current Carrying**: 100-500mA per bump; limited by electromigration; parallel bumps for high-current power delivery; 10-100 bumps for power/ground
- **Thermal Conductivity**: Cu pillar provides thermal path; 400 W/m·K; helps heat dissipation from die; but solder interface (50 W/m·K) limits overall thermal performance

**Applications:**
- **HBM-Logic Connection**: 2.5D package with HBM memory on silicon interposer; 40-55μm pitch micro-bumps; >10,000 bumps per HBM stack; bandwidth 1-2 TB/s
- **Chiplet Integration**: connect multiple logic die in 2.5D/3D package; 40-100μm pitch; die-to-die bandwidth 100-500 GB/s; used in AMD EPYC, Intel Ponte Vecchio
- **3D Stacking**: stack logic on logic or memory on logic; through-silicon vias (TSV) and micro-bumps; enables compact 3D integration
- **Advanced FOWLP**: fine-pitch bumps (40-80μm) for high I/O count; 2000-5000 bumps per die; used in mobile processors, AI edge chips

**Reliability and Challenges:**
- **Electromigration**: high current density (10⁴-10⁵ A/cm²) causes Cu migration; design rules limit current per bump; redundant bumps for critical signals
- **Thermal Cycling**: CTE mismatch causes stress; Cu (17 ppm/°C) vs Si (2.6 ppm/°C); underfill required for reliability; 1000-2000 cycles typical
- **Solder Fatigue**: repeated thermal cycling causes solder crack propagation; Cu pillar improves reliability vs pure solder; taller pillars provide more compliance
- **Non-Wet Opens (NWO)**: solder doesn't wet properly; causes open circuit; flux chemistry and reflow profile critical; <10 ppm defect rate target

**Manufacturing Equipment:**
- **Plating**: Ebara, Atotech for Cu and solder electroplating; automated plating lines; thickness uniformity ±3-5μm; throughput 100-200 wafers/hour
- **Lithography**: Canon, Nikon i-line steppers for bump patterning; overlay ±2-3μm; critical for fine pitch; throughput 50-100 wafers/hour
- **Reflow**: BTU, Heller for mass reflow; N₂ atmosphere; peak temperature 250-260°C; profile control ±5°C; throughput 100-200 wafers/hour
- **Inspection**: KLA, Camtek for bump height, co-planarity measurement; AOI for defects; 100% inspection for critical applications

**Process Control and Metrology:**
- **Bump Height**: laser profilometry or white-light interferometry; target ±5μm uniformity; critical for bonding yield
- **Co-Planarity**: <10μm across die; ensures all bumps contact during bonding; measured by 3D optical profiler
- **Composition**: X-ray fluorescence (XRF) for solder thickness and composition; ±10% control; affects melting temperature and reliability
- **Defects**: AOI for missing bumps, bridging, contamination; <0.01 defects/cm² target; electrical test for opens/shorts

**Cost and Economics:**
- **Process Cost**: UBM $5-10 per wafer; lithography $10-20; plating $20-40; reflow $5-10; total $40-80 per wafer; fine pitch more expensive
- **Yield Impact**: bump defects reduce die yield by 1-3%; offset by functionality; critical for high-value die (AI, HPC)
- **Equipment Cost**: complete bumping line $20-40M; includes plating, lithography, reflow, inspection; significant capital investment
- **Market Size**: micro-bump materials and equipment $1-2B annually; growing 15-20% per year; driven by 2.5D/3D packaging adoption

**Industry Adoption:**
- **HBM Packages**: all HBM suppliers (SK Hynix, Samsung, Micron) use micro-bumps; 40-55μm pitch; production since 2015; mature technology
- **AMD EPYC/Instinct**: chiplet architecture with 2.5D interposer; 45-55μm pitch micro-bumps; production since 2019; high-volume
- **Intel Ponte Vecchio**: 3D stacking with micro-bumps and hybrid bonding; 40-50μm pitch; production 2022; advanced integration
- **TSMC CoWoS**: 2.5D packaging service; 40-45μm pitch micro-bumps; used by NVIDIA, AMD, Broadcom; leading foundry offering

**Future Developments:**
- **Finer Pitch**: 20-30μm pitch for higher density; requires advanced lithography and plating; enables >50,000 bumps per die
- **Hybrid Integration**: combine micro-bumps (40-100μm) with hybrid bonding (<10μm); multi-tier interconnect; optimal cost-performance
- **New Materials**: exploring alternative solders (SnBi, SnIn) for lower temperature; Cu-Ni alloy pillars for better electromigration resistance
- **Wafer-Level Bumping**: bump entire wafer before dicing; economies of scale; lower cost than die-level bumping; industry trend

Micro-Bump Technology is **the high-density interconnect that enables 2.5D and 3D integration** — by providing 20-150μm pitch connections with low resistance and inductance, micro-bumps enable the >1 TB/s bandwidth and >10,000 I/O connections required for HBM memory, chiplet architectures, and 3D stacking that power modern AI accelerators, GPUs, and HPC processors.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/micro-bump-technology) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
