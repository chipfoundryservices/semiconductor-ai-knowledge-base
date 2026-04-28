# Complementary FET (CFET)

**Keywords**: complementary fet cfet,3d stacked transistors,cfet architecture,nmos over pmos,monolithic 3d integration

---

**Complementary FET (CFET)** is **the revolutionary 3D transistor architecture that vertically stacks nMOS directly over pMOS in a monolithic structure** — achieving 2× logic density vs forksheet, reducing standard cell area to 0.010-0.015 μm² at 1nm node, and enabling continued scaling beyond 2025 through elimination of lateral nMOS-pMOS spacing, where vertical integration provides the most aggressive area scaling path for future CMOS technology.

**CFET Architecture:**
- **Vertical Stacking**: pMOS nanosheets on bottom tier (3-5 sheets); nMOS nanosheets on top tier (3-5 sheets); separated by inter-tier dielectric (ITD) 10-20nm thick
- **Shared Gate**: single gate structure wraps both nMOS and pMOS; connects through ITD; reduces gate capacitance; simplifies routing
- **Monolithic Integration**: both tiers fabricated on same wafer; no wafer bonding; sequential processing; bottom tier first, then top tier
- **Zero Footprint**: nMOS and pMOS occupy same lateral area; eliminates lateral spacing; 2× density vs planar or forksheet

**Fabrication Approaches:**
- **Sequential Processing**: fabricate pMOS tier first; deposit ITD; fabricate nMOS tier on top; most common approach; thermal budget challenge for bottom tier
- **Folded Processing**: fabricate both tiers side-by-side; fold one tier over the other; bond and planarize; avoids thermal budget issue but adds complexity
- **Wafer Bonding**: fabricate nMOS and pMOS on separate wafers; bond face-to-face; thin and process; hybrid bonding at <10μm pitch; alternative approach
- **Thermal Budget**: top tier processing must not degrade bottom tier; <400-500°C for top tier; limits process options; requires low-temperature techniques

**Key Process Steps:**
- **Bottom Tier Formation**: standard GAA process for pMOS; superlattice growth, fin patterning, gate formation, S/D epitaxy; complete bottom tier
- **Inter-Tier Dielectric (ITD)**: deposit thick dielectric (50-100nm); planarize; provides isolation between tiers; must withstand top tier processing
- **Top Tier Channel Transfer**: transfer or grow nMOS channel material on ITD; options include wafer bonding, epitaxial growth, or layer transfer; critical step
- **Top Tier Processing**: form nMOS GAA transistors; low-temperature process (<500°C); selective etching, gate formation, S/D formation
- **Vertical Interconnect**: through-ITD vias connect top and bottom tiers; diameter 10-20nm; aspect ratio 1:1 to 2:1; low resistance (<100Ω) required
- **BEOL Integration**: standard back-end-of-line processing; connects both tiers to metal layers; no fundamental changes vs planar

**Electrical Performance:**
- **Drive Current**: similar to standard GAA; Ion 1.5-2.0 mA/μm for nMOS, 1.2-1.5 mA/μm for pMOS; vertical stacking doesn't degrade performance
- **Leakage**: Ioff <10 nA/μm; excellent electrostatic control from GAA structure; inter-tier leakage <1 pA/μm with proper ITD
- **Capacitance**: reduced gate capacitance from shared gate; Ceff 0.6-0.8 fF/μm; 20-30% lower than separate gates; improves speed
- **Variability**: potential for increased variability from sequential processing; requires tight process control; ±50mV Vt variation target

**Area Scaling:**
- **Logic Density**: 2× vs forksheet, 3-4× vs standard GAA, 5-6× vs FinFET at same node; most aggressive scaling
- **Standard Cell**: cell height 4-5 track vs 6-7 track for forksheet; cell area 0.010-0.015 μm² at 1nm node
- **SRAM**: 6T SRAM cell 0.012-0.018 μm² vs 0.020-0.025 μm² for forksheet; critical for cache-heavy designs
- **Routing**: reduced cell area increases routing density; may require more metal layers; trade-off between cell area and routing

