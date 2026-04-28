# Pocket Implants

**Keywords**: pocket implant technique,pocket vs halo implant,ultra steep pocket,pocket implant angle,localized channel doping

---

**Pocket Implants** are **the extreme variant of halo implantation using very high angles (45-60°) and low energies to create highly localized, ultra-steep doping pockets immediately adjacent to source/drain junctions — providing maximum short-channel effect suppression with minimal impact on channel mobility by confining the counter-doping to a narrow 10-30nm region rather than extending 50-100nm into the channel like conventional halos**.

**Pocket vs Halo Distinction:**
- **Implant Angle**: pockets use 45-60° angles vs 15-30° for halos; steeper angles create more localized doping confined near S/D edges with minimal channel center penetration
- **Energy**: pockets use lower energy (5-20keV vs 20-50keV for halos); lower energy combined with steep angle produces shallow, narrow doping peaks 10-30nm wide
- **Lateral Extent**: pocket doping extends only 10-30nm into channel from S/D junction; halo doping extends 40-80nm; pockets minimize overlap in channel center for short gates
- **Dose**: pockets often use higher doses (3-8×10¹³ cm⁻²) than halos since the doped region is smaller; higher local doping concentration achieved with similar or lower total dopant count

**Ultra-Steep Pocket Formation:**
- **High-Angle Implantation**: 50-60° implants penetrate under gate edge at very shallow depth; ions travel nearly parallel to gate sidewall, creating vertical doping walls
- **Gate Shadowing**: tall gates (>80nm) and steep angles create significant shadowing; pocket placement highly sensitive to gate height, spacer width, and exact implant angle
- **Depth Control**: pocket depth 15-40nm controlled by implant energy; shallower pockets provide stronger SCE control but require precise energy control (±0.5keV) to avoid variability
- **Abruptness**: pocket profiles have gradients >10¹⁹ cm⁻³/decade; ultra-steep profiles maximize electrostatic control while minimizing mobility-degrading doping in channel bulk

**Process Implementation:**
- **Quadrant Implants**: four implants at 0°, 90°, 180°, 270° rotation ensure symmetric pockets; any asymmetry causes device mismatch and orientation-dependent performance
- **Implant Sequence**: pockets typically implanted after gate patterning and before or after extension implants; some processes use dual pockets (before and after spacer formation)
- **Species Selection**: boron or BF₂ for NMOS pockets (p-type counter-doping); phosphorus or arsenic for PMOS pockets (n-type); BF₂ provides shallower profiles due to molecular mass
- **Activation**: low-temperature activation (900-1000°C spike anneal or laser anneal) minimizes diffusion and preserves steep as-implanted profiles; excessive thermal budget degrades pocket abruptness

**Short-Channel Control Benefits:**
- **DIBL Suppression**: pockets reduce DIBL by 40-60% compared to no halos/pockets; 10-20% better than conventional halos at same mobility impact
- **Subthreshold Swing**: pockets improve subthreshold swing by 5-10mV/decade; steeper swing enables lower threshold voltage at same off-state leakage
- **Vt Roll-Off**: pockets reduce Vt roll-off to 30-50mV from long-channel to minimum-length vs 100-200mV without pockets; enables more aggressive scaling
- **Punch-Through Margin**: localized high doping near S/D provides excellent punch-through protection; allows shallower junctions without punch-through risk

**Mobility Preservation:**
- **Reduced Impurity Scattering**: confining high doping to narrow regions near S/D minimizes scattering in the channel bulk where most current flows; 5-10% mobility improvement vs conventional halos at same SCE control
- **Channel Center Doping**: pocket profiles create low doping in channel center even for very short gates; channel center doping 30-50% lower than halo-based designs
- **Effective Mobility**: overall effective mobility 10-15% higher with pockets vs halos for same gate length and DIBL; enables performance recovery or further scaling
- **Velocity Saturation**: reduced channel doping allows higher peak velocity before saturation; particularly beneficial for high-field transport in short channels

**Challenges and Limitations:**
- **Process Window**: pocket placement extremely sensitive to angle (±1° causes 20-30mV Vt shift), energy (±1keV causes 15-25mV shift), and gate height variation
- **Shadowing Variability**: gate height variation (±5nm) causes pocket position variation; taller gates create larger shadows, moving pockets away from channel
- **Spacer Interaction**: pocket position relative to extension depends critically on spacer width; spacer width variation (±1nm) causes 10-15mV Vt variation
- **Activation Challenges**: achieving high activation (>80%) without significant diffusion requires advanced annealing (laser, flash) which adds cost and complexity

**Advanced Pocket Strategies:**
- **Dual Pocket**: shallow pocket (60° angle, 8keV) for SCE control plus deeper pocket (45° angle, 20keV) for punch-through; provides multi-scale electrostatic control
- **Graded Pockets**: multiple pocket implants at slightly different angles and energies create graded doping profile; smoother transition reduces mobility impact
- **Selective Pockets**: pockets applied only to minimum-length devices; longer gates use conventional halos or no halos; reduces process complexity while optimizing critical devices
- **Asymmetric Pockets**: stronger pocket on drain side than source side; optimizes for specific circuit topologies but complicates layout and modeling

**Characterization and Modeling:**
- **SIMS Profiling**: 2D SIMS with 5nm spatial resolution maps pocket doping distribution; validates implant angle and energy settings
- **TEM Analysis**: transmission electron microscopy with energy-dispersive X-ray spectroscopy (EDS) visualizes pocket structure and position relative to gate and S/D
- **Electrical Extraction**: Vt roll-off, DIBL, and subthreshold swing measurements vs gate length extract pocket effectiveness; compared to TCAD simulations for model calibration
- **Variability Analysis**: large-scale device arrays measure pocket-induced Vt variability; separates systematic (angle, dose) from random (RDF) components

Pocket implants represent **the ultimate refinement of channel doping engineering — by confining counter-doping to ultra-narrow regions immediately adjacent to source/drain junctions, pockets provide the short-channel control necessary for sub-50nm planar CMOS while preserving the mobility benefits of lightly-doped channel centers, squeezing the last performance from planar architectures before the FinFET transition**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/pocket-implant-technique) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
