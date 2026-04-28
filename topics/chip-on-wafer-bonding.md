# Chip-on-Wafer (C2W) Bonding

**Keywords**: chip on wafer bonding,c2w bonding process,known good die bonding,die to wafer alignment,c2w yield optimization

---

**Chip-on-Wafer (C2W) Bonding** is **the 3D integration technique that places and bonds pre-tested known-good dies onto a processed wafer — enabling heterogeneous integration of dies from different technologies, wafer sizes, and vendors with alignment accuracy ±0.5-2μm, achieving yield multiplication where system yield equals base wafer yield times die yield rather than their product as in wafer-to-wafer bonding**.

**Process Flow:**
- **Die Preparation**: source wafer diced into individual dies; dies tested and sorted; known-good dies (KGD) selected for bonding; die backside may be thinned to 20-100μm; die backlap and backside metallization if required
- **Die Pick-Up**: vacuum collet or electrostatic chuck picks die from wafer tape or gel-pak; die inspection (optical or X-ray) verifies quality; die flipped if face-down bonding required; Besi Esec or ASM AMICRA die handlers
- **Alignment**: vision system locates fiducial marks on die and target wafer; calculates position offset and rotation; accuracy ±0.3-1μm depending on equipment and mark quality; SUSS MicroTec XBC300 or EV Group SmartView alignment
- **Bonding**: die placed on target wafer location with controlled force (0.1-10N); bonding mechanism: hybrid bonding (Cu-Cu + oxide-oxide), thermocompression (Au-Au or Cu-Cu), or adhesive bonding; bond force and temperature optimized per technology

**Bonding Technologies:**
- **Hybrid Bonding**: simultaneous Cu-Cu metallic and oxide-oxide dielectric bonding; room-temperature pre-bond followed by 200-300°C anneal for 1-4 hours; achieves <10μm pitch interconnects; TSMC SoIC and Sony image sensor stacking use C2W hybrid bonding
- **Thermocompression Bonding (TCB)**: Au-Au or Cu-Cu bonding at 250-400°C with 50-200 MPa pressure; bond time 1-10 seconds per die; Besi Esec 3100 or ASM AMICRA NOVA TCB bonders; used for micro-bump bonding with 40-100μm pitch
- **Adhesive Bonding**: polymer adhesive (BCB, polyimide) between die and wafer; curing at 200-350°C; lower alignment accuracy (±2-5μm) but simpler process; used for MEMS and sensor integration
- **Solder Reflow**: solder bumps on die reflowed onto wafer pads; reflow temperature 240-260°C (Sn-Ag) or 180-200°C (Pb-Sn); flux application and cleaning required; lower cost but coarser pitch (>50μm)

**Alignment Accuracy:**
- **Vision System**: high-resolution cameras (0.5-2μm pixel size) image fiducial marks on die and wafer; pattern recognition algorithms calculate position; accuracy ±0.3-1μm for marks >10μm size
- **Fiducial Mark Design**: cross, box, or frame marks 10-50μm size; high contrast (metal on dielectric); placed at die corners or edges; mark quality (edge sharpness, contrast) critical for alignment accuracy
- **Alignment Errors**: mark detection error (±0.2-0.5μm), mechanical positioning error (±0.3-0.8μm), thermal drift (±0.1-0.3μm), die tilt (±0.2-0.5μm); total error RSS (root sum square) of individual errors
- **Throughput vs Accuracy Trade-Off**: high accuracy requires longer alignment time (5-15 seconds per die); lower accuracy enables faster bonding (1-3 seconds per die); application requirements determine optimal balance

