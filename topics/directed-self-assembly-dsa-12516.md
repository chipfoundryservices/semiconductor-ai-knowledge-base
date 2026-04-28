# Directed Self Assembly DSA Patterning

**Keywords**: directed self assembly dsa,block copolymer lithography,dsa grapho-epitaxy,bcp lamellar cylinder phase,dsa pattern placement error

---

**Directed Self Assembly DSA Patterning** is a **materials-driven lithography technique exploiting block copolymer phase-separation physics to self-organize nanostructures at resolution exceeding conventional lithography limits — enabling economical patterning below EUV resolution without extreme UV source**.

**Block Copolymer Phase Separation**

Block copolymers (BCPs) consist of two chemically distinct polymer chains (typically PS-poly(styrene) and PMMA-poly(methyl methacrylate)) covalently bonded at chain ends. Thermal processing (annealing above glass-transition temperature Tg ~200°C for PS-PMMA) enables polymer chain mobility allowing blocks to microphase-separate: immiscible blocks spontaneously segregate forming ordered domains (microdomains) with characteristic size 10-100 nm (tunable through polymer molecular weight). Driving force: entropy of mixing negative for incompatible polymers, free energy minimized through phase separation.

**BCP Morphologies and Ordering**

- **Lamellar Phase**: Parallel alternating lamellae of PS/PMMA layers (pitch = 2 × repeat unit size); typical pitch 20-40 nm achievable; favorable for line-space patterns replicating to dense interconnect or gate arrays
- **Cylindrical Phase**: PMMA cylinders dispersed in PS matrix (or vice versa depending on molecular weight and composition); cylinder diameter 15-30 nm, inter-cylinder spacing 30-50 nm; favorable for dense dot patterns (contacts, vias)
- **Gyroid and Complex Phases**: Higher-order morphologies accessible through precise composition and processing; complex structures enable sophisticated patterning beyond simple line/dot arrays
- **Order-Disorder Transition (ODT)**: BCP order degrades above critical temperature; precise temperature control (±2°C) essential maintaining ordered domains during subsequent processing

**Directed Self Assembly Grapho-Epitaxy**

Grapho-epitaxy employs chemical or topographic templates pre-patterned via conventional lithography (photolithography, EUV) to direct BCP assembly. Templates contain chemical contrast (alternating patterns of energy-favorable and energy-unfavorable surfaces) or topographic trenches encouraging specific BCP orientation.

- **Chemical Templating**: Substrate patterned with alternating regions of PS-favoring and PMMA-favoring chemistry; thermal annealing directs BCP assembly aligning lamellar/cylindrical domains to template pattern
- **Topographic Templating**: Trenches (10-50 nm width) etched into substrate; BCP confined within trenches self-assembles into parallel lamellae or cylinder arrays aligned with trench geometry
- **Template Pitch**: Template pitch determines number of BCP domains fitting within template region; templating achieves multiplication of pattern density — coarse template (80 nm pitch) generates fine BCP pattern (20 nm pitch) through self-assembly

**DSA Pattern Transfer and Processing**

- **Selective Chemical Etch**: PMMA preferentially removed via reactive oxygen plasma (RIE in O₂ plasma) while PS survives as pattern mask; alternatively, PS removed via ozonolysis or plasma etch
- **Hard Mask Transfer**: PS/PMMA pattern transferred to underlying hard mask (SiO₂ or SiN) via additional RIE step creating durable mask for subsequent substrate etch
- **Final Pattern Definition**: Hard mask etch pattern transferred to substrate (silicon, interconnect layers) via conventional RIE completing pattern transfer
- **Multiple Etch Steps**: Pitch-doubling demonstrated through sequential BCP assembly/etch cycles enabling 40 nm pitch templates to generate 10 nm final features

**Pattern Placement Error and Alignment**

- **Fundamental Limitation**: BCP domains self-assemble to minimize free energy; however, multiple energetically-equivalent arrangements possible (polydomain formation). Pattern placement error: deviation between desired position (defined by template) and actual position (determined by self-assembly)
- **Typical PPE**: Unguided DSA exhibits 5-10 nm placement error; templated DSA reduces PPE to 2-3 nm through template constraints
- **Cumulative Error**: Multiple pitch-doubling steps accumulate errors — single 2 nm error per step results in 10 nm total error after 5 doublings, potentially unacceptable for <1 nm tolerance critical dimensions
- **Error Mitigation**: Feedback algorithms, improved chemical contrast templates, and optimized annealing conditions progressively reducing PPE

**Chemical Contrast and Surface Energy**

- **Brush Polymers**: Patterned polymer brush layers (500-1000 Å thickness) control surface energy: PS-brush favors PS domains, PMMA-brush favors PMMA domains
- **Chemically Patterned Surfaces**: Alternating patterns of CF₃-terminated (hydrophobic) and OH-terminated (hydrophilic) surfaces created via photochemistry or post-etch functionalization; enables chemical contrast for BCP templating
- **Wettability Control**: Surface wettability differences drive BCP alignment; small energy differences (1-5 mJ/m²) sufficient for directed assembly

**Defects and Defect Annihilation**

BCP assembly produces inevitable defects: domain boundaries misaligned, threading defects (chain topology errors), and grain boundaries (orientation discontinuities). Defect annealing through controlled thermal cycling or solvent vapor annealing reduces defect density; timescale 10-100 minutes for large-area ordering. Fundamental defect density limit ~10⁶ cm⁻² (comparable to photolithography defect levels) achievable through optimized annealing protocols.

**Industry Commercialization Status**

DSA technology demonstrated in academic labs achieving 10-15 nm features; commercial viability hinges on throughput and defect reduction. Imec (Belgium), Samsung, and TSMC actively researching DSA applications for advanced nodes; targeting integration 5-7 nm nodes (2023-2025 timeframe) as supplementary patterning technique where pitch multiplication enables cheaper masks than equivalent EUV exposure.

**Closing Summary**

Directed self-assembly represents **a materials-driven patterning paradigm leveraging polymer physics to achieve sub-EUV resolution through self-organizing nanostructures, enabling economical pitch-doubling and multiplication schemes — positioning DSA as complementary patterning technology extending photolithography capability toward ultimate scaling limits**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/directed-self-assembly-dsa-12516) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
