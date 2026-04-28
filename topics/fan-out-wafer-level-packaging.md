# Fan-Out Wafer-Level Packaging (FOWLP)

**Keywords**: fan out wafer level packaging,fowlp technology,embedded wafer level,info package tsmc,fan out process flow

---

**Fan-Out Wafer-Level Packaging (FOWLP)** is **the advanced packaging technology that embeds dies in mold compound and fabricates redistribution layers (RDL) on a reconstituted wafer — enabling I/O fan-out beyond the die perimeter, eliminating substrate costs, achieving 2-10μm RDL pitch, and reducing package thickness to 200-600μm while supporting multiple dies and passive components in a single package**.

**Process Flow:**
- **Die Attach**: known-good dies placed face-down on temporary carrier (glass or Si wafer) using thermal-release tape; die placement accuracy ±10-50μm; Besi Esec or ASM die placement tools; throughput 5,000-15,000 units per hour (UPH)
- **Compression Molding**: liquid or granular epoxy mold compound (EMC) fills space between dies and covers die backsides; compression molding at 175-185°C, 5-10 MPa pressure, 90-180 seconds cure time; Towa YPS or ASM AMICRA molding presses
- **Carrier Debonding**: thermal-release tape heated to 120-180°C; carrier wafer separated from reconstituted wafer; reconstituted wafer now has die faces exposed in mold compound matrix
- **RDL Fabrication**: 2-5 layers of Cu redistribution with polymer dielectric; connects die pads to fan-out I/O locations; 2-10μm line/space lithography; process identical to wafer-level RDL (seed, lithography, plating, dielectric coating)

**Technology Variants:**
- **eWLB (Infineon/STATS ChipPAC)**: first-generation FOWLP; single die per package; 2-3 RDL layers; 40μm line/space; used for power management ICs and RF transceivers; production since 2009
- **InFO (TSMC Integrated Fan-Out)**: advanced FOWLP with 2-10μm line/space; 4-5 RDL layers; supports multiple dies (logic + memory) and passive components; used in Apple A-series and M-series processors; production since 2016
- **InFO-PoP (Package-on-Package)**: InFO base package with memory package stacked on top; combines fan-out logic with high-density memory; total package height <1mm; used in iPhone and iPad processors
- **FOPLP (Fan-Out Panel-Level Package)**: fan-out on 510×515mm or 600×600mm panels instead of wafers; 4-9× larger area enables cost reduction; pilot production by ASE, Deca Technologies, and Nepes

**Mold Compound Properties:**
- **Composition**: epoxy resin (20-30%), silica filler (60-70%), hardener (5-10%), additives (flame retardant, stress modifier, adhesion promoter); Sumitomo EME-G700 series or Henkel Hysol
- **CTE**: 8-15 ppm/K (filler-dependent); lower CTE reduces warpage and stress; high filler loading (70%) achieves CTE closer to Si (2.6 ppm/K) but increases viscosity and voids
- **Moisture Absorption**: <0.3% after 168 hours at 85°C/85% RH; low absorption critical for reliability; moisture causes delamination and popcorning during reflow
- **Thermal Conductivity**: 0.8-1.5 W/m·K (filler-dependent); higher conductivity improves heat dissipation; thermally conductive fillers (AlN, BN) increase cost but enable higher power applications

**Warpage Management:**
- **Sources**: CTE mismatch between mold compound (8-15 ppm/K), Cu RDL (16.5 ppm/K), and Si die (2.6 ppm/K); process-induced stress from molding, RDL deposition, and thermal cycling
- **Magnitude**: reconstituted wafer warpage 500-2000μm across 300mm diameter after molding; increases to 1000-3000μm after RDL fabrication; excessive warpage causes lithography defocus and handling issues
- **Mitigation**: balanced RDL design (symmetric metal layers top and bottom); low-CTE mold compound; stress-relief anneals (150-200°C, 1-2 hours); temporary carrier support during RDL processing; optimized die placement pattern
- **Measurement**: shadow moiré or laser profilometry measures warpage at each process step; specification typically <500μm for lithography compatibility; KLA-Tencor WaferSight or Corning Tropel FlatMaster

**Die Shift and Placement Accuracy:**
- **Die Shift**: dies move during mold compound flow; typical shift 10-50μm from intended position; shift direction and magnitude depend on die size, spacing, and mold flow pattern
- **Impact**: die shift causes misalignment between die pads and RDL vias; >50μm shift may cause open circuits; alignment tolerance in RDL design must accommodate expected shift
- **Mitigation**: optimized mold compound viscosity and flow rate; pre-cure (B-stage) mold compound before full cure; die placement pattern optimization; vision-based die position measurement before RDL
- **Compensation**: measure actual die positions after molding; adjust RDL lithography mask alignment per die; requires flexible lithography system (stepper with die-by-die alignment)

**Advantages Over Flip-Chip BGA:**
- **Cost**: eliminates organic substrate ($5-20 per unit); wafer-level processing more efficient than unit-level assembly; 20-40% cost reduction for high-volume applications
- **Thickness**: 200-600μm total package thickness vs 800-1200μm for flip-chip BGA; critical for mobile devices with <7mm total thickness
- **Electrical Performance**: short RDL interconnects (1-5mm) vs long substrate traces (10-30mm); lower resistance (10-50 mΩ vs 50-200 mΩ) and inductance (0.5-2 nH vs 2-10 nH); enables higher frequency operation
- **Form Factor**: fan-out enables package size smaller than die size (for small dies) or larger than die size (for high I/O count); flexible I/O placement optimizes board-level routing

**Challenges:**
- **Yield**: die shift, warpage, RDL defects, and mold voids reduce yield; typical yield 85-95% vs >98% for flip-chip BGA; yield learning critical for cost competitiveness
- **Thermal Performance**: mold compound thermal conductivity (0.8-1.5 W/m·K) lower than substrate (3-5 W/m·K); limits power dissipation to 5-15W without heat spreader or heat sink
- **Design Complexity**: RDL routing, die placement optimization, and warpage simulation require specialized design tools; longer design cycle than standard packages
- **Equipment Investment**: dedicated molding, RDL, and inspection equipment; $50-200M capital investment for high-volume production line; justified only for high-volume products (>10M units/year)

**Applications:**
- **Mobile Processors**: Apple A-series (InFO), Qualcomm Snapdragon (InFO-AiP for antenna-in-package); combines logic, memory, and RF in thin, high-performance package
- **RF Front-End**: Qorvo and Skyworks use FOWLP for RF power amplifiers and antenna switches; low inductance and thin profile critical for 5G mmWave
- **Power Management**: Infineon and Texas Instruments use eWLB for power management ICs; cost-effective for medium I/O count (50-200 balls)
- **Automotive**: NXP and Renesas adopt FOWLP for automotive processors; reliability qualification (AEC-Q100) completed; production ramp for ADAS and infotainment applications

Fan-out wafer-level packaging is **the disruptive technology that eliminates the substrate bottleneck in advanced packaging — enabling thin, high-performance, cost-effective packages through wafer-level processing and RDL interconnects, fundamentally changing the economics of heterogeneous integration and system-in-package solutions for mobile, automotive, and IoT applications**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/fan-out-wafer-level-packaging) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
