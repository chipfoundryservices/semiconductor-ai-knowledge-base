# Redistribution Layer (RDL)

**Keywords**: redistribution layer rdl,rdl process,fine line rdl,rdl lithography,rdl metallization

---

**Redistribution Layer (RDL)** is **the thin-film metal interconnect structure that reroutes I/O from chip pads to package bumps or between die in advanced packages** — achieving 2/2μm to 10/10μm line/space, 2-10 metal layers, <1Ω/mm resistance, enabling fan-out packaging, 2.5D interposers, and heterogeneous integration with 500-5000 I/O connections at 0.15-0.5mm pitch for applications from mobile processors to AI accelerators.

**RDL Structure and Materials:**
- **Metal Layers**: Cu electroplating most common; 2-10 layers typical; thickness 2-10μm per layer; seed layer Ti/Cu or Ta/Cu by sputtering; photolithography for patterning
- **Dielectric Layers**: polyimide (PI) or polybenzoxazole (PBO) between metal layers; spin-coat or laminate; thickness 5-15μm; dielectric constant 2.8-3.5; low CTE (<30 ppm/°C) for reliability
- **Via Formation**: photolithography or laser drilling; via diameter 10-50μm; aspect ratio 1:1 to 2:1; Cu fill by electroplating; connects metal layers
- **Passivation**: final protective layer; polyimide or solder resist; thickness 5-20μm; openings for bump pads; protects RDL from environment

**RDL Fabrication Processes:**
- **Semi-Additive Process (SAP)**: sputter thin seed layer (0.1-0.5μm); photolithography defines pattern; electroplate Cu (2-10μm); strip resist; etch seed layer; fine-line capability (2/2μm)
- **Subtractive Process**: sputter or electroplate thick Cu (5-15μm); photolithography; wet or dry etch Cu; coarser lines (10/10μm); simpler but less precise
- **Dual Damascene**: deposit dielectric; etch trenches and vias; fill with Cu; CMP planarization; borrowed from BEOL; used for finest pitch (<2μm)
- **Process Selection**: SAP for fine-line (<5μm); subtractive for coarse-line (>10μm); dual damascene for ultra-fine (<2μm); cost-performance trade-off

**Line Width and Pitch Scaling:**
- **Coarse RDL**: 10/10μm line/space; used in standard FOWLP, WLP; i-line lithography (365nm); mature process; low cost
- **Fine RDL**: 2/2μm to 5/5μm line/space; used in advanced FOWLP, 2.5D interposers; KrF lithography (248nm); higher cost but enables higher density
- **Ultra-Fine RDL**: <2/2μm line/space; research and development; ArF lithography (193nm) or EUV; for future ultra-high-density packages
- **Scaling Trend**: moving from 10μm to 2μm over past decade; driven by I/O density requirements; 1μm target for next generation

**Electrical Performance:**
- **Resistance**: 2-5μm thick Cu; sheet resistance 3-10 mΩ/sq; line resistance 0.5-2Ω/mm depending on width; lower than PCB traces (5-20Ω/mm)
- **Capacitance**: dielectric k=2.8-3.5; line-to-line capacitance 0.1-0.5 pF/mm; lower than on-chip interconnect (k=3-4); suitable for high-speed signals
- **Inductance**: 0.5-2 nH/mm depending on geometry; lower than wire bonds (1-5 nH/mm); enables multi-Gb/s signaling
- **Signal Integrity**: low R, L, C enable clean signal transmission; suitable for DDR, PCIe, USB, high-speed interfaces; simulation and optimization critical

**Applications by Package Type:**
- **FOWLP**: 2-6 RDL layers; 2/2μm to 10/10μm line/space; fan-out area for I/O redistribution; enables 500-2000 I/O; used in mobile processors, AI edge chips
- **2.5D Interposer**: 2-4 RDL layers on silicon; 0.4/0.4μm to 2/2μm line/space; ultra-high density; connects HBM to logic; bandwidth >1 TB/s
- **Panel-Level Packaging**: RDL on large panels (510×515mm); 5/5μm to 10/10μm typical; cost-effective for high volume; used in consumer, IoT
- **Chip-on-Wafer (CoW)**: RDL on wafer before die attach; adaptive patterning compensates die placement variation; used in some FOWLP variants

**Design and Routing:**
- **Design Rules**: minimum line width, space, via size; design rule manual (DRM) from package house; typically 2-10× coarser than on-chip
- **Routing Density**: 50-200 wires per mm depending on pitch; sufficient for most applications; bottleneck is bump pitch, not RDL routing
- **Power Distribution**: dedicated power/ground planes or mesh; IR drop analysis critical; <50mV drop target; wide traces for low resistance
- **Signal Integrity**: impedance control (50Ω single-ended, 100Ω differential); length matching for high-speed buses; simulation with 3D EM tools

**Manufacturing Challenges:**
- **Overlay**: multi-layer RDL requires tight overlay; ±2-5μm depending on pitch; stepper alignment critical; warpage affects overlay
- **Uniformity**: Cu thickness uniformity ±10% across wafer/panel; affects resistance and impedance; plating optimization critical
- **Defects**: particles, scratches, opens, shorts; <0.1 defects/cm² target; cleanroom environment, process control essential
- **Yield**: RDL yield 95-98% typical; lower for fine-line; improving with process maturity; defects main yield detractor

**Equipment and Suppliers:**
- **Lithography**: Canon, Nikon i-line or KrF steppers; overlay ±1-3μm; throughput 50-100 wafers/hour; older generation tools cost-effective
- **Plating**: Ebara, Atotech, Technic for Cu electroplating; automated plating lines; thickness uniformity ±5-10%; throughput 100-200 wafers/hour
- **Metrology**: KLA, Onto Innovation for overlay, CD, film thickness; inline monitoring; critical for multi-layer RDL
- **Materials**: DuPont, HD MicroSystems, Fujifilm for polyimide; Rohm and Haas for photoresist; continuous development for finer pitch

**Cost and Economics:**
- **Process Cost**: $10-50 per wafer per RDL layer depending on pitch; fine-line more expensive; 2-6 layers typical; total RDL cost $50-300 per wafer
- **Yield Impact**: RDL defects reduce package yield by 2-5%; offset by functionality and performance benefits
- **Value Proposition**: enables high I/O density, heterogeneous integration; critical for advanced packages; cost justified by system-level benefits
- **Market Size**: RDL materials and equipment market $2-3B annually; growing 10-15% per year; driven by advanced packaging adoption

**Future Trends:**
- **Finer Pitch**: 1/1μm line/space for ultra-high density; requires ArF or EUV lithography; enables >5000 I/O packages
- **Thicker Metal**: 10-20μm Cu for low-resistance power delivery; challenges in patterning and stress; required for high-power devices
- **New Materials**: exploring Ru, Co for lower resistance; alternative dielectrics for lower k; improving performance
- **Hybrid Processes**: combine RDL with hybrid bonding; ultra-high bandwidth (>2 TB/s); next-generation heterogeneous integration

Redistribution Layer is **the critical interconnect technology that enables advanced packaging** — by providing flexible, high-density metal routing at package level, RDL enables fan-out packaging, 2.5D integration, and heterogeneous die integration with 500-5000 I/O connections, forming the foundation of modern advanced packaging that powers everything from smartphones to AI supercomputers.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/redistribution-layer-rdl-13786) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