**Yield Multiplication:**
- **W2W Yield**: wafer-to-wafer bonding yield = wafer1_yield × wafer2_yield; if both wafers are 80% yield, system yield is 64%; bad dies on either wafer create bad stacks
- **C2W Yield**: chip-on-wafer bonding yield = wafer_yield × die_yield; if wafer is 80% yield and dies are 90% yield (after test and KGD selection), system yield is 72%; 12.5% improvement over W2W
- **Economic Benefit**: C2W enables integration of expensive dies (e.g., III-V RF, photonics) with Si logic; only known-good expensive dies bonded; reduces cost of bad stacks by 50-80%
- **Rework Capability**: if die bonding fails, die can be removed and replaced (for some bonding technologies); W2W bonding has no rework option; rework capability further improves effective yield

**Throughput Challenges:**
- **Sequential Processing**: dies bonded one at a time; throughput 50-500 dies per hour depending on die size, alignment accuracy, and bonding technology; W2W bonds entire wafer (1000-10,000 dies) simultaneously
- **Equipment Parallelization**: multiple bonding heads or tools operate in parallel; 4-8 tools achieve 200-4000 dies per hour; capital investment $2-8M per tool; justified for high-value applications
- **Hybrid Approach**: C2W for heterogeneous dies (different technologies), W2W for homogeneous dies (same technology); optimizes throughput and yield for each integration scenario
- **Cost Crossover**: C2W more cost-effective than W2W when die cost >$10 and wafer yield <90%; W2W preferred for low-cost, high-yield homogeneous integration

**Applications:**
- **HBM (High Bandwidth Memory)**: 8-12 DRAM dies stacked on logic base using C2W with micro-bumps; each die tested before stacking ensures high system yield; SK Hynix, Samsung, and Micron production
- **Heterogeneous Integration**: III-V laser dies bonded to Si photonics wafer; GaN RF dies bonded to Si CMOS wafer; enables integration of incompatible materials and processes
- **Chiplet Integration**: multiple logic chiplets (CPU, GPU, I/O) bonded to Si interposer or base die; each chiplet from optimized process node; Intel EMIB and AMD 3D V-Cache use C2W-like processes
- **Image Sensors**: backside-illuminated (BSI) sensor die bonded to ISP logic wafer; Sony and Samsung production; hybrid bonding enables 1.1μm pixel pitch with Cu-Cu connections

**Process Optimization:**
- **Die Warpage**: thin dies (<50μm) warp due to film stress; warpage >20μm causes alignment errors and bonding voids; die backside grinding stress relief and metallization reduce warpage
- **Particle Control**: particles >1μm cause bonding voids; cleanroom class 1 (<10 particles/m³ >0.1μm) required; die and wafer cleaning before bonding; vacuum bonding environment
- **Bond Force Uniformity**: non-uniform force causes incomplete bonding; die tilt <0.5° required; bonding head flatness <1μm; force feedback control maintains target force ±10%
- **Thermal Management**: bonding temperature uniformity ±2°C across die; non-uniform heating causes thermal stress and warpage; multi-zone heaters and thermal simulation optimize temperature profile

**Inspection and Metrology:**
- **Pre-Bond Inspection**: optical inspection of die and wafer surfaces; particle detection; surface roughness measurement (AFM); ensures bonding quality before expensive bonding step
- **Post-Bond Inspection**: acoustic microscopy (C-SAM) detects voids and delamination; void area <1% of die area required; IR imaging (for transparent materials) shows bond interface quality
- **Alignment Metrology**: X-ray or IR imaging measures die-to-wafer alignment after bonding; overlay accuracy ±0.5-2μm verified; misalignment >5μm may cause electrical failures
- **Electrical Test**: continuity and resistance testing of bonded interconnects; 4-wire Kelvin measurement; typical specification 20-100 mΩ per connection; >200 mΩ indicates poor bonding

Chip-on-wafer bonding is **the flexible integration platform that enables heterogeneous 3D systems — combining the yield benefits of known-good-die selection with the performance advantages of fine-pitch 3D interconnects, making economically viable the integration of diverse technologies that would be impossible or prohibitively expensive with wafer-to-wafer bonding**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/chip-on-wafer-bonding) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
