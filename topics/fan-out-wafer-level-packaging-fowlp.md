# Fan-Out Wafer-Level Packaging (FOWLP)

**Keywords**: fan out wafer level packaging fowlp,fan out package,embedded wafer level,info package tsmc,fowlp rdl

---

**Fan-Out Wafer-Level Packaging (FOWLP)** is **the advanced packaging technology that redistributes I/O beyond the die edge by embedding die in molding compound and forming RDL on the reconstituted wafer** — enabling 2-10× higher I/O density than traditional WLP, supporting 0.2-0.4mm pitch, integrating multiple die with <100μm spacing, and powering flagship smartphones, AI accelerators, and HPC processors with TSMC InFO, Samsung FOPLP capturing 60-70% of premium mobile market.

**FOWLP Architecture and Process:**
- **Die Placement**: pick tested good die from wafer; place face-down on temporary carrier with adhesive; spacing 100-500μm between die; precision ±10μm required
- **Molding**: compression mold epoxy molding compound (EMC) around die; thickness 100-300μm; covers die backside; creates reconstituted wafer; 300mm format typical
- **Carrier Release**: remove temporary carrier; expose die face; clean adhesive residue; ready for RDL formation
- **RDL Formation**: deposit and pattern 2-6 metal layers; line/space 2/2μm to 10/10μm; via diameter 10-30μm; extends beyond die edge (fan-out); enables high I/O count
- **Bumping and Singulation**: form solder bumps or Cu pillars; saw into individual packages; package size larger than die (fan-out area); typical 1.2-2× die size

**FOWLP Variants:**
- **TSMC InFO (Integrated Fan-Out)**: chip-first process; RDL on die face; 2-6 RDL layers; used in Apple A-series, M-series processors; 40-50% of FOWLP market
- **Samsung FOPLP (Fan-Out Panel-Level Package)**: panel-based (510×515mm) instead of wafer; higher throughput; lower cost; used in Exynos processors
- **Deca M-Series**: chip-last process; RDL before die attach; adaptive patterning compensates die placement variation; used by Qualcomm, MediaTek
- **ASE FOCoS (Fan-Out Chip-on-Substrate)**: hybrid approach; FOWLP on substrate; combines benefits of both; used for high-performance applications

**Multi-Die Integration:**
- **Heterogeneous Integration**: integrate logic, memory, RF, power management in single package; die spacing 100-500μm; RDL connects die; system-in-package (SiP)
- **2.5D-Like Performance**: achieve near-2.5D bandwidth (100-500 GB/s) at lower cost; no silicon interposer; RDL provides die-to-die interconnect
- **Memory Stacking**: stack HBM or LPDDR on logic die; through-mold vias (TMV) for vertical connection; enables high-bandwidth memory access
- **Example**: Apple M1 Ultra uses InFO_LSI (locally silicon interconnect) to connect two M1 Max die; 2.5 TB/s bandwidth; seamless integration

**RDL Technology:**
- **Fine-Line RDL**: 2/2μm line/space for high-density routing; semi-additive process (SAP); Cu electroplating; 5-10 metal layers typical
- **Dielectric**: polyimide (PI) or polybenzoxazole (PBO); spin-coat or laminate; thickness 5-15μm per layer; low CTE (<30 ppm/°C) for reliability
- **Via Formation**: laser drill or photolithography; via diameter 10-30μm; aspect ratio 1:1 to 2:1; Cu fill by electroplating
- **Thickness**: total RDL stack 50-150μm; thinner than substrate (200-400μm); enables thin packages; critical for mobile devices

**Warpage Management:**
- **Warpage Challenge**: CTE mismatch between die (2.6 ppm/°C), mold (8-15 ppm/°C), RDL (17-25 ppm/°C); causes warpage up to 500μm for 300mm wafer
- **Mitigation Strategies**: balanced RDL design (symmetric metal distribution); low-CTE mold compound; thicker mold (200-300μm); carrier support during processing
- **Measurement**: shadow moiré, laser scanning measure warpage; <200μm target for assembly; <100μm for fine-pitch bumping
- **Impact**: excessive warpage causes assembly failures; bump co-planarity issues; yield loss; critical control parameter

