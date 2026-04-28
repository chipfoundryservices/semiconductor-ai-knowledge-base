# High-NA EUV Lithography

**Keywords**: euv lithography high-na, numerical aperture 0.55, high-na euv, anamorphic optics euv, next generation euv

---

**High-NA EUV Lithography** is **the next-generation extreme ultraviolet lithography technology with numerical aperture increased from 0.33 to 0.55, enabling 8nm resolution and supporting 3nm, 2nm, and 1nm node production** — utilizing anamorphic optics with 4× reduction in one direction and 8× in the other, requiring new mask infrastructure and reticle handling, with first systems shipping in 2023-2024 for high-volume manufacturing ramp in 2025-2026.

**Numerical Aperture and Resolution:**
- **Resolution Limit**: R = k1 × λ / NA where λ=13.5nm for EUV; current 0.33 NA achieves 13nm resolution (k1=0.32); High-NA 0.55 achieves 8nm resolution; 1.67× improvement
- **Depth of Focus**: DOF = k2 × λ / NA²; High-NA reduces DOF from 90nm to 33nm; 2.7× reduction; challenges for wafer flatness and focus control; requires advanced leveling
- **Single Exposure Capability**: 0.33 NA requires multi-patterning for <13nm features; High-NA enables single exposure down to 8nm; reduces process complexity; improves overlay and throughput
- **Node Enablement**: 0.33 NA supports 7nm, 5nm with multi-patterning; High-NA targets 3nm, 2nm, 1nm with reduced patterning; critical for continued scaling

**Anamorphic Optics Design:**
- **Asymmetric Magnification**: 4× reduction in scan direction, 8× in slit direction; breaks traditional 4× symmetric reduction; enables larger NA while maintaining reticle size constraints
- **Reticle Size**: 26mm × 33mm field vs 26mm × 33mm for 0.33 NA; asymmetric field at wafer: 26mm × 16.5mm; requires reticle stitching for full die coverage in some cases
- **Optical System**: 6-mirror design vs 6-mirror for 0.33 NA; larger mirrors (up to 1m diameter); more complex alignment; tighter tolerances (±50pm mirror positioning)
- **Pupil Fill**: optimized illumination for asymmetric pupil; dipole and quadrupole illumination adapted for anamorphic system; maintains imaging performance

**Mirror Technology Advances:**
- **Mirror Size**: largest mirror 1.0-1.2m diameter; 2× larger than 0.33 NA; manufacturing challenges; weight and thermal management
- **Surface Accuracy**: <50pm RMS surface error; 2× tighter than 0.33 NA; requires advanced polishing and metrology; ion beam figuring for final correction
- **Coating**: Mo/Si multilayer mirrors; 40-50 layer pairs; 6.8nm period; >70% reflectivity per mirror; total system transmission 8-10% (6 mirrors)
- **Thermal Stability**: mirrors absorb EUV power; active cooling required; temperature stability ±1mK; prevents distortion; critical for overlay performance

**Reticle and Mask Infrastructure:**
- **Anamorphic Reticle**: new reticle format for 4×/8× reduction; different pattern density in X vs Y; mask writing tools require updates; EBM (electron beam mask) writers adapted
- **Mask Blank**: same 6-inch mask blank as 0.33 NA; TaBN absorber, Mo/Si multilayer reflector; but pattern layout optimized for anamorphic imaging
- **Mask Inspection**: inspection tools updated for anamorphic patterns; actinic inspection (13.5nm) critical; defect detection algorithms adapted; KLA, Applied Materials tools
- **Pellicle**: High-NA compatible pellicles required; higher power handling (500W+ sources); >95% transmission target; thermal management more critical

**System Performance and Specifications:**
- **Throughput**: target 185 wafers per hour (WPH) at 30mJ/cm² dose; comparable to 0.33 NA systems; enabled by 500W+ EUV source power
- **Overlay**: <1.5nm on-product overlay (3σ); tighter than 0.33 NA (2-2.5nm); required for 2nm/1nm nodes; advanced metrology and correction
- **Focus Control**: ±10nm focus budget; 3× tighter than 0.33 NA; requires advanced wafer leveling; <20nm wafer flatness; challenging for warped wafers
- **Availability**: >90% uptime target; comparable to mature 0.33 NA systems; requires reliable 500W source; robust subsystems

**Source Power Requirements:**
- **Power Scaling**: 500W source power for High-NA vs 250W for 0.33 NA; 2× increase; required for throughput despite lower transmission
- **LPP Source**: laser-produced plasma (LPP) tin droplet source; 500W demonstrated in lab; production-ready systems shipping 2024-2025
- **Collector Optics**: larger collector mirror for High-NA; improved efficiency; contamination control critical; lifetime >30,000 hours target
- **Power Roadmap**: 750W+ sources in development; enables higher throughput or lower dose; continuous improvement expected

**Manufacturing Challenges:**
- **Wafer Flatness**: 33nm DOF requires <20nm wafer flatness (vs <50nm for 0.33 NA); advanced CMP, stress control; backside grinding optimization
- **Leveling System**: advanced wafer stage with 1000+ measurement points; real-time focus correction; <5nm leveling accuracy; critical for yield
- **Reticle Stitching**: for large dies, multiple reticle exposures required; <2nm stitching overlay; adds process complexity; alternative: smaller dies
- **Process Integration**: new resist materials for 8nm resolution; reduced dose sensitivity; improved LER (line edge roughness); materials development ongoing

**Cost and Economics:**
- **System Cost**: $350-400M per High-NA scanner vs $150-200M for 0.33 NA; 2× cost increase; justified by single-exposure capability and node enablement
- **Operating Cost**: higher source power increases electricity and maintenance costs; offset by reduced multi-patterning; net CoO (cost of ownership) favorable for advanced nodes
- **Mask Cost**: anamorphic masks similar cost to standard masks ($150-300K); but fewer masks needed due to single exposure; total mask cost may decrease
- **ROI**: for 2nm/1nm production, High-NA essential; no viable alternative; cost justified by market demand for leading-edge chips; foundries committed

**Deployment Timeline:**
- **2023**: first High-NA systems delivered to Intel, TSMC, Samsung; installation and qualification; initial process development
- **2024**: process development and yield ramp; resist and materials optimization; first test wafers; learning phase
- **2025**: pilot production for 2nm node; limited volume; yield improvement; supply chain ramp
- **2026+**: high-volume manufacturing for 2nm and beyond; multiple fabs; industry-wide adoption; mature technology

**Vendor and Industry Ecosystem:**
- **ASML**: sole supplier of High-NA EUV systems (EXE:5000 series); $5B+ development investment; 10+ years development; first systems shipping
- **Foundries**: Intel, TSMC, Samsung committed; multi-billion dollar investments; new fabs designed for High-NA; competitive advantage
- **Materials**: JSR, Tokyo Ohka, Shin-Etsu developing High-NA resists; improved resolution and sensitivity; critical for success
- **Metrology**: KLA, Applied Materials, Onto Innovation providing High-NA metrology; overlay, CD, defect inspection; essential for yield

High-NA EUV Lithography is **the technology that extends Moore's Law through the 2nm and 1nm nodes** — by increasing numerical aperture to 0.55 and employing innovative anamorphic optics, it enables single-exposure patterning of 8nm features, reducing process complexity and cost while maintaining the resolution roadmap that sustains the semiconductor industry's 50-year trajectory of exponential improvement.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/euv-lithography-high-na) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
