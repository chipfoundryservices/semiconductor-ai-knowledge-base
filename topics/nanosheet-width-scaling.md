# Nanosheet Width Scaling

**Keywords**: nanosheet width scaling,gaa nanosheet width,sheet width optimization,nanosheet geometry,width vs performance tradeoff

---

**Nanosheet Width Scaling** is **the critical design parameter in gate-all-around transistors that determines the trade-off between drive current, area efficiency, and electrostatic control** — where sheet widths ranging from 10nm to 50nm enable optimization for different applications, with wider sheets (30-50nm) providing 50-80% higher drive current for high-performance logic while narrower sheets (10-20nm) enable 30-40% smaller SRAM cells and better short-channel control, making width scaling the primary knob for customizing GAA transistors to specific performance, power, and area requirements.

**Nanosheet Width Fundamentals:**
- **Width Definition**: horizontal dimension of nanosheet perpendicular to current flow; typically 10-50nm range; independent of gate length; can be varied within same technology node
- **Drive Current Scaling**: Ion scales linearly with width; wider sheets provide more current; 50nm sheet gives 2.5× current vs 20nm sheet; critical for high-performance applications
- **Effective Width**: total device width = (number of sheets) × (width per sheet); 3 sheets × 30nm = 90nm effective width; comparable to FinFET with 3 fins
- **Width Uniformity**: ±2-5nm variation across wafer; affects Vt and performance matching; critical for analog and SRAM circuits

**Width Impact on Performance:**
- **Drive Current (Ion)**: scales linearly with width; 30nm sheet provides 1.5-2.0 mA/μm normalized current; 50nm sheet provides 2.5-3.0 mA/μm; wider is better for speed
- **Leakage Current (Ioff)**: increases with width but sublinearly; wider sheets have slightly higher leakage per unit width; but Ion/Ioff ratio remains favorable
- **Transconductance (gm)**: scales with width; higher gm improves gain in analog circuits; 50nm sheets provide 2-3× higher gm than 20nm sheets
- **Output Resistance (ro)**: decreases with width; affects analog circuit design; trade-off between gain and current drive

**Width Impact on Area:**
- **Cell Height**: wider sheets require larger cell height to accommodate sheet width plus spacing; 50nm sheets may require 6-7 track cells vs 4-5 tracks for 20nm sheets
- **SRAM Cell Size**: narrower sheets enable smaller SRAM cells; 15-20nm sheets achieve 0.020-0.025 μm² 6T cell at 2nm node; 30-40nm sheets result in 0.030-0.040 μm² cells
- **Logic Density**: narrower sheets improve logic density by 20-30% vs wider sheets; but may sacrifice performance; trade-off depends on application
- **Fin Pitch Equivalent**: sheet width + spacing determines effective fin pitch; 30nm sheet + 20nm spacing = 50nm pitch; comparable to FinFET fin pitch

**Width Impact on Electrostatic Control:**
- **Short-Channel Effects**: narrower sheets provide better electrostatic control; gate wraps around smaller volume; DIBL <20 mV/V for 15nm sheets vs <30 mV/V for 40nm sheets
- **Subthreshold Slope (SS)**: narrower sheets achieve SS closer to ideal 60 mV/decade; 15nm sheets: 65-70 mV/decade; 40nm sheets: 70-80 mV/decade
- **Threshold Voltage Variation**: narrower sheets have higher Vt variation due to edge roughness; ±30-50mV for 15nm sheets vs ±20-30mV for 40nm sheets
- **Gate Length Scaling**: narrower sheets enable shorter gate lengths with acceptable short-channel effects; 15nm sheets work at Lg=10nm; 40nm sheets need Lg=12-15nm

**Width Optimization by Application:**
- **High-Performance Logic**: 30-50nm sheets; maximize drive current; accept larger area; target frequency >3-5 GHz; server processors, HPC
- **Low-Power Logic**: 20-30nm sheets; balance performance and leakage; optimize energy efficiency; mobile processors, IoT devices
- **SRAM**: 15-20nm sheets; minimize cell area; acceptable performance; 6T cell size 0.020-0.025 μm²; cache memory
- **Analog/RF**: 30-50nm sheets; maximize gm and current drive; precision matching; ADCs, PLLs, RF circuits
- **I/O Circuits**: 40-60nm sheets; high current drive for off-chip drivers; larger devices acceptable; I/O buffers, ESD protection

**Fabrication Considerations:**
- **Lithography**: sheet width defined by lithography and etch; EUV single patterning for 30-50nm; SADP or SAQP for 15-25nm; ±2nm CD control required
- **Etch Process**: anisotropic etch to define sheet width; sidewall roughness <1nm; width uniformity ±2-5nm across wafer; critical dimension control
- **Epitaxial Growth**: sheet width affects SiGe release etch; narrower sheets release faster; etch time optimization; HCl vapor etch selectivity >100:1
- **Gate Fill**: narrower sheets easier to fill with gate metal; conformal deposition; void-free fill; wider sheets may have fill challenges at tight pitch

