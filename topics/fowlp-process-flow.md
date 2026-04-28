# Fan-Out Wafer-Level Packaging Process

**Keywords**: fowlp process flow,embedded wafer level bga,chip first chip last,reconstituted wafer fowlp,fan out routing

---

**Fan-Out Wafer-Level Packaging Process** is a **revolutionary packaging technology placing bare dies directly on redistribution layers without interposer substrates, enabling fan-out routing and wafer-scale integration — eliminating intermediate packaging substrates and reducing cost-per-unit**.

**FOWLP Architecture Overview**

Fan-out packaging reorganizes die arrangement in wafer format: multiple dies bonded sparsely across wafer surface (spacing between dies enables RDL routing underneath), followed by RDL deposition creating electrical routing. Finished package contains dozens of dies per wafer; wafer-level sawn into individual package units. Cost advantage significant: substrate cost (~$5-20 per unit in traditional packages) eliminated, replaced by thin RDL ($0.50-2 per unit); net savings 50-70% depending on package complexity. Density improvement: dies no longer constrained by package body outline, enabling arbitrary spatial arrangement.

**Chip-First vs Chip-Last Process Flows**

Chip-first sequence: dies bonded to temporary carrier substrate, micro-bumps formed on die pads, RDL subsequently deposited/routed, interconnect completed, dies singulated from temporary carrier. Advantages: rework capability (defective dies can be removed before RDL complete), simpler RDL patterning (no die obstruction). Disadvantages: temporary carrier removal adds process complexity, potential damage during carrier peel-off.

Chip-last sequence: RDL fabricated on temporary substrate first (all metal layers, vias, and pads complete), dies subsequently bonded to RDL pads (micro-bump bonding or solder-reflow with flux), underfill applied, singulation follows. Advantages: tighter RDL pitch (no die presence constrains patterning), simplified assembly. Disadvantages: no die rework capability (defective dies cannot be removed), RDL lithography complexity managing registration around future die bonding pads.

**Temporary Carrier Technology**

- **Carrier Materials**: Silicon or glass wafers serve as temporary mechanical support; alternative polymeric carriers reduce processing cost
- **Release Mechanisms**: Thermal release polymers (TRP) with temperature-dependent adhesion enable carrier removal at elevated temperature without mechanical stress
- **Adhesion Control**: Careful process parameter tuning controls adhesion strength — sufficient to prevent die slippage during processing, but enabling clean separation afterward
- **Reuse Strategy**: Carriers cleaned and reused 50-100 times improving process economics

**Underfill Material and Encapsulation**

- **Epoxy Systems**: Thermosetting epoxy underfill provides mechanical stability through thermal cross-linking (cure at 150-180°C)
- **Curing Chemistry**: Aliphatic or cycloaliphatic epoxy resins cured with anhydride or amine hardeners; cure kinetics optimized for processing speed
- **Coefficient of Thermal Expansion (CTE)**: Underfill CTE matched to silicon (approximately 3 ppm/K) minimizing stress during thermal cycling
- **Hydrophobicity**: Hydrophobic resins resist moisture ingress protecting internal structures

**RDL Integration in FOWLP**

- **Multi-Layer RDL**: Typically 3-4 metal layers with 2-5 μm pitch enable complex routing patterns under sparse die placement
- **Via-Rich Areas**: High via density (20-40% area) under dies provides electrical distribution from die bumps to RDL routing network
- **Routing Layers**: Upper metal layers route signals across wafer enabling arbitrary die-to-die connection patterns
- **Power Distribution**: Dedicated power/ground layers carry high current from substrate pads to all dies

**Reconstituted Wafer Processing**

After die bonding and underfill cure, assembly treated as standard wafer enabling back-end-of-line processing: backside substrate removal (if used), additional RDL layers, and final substrate pads. This wafer-level processing provides efficiency advantage — tool utilization matches standard wafer manufacturing (no per-unit assembly, handled at wafer scale). Finishing requires wafer singulation through saw or laser scribing separating packages.

**Embedded Wafer-Level BGA (eWLB)**

eWLB variant embeds dies within molded compound — dies bonded to temporary carrier, RDL deposited, subsequently encapsulated in mold compound creating solid package body. Mold compound provides mechanical robustness and hermetic-equivalent protection (moisture resistance adequate for most non-military applications). Backside solder balls attached through solder-mask patterning and ball attachment completing package. eWLB combines fan-out benefits with traditional ball-grid-array form factor enabling direct PCB assembly without specialized equipment.

**Design Considerations and Constraints**

- **Die Pitch Optimization**: Sparse die placement enables cost-effective RDL routing; typical inter-die spacing 2-5 mm balances routing flexibility against wafer area utilization
- **Power Delivery Network**: Multiple dies sharing power/ground infrastructure require careful voltage drop analysis ensuring <50 mV drop across wafer under worst-case current transients
- **Thermal Management**: Dies dissipating significant power require direct thermal connection to substrate — alternative thermal vias (large-diameter high-conductivity paths) route heat away from sensitive circuits
- **Signal Integrity**: Long RDL traces introduce parasitic inductance and capacitance; differential routing pairs and controlled impedance essential for high-speed signals

**Yield and Reliability**

- **Process Yield**: Defect probability increases with RDL complexity; layer-by-layer yield (95%+ per layer) cumulative across 3-4 layers results in 85-95% RDL yield
- **Thermal Cycling Reliability**: CTE mismatch between underfill (≈50 ppm/K), silicon dies (3 ppm/K), and solder interconnect (20 ppm/K) creates thermal stress; reliability assessed through -40°C to +85°C cycling
- **Moisture Absorption**: Polymer underfill absorbs moisture (2-5% water content after humidity conditioning) causing expansion; moisture-induced stresses critical failure mechanism

**Closing Summary**

Fan-out wafer-level packaging represents **a paradigm-shifting technology enabling direct die-to-RDL bonding at wafer scale, eliminating expensive interposer substrates while enabling dense heterogeneous integration — transforming packaging economics and enabling next-generation multi-chiplet systems through wafer-scale manufacturing efficiency**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/fowlp-process-flow) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
