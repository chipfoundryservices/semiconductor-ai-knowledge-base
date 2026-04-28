# Trench-First Dual Damascene

**Keywords**: trench-first dual damascene,beol

---

**Trench-First Dual Damascene** is a **back-end-of-line (BEOL) copper interconnect patterning sequence where the metal wiring trench is etched before the underlying via** — defining the upper metal line profile first using lithography on a planar surface, then etching the via opening through the bottom of the already-formed trench into the etch stop layer, offering superior trench critical dimension control at the cost of more complex via lithography on non-planar topology.

**What Is Trench-First Dual Damascene?**

- **Definition**: One of two primary integration approaches for forming dual damascene copper interconnect structures — the trench (horizontal metal wire) is patterned and partially etched first, then the via (vertical connection to lower metal level) is patterned and etched through the trench bottom.
- **Dual Damascene Context**: Dual damascene fills both the via and trench with copper in a single electroplating step — eliminating the separate tungsten via fill step of older process flows and reducing manufacturing cost and resistance.
- **Sequence**: Deposit interlayer dielectric (ILD) → planarize (CMP) → coat photoresist → expose trench pattern → etch trench partway into ILD → strip resist → coat again → expose via pattern → etch via through trench bottom to etch stop layer → strip → fill both with copper → CMP.
- **Alternative**: Via-first dual damascene reverses the order — via etched and filled with sacrificial material, then trench patterned above — more common at advanced nodes below 28nm.

**Why Trench-First Dual Damascene Matters**

- **CD Control for Trench**: Trench lithography occurs on a flat, planarized wafer surface — optimum focus and exposure conditions produce the tightest critical dimension (CD) control for the metal wire width.
- **Metal Line Definition**: At many nodes, metal line width (trench CD) is more critical to resistance and timing than via diameter — trench-first prioritizes the more performance-critical dimension.
- **Etch Simplicity**: Trench etch into a homogeneous dielectric is simpler to control than via etch through multiple dielectric layers — trench-first separates these into independent, optimized etch steps.
- **Profile Optimization**: Trench profile (sidewall angle, depth uniformity) is independent of via etch chemistry — each etch optimized separately without competing constraints.
- **Process Window**: Trench-first offers wider process window for low-k dielectric integration — mechanical fragility of porous low-k dielectrics is better managed with planar trench patterning.

**Trench-First vs. Via-First Integration**

**Trench-First Advantages**:
- Better trench CD control — lithography on flat surface.
- Simpler trench etch — no topography to navigate.
- Flexible etch stop placement — trench depth controlled by timed etch or etch stop layer.
- Better for nodes where metal line CD is the critical parameter.

**Trench-First Disadvantages**:
- Via lithography on non-planar topology — reduced focus latitude, CD variation across trench.
- Via pattern must align to pre-etched trench — additional overlay requirement.
- Photoresist on trench topography has non-uniform thickness — exposure dose variation.

**Via-First Advantages**:
- Via lithography on planar surface — tight CD control for via.
- Simpler resist coat for via patterning.
- More robust integration below 28nm — dominant approach at advanced nodes.

**Via-First Disadvantages**:
- Trench lithography over etched vias — topography affects trench patterning.
- Via protection during trench etch — sacrificial fill or hardmask required.

**Critical Integration Details**

**Etch Stop Layer**:
- SiCN (silicon carbonitride) or SiN (silicon nitride) etch stop between metal levels — 5-15 nm thick.
- Trench etch stops on or in etch stop layer (partial punch-through).
- Via etch punches through etch stop to reach underlying copper.
- Etch stop selectivity: dielectric etch rate / etch stop etch rate > 20:1 required.

**Dielectric Stack (Low-k Integration)**:
- Hard mask (TiN or TaN): protects dielectric during etch, defines final dimension.
- Tetraethyl orthosilicate (TEOS) cap: mechanical support for fragile low-k.
- Ultra-low-k (ULK) porous SiOC: main ILD, k = 2.0-2.4.
- Etch stop layer: SiCN, k = 4-5.

**Photoresist and Lithography**:
- Trench resist coat: uniform on planar surface — standard process.
- Via resist coat: fills trench and covers surrounding areas — thinner over trench bottom, thicker at edges.
- Anti-reflection coating (ARC): critical for via lithography on topography — reduces standing wave effects.
- Overlay: via pattern must align to trench — typically ±10-15% of via CD budget.

**Dual Damascene Technology Nodes**

| Node | Primary Approach | Metal Pitch | ILD Material |
|------|-----------------|------------|--------------|
| **180nm** | Via-first or trench-first | 720 nm | SiO₂ (k=4.0) |
| **130nm** | Trench-first | 520 nm | FSG (k=3.5) |
| **90nm** | Trench-first | 360 nm | CDO (k=2.9) |
| **45nm** | Via-first dominant | 180 nm | Porous SiOC (k=2.4) |
| **28nm** | Via-first | 112 nm | ULK (k=2.2) |

**Process Control Metrics**

- **Trench CD Uniformity**: 3-sigma < 5% of nominal CD across wafer — controlled by lithography dose and focus uniformity.
- **Via CD**: 3-sigma < 8% — harder to control due to topography.
- **Trench Depth**: ±5% across wafer — controlled by etch time, loading effects, and etch stop uniformity.
- **Via Resistance**: Kelvin resistance measurement on test structures — target < 2 ohm/via at 45nm.

**Failure Modes**

- **Trench-Via Misalignment**: Via offset from trench bottom — increased via resistance or via bridging to adjacent trench.
- **Incomplete Via Etch**: Via does not fully punch through etch stop — high resistance or open.
- **Trench CD Variation**: Narrow trench = high resistance; wide trench = shorts to adjacent lines.
- **Profile Degradation**: Bowing or tapered trench profile — affects fill and electrical performance.

Trench-First Dual Damascene is **carving the channel before drilling the hole** — a specific ordering of lithography and etch operations that prioritizes metal line critical dimension control, enabling the precise copper interconnect structures that wire together billions of transistors in modern semiconductor devices.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/trench-first-dual-damascene) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