**Multi-Width Design Strategy:**
- **Width Binning**: offer 2-4 discrete width options within same technology; e.g., 15nm (SRAM), 25nm (low-power logic), 40nm (high-performance logic)
- **Library Optimization**: separate standard cell libraries for each width; optimized for different PPA targets; designers choose appropriate library
- **Mixed-Width Design**: combine different widths on same die; SRAM with narrow sheets, logic with medium sheets, I/O with wide sheets; requires careful process integration
- **Mask Cost**: each width option requires separate masks; 2-4 additional mask layers per width variant; cost vs. flexibility trade-off

**Design Tool Support:**
- **Width-Aware Synthesis**: synthesis tools select appropriate width based on timing constraints; automatic width selection for each cell instance
- **Width-Dependent Models**: SPICE models parameterized by width; accurate performance prediction; separate models for each width option
- **Place and Route**: P&R tools handle mixed-width designs; cell height variations; power planning for different widths
- **Parasitic Extraction**: width affects parasitic capacitance; accurate extraction for each width; timing closure with width variations

**Process Variability:**
- **Width Variation Sources**: lithography CD variation (±1-2nm), etch loading effects (±1-2nm), epitaxial growth non-uniformity (±1-2nm); total ±2-5nm
- **Impact on Vt**: width variation causes Vt variation; ±20-40mV typical; narrower sheets more sensitive; affects yield and binning
- **Impact on Ion**: width variation causes Ion variation; ±5-10% typical; affects frequency binning; wider sheets more tolerant
- **Compensation Techniques**: work function metal tuning, channel doping adjustment, gate length compensation; reduce Vt variation to ±20-30mV

**Scaling Trends:**
- **2nm Node**: typical widths 20-40nm; 3-5 sheets per device; effective width 60-200nm; comparable to FinFET with 2-6 fins
- **1nm Node**: narrower widths 15-30nm; 4-6 sheets per device; improved electrostatic control; enables shorter gate lengths
- **Beyond 1nm**: exploring <15nm widths; requires advanced patterning; may approach quantum confinement effects; fundamental limits
- **Width Scaling Rate**: width scales slower than gate length; width reduces 10-20% per node vs 30-40% for gate length; width becomes limiting factor

**Economic Considerations:**
- **Mask Cost**: each width option adds 2-4 mask layers; $2-5M per mask set; limits number of width options; typically 2-3 widths offered
- **Design Cost**: separate libraries for each width; characterization and validation; $10-50M per width option; amortized over multiple products
- **Yield Impact**: width variation affects yield; tighter width control improves yield; ±2nm control target; <5% yield loss from width variation
- **Performance Binning**: width variation enables frequency binning; wider sheets bin higher; 10-20% frequency range; improves revenue

**Comparison with FinFET:**
- **Width Quantization**: FinFET has fixed fin width (5-8nm); GAA nanosheet width is continuous (10-50nm); GAA provides more flexibility
- **Width Scaling**: FinFET width doesn't scale with node; GAA width can be optimized per node; GAA advantage for future scaling
- **Multi-Width**: FinFET uses multiple fins (1-6 fins); GAA uses multiple sheets with variable width; GAA provides finer granularity
- **Area Efficiency**: GAA with optimized width is 20-30% more area-efficient than FinFET for same performance; GAA advantage

**Advanced Width Engineering:**
- **Tapered Sheets**: width varies along channel length; wider at source/drain, narrower at center; improves electrostatics while maintaining current; research phase
- **Graded Width**: width varies between sheets in stack; bottom sheets wider, top sheets narrower; optimizes current distribution; complex fabrication
- **Width Modulation**: intentional width variation for analog circuits; creates matched device pairs; precision width control required
- **Quantum Effects**: <10nm widths may exhibit quantum confinement; affects band structure and mobility; fundamental limit to width scaling

**Future Outlook:**
- **Optimal Width Range**: 15-40nm range likely for 2nm and 1nm nodes; balances performance, area, and manufacturability
- **Width Standardization**: industry may converge on 2-3 standard widths; simplifies design ecosystem; reduces mask costs
- **Forksheet and CFET**: width optimization extends to future architectures; narrower widths enable tighter spacing; critical for area scaling
- **Material Integration**: alternative channel materials (Ge, III-V) may enable narrower widths with higher mobility; research ongoing

Nanosheet Width Scaling is **the primary design knob for optimizing GAA transistors** — by varying sheet width from 10nm to 50nm, designers can tune the trade-off between drive current, area efficiency, and electrostatic control to meet specific application requirements, making width scaling as important as gate length scaling for achieving optimal power, performance, and area across diverse workloads from high-performance computing to ultra-low-power IoT devices.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/nanosheet-width-scaling) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
