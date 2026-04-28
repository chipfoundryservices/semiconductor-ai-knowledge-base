# Optical Proximity Correction (OPC)

**Keywords**: optical proximity correction opc,resolution enhancement techniques ret,sub resolution assist features sraf,inverse lithography technology ilt,opc model calibration

---

**Optical Proximity Correction (OPC)** is **the computational lithography technique that systematically modifies mask shapes to compensate for optical diffraction, interference, and resist effects during photolithography — adding edge segments, serifs, hammerheads, and sub-resolution assist features to ensure that the printed silicon pattern matches the intended design geometry despite extreme sub-wavelength imaging at advanced nodes**.

**Lithography Challenges:**
- **Sub-Wavelength Imaging**: 7nm/5nm nodes use 193nm ArF lithography with immersion (193i) to print features as small as 36nm pitch — feature size is 5× smaller than wavelength; diffraction and interference dominate, causing severe image distortion
- **Optical Proximity Effects**: nearby features interact through optical interference; isolated lines print wider than dense lines; line ends shrink (end-cap effect); corners round; the printed shape depends on the surrounding pattern within ~1μm radius
- **Process Window**: the range of focus and exposure dose over which features print within specification; sub-wavelength lithography has narrow process windows (±50nm focus, ±5% dose); OPC must maximize process window for manufacturing robustness
- **Mask Error Enhancement Factor (MEEF)**: ratio of wafer CD error to mask CD error; MEEF > 1 means mask errors are amplified on wafer; typical MEEF is 2-5 at advanced nodes; OPC must account for MEEF when sizing mask features

**OPC Techniques:**
- **Rule-Based OPC**: applies pre-defined correction rules based on feature type and local environment; e.g., add 10nm bias to line ends, add serifs to outside corners, add hammerheads to line ends; fast but limited accuracy; used for mature nodes (≥28nm) or non-critical layers
- **Model-Based OPC**: uses calibrated lithography models to simulate printed images and iteratively adjust mask shapes until printed shape matches target; accurate but computationally intensive; required for critical layers at 7nm/5nm
- **Inverse Lithography Technology (ILT)**: formulates OPC as an optimization problem — find the mask shape that produces the best wafer image; uses gradient-based optimization or machine learning; produces curvilinear mask shapes (not Manhattan); highest accuracy but most expensive
- **Sub-Resolution Assist Features (SRAF)**: add small features near main patterns that print on the mask but not on the wafer (below resolution threshold); SRAFs modify the optical interference pattern to improve main feature printing; critical for isolated features

**OPC Flow:**
- **Model Calibration**: measure CD-SEM images of test patterns across focus-exposure matrix; fit optical and resist models to match measured data; model accuracy is critical — 1nm model error translates to 2-5nm wafer error via MEEF
- **Fragmentation**: divide mask edges into small segments (5-20nm); each segment can be moved independently during OPC; finer fragmentation improves accuracy but increases computation time and mask complexity
- **Simulation and Correction**: simulate lithography for current mask shape; compare printed contour to target; move edge segments to reduce error; iterate until error is below threshold (typically <2nm); convergence requires 10-50 iterations
- **Verification**: simulate final mask across process window (focus-exposure variations); verify that all features print within specification; identify process window violations requiring additional correction or design changes

**SRAF Placement:**
- **Rule-Based SRAF**: place SRAFs at fixed distance from main features based on pitch and feature type; simple but may not be optimal for all patterns; used for background SRAF placement
- **Model-Based SRAF**: optimize SRAF size and position using lithography simulation; maximizes process window and image quality; computationally expensive; used for critical features
- **SRAF Constraints**: SRAFs must not print on wafer (size below resolution limit); must not cause mask rule violations (minimum SRAF size, spacing); must not interfere with nearby main features; constraint satisfaction is challenging in dense layouts
- **SRAF Impact**: properly placed SRAFs improve process window by 20-40% (larger focus-exposure latitude); reduce CD variation by 10-20%; essential for isolated features which otherwise have poor depth of focus

**Advanced OPC Techniques:**
- **Source-Mask Optimization (SMO)**: jointly optimizes illumination source shape and mask pattern; custom source shapes (freeform, pixelated) improve imaging for specific design patterns; SMO provides 15-30% process window improvement over conventional illumination
- **Multi-Patterning OPC**: 7nm/5nm use LELE (litho-etch-litho-etch) double patterning or SAQP (self-aligned quadruple patterning); OPC must consider decomposition into multiple masks; stitching errors and overlay errors complicate OPC
- **EUV OPC**: 13.5nm EUV lithography has different optical characteristics than 193nm; mask 3D effects (shadowing) and stochastic effects require EUV-specific OPC models; EUV OPC is less aggressive than 193i OPC due to better resolution
- **Machine Learning OPC**: neural networks predict OPC corrections from layout patterns; 10-100× faster than model-based OPC; used for initial correction with model-based refinement; emerging capability in commercial OPC tools (Synopsys Proteus, Mentor Calibre)

**OPC Verification:**
- **Mask Rule Check (MRC)**: verify that OPC-corrected mask satisfies mask manufacturing rules (minimum feature size, spacing, jog length); OPC may create mask rule violations requiring correction or design changes
- **Lithography Rule Check (LRC)**: simulate lithography and verify that printed features meet design specifications; checks CD, edge placement error (EPE), and process window; identifies locations requiring additional OPC or design modification
- **Process Window Analysis**: simulate across focus-exposure matrix (typically 7×7 = 49 conditions); compute process window for each feature; ensure all features have adequate process window (>±50nm focus, >±5% dose)
- **Hotspot Detection**: identify locations with high probability of lithography failure; use pattern matching or machine learning to flag known problematic patterns; hotspots require design changes or aggressive OPC

**OPC Computational Cost:**
- **Runtime**: full-chip OPC for 7nm design takes 100-1000 CPU-hours per layer; critical layers (metal 1-3, poly) require most aggressive OPC; upper metal layers use simpler OPC; total OPC runtime for all layers is 5000-20000 CPU-hours
- **Mask Data Volume**: OPC-corrected masks have 10-100× more vertices than original design; mask data file sizes reach 100GB-1TB; mask writing time increases proportionally; data handling and storage become challenges
- **Turnaround Time**: OPC is on the critical path from design tapeout to mask manufacturing; fast OPC turnaround (1-3 days) requires massive compute clusters (1000+ CPUs); cloud-based OPC is emerging to provide elastic compute capacity
- **Cost**: OPC software licenses, compute infrastructure, and engineering effort cost $1-5M per tapeout for advanced nodes; mask set cost including OPC is $3-10M at 7nm/5nm; OPC cost is amortized over high-volume production

Optical proximity correction is **the computational bridge between design intent and silicon reality — without OPC, modern sub-wavelength lithography would be impossible, and the semiconductor industry's ability to scale transistors to 7nm, 5nm, and beyond depends fundamentally on increasingly sophisticated OPC algorithms that compensate for the laws of physics**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/optical-proximity-correction-opc-13624) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
