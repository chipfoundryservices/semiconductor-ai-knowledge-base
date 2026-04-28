# Forksheet Transistor Technology

**Keywords**: forksheet transistor technology,forksheet fet structure,forksheet vs gaa,forksheet dielectric wall,forksheet nmos pmos isolation

---

**Forksheet Transistor Technology** is **an advanced GAA architecture that inserts a tall dielectric wall between adjacent NMOS and PMOS devices to eliminate the need for traditional shallow trench isolation (STI) — reducing the NMOS-PMOS spacing from 16-20nm to 6-8nm and enabling 15-20% logic cell area reduction at 2nm and 1nm nodes while maintaining the electrostatic benefits of nanosheet gate-all-around structures**.

**Forksheet Architecture:**
- **Dielectric Wall**: vertical SiO₂ or low-k dielectric barrier (height 80-120nm, thickness 5-8nm) separates NMOS and PMOS regions; replaces conventional STI which requires 16-20nm spacing due to lithography and etch constraints; wall inserted after fin patterning but before S/D formation
- **Continuous Nanosheet Stack**: Si/SiGe superlattice runs continuously under the dielectric wall; NMOS nanosheets on one side, PMOS nanosheets on other side; single epitaxial growth step forms both device types; eliminates the need for separate NMOS/PMOS active regions
- **Independent Gate Control**: NMOS and PMOS gates formed separately on opposite sides of the dielectric wall; gate metals can be optimized independently for each device type; gate-to-gate spacing reduced to wall thickness (5-8nm) vs 16-20nm in conventional GAA
- **Scaling Advantage**: standard cell height reduction of 15-20% by eliminating STI overhead; track height reduced from 5-6 tracks to 4-5 tracks; enables more aggressive cell library optimization; area-performance-power benefits compound across full chip design

**Fabrication Process Flow:**
- **Superlattice and Fin Formation**: identical to standard nanosheet process; Si/SiGe stack epitaxy (3-5 alternating layers); fin patterning by EUV lithography at 24-30nm pitch; fins run continuously across future NMOS and PMOS regions without interruption
- **Dielectric Wall Insertion**: critical innovation step; lithography defines wall location between NMOS and PMOS; trench etch through Si/SiGe stack to substrate; depth 80-120nm, width 5-8nm; high aspect ratio (15:1 to 20:1) requires advanced etch chemistry (Cl₂/HBr with pulsed plasma)
- **Wall Fill**: conformal SiO₂ deposition by ALD or PECVD; void-free fill of high aspect ratio trench; alternatively, low-k dielectric (SiOCN, k~4.5) for reduced parasitic capacitance; CMP planarization; wall must withstand subsequent high-temperature processing (>1000°C anneals)
- **Selective Device Formation**: block PMOS region with photoresist; form NMOS S/D (SiP epitaxy); block NMOS region; form PMOS S/D (SiGe:B epitaxy); dielectric wall prevents cross-contamination of dopants between NMOS and PMOS

**Gate Stack Integration:**
- **Dummy Gate and Spacer**: poly-Si dummy gates formed on both sides of dielectric wall; spacers deposited and etched; S/D recesses and epitaxy proceed as in standard GAA; wall remains intact through all processing
- **SiGe Release**: dummy gate removal exposes Si/SiGe stack edges; selective SiGe etch (vapor HCl or wet chemistry) removes sacrificial layers; etch proceeds from both NMOS and PMOS sides but stops at dielectric wall; suspended nanosheets formed on both sides independently
- **Dual Work Function Metals**: NMOS side receives TiAlC or TaN (4.2-4.4 eV work function); PMOS side receives TiN (4.6-4.8 eV); independent optimization without compromise; block mask protects one side while depositing on the other; two additional lithography steps vs standard GAA
- **Gate Fill and Planarization**: W or Co fills gate trenches on both sides; CMP planarizes to ILD level; gate resistance slightly higher than standard GAA due to narrower gate trench (wall consumes 5-8nm of available space)

**Design and Layout Implications:**
- **Cell Architecture**: standard cells redesigned to exploit reduced NMOS-PMOS spacing; P-N spacing reduced from 4-5 fin pitches to 1-2 fin pitches; cell height reduction enables more routing tracks in same area or smaller cell footprint
- **Power Rail Placement**: VDD and VSS rails can be placed closer together; buried power rail (BPR) architecture synergizes with forksheet (power rails in substrate, signals above); eliminates M1 power routing overhead
- **Routing Congestion**: reduced cell height may increase routing congestion in lower metal layers; requires co-optimization of cell library and place-and-route algorithms; 10-15% wirelength reduction observed in test chips due to tighter cell packing
- **Design Rule Complexity**: new rules for dielectric wall placement, minimum wall-to-contact spacing, and wall-to-gate alignment; EDA tool updates required for forksheet-aware layout generation and verification

**Challenges and Solutions:**
- **Wall Integrity**: dielectric wall must survive 1000°C anneals without cracking or delamination; thermal expansion mismatch between SiO₂ and Si creates stress; stress-relief structures (periodic breaks in wall) or engineered dielectrics (SiON with tuned composition) mitigate cracking
- **Etch Selectivity**: SiGe release etch must not attack the dielectric wall; SiO₂ etch rate <0.1 nm/min during HCl vapor etch; wall thickness loss <1nm over full process flow; surface treatment (densification anneal) improves wall resistance to etchants
- **Alignment Tolerance**: dielectric wall must align to fin structures within ±2nm; overlay error causes asymmetric nanosheet formation or wall-to-S/D shorts; advanced lithography (EUV with improved overlay <1.5nm) and metrology (after-develop inspection) required
- **Parasitic Capacitance**: NMOS-PMOS coupling capacitance through dielectric wall; wall thickness and dielectric constant trade-off (thicker wall reduces capacitance but increases spacing); low-k wall material (k~4) reduces coupling by 30% vs SiO₂ (k~3.9)

**Performance and Scaling:**
- **Area Reduction**: 15-20% standard cell area reduction vs conventional GAA at 2nm node; translates to 10-15% chip area reduction for logic-dense designs (CPU cores, AI accelerators); SRAM area unchanged (forksheet not applicable to memory arrays)
- **Performance Impact**: drive current density unchanged vs standard GAA (same nanosheet structure); slightly higher gate resistance (+5-10%) due to narrower gate trench; overall performance neutral to +5% due to reduced interconnect parasitics from tighter layout
- **Power Efficiency**: 10-15% active power reduction from area scaling at constant performance; leakage power unchanged (same transistor electrostatics); power density increases (more transistors per mm²) requiring enhanced thermal management
- **Roadmap**: forksheet targets 2nm node introduction (2025-2026); 1nm node (2028-2030) may combine forksheet with complementary FET (CFET) for further density improvement; beyond 1nm, monolithic 3D integration becomes necessary

Forksheet transistor technology is **the next step in CMOS miniaturization beyond standard GAA — eliminating wasted isolation space between NMOS and PMOS through an elegant dielectric wall structure, enabling continued area scaling when gate length and nanosheet dimensions approach their physical limits in the sub-2nm era**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/forksheet-transistor-technology) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
