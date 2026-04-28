# Junctionless Transistors

**Keywords**: junctionless transistors,junctionless fet fabrication,junctionless vs inversion mode,junctionless doping profile,junctionless process simplification

---

**Junctionless Transistors** are **the alternative FET architecture where the source, drain, and channel are uniformly doped to the same high concentration (>10¹⁹ cm⁻³) with no metallurgical junctions — operating by full depletion of the thin channel in the off-state and bulk conduction in the on-state, eliminating dopant gradients, junction formation, and activation anneals while providing improved subthreshold slope, reduced variability, and simplified processing for nanowire and thin-film transistor applications**.

**Operating Principle:**
- **Bulk Conduction Mode**: channel is heavily doped (N⁺ for NMOS, P⁺ for PMOS); in on-state (Vgs > Vt), channel conducts through bulk majority carriers; no inversion layer required; current flows through entire channel cross-section; mobility equals bulk mobility (not degraded by surface scattering)
- **Full Depletion Mode**: in off-state (Vgs < Vt), gate depletes the thin channel completely; depletion width W_dep = √(2ε_si × Vgs / (q × N_d)); for complete depletion, channel thickness < 2 × W_dep; typical channel thickness 5-10nm requires doping 1-5×10¹⁹ cm⁻³
- **Flat-Band Voltage**: Vt ≈ V_fb = Φ_ms - Q_channel / C_ox where Φ_ms is work function difference, Q_channel is channel charge; Vt tuned by gate work function and channel doping; no threshold voltage roll-off with gate length (major advantage vs inversion-mode)
- **Subthreshold Behavior**: off-current controlled by channel depletion; subthreshold swing S = (kT/q) × ln(10) × (1 + C_dep/C_ox); for thin channels, C_dep << C_ox, S approaches ideal 60 mV/decade; better than inversion-mode for short channels

**Fabrication Process:**
- **Uniform Doping**: entire Si film doped uniformly by ion implantation or in-situ doped epitaxy; NMOS: P or As doping 1-5×10¹⁹ cm⁻³; PMOS: B doping 1-5×10¹⁹ cm⁻³; no source/drain implants required; eliminates junction formation and dopant activation anneals
- **Thin Channel Formation**: SOI wafer with thin top Si layer (5-15nm); or nanowire/nanosheet with small thickness/diameter; channel must be thin enough for full depletion; thickness uniformity <1nm (3σ) required for Vt control
- **Gate Stack**: high-k metal gate (HfO₂ + work function metal) deposited by ALD; work function metal selected to achieve target Vt; NMOS requires low work function metal (TiAlC, 4.2-4.4 eV); PMOS requires high work function metal (TiN, 4.6-4.8 eV)
- **S/D Contact Formation**: contacts directly to heavily-doped S/D regions; no additional S/D implants or epitaxy; silicide (NiSi, TiSi) reduces contact resistance; contact resistance <1×10⁻⁸ Ω·cm² achievable due to high doping

**Advantages Over Inversion-Mode:**
- **Process Simplification**: eliminates S/D ion implantation, activation anneals, and halo/pocket implants; reduces thermal budget; fewer process steps; lower cost; particularly beneficial for thin-film transistors (TFTs) on glass or flexible substrates
- **No Dopant Gradients**: uniform doping eliminates random dopant fluctuation (RDF) at S/D junctions; reduces Vt variability by 30-50% vs inversion-mode; critical for sub-10nm devices where RDF dominates variability
- **Improved Subthreshold Slope**: S = 60-65 mV/decade maintained to shorter gate lengths than inversion-mode; enables lower Vt and lower operating voltage; 10-20% power reduction at same performance
- **Reduced Short-Channel Effects**: no Vt roll-off with gate length (flat Vt vs L curve); DIBL <20 mV/V for gate lengths down to 10nm; enables aggressive scaling without electrostatic degradation

**Challenges and Limitations:**
- **High Doping Requirement**: 10¹⁹-10²⁰ cm⁻³ doping required for proper operation; approaches solid solubility limits; high doping increases junction leakage and band-to-band tunneling (BTBT); limits off-state leakage reduction
- **Mobility Degradation**: high doping causes ionized impurity scattering; mobility reduced by 30-50% vs lightly-doped inversion-mode channels; partially offset by bulk conduction (no surface roughness scattering)
- **Thin Channel Requirement**: channel thickness must be <10nm for full depletion at reasonable doping; limits drive current (current ∝ channel cross-section); requires multiple parallel nanowires or nanosheets to achieve adequate drive current
- **Work Function Engineering**: Vt tuning relies entirely on gate work function (no channel doping adjustment); requires precise work function metal composition control; multi-Vt libraries challenging (need different metals for each Vt)

**Device Architectures:**
- **Planar Junctionless (SOI)**: thin SOI (5-10nm top Si) with uniform doping; gate wraps three sides (tri-gate) or top only (planar); simplest junctionless structure; limited electrostatic control; suitable for gate lengths >20nm
- **Junctionless Nanowire**: cylindrical nanowire (diameter 5-10nm) with uniform doping; gate wraps completely (GAA); excellent electrostatics; subthreshold slope 62-65 mV/decade; scalable to <10nm gate length; used in research demonstrations
- **Junctionless FinFET**: fin width 5-10nm, height 20-40nm, uniform doping; gate wraps three sides; better electrostatics than planar; drive current higher than nanowire (larger cross-section); practical for manufacturing
- **Junctionless Nanosheet**: horizontal nanosheets (thickness 5-7nm) with uniform doping; gate wraps all surfaces; combines GAA electrostatics with higher drive current than nanowires; potential for 3nm node and beyond

**Performance Characteristics:**
- **Drive Current**: limited by channel cross-section and mobility; 10nm diameter nanowire: 50-80 μA at Vdd=0.7V; 30-40% lower than inversion-mode due to mobility degradation; requires more parallel channels to match performance
- **Off-State Leakage**: 10-100 pA per device depending on doping and dimensions; BTBT leakage increases with doping (∝ N_d²); trade-off between on-current (higher doping) and off-current (lower doping)
- **Switching Speed**: comparable to inversion-mode at same drive current; lower gate capacitance (no inversion charge) partially compensates for lower mobility; delay 10-20% higher than optimized inversion-mode
- **Variability**: σVt = 15-25mV for 10nm nanowire; 30-40% better than inversion-mode due to elimination of RDF; line-edge roughness becomes dominant variability source; diameter/thickness control critical

**Applications:**
- **Thin-Film Transistors (TFTs)**: junctionless TFTs on glass or flexible substrates for displays; low-temperature process (<400°C) compatible with glass; eliminates high-temperature dopant activation; mobility 10-50 cm²/V·s sufficient for display backplanes
- **3D NAND Flash**: junctionless vertical channel in 3D NAND; uniform poly-Si channel doping; eliminates junction formation in vertical structure; enables >100 layer stacking; used in production by some manufacturers
- **Biosensors**: junctionless nanowire FETs for label-free biosensing; uniform doping provides stable baseline; surface charge from biomolecule binding modulates channel depletion; sensitivity 10-100× higher than inversion-mode
- **Radiation-Hard Electronics**: junctionless devices show improved radiation tolerance; no junctions to degrade; uniform doping reduces single-event effects; used in space and nuclear applications

Junctionless transistors are **the elegant simplification of FET physics — eliminating the source/drain junctions that have defined transistors for 70 years, trading some performance for dramatically reduced process complexity and variability, finding applications in thin-film electronics, 3D memory, and sensors where their unique advantages outweigh the drive current limitations**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/junctionless-transistors) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