**Equipment and Process Control:**
- **Die Bonder**: Besi, ASM for high-precision die placement; throughput 5,000-10,000 UPH (units per hour); ±5μm placement accuracy
- **Molding**: Towa, ASMPT for compression molding; 300mm wafer format; void-free molding critical; cycle time 60-120 seconds
- **Lithography**: Canon, Nikon i-line or KrF steppers for RDL; overlay ±2-3μm; older generation tools sufficient; cost-effective
- **Metrology**: KLA, Onto Innovation for overlay, CD, defect inspection; critical for multi-layer RDL; inline monitoring essential

**Cost and Performance:**
- **Cost Position**: 20-40% more expensive than standard WLP; 50-70% cheaper than 2.5D with interposer; sweet spot for high-performance mobile
- **I/O Density**: 500-2000 I/O per package; 5-10× higher than WLP; sufficient for mobile processors, mid-range AI accelerators
- **Bandwidth**: 50-200 GB/s for single die; 100-500 GB/s for multi-die with short RDL interconnect; competitive with 2.5D for many applications
- **Thermal Performance**: mold compound has poor thermal conductivity (0.5-1 W/m·K); limits power dissipation; <15W typical; heat spreader or TIM required for higher power

**Applications and Market:**
- **Mobile Processors**: Apple A/M-series, Qualcomm Snapdragon, MediaTek Dimensity; 60-70% of premium smartphone market; flagship devices
- **AI Accelerators**: edge AI chips, mobile AI processors; 5-15W power range; FOWLP provides sufficient I/O and thermal performance
- **RF Front-End**: integrate PA, LNA, switches, filters; FOWLP enables compact SiP; used in 5G smartphones
- **Automotive**: ADAS processors, infotainment SoCs; FOWLP provides reliability and integration; growing market

**Reliability and Quality:**
- **Board-Level Reliability**: 1000-2000 thermal cycles (-40 to 125°C); underfill required for >10mm packages; comparable to flip-chip BGA
- **Moisture Sensitivity**: MSL 3-4 typical; mold compound absorbs moisture; baking before assembly; popcorning risk during reflow
- **Drop Test**: critical for mobile devices; 1.5m drop on concrete; 50-100 drops typical; package design and underfill critical
- **Yield**: 90-95% package yield typical; lower than traditional packaging; improving with process maturity; defects in RDL, molding main issues

**Industry Landscape:**
- **TSMC InFO**: market leader; 40-50% market share; used by Apple, AMD, Broadcom; continuous innovation (InFO_oS, InFO_LSI)
- **Samsung FOPLP**: panel-level approach; cost advantage; used in Exynos, some Qualcomm; 15-20% market share
- **OSATs**: Amkor, ASE, JCET offer FOWLP services; licensed technologies or proprietary; combined 30-40% market share
- **Market Size**: $3-5B annually; growing 15-20% per year; driven by mobile, AI, automotive; expected to reach $10B by 2028

**Future Developments:**
- **Finer Pitch**: 0.15-0.2mm bump pitch for higher I/O; requires advanced RDL (1/1μm line/space); enabling 3000-5000 I/O packages
- **Thicker Mold**: 400-600μm for better thermal performance; enables higher power devices (20-30W); challenges in warpage control
- **Hybrid Bonding**: combine FOWLP with hybrid bonding for ultra-high bandwidth; 10-20μm pitch die-to-die connection; next-generation integration
- **Panel-Level**: 600×600mm panels for higher throughput; 30-50% cost reduction potential; Samsung leading; industry adoption expected 2025-2027

Fan-Out Wafer-Level Packaging is **the technology that bridges the gap between traditional packaging and advanced 2.5D/3D** — by enabling high I/O density, multi-die integration, and heterogeneous integration at 50-70% lower cost than interposer-based approaches, FOWLP has become the packaging of choice for premium mobile processors and mid-range AI accelerators, powering billions of devices worldwide.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/fan-out-wafer-level-packaging-fowlp) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
