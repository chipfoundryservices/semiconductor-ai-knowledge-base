# Stress Engineering

**Keywords**: stress engineering,process

---

**Stress Engineering** is the **deliberate introduction of controlled mechanical stress into semiconductor devices to enhance carrier mobility and transistor performance** — exploiting the piezoresistive effect where mechanical stress modifies the silicon band structure, reducing effective carrier mass and increasing drift velocity to achieve 10-50% performance improvement without requiring additional transistor scaling.

**What Is Stress Engineering?**

- **Physical Basis**: Mechanical stress distorts the silicon crystal lattice, modifying the valence and conduction band structure — specifically altering the effective mass of holes and electrons and reducing inter-valley scattering, both of which increase carrier mobility and transistor drive current.
- **Piezoresistive Effect**: Silicon resistivity changes under mechanical stress — tensile stress parallel to current flow enhances electron mobility in NMOS; compressive stress perpendicular to current flow enhances hole mobility in PMOS.
- **Performance Impact**: Stress-induced mobility enhancement translates directly to higher drain saturation current (Idsat) — faster transistors without reducing gate length or oxide thickness.
- **Industry Adoption**: Intel introduced strain engineering at the 90nm technology node (2003) — strained silicon became ubiquitous at 65nm and below, providing performance gains that supplemented dimensional scaling.

**Why Stress Engineering Matters**

- **Performance Without Scaling**: Traditional scaling (Moore's Law) provides diminishing returns below 28nm — stress engineering provides performance boosts decoupled from physical dimensions.
- **Dual Polarity Benefit**: NMOS benefits from tensile stress; PMOS from compressive stress — stress engineering can simultaneously optimize both device types in CMOS technology.
- **Cumulative Gains**: Multiple stress techniques stack — embedded SiGe + stress liner + stress memorization can provide 50-80% total mobility enhancement.
- **Energy Efficiency**: Higher mobility at same voltage means higher performance — or same performance at lower voltage, reducing dynamic power consumption.
- **Chip Cost**: Performance gains from stress engineering reduce the number of process nodes needed to meet performance targets — extending the economic lifetime of each technology node.

**Stress Engineering Techniques**

**Strained Silicon Epitaxy**:
- Grow silicon on relaxed silicon-germanium (SiGe) substrate or buffer layer.
- Si lattice constant (5.43 Å) is smaller than SiGe — Si layer stretches to match SiGe, creating biaxial tensile strain.
- Enhances both electron and hole mobility in the strained Si layer.
- Intel's 90nm "Strained Silicon" process used this approach for initial strain introduction.

**Embedded SiGe Source/Drain (eSiGe)**:
- Etch selective recesses in PMOS source and drain regions.
- Epitaxially grow SiGe (25-35% Ge content) in the recesses.
- SiGe has larger lattice constant than Si — squeezes the Si channel laterally (compressive stress).
- Compressive stress along channel direction enhances hole mobility 30-50%.
- Used in all major foundries from 90nm through FinFET nodes.

**Stress Liner (Contact Etch Stop Layer, CESL)**:
- Deposit tensile or compressive nitride (Si₃N₄) film over completed transistors.
- Tensile nitride over NMOS: applies longitudinal tensile stress to channel — enhances electron mobility.
- Compressive nitride over PMOS: applies longitudinal compressive stress — enhances hole mobility.
- Dual stress liner: deposit tensile nitride, mask PMOS, remove, deposit compressive nitride over PMOS.
- Simpler than eSiGe but lower stress magnitude.

**Stress Memorization Technique (SMT)**:
- Apply tensile nitride capping layer before source/drain anneal.
- During high-temperature anneal, stress is "memorized" into the recrystallized source/drain regions.
- Remove nitride after anneal — crystal retains stress imprint.
- Particularly effective for NMOS with minimal process complexity addition.

**Embedded SiC Source/Drain (eSiC)**:
- Silicon carbide (SiC) has smaller lattice constant than Si — pulls Si channel into tensile stress.
- Applied to NMOS source/drain regions to enhance electron mobility.
- Less widely used than eSiGe due to lower Ge-equivalent strain and epitaxy complexity.

**Process Challenges**

- **Pattern Dependency**: Stress level varies with device geometry, pitch, and neighboring structures — isolated transistors differ from dense arrays; requires design rule constraints.
- **Stress Relaxation**: High-temperature processing steps can relax engineered stress — process sequence must preserve stress through thermal budget.
- **Integration Complexity**: Dual stress liner requires additional masking steps; eSiGe requires selective epitaxy and etch — adds process cost and variability.
- **FinFET Stress Challenges**: 3D FinFET geometry makes stress application less efficient — stress liners apply to fin sidewalls; embedded source/drain geometry changes stress transfer.

**Stress Measurement Techniques**

| Technique | Resolution | Depth | Application |
|-----------|-----------|-------|------------|
| **Raman Spectroscopy** | 0.05% strain | Near-surface | Wafer-level mapping |
| **Nano-beam Diffraction (NBD)** | 0.01% | TEM cross-section | Transistor-level |
| **EBSD** | 0.1% | SEM cross-section | Package-level |
| **Electrical (Ring Oscillator)** | Indirect | Full stack | Performance validation |

**Technology Integration by Node**

- **90nm**: First strained silicon commercialization — Intel's "Strained Silicon" NMOS with tensile SiN liner.
- **65nm**: Dual stress liner + embedded SiGe for PMOS — industry-wide adoption.
- **45nm/32nm**: Stress memorization + enhanced eSiGe — cumulative stress techniques.
- **22nm FinFET**: Epitaxial SiGe fin replacement + embedded SiGe — stress in 3D geometry.
- **7nm/5nm**: SiGe channel PMOS (not just source/drain) — channel material change for maximum hole mobility.

Stress Engineering is **mechanical performance enhancement for silicon** — the ingenious exploitation of crystal physics to squeeze additional transistor performance out of silicon by deliberately distorting its atomic lattice, demonstrating that materials innovation and physical engineering can extend Moore's Law beyond what dimensional scaling alone can achieve.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/stress-engineering) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
