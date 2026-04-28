# Multi-Bridge-Channel FET (MBCFET)

**Keywords**: multi bridge channel fet mbcfet,multi bridge channel structure,mbcfet vs nanosheet,mbcfet fabrication process,mbcfet electrostatics

---

**Multi-Bridge-Channel FET (MBCFET)** is **Samsung's implementation of gate-all-around transistor architecture featuring multiple horizontally-stacked silicon bridge channels with gate electrodes wrapping all surfaces — providing the electrostatic control and drive current density required for 3nm and 2nm nodes through 3-5 vertically-stacked nanosheets with optimized width (15-35nm), thickness (5-7nm), and spacing (10-12nm) to balance performance, power, and manufacturability**.

**MBCFET Architecture:**
- **Bridge Channel Geometry**: each channel is a horizontal Si nanosheet (bridge) suspended between S/D regions; width 15-35nm (lithographically defined, continuously variable); thickness 5-7nm (epitaxially defined); length 12-16nm (gate length); 3-5 bridges stacked vertically with 10-12nm spacing
- **Gate-All-Around Wrapping**: gate electrode (work function metal + fill metal) wraps all four sides of each bridge plus top and bottom surfaces; 360° gate control provides superior electrostatics vs FinFET (270° control); enables aggressive gate length scaling to 12nm with acceptable short-channel effects
- **Effective Width**: W_eff = N_bridges × (2 × thickness + width) where N_bridges is stack count; for 3 bridges, 6nm thick, 25nm wide: W_eff = 3 × (12 + 25) = 111nm; drive current scales linearly with W_eff; width tuning enables precise current matching for standard cells
- **Comparison to FinFET**: FinFET width quantized to fin pitch (20-30nm); MBCFET width continuously variable; MBCFET achieves 30-40% higher drive current per footprint through optimized width and superior electrostatics; MBCFET leakage 2-3× lower at same performance

**Samsung 3nm Process (3GAE):**
- **First-Generation MBCFET**: 3 nanosheet stack; sheet width 20-30nm; sheet thickness 6nm; vertical spacing 12nm; gate length 14-16nm; gate pitch 48nm; fin pitch 24nm; contacted poly pitch (CPP) 48nm; metal pitch (MP) 24nm (M0/M1)
- **Performance Targets**: NMOS drive current 1.8-2.0 mA/μm at Vdd=0.75V, 100nA/μm off-current; PMOS drive current 1.4-1.6 mA/μm; 45% performance improvement vs 5nm FinFET at same power; 50% power reduction at same performance
- **Transistor Density**: 150-170 million transistors per mm² for logic; 2× density vs 5nm FinFET; enabled by GAA electrostatics allowing tighter spacing and lower voltage operation
- **Production Status**: mass production started Q2 2022; yields >90% by Q4 2022; customers include Qualcomm (Snapdragon 8 Gen 2), Google (Tensor G3), and Samsung Exynos; first high-volume GAA production in industry

**Samsung 2nm Process (2GAP):**
- **Second-Generation MBCFET**: 4-5 nanosheet stack; sheet width 15-25nm; sheet thickness 5nm; vertical spacing 10nm; gate length 12-14nm; gate pitch 44nm; fin pitch 22nm; CPP 44nm; MP 20nm (M0/M1)
- **Advanced Features**: backside power delivery network (BS-PDN) separates power and signal routing; buried power rails reduce standard cell height by 10-15%; nanosheet width optimization per standard cell for area-performance-power balance
- **Performance Targets**: 15-20% performance improvement vs 3nm at same power; 25-30% power reduction at same performance; operating voltage 0.65-0.70V for high-performance, 0.55-0.60V for low-power
- **Production Timeline**: risk production 2024; mass production 2025-2026; target customers include Qualcomm, Google, and Samsung mobile processors; competing with TSMC N2 (also GAA-based)

**Fabrication Process Highlights:**
- **Superlattice Epitaxy**: Si (6nm) / SiGe (12nm) alternating layers grown by RPCVD at 600°C; SiGe composition 30% Ge for etch selectivity; 3-layer stack for 3nm, 4-5 layer stack for 2nm; thickness uniformity <3% across 300mm wafer
- **EUV Lithography**: 0.33 NA EUV for critical layers (fin, gate, via); single EUV exposure replaces 193i multi-patterning; reduces overlay error to <1.5nm; enables tighter pitches and improved yield; 10-12 EUV layers in 3nm process, 13-15 layers in 2nm
- **Inner Spacer**: SiOCN (k~4.5) deposited by PEALD; thickness 4nm; length 6nm; reduces gate-to-S/D capacitance by 30% vs SiN spacer; critical for high-frequency performance; conformality >90% in 12nm vertical gaps
- **High-k Metal Gate**: HfO₂ (2.5nm, EOT 0.8nm) + work function metal (TiN for PMOS, TiAlC for NMOS) + W fill; conformal ALD wraps all nanosheet surfaces; work function tuning provides multi-Vt options (3-4 Vt flavors for standard cell library)

**Electrostatic Advantages:**
- **Short-Channel Control**: subthreshold swing 65-68 mV/decade maintained to 12nm gate length; DIBL <20 mV/V; off-state leakage <50 pA/μm; enables 0.65V operation for low-power applications without excessive leakage
- **Vt Roll-Off Suppression**: Vt variation with gate length <30 mV for 12-16nm range; FinFET shows >100 mV roll-off in same range; GAA electrostatics suppress short-channel effects through complete gate control
- **Variability Reduction**: random dopant fluctuation (RDF) eliminated by undoped channels; line-edge roughness (LER) becomes dominant variability source; σVt <15mV achieved with <1nm LER control; 30% better than FinFET
- **Scalability**: GAA architecture scales to 1nm node and beyond; nanosheet thickness reduces to 3-4nm; width reduces to 10-15nm; stack count increases to 5-6; gate length approaches 10nm; electrostatic control maintained through geometry optimization

**Design and Integration:**
- **Standard Cell Library**: 5-6 track height cells for 3nm; 4-5 track height for 2nm; multiple Vt options (ULVT, LVT, RVT, HVT) for power-performance optimization; nanosheet width varied per cell for drive strength tuning without area penalty
- **SRAM**: 6T SRAM cell size 0.021 μm² (3nm), 0.016 μm² (2nm); bit cell height 12-14 fins; GAA enables lower Vmin (0.6-0.65V) vs FinFET (0.7-0.75V); improves SRAM yield and power efficiency
- **Analog and I/O**: thick-oxide devices for 1.8V and 3.3V I/O; longer gate length (50-100nm) for better matching and lower noise; separate mask set for analog-optimized transistors; RF performance to 100+ GHz for mmWave applications
- **EDA Tool Support**: Samsung PDK (process design kit) includes SPICE models, layout rules, and standard cell libraries; place-and-route tools optimized for MBCFET; timing and power analysis tools account for nanosheet-specific parasitics

Multi-Bridge-Channel FET is **Samsung's successful commercialization of gate-all-around transistor technology — demonstrating that GAA can be manufactured at high volume with acceptable yields and costs, enabling continued Moore's Law scaling through 3nm and 2nm nodes and establishing the architectural foundation for 1nm and beyond in the late 2020s**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/multi-bridge-channel-fet-mbcfet) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
