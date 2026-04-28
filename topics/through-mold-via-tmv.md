# Through-Mold Via (TMV)

**Keywords**: through mold via tmv,vertical interconnect fowlp,3d fowlp,stacked die fowlp,tmv formation

---

**Through-Mold Via (TMV)** is **the vertical interconnect technology that creates conductive vias through molding compound in FOWLP to enable 3D stacking and backside connections** — achieving 50-100μm via diameter, 100-200μm pitch, and <10Ω resistance per via, enabling memory-on-logic integration, power delivery from backside, and multi-layer FOWLP with 2-4 stacked die for bandwidth >1 TB/s in AI accelerators and HPC applications.

**TMV Formation Process:**
- **Via Drilling**: laser ablation (CO₂ or UV laser) creates holes through mold compound; diameter 50-150μm; depth 100-400μm; taper <5°; drill after mold cure
- **Desmear**: plasma or wet chemical cleaning removes mold residue from via walls; ensures good adhesion; critical for reliability
- **Metallization**: sputter Ti/Cu seed layer on via walls; electroplate Cu to fill via; planarize by CMP; via resistance 5-20mΩ depending on diameter and depth
- **RDL Connection**: TMV connects to RDL on both sides; enables vertical signal/power routing; typical stack: bottom RDL → TMV → top RDL

**TMV Design and Characteristics:**
- **Via Diameter**: 50-100μm typical; smaller diameter increases resistance; larger diameter reduces routing density; trade-off between performance and area
- **Pitch**: 100-200μm for signal vias; 200-500μm for power vias; denser than TSV (through-silicon via) at 40-80μm pitch but sufficient for many applications
- **Aspect Ratio**: 2:1 to 4:1 (depth:diameter); limited by laser drilling and Cu filling capability; lower than TSV (10:1) due to mold compound properties
- **Resistance**: 5-20mΩ per via; 2-5× higher than TSV (2-5mΩ) but acceptable for most applications; parallel vias reduce effective resistance

**Applications and Integration:**
- **Memory-on-Logic**: stack HBM or LPDDR memory on logic die; TMV provides vertical connection; bandwidth 500 GB/s to 1 TB/s; used in AI accelerators, GPUs
- **Backside Power Delivery**: route power through TMV to die backside; reduces IR drop; improves signal integrity; enables higher performance
- **Multi-Die Stacking**: stack 2-4 die vertically; TMV connects die; compact 3D integration; used in advanced SiP (system-in-package)
- **Antenna Integration**: connect RF die to antenna on package top; TMV provides low-loss vertical path; used in 5G mmWave modules

**Comparison with TSV:**
- **Cost**: TMV 50-70% cheaper than TSV; no silicon processing; simpler fabrication; laser drilling vs DRIE (deep reactive ion etching)
- **Pitch**: TMV 100-200μm vs TSV 40-80μm; lower density but sufficient for many applications; trade-off between cost and performance
- **Resistance**: TMV 5-20mΩ vs TSV 2-5mΩ; higher but acceptable; parallel vias compensate; thermal performance similar
- **Process Integration**: TMV integrates with FOWLP; TSV requires wafer thinning, backside processing; TMV simpler and more flexible

**Thermal and Electrical Performance:**
- **Current Carrying**: 100-500mA per via depending on diameter; parallel vias for higher current; power delivery requires 10-100 vias
- **Inductance**: 50-200pH per via; lower than wire bonds (1-5nH); suitable for high-frequency signals; important for RF and high-speed digital
- **Thermal Conductivity**: Cu via provides thermal path; 400 W/m·K; helps heat dissipation from stacked die; but mold compound (0.5-1 W/m·K) limits overall thermal performance
- **Signal Integrity**: low inductance and resistance enable clean signal transmission; suitable for multi-Gb/s signaling; used in high-speed interfaces

**Manufacturing Challenges:**
- **Laser Drilling**: achieving uniform via diameter and taper; mold compound properties affect drilling; process optimization critical
- **Cu Filling**: void-free filling of high aspect ratio vias; requires optimized plating chemistry and current density; voids increase resistance
- **Alignment**: TMV must align with RDL on both sides; ±10-20μm alignment tolerance; requires precise lithography and metrology
- **Yield**: defects in drilling, filling, or alignment affect yield; 95-98% yield typical; improving with process maturity

**Equipment and Process:**
- **Laser Drilling**: ESI, LPKF, 3D-Micromac for via drilling; throughput 1000-5000 vias/second; multiple lasers for parallel processing
- **Plating**: Ebara, Atotech for Cu electroplating; optimized chemistry for high aspect ratio; uniform filling critical
- **Inspection**: X-ray for void detection; cross-section SEM for via profile; electrical test for resistance; 100% inspection for critical applications
- **Integration**: TMV process integrated into FOWLP flow; adds 2-3 days to cycle time; acceptable for performance benefit

**Reliability and Testing:**
- **Thermal Cycling**: -40 to 125°C, 1000 cycles; TMV survives due to low CTE mismatch; Cu (17 ppm/°C) vs mold (8-15 ppm/°C); better than TSV (Cu vs Si at 2.6 ppm/°C)
- **Electromigration**: high current density can cause Cu migration; design rules limit current per via; parallel vias for high current paths
- **Mechanical Stress**: package warpage and board flexing stress TMV; robust design and underfill mitigate; drop test critical for mobile applications
- **Failure Analysis**: X-ray, acoustic microscopy detect voids; FIB (focused ion beam) cross-section for detailed analysis; resistance measurement for electrical integrity

**Cost and Economics:**
- **Process Cost**: laser drilling $0.05-0.10 per via; Cu plating $0.10-0.20 per wafer; total TMV cost $1-3 per package; acceptable for high-value applications
- **Yield Impact**: TMV defects reduce yield by 2-5%; offset by performance and integration benefits; continuous improvement reduces defect rate
- **Value Proposition**: enables memory-on-logic, backside power delivery; performance improvement justifies cost; critical for AI, HPC applications
- **Market Adoption**: growing 20-30% annually; driven by AI accelerators, HPC, advanced mobile; expected to reach $1-2B market by 2027

**Industry Adoption:**
- **TSMC InFO_LSI**: uses TMV for die-to-die connection in Apple M1 Ultra; 2.5 TB/s bandwidth; production since 2022
- **Samsung**: TMV in advanced FOPLP; memory-on-logic integration; used in Exynos and customer products
- **OSATs**: Amkor, ASE developing TMV capability; licensed or proprietary technologies; production ramp 2024-2025
- **Applications**: AI accelerators (NVIDIA, AMD, Google), HPC processors, advanced mobile SoCs; high-performance applications

**Future Developments:**
- **Finer Pitch**: 50-100μm pitch for higher density; requires advanced laser drilling and alignment; enables >2000 TMV per package
- **Lower Resistance**: larger diameter (100-150μm) or Cu pillar TMV; <5mΩ resistance; competitive with TSV; for high-current applications
- **Hybrid Integration**: combine TMV with hybrid bonding; ultra-high bandwidth (>2 TB/s); next-generation 3D integration
- **New Materials**: exploring alternative mold compounds with better thermal conductivity; 2-5 W/m·K target; enables higher power devices

Through-Mold Via is **the cost-effective 3D interconnect that enables vertical integration in FOWLP** — by providing low-resistance vertical connections through molding compound at 50-70% lower cost than TSV, TMV enables memory-on-logic stacking, backside power delivery, and multi-die integration for AI accelerators and HPC processors where bandwidth and integration density are critical.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/through-mold-via-tmv) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