**Integration Challenges:**
- **Thermal Budget**: top tier processing at <500°C; limits dopant activation, annealing, epitaxy; requires novel low-temperature processes
- **Alignment**: top tier must align to bottom tier; ±5-10nm alignment tolerance; critical for gate and S/D formation
- **Selective Processing**: top tier processing must not affect bottom tier; requires highly selective etching and deposition
- **Defect Density**: sequential processing increases defect opportunities; must maintain <0.01 defects/cm² for both tiers
- **Yield**: multiplicative yield impact from two tiers; 90% yield per tier = 81% combined; requires >95% per tier for viable manufacturing

**Design Implications:**
- **Standard Cell Library**: completely new cell designs; exploit vertical stacking; 2× density but new layout rules
- **Power Delivery**: both tiers need power; options include shared power rails, separate rails, or backside power delivery
- **Thermal Management**: power density doubles with 2× transistor density; thermal challenges; may limit frequency or require advanced cooling
- **EDA Tools**: new place-and-route algorithms; 3D-aware timing analysis; parasitic extraction for vertical structures

**Industry Development:**
- **imec**: demonstrated first CFET devices in 2021; continues development; industry collaboration for 1nm and beyond
- **Intel**: exploring CFET for future nodes (Intel 14A, 1.4nm or beyond); part of long-term roadmap
- **Samsung**: evaluating CFET for post-2nm nodes; following forksheet at 2nm; potential for 2027-2030 timeframe
- **TSMC**: research phase; no announced plans; likely post-2nm consideration; conservative approach

**Cost and Economics:**
- **Process Complexity**: significantly more complex than forksheet; 20-30% more process steps; higher cost per wafer
- **Area Benefit**: 2× density offsets higher process cost; net 30-50% cost reduction per transistor; economics favorable
- **Yield Risk**: lower yield from sequential processing; requires mature process; may take 2-3 years to reach acceptable yield
- **Time to Market**: 5-7 years after standard GAA; earliest production 2027-2030; high development cost

**Comparison with Alternatives:**
- **vs Forksheet**: 2× density advantage; but 2-3× more complex; CFET for ultimate scaling, forksheet for near-term
- **vs Monolithic 3D**: CFET is specific implementation of monolithic 3D; optimized for CMOS logic; other 3D approaches for memory or heterogeneous integration
- **vs 2.5D/3D Packaging**: CFET is transistor-level 3D; much finer pitch (<100nm) vs packaging (>10μm); different application
- **vs Backside Power**: complementary technologies; CFET for area scaling, backside power for performance; can combine both

**Technical Risks:**
- **Thermal Budget**: low-temperature processing may limit performance; dopant activation, defect annealing challenging
- **Reliability**: long-term reliability of ITD, vertical interconnects unknown; requires extensive testing
- **Variability**: sequential processing may increase device variability; affects yield and performance
- **Manufacturability**: complexity may limit yield; requires breakthrough in process control

**Future Outlook:**
- **1nm Node**: CFET likely required for 1nm node (2027-2030); no other path provides sufficient scaling
- **Beyond 1nm**: CFET enables scaling to 0.7nm, 0.5nm; combined with other innovations (new materials, backside power)
- **Heterogeneous Integration**: CFET logic tier combined with memory, analog, or RF tiers; ultimate integration
- **Economic Viability**: success depends on achieving >90% yield; cost per transistor must decrease despite complexity

Complementary FET is **the ultimate CMOS scaling solution** — by vertically stacking nMOS over pMOS in a monolithic structure, CFET achieves 2× logic density vs forksheet and enables continued Moore's Law scaling to 1nm and beyond, representing the most aggressive transistor architecture for future high-performance computing despite significant fabrication challenges.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/complementary-fet-cfet) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
