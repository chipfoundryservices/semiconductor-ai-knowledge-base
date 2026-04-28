# Wafer-Level Packaging (WLP)

**Keywords**: wafer level packaging wlp,chip scale package,bumping process,wafer level redistribution,wlp assembly

---

**Wafer-Level Packaging (WLP)** is **the packaging technology that performs all assembly processes at wafer level before singulation, creating chip-scale packages where package size equals die size** — eliminating traditional package substrate, reducing package footprint by 50-80%, lowering cost by 30-50%, and enabling 0.4-0.5mm pitch I/O for mobile, IoT, and consumer applications with volumes exceeding 50 billion units annually.

**WLP Process Flow:**
- **Wafer Preparation**: start with tested good die on wafer; apply passivation layer (polyimide or BCB) 5-15μm thick; protects active circuits; provides mechanical support
- **Redistribution Layer (RDL)**: deposit and pattern metal traces (Cu or Al) to redistribute I/O from chip pads to bump locations; single or dual RDL; line width 2-10μm; enables area array I/O
- **Under Bump Metallization (UBM)**: deposit Ti/Cu or Ni/Au seed layer; defines bump locations; provides adhesion and diffusion barrier; thickness 0.5-2μm
- **Bump Formation**: electroplate solder bumps (SnAg, SnAgCu) or Cu pillars; bump height 50-150μm; pitch 0.4-0.5mm; reflow to form spherical shape
- **Wafer Singulation**: saw or laser dice wafer into individual packages; package size = die size; no substrate overhang; chip-scale package (CSP)

**WLP Variants:**
- **WLCSP (Wafer-Level Chip-Scale Package)**: simplest form; single RDL; solder bumps directly on wafer; lowest cost; used for memory, simple logic; pitch >0.5mm
- **eWLB (Embedded Wafer-Level Ball Grid Array)**: die embedded in molding compound on carrier; RDL on mold surface; enables fan-out; developed by Infineon; now industry standard
- **FOWLP (Fan-Out Wafer-Level Package)**: RDL extends beyond die edge; enables higher I/O count; multiple die integration; discussed separately (entry 13784)
- **WL-CSP with Stiffener**: add metal or polymer stiffener ring; improves board-level reliability; reduces warpage; used for larger die (>10mm)

**Materials and Processes:**
- **Passivation**: polyimide (PI) most common; BCB (benzocyclobutene) for low-k applications; spin-coat and cure; thickness 5-15μm; protects circuits from moisture
- **RDL Metal**: Cu electroplating for fine pitch (<5μm); Al sputtering for coarse pitch; seed layer (Ti/Cu) by sputtering; photolithography for patterning
- **Dielectric**: polyimide or polybenzoxazole (PBO) for RDL insulation; spin-coat between metal layers; thickness 5-10μm; low CTE (coefficient of thermal expansion) preferred
- **Solder Bumps**: SnAg (96.5/3.5) or SnAgCu (95.5/4/0.5) lead-free solder; electroplating or printing; reflow at 250-260°C; spherical shape after reflow

**Equipment and Suppliers:**
- **Coating**: Tokyo Electron, SUSS MicroTec for spin coating; EVG for lamination; throughput 50-100 wafers/hour
- **Lithography**: Canon, Nikon i-line steppers for RDL patterning; 2-5μm resolution; older generation tools sufficient; cost-effective
- **Plating**: Ebara, Atotech, Technic for Cu and solder electroplating; automated plating lines; 100-200 wafers/hour throughput
- **Bumping**: K&S, Kulicke & Soffa for stud bumping; ASMPT for mass reflow; specialized bumping houses (Amkor, ASE, JCET)

**Cost and Economics:**
- **Cost Advantage**: WLP eliminates substrate ($0.50-2.00 per unit); reduces assembly steps; 30-50% cost reduction vs traditional packaging; critical for cost-sensitive applications
- **Wafer-Level Economies**: process entire wafer simultaneously; 1000-5000 die per wafer; amortizes equipment cost; high throughput (100-200 wafers/hour)
- **Capital Investment**: $20-50M for complete WLP line; lower than traditional packaging line ($50-100M); faster ROI
- **Unit Cost**: $0.10-0.50 per package depending on complexity; competitive with traditional packages; enables $1-5 chip products

**Applications and Markets:**
- **Mobile Devices**: application processors, baseband, RF, power management; 40-50% of WLP volume; driven by smartphone/tablet demand
- **Memory**: DRAM, Flash in WLCSP; low-cost packaging for commodity memory; 20-30% of WLP volume
- **Sensors**: MEMS accelerometers, gyroscopes, pressure sensors; WLP protects sensitive structures; 10-15% of volume
- **IoT Devices**: Bluetooth, WiFi, MCU in WLP; small size critical for wearables, smart home; fastest growing segment

**Reliability and Challenges:**
- **Board-Level Reliability**: solder joint fatigue from CTE mismatch; die (2.6 ppm/°C) vs PCB (17 ppm/°C); underfill required for >5mm die; 1000-2000 thermal cycles typical
- **Warpage**: thin package warps during reflow; causes assembly issues; controlled by balanced RDL design, thicker passivation, stiffener ring
- **Moisture Sensitivity**: thin package absorbs moisture; popcorning during reflow; MSL (moisture sensitivity level) 3-4 typical; baking before assembly
- **Yield**: defects in RDL, bumping affect yield; 95-98% yield typical; lower than traditional packaging (98-99%); improving with process maturity

**Testing and Quality:**
- **Wafer-Level Test**: electrical test before packaging; probe all die; mark bad die; only package known good die; reduces packaging cost
- **Post-Package Test**: final electrical test after singulation; verify package integrity; 100% testing for high-reliability applications
- **Reliability Testing**: thermal cycling (-40 to 125°C, 1000 cycles); HAST (highly accelerated stress test); drop test for mobile applications
- **Inspection**: AOI (automated optical inspection) for RDL defects; X-ray for bump voids; SEM for cross-section analysis

**Advanced Developments:**
- **Fine Pitch WLP**: 0.3-0.4mm pitch for high I/O devices; requires advanced lithography; Cu pillar bumps for better reliability
- **Multi-Die WLP**: integrate multiple die in single package; system-in-package (SiP); requires precise die placement and RDL routing
- **Heterogeneous Integration**: combine logic, memory, RF, sensors; WLP enables compact integration; active research area
- **Thinner Packages**: <200μm total thickness for ultra-thin devices; challenges in handling and reliability; required for wearables

**Industry Adoption:**
- **OSAT Leaders**: Amkor, ASE, JCET, SPIL offer WLP services; combined capacity >100 billion units/year; continuous expansion
- **IDMs**: Intel, TI, STMicroelectronics have in-house WLP; vertical integration for cost and control
- **Foundries**: TSMC, UMC offer integrated WLP (InFO, FOWLP); one-stop solution for fabless customers
- **Market Size**: $5-7B annually; growing 8-10% per year; driven by mobile, IoT, automotive electronics

Wafer-Level Packaging is **the cost-effective solution that revolutionized semiconductor packaging** — by eliminating the substrate and performing all processes at wafer level, WLP achieves chip-scale packages at 30-50% lower cost, enabling the $1-5 chips that power billions of mobile devices, IoT sensors, and consumer electronics worldwide.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/wafer-level-packaging-wlp) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
