# Forksheet Transistor Architecture

**Keywords**: forksheet transistor architecture,forksheet fet,forksheet vs gaa,forksheet nmos pmos,forksheet scaling

---

**Forksheet Transistor Architecture** is **the advanced CMOS device structure where nMOS and pMOS transistors share a common dielectric wall between channels, eliminating the need for spacer isolation** — reducing cell height by 15-20%, improving area scaling by 1.3-1.5× vs standard GAA, and enabling continued Moore's Law scaling at 2nm and 1nm nodes through tighter nMOS-pMOS spacing (10-15nm vs 20-30nm for GAA) while maintaining electrostatic control and performance.

**Forksheet Structure:**
- **Shared Dielectric Wall**: single dielectric wall separates nMOS and pMOS channels; replaces traditional spacer and STI isolation; thickness 5-10nm; typically SiO₂ or low-k dielectric
- **Nanosheet Channels**: both nMOS and pMOS use stacked nanosheet channels; 3-5 sheets per device; sheet width 15-30nm; sheet thickness 5-8nm; identical to standard GAA
- **Gate-All-Around**: gate wraps around all four sides of each nanosheet; provides excellent electrostatic control; suppresses short-channel effects; DIBL <30 mV/V
- **Source/Drain**: epitaxial SiGe for pMOS, Si:P for nMOS; grown on both sides of channel stack; provides low contact resistance; <100 Ω·μm

**Key Advantages vs Standard GAA:**
- **Reduced Cell Height**: nMOS-pMOS spacing 10-15nm vs 20-30nm for standard GAA; eliminates one spacer and STI region; reduces standard cell height by 15-20%
- **Area Scaling**: 1.3-1.5× area reduction vs standard GAA at same technology node; enables more transistors per mm²; critical for continued scaling
- **Simplified Process**: fewer isolation steps; no STI between nMOS and pMOS; reduces process complexity by 5-10 mask layers
- **Improved Density**: tighter packing enables smaller SRAM cells; 6T SRAM cell size 0.020-0.025 μm² at 2nm vs 0.030-0.035 μm² for standard GAA

**Fabrication Process:**
- **Superlattice Formation**: epitaxial growth of Si/SiGe superlattice; 3-5 pairs; Si thickness 5-8nm (channel), SiGe thickness 8-12nm (sacrificial); precise thickness control ±0.5nm
- **Fin Patterning**: define fins for both nMOS and pMOS; fin pitch 20-30nm; lithography (EUV) and etch (DRIE); critical dimension control ±1nm
- **Dielectric Wall Formation**: deposit dielectric between nMOS and pMOS regions; planarize; thickness 5-10nm; replaces traditional STI; critical for isolation
- **Dummy Gate**: deposit poly-Si dummy gate; pattern and etch; serves as placeholder; removed later in replacement metal gate (RMG) process
- **Spacer Formation**: deposit SiN spacers on sides (not between nMOS/pMOS); thickness 5-8nm; protects gate during S/D formation
- **Source/Drain Epitaxy**: selective epitaxial growth of SiGe (pMOS) and Si:P (nMOS); in-situ doping; thickness 20-40nm; provides low resistance
- **SiGe Release**: selective etch removes SiGe sacrificial layers; creates suspended Si nanosheets; HCl vapor etch at 600-700°C; etch rate 5-10nm/min
- **Gate Stack Formation**: deposit high-k dielectric (HfO₂, 1-2nm) and metal gate (TiN/W, 5-10nm); fills around nanosheets; provides gate-all-around structure
- **RMG Process**: remove dummy gate; deposit gate stack; planarize; forms final gate structure

**Electrostatic Control:**
- **Gate Control**: gate-all-around provides superior electrostatic control; effective gate length (Leff) 10-15nm at 2nm node; maintains performance at short lengths
- **DIBL (Drain-Induced Barrier Lowering)**: <30 mV/V typical; 2-3× better than FinFET; critical for low leakage and good subthreshold slope
- **Subthreshold Slope (SS)**: 65-75 mV/decade; close to ideal 60 mV/decade; enables low-voltage operation; reduces power consumption
- **Threshold Voltage Control**: work function metal tuning; multiple metals for different Vt options; ±100-200mV Vt range; enables multi-Vt design

