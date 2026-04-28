# Metal Gate Work Function Engineering

**Keywords**: metal gate work function,work function engineering,nmos pmos work function,metal gate materials,work function tuning

---

**Metal Gate Work Function Engineering** is **the precise control of the metal gate electrode's work function (4.0-5.2eV range) to set proper NMOS and PMOS threshold voltages without heavy channel doping — using different metal compositions, interface dipoles, and thermal treatments to achieve multiple threshold voltage options while maintaining low gate resistance and compatibility with high-k dielectrics in advanced CMOS processes**.

**Work Function Fundamentals:**
- **Work Function Definition**: energy required to remove an electron from the Fermi level to vacuum; determines the band alignment between metal gate and silicon channel
- **Threshold Voltage Relationship**: Vt = Φms + 2Φf + Qdepl/Cox where Φms is the metal-semiconductor work function difference; proper Φm sets desired Vt without excessive channel doping
- **NMOS Requirements**: work function 4.0-4.3eV (near silicon conduction band at 4.05eV) provides low Vt for NMOS; too high Φm requires heavy channel doping or produces high Vt
- **PMOS Requirements**: work function 4.9-5.2eV (near silicon valence band at 5.17eV) provides low |Vt| for PMOS; too low Φm causes threshold voltage issues

**Metal Gate Materials:**
- **TiN Base Material**: titanium nitride work function 4.5-4.8eV depending on composition, deposition method, and thermal history; serves as starting point for work function tuning
- **NMOS Metals**: TiAlN (titanium aluminum nitride) with Al content 20-50%; aluminum incorporation lowers work function by 0.1-0.3eV per 10% Al; Ti₀.₆Al₀.₄N provides ~4.2eV
- **PMOS Metals**: TiN with controlled oxygen or nitrogen content; oxygen incorporation increases work function; some processes use TaN, MoN, or RuO₂ for PMOS
- **Deposition Methods**: physical vapor deposition (PVD) or atomic layer deposition (ALD) at 300-450°C; ALD provides better conformality in high-aspect-ratio gates; PVD offers simpler process

**Work Function Tuning Mechanisms:**
- **Composition Tuning**: varying metal ratios (Ti/Al, Ti/Ta) adjusts work function over 0.5-1.0eV range; requires separate depositions for NMOS and PMOS with block masks
- **Oxygen/Nitrogen Content**: TiN work function shifts 0.2-0.4eV with oxygen incorporation during high-k deposition or post-deposition anneal; nitrogen content also affects work function
- **Thickness Effects**: very thin metal gates (<3nm) show work function shifts due to interface effects; work function stabilizes for thickness >5nm
- **Grain Size and Texture**: metal grain structure affects work function; (111) vs (200) texture can shift work function by 0.1-0.2eV; annealing modifies grain structure

**Interface Dipole Engineering:**
- **Lanthanum Doping**: La incorporation at high-k/SiO₂ interface creates interface dipole; shifts bands to reduce NMOS Vt by 0.2-0.4V without changing metal work function
- **Aluminum Doping**: Al at interface shifts PMOS Vt positive by 0.2-0.3V; enables Vt tuning without multiple metal depositions
- **Dipole Mechanism**: La or Al atoms create charge redistribution at interface; electric dipole modifies band alignment between metal and silicon
- **Implementation**: La or Al deposited as thin layer (0.2-0.5nm) at specific interface location; or incorporated during high-k deposition; requires precise control for reproducibility

**Multi-Vt Implementation:**
- **Dual Metal Gates**: separate NMOS metal (TiAlN) and PMOS metal (TiN) provide two Vt options; requires one block mask for selective deposition or removal
- **Triple Metal Gates**: three different metals or dipole combinations provide low-Vt, standard-Vt, and high-Vt options; requires two block masks
- **Work Function Span**: typical multi-Vt process provides 0.15-0.25V Vt spacing between options; total span 0.3-0.5V covers performance-power optimization range
- **Process Complexity**: each additional Vt option adds 1-2 mask layers; trade-off between design flexibility and manufacturing cost

**Thermal Stability:**
- **Work Function Shift**: metal gate work function shifts during high-temperature processing; TiN shifts 0.1-0.3eV during 1000°C anneals
- **Gate-First Challenges**: in gate-first integration, metal gate experiences full source/drain activation thermal budget (1000-1050°C); limits metal choices to thermally stable materials
- **Gate-Last Advantages**: replacement gate process deposits metal after high-temperature steps; enables use of less stable but optimal work function metals
- **Oxygen Diffusion**: oxygen from high-k or ambient diffuses into metal gate during anneals; oxygen incorporation shifts work function and must be controlled

**Integration Schemes:**
- **Gate-First with Stable Metals**: use thermally stable TiN-based metals; accept work function shifts and compensate with dipole engineering or channel doping
- **Gate-Last (Replacement Gate)**: deposit sacrificial poly gate, complete thermal processing, remove poly, deposit optimized metal gates; provides best work function control
- **Hybrid Approach**: deposit high-k gate-first (better interface), use poly placeholder, replace with metal gate-last; balances interface quality and work function optimization
- **Work Function Metal Thickness**: thin work function metal (3-10nm) followed by low-resistivity fill metal (W, Al); minimizes work function metal volume while maintaining low gate resistance

**Variability and Matching:**
- **Work Function Variation (WFV)**: metal grain structure and composition variations cause work function variability; σΦm = 30-80meV depending on metal and grain size
- **Threshold Voltage Impact**: WFV directly translates to Vt variability; 50meV work function variation causes 50mV Vt variation
- **Grain Size Effects**: larger grains reduce WFV; grain size 10-30nm typical; annealing increases grain size but may shift average work function
- **Matching**: analog circuits require Vt matching <5mV; large device areas average over many grains, reducing WFV impact; digital circuits tolerate 30-50mV mismatch

**Gate Resistance:**
- **Work Function Metal Resistivity**: TiN 50-100 μΩ·cm, TaN 200-300 μΩ·cm, TiAlN 100-200 μΩ·cm; higher than polysilicon (500-1000 μΩ·cm after silicidation)
- **Fill Metal**: tungsten (10-15 μΩ·cm) or aluminum (3-4 μΩ·cm) fills gate above thin work function metal; provides low gate resistance for high-frequency circuits
- **Gate RC Delay**: gate resistance × gate capacitance limits circuit speed; thin work function metal + thick fill metal optimizes work function and resistance
- **Scaling Challenges**: as gate width shrinks, gate resistance increases; requires careful optimization of metal stack and thickness

Metal gate work function engineering is **the critical enabler of high-k metal gate technology — by providing precise control over threshold voltage through material selection rather than channel doping, work function engineering enables low EOT scaling, reduced variability, and multiple Vt options that define the performance and power characteristics of every advanced CMOS technology from 45nm to 3nm**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/metal-gate-work-function) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
