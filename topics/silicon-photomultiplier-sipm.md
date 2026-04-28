# Silicon Photomultiplier (SiPM)

**Keywords**: silicon photomultiplier sipm,geiger mode apd,sipm photon detection efficiency,sipm dark count rate,sipm application lidar pet

---

**Silicon Photomultiplier (SiPM)** is the **solid-state single-photon detector comprising Geiger-mode avalanche photodiode (APD) array — enabling compact, low-voltage photon counting with excellent timing resolution and sensitivity for medical imaging and LiDAR applications**.

**Geiger-Mode APD Concept:**
- Geiger mode operation: reverse bias above breakdown voltage V_BD; single charge carriers trigger full breakdown
- Avalanche multiplication: primary photon-generated electron triggers exponential impact ionization; develops into macroscopic current
- Full breakdown: voltage above V_BD enables complete breakdown; large pulse (mV amplitude) from single photon
- Recovery mechanism: quenching resistor limits current; allows voltage recovery after breakdown
- Binary response: Geiger-mode output essentially binary (triggered or not); photon detection probability-based

**SiPM Microcell Array:**
- Array structure: hundreds to thousands of Geiger-mode APD cells (~10-100 μm scale) in parallel
- Cell density: pixel contains ~1000 cells typical; enables higher detection efficiency and reduced dark count
- Independent biasing: each cell biased above breakdown; independent quenching resistors
- Additive output: total pixel output is sum of fired cells; number of cells firing indicates photon number
- Photon number resolution: multiple photons create multi-level signal; number of photons counted (up to saturation)

**Photon Detection Efficiency (PDE):**
- Definition: PDE = quantum efficiency × collection efficiency × Geiger efficiency; probability of detecting single photon
- Quantum efficiency: fraction of incident photons generating electron-hole pairs; typically 30-50% for Si PD
- Collection efficiency: fraction of generated carriers collected (geometry dependent); ~90% typical
- Geiger efficiency: fraction of collected carriers triggering full breakdown; typically 50-80%
- Wavelength dependence: quantum efficiency peaks in near-IR (400-600 nm); decreases for blue/UV
- PDE improvement: new device structures, improved collection, enhanced Geiger probability; ongoing development

**Dark Count Rate (DCR):**
- Thermal generation: thermally-generated carriers triggering Geiger breakdown without incident photon
- Temperature dependence: DCR doubles every ~7-8°C; exponential T dependence; cooling reduces DCR
- Bias dependence: DCR increases exponentially with excess bias (V - V_BD); higher bias = more dark counts
- Measurement: dark count rate typically few hundred kHz to few MHz at room temperature
- Cooling benefit: cryogenic operation dramatically reduces DCR; enables single-photon sensitivity in dim light

**Optical Crosstalk:**
- Breakdown-induced photons: Geiger breakdown generates optical photons; can trigger neighboring cells
- Secondary breakdown: optical photons from one cell trigger neighbor cells; correlated firing
- Crosstalk probability: few percent typical; depends on cell density and optical design
- Spectral dependence: crosstalk wavelength matched to Si bandgap (~1100 nm); infrared photons
- Reduction techniques: absorbing trenches between cells; optical isolation improves independence

**Quenching Resistor:**
- Passive quenching: on-chip resistor provides bias current limiting; current-limited breakdown
- Quenching time: RC time constant; longer time → lower noise but slower recovery
- Recovery time: ~10-100 ns typical; determines maximum count rate (saturation)
- Dead time: fraction of time cell unable to detect photons (during recovery); affects count rate at high photon flux
- Active quenching: external active circuits faster quenching; >100 MHz count rates possible

**Dynamic Range and Saturation:**
- Number of cells: pixel with N cells provides N levels of output (up to N saturated)
- Saturation: when all cells fired; further photons not counted; output saturates
- Linear range: typically 10-50% of maximum cells; beyond this, counting becomes nonlinear
- Extending range: multiple lower-gain stages; hybrid devices; logarithmic output
- Photon flux limits: single photon detectors typically limited to ~10 MHz count rates without saturation

**Timing Resolution:**
- Time resolution: excellent timing; individual cell has ~30-100 ps resolution
- Aggregate timing: pixel-level timing derived from fastest cell trigger; ~100-200 ps typical
- Application: time-of-flight (ToF) LiDAR applications benefit from excellent timing
- Timing jitter: small jitter enables accurate time-of-flight distance measurements; depth precision

**Temperature Dependence:**
- Breakdown voltage drift: V_BD increases with temperature (~+40 mV/°C typical); requires voltage adjustment
- Gain changes: excess bias changes with temperature; automatic gain control circuits compensate
- Crosstalk temperature: increases with temperature; more photon overlap
- Dark count temperature: dominant limitation; exponential increase motivates cooling

**Applications in LiDAR:**
- ToF LiDAR: measure light flight time to target; depth/range image creation
- Single-photon detection: photons scattered from target; SiPM excellent single-photon sensitivity
- Long-range capability: improved SNR enables longer range (100+ meters)
- Daytime operation: timing resolution enables operation in sunlight (background photons rejected via time gating)

**PET Imaging Application:**
- Scintillation coupling: SiPM coupled to scintillation crystals (BGO, LYSO); detect gamma rays indirectly
- Timing coincidence: two SiPMs detect annihilation photons; timing coincidence identifies true events vs background
- Timing resolution importance: better timing → improved SNR and image quality
- Compact design: solid-state SiPM vs PMT (vacuum tube); enables compact portable PET scanners
- Cost reduction: integrated SiPM+electronics enables affordable high-volume PET scanners

**Comparison with Photomultiplier Tube (PMT):**
- Voltage: SiPM ~70 V vs PMT ~1000 V; SiPM battery-compatible
- Size: SiPM mm-scale vs PMT cm-scale; enables compact detectors
- Immunity: SiPM immune to magnetic fields; operates in MRI unlike PMT
- Cooling: SiPM benefits from cooling (reduce DCR); PMT no temperature benefit
- Cost: SiPM lower cost at scale; enables widespread deployment

**Silicon photomultipliers provide solid-state single-photon detection through Geiger-mode avalanche arrays — enabling compact, low-voltage photon counting for LiDAR and medical imaging with excellent timing and detection efficiency.**

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/silicon-photomultiplier-sipm) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