**Performance Characteristics:**
- **Drive Current**: Ion 1.5-2.0 mA/μm for nMOS, 1.2-1.5 mA/μm for pMOS at Vdd=0.7V; 20-30% higher than FinFET at same node
- **Leakage Current**: Ioff <10 nA/μm at room temperature; <100 nA/μm at 125°C; excellent for low-power applications
- **Effective Capacitance**: Ceff 0.8-1.0 fF/μm; lower than FinFET due to reduced parasitic capacitance; improves switching speed
- **Intrinsic Delay**: τ = CV/I improves by 25-35% vs FinFET; enables higher frequency or lower power

**Integration Challenges:**
- **Dielectric Wall Formation**: precise thickness and position control; ±1nm tolerance; affects isolation and capacitance; requires advanced deposition and CMP
- **Selective Epitaxy**: must grow on nMOS and pMOS simultaneously; different materials (Si:P vs SiGe); requires careful process control
- **Gate Fill**: filling gate metal around nanosheets with dielectric wall nearby; requires conformal deposition; voids cause reliability issues
- **Stress Engineering**: strain from S/D epitaxy affects both nMOS and pMOS; must optimize for both; trade-off between nMOS and pMOS performance

**Design Considerations:**
- **Standard Cell Design**: new cell layouts to exploit reduced height; 15-20% area reduction; requires EDA tool updates
- **SRAM Design**: tighter packing enables smaller SRAM cells; 6T cell size 0.020-0.025 μm²; critical for cache-heavy designs
- **Power Delivery**: reduced cell height affects power rail placement; may require backside power delivery; design-technology co-optimization
- **Parasitic Extraction**: new parasitic models for dielectric wall coupling; affects timing and power analysis; requires accurate modeling

**Industry Development:**
- **imec Leadership**: imec demonstrated first forksheet devices in 2019; continues development for 2nm and beyond; industry collaboration
- **Samsung**: announced forksheet for 2nm node (2025-2026 production); follows GAA at 3nm; aggressive roadmap
- **TSMC**: evaluating forksheet for future nodes; currently focused on standard GAA for 2nm; may adopt for 1nm or beyond
- **Intel**: exploring forksheet as part of RibbonFET evolution; potential for Intel 18A (1.8nm) or beyond

**Cost and Economics:**
- **Process Complexity**: similar to standard GAA; dielectric wall adds steps but eliminates others; net neutral on process cost
- **Area Benefit**: 1.3-1.5× area scaling improves die economics; more die per wafer; 30-50% cost reduction per transistor
- **Yield**: similar yield challenges as GAA; dielectric wall adds new failure modes; requires process maturity
- **Time to Market**: 2-3 years after standard GAA; Samsung targeting 2025-2026; industry adoption 2025-2028

**Comparison with Alternatives:**
- **vs Standard GAA**: 15-20% smaller cell height; 1.3-1.5× area scaling; similar performance and power; preferred for 2nm and beyond
- **vs CFET**: simpler than CFET (no 3D stacking); easier to manufacture; but less aggressive scaling; forksheet is stepping stone to CFET
- **vs FinFET**: 2-3× better electrostatic control; 20-30% higher performance; enables continued scaling; clear successor to FinFET
- **vs Monolithic 3D**: forksheet is 2D planar; simpler than 3D; but less aggressive scaling; different application spaces

**Future Evolution:**
- **Forksheet+ Variants**: exploring thinner dielectric walls (3-5nm); tighter spacing (5-10nm); further area reduction
- **Hybrid Approaches**: combine forksheet with backside power delivery; optimize for both area and performance
- **Material Innovation**: exploring alternative channel materials (Ge, III-V) in forksheet structure; improve mobility
- **Path to CFET**: forksheet develops key technologies (dielectric wall, tight spacing) needed for CFET; natural progression

Forksheet Transistor Architecture is **the next step in CMOS scaling beyond standard GAA** — by sharing a dielectric wall between nMOS and pMOS to eliminate spacer isolation, forksheet reduces cell height by 15-20% and enables 1.3-1.5× area scaling at 2nm and 1nm nodes, providing a practical path to continued Moore's Law scaling while maintaining the electrostatic control and performance required for high-performance computing.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/forksheet-transistor-architecture) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
