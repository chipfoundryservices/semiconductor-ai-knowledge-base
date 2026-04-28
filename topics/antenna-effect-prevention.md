# Antenna Effect Prevention

**Keywords**: antenna effect prevention,plasma induced damage,antenna ratio rules,diode insertion antenna,antenna fixing techniques

---

**Antenna Effect Prevention** is **the design practice of limiting the ratio of metal area to gate area during manufacturing to prevent plasma-induced gate oxide damage — ensuring that charge accumulated on metal interconnects during plasma etching does not exceed the gate oxide breakdown threshold by adding protection diodes, breaking metal connections, or routing through upper layers**.

**Antenna Effect Physics:**
- **Charge Accumulation**: during plasma etching of metal layers, the metal acts as an antenna collecting charged particles (ions, electrons); accumulated charge has no discharge path until the via to the next layer is etched
- **Gate Oxide Stress**: if the metal antenna connects to a transistor gate, accumulated charge flows through the gate oxide when the via is opened; high charge density creates electric field stress across the thin gate oxide (1-2nm at 7nm/5nm)
- **Oxide Damage**: electric field exceeding ~10 MV/cm causes oxide breakdown or trap generation; damaged gates have increased leakage current, threshold voltage shift, or complete failure; damage is permanent and causes yield loss
- **Process Dependence**: antenna damage depends on plasma conditions (power, pressure, chemistry), etch time, and oxide thickness; thinner oxides (advanced nodes) are more susceptible; foundries characterize antenna limits through test structures

**Antenna Rules:**
- **Antenna Ratio**: ratio of metal area to gate area; typical limit is 200-1000 depending on metal layer and oxide thickness; lower layers have tighter limits (more etch steps remaining); ratio = (metal_area) / (gate_area)
- **Cumulative Antenna**: metal area includes all layers below the current layer that are connected; e.g., M3 antenna includes M1+M2+M3 area; cumulative effect is more severe than single-layer
- **Partial Antenna**: metal area between the gate and the first via to upper layer; partial antenna is less severe because charge can discharge through the via
- **Side Area**: some foundries include metal sidewall area in antenna calculation; sidewall area = perimeter × thickness; sidewall contribution is 10-30% of total antenna area

**Antenna Violation Fixing:**
- **Diode Insertion**: add a reverse-biased diode from the metal net to substrate; diode provides a discharge path for accumulated charge; diode breaks down at ~5-7V (below oxide damage threshold) and safely dissipates charge
- **Metal Jumping**: route the net through an upper metal layer before connecting to the gate; upper layer connection resets the antenna ratio because subsequent etch steps don't affect already-processed layers; adds routing complexity and via count
- **Wire Breaking**: split long metal segments with intermediate vias to upper layers; reduces antenna area per segment; each segment must satisfy antenna rules independently
- **Gate Protection**: use thick-oxide I/O transistors or protection devices at the gate; thick oxide is more resistant to antenna damage; adds area and may impact performance

**Diode Insertion Strategy:**
- **Diode Placement**: place diode as close as possible to the violating gate; minimizes resistance between diode and gate; typical placement is within 10-50μm of the gate
- **Diode Sizing**: diode must be large enough to discharge the accumulated charge without self-destructing; typical diode size is 1-5μm²; larger antennas require larger diodes
- **Diode Types**: standard diode (p+/n-well or n+/p-well), Zener diode (controlled breakdown voltage), or diode-connected transistor; foundries provide antenna diode cells in standard cell libraries
- **Diode Leakage**: antenna diodes add leakage current (typically 1-10 pA per diode); thousands of diodes can add 1-10 nA total leakage; acceptable for most designs but may impact ultra-low-power applications

**Antenna Checking Flow:**
- **Extraction**: extract metal area and gate area for each net from layout; consider all metal layers and cumulative effects; Mentor Calibre and Synopsys IC Validator perform antenna extraction
- **Rule Checking**: compare antenna ratios against foundry limits; violations reported with net name, metal layer, antenna ratio, and violation severity
- **Incremental Checking**: after fixing violations, re-check only modified nets; reduces runtime for iterative fixing; modern tools support incremental antenna checking
- **Hierarchical Checking**: check antenna rules at block level and top level; block-level violations must be fixed before integration; top-level checking verifies that integration doesn't create new violations

**Advanced Antenna Techniques:**
- **Antenna-Aware Routing**: router considers antenna rules during routing; avoids creating violations by preferring upper metal layers for gate connections; Cadence Innovus and Synopsys ICC2 support antenna-aware routing
- **Preventive Diode Insertion**: insert diodes on all gate nets during placement; eliminates antenna violations before routing; may insert unnecessary diodes (area overhead) but simplifies flow
- **Jumper Insertion**: automatically insert metal jumpers (route through upper layer) to fix violations; avoids diode leakage; preferred for low-power designs
- **Antenna Budgeting**: allocate antenna budget across hierarchical blocks; each block must satisfy its budget; enables parallel block-level implementation without top-level antenna violations

**Advanced Node Challenges:**
- **Thinner Oxides**: 7nm/5nm nodes have 1-1.5nm gate oxide; more susceptible to antenna damage; antenna ratio limits reduced by 2-3× compared to 28nm
- **Multi-Patterning**: double/quadruple patterning requires multiple etch steps per metal layer; increases antenna exposure time; more stringent antenna rules required
- **FinFET Geometry**: FinFET gates have larger perimeter than planar transistors; gate area calculation includes fin sidewalls; effective antenna ratio is different from planar
- **EUV Lithography**: EUV uses different plasma chemistry; antenna damage characteristics differ from 193nm lithography; EUV-specific antenna rules emerging

**Antenna Impact on Design:**
- **Area Overhead**: antenna diodes add 0.5-2% area overhead; metal jumping increases routing congestion and via count; acceptable cost for preventing yield loss
- **Timing Impact**: diode capacitance (10-50 fF per diode) adds load to nets; typically negligible for non-critical nets; critical nets may use metal jumping instead of diodes
- **Power Impact**: diode leakage adds to total chip leakage; typically <1% of total leakage; negligible for most designs
- **Design Effort**: antenna checking and fixing adds 5-10% to physical design schedule; automated fixing tools reduce manual effort; essential for first-pass silicon success

Antenna effect prevention is **the manufacturing-aware design practice that protects transistor gates from plasma-induced damage — a subtle but critical reliability concern that, if ignored, causes random yield loss and field failures that are difficult to debug, making antenna checking and fixing a mandatory step in every physical design flow**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/antenna-effect-prevention) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
