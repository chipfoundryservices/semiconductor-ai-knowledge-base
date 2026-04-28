# CMOS Image Sensor Pixel Architecture

**Keywords**: cmos image sensor pixel architecture,4t pixel shared readout,correlated double sampling cds,pixel source follower,rolling global shutter

---

**CMOS Image Sensor Pixel Architecture** is the **active pixel sensor with integrated transistor amplification enabling parallel readout — achieving high frame rates and flexible architecture compared to passive CCD sensors through source-follower and correlated double sampling**.

**4T Pixel (Four-Transistor) Architecture:**
- Photodiode: converts photons to charge; collects photocurrent during integration
- Transfer transistor (TX): switches charge transfer from photodiode to floating diffusion
- Reset transistor (RST): resets floating diffusion to V_DD before integration
- Source follower (SF): buffered output amplifier; converts voltage for readout
- Select transistor (SEL): selects pixel for readout; gates off unselected rows
- Signal flow: photon → photodiode charge → TX transfer → SF amplification → column output

**Pinned Photodiode (PPD):**
- Pinned design: special photodiode with surface potential pinned by dopant layer
- Pinning benefit: reduces dark current (no surface recombination); improves noise
- Surface potential: pinned to constant value; enables stable operation over temperature
- Full-well capacity: set by pinning doping and design; typically 3,000-10,000 electrons
- Dark current: greatly reduced via pinning vs conventional photodiode; low noise

**Correlated Double Sampling (CDS):**
- Reset noise (kTC noise): thermal noise from reset transistor reset operation; dominant noise at low signal
- Two-sample approach: sample reset level; sample signal+reset level
- Noise cancellation: subtract reset noise from signal; ideally eliminates reset noise
- CDS implementation: analog or digital correlated double sampling
- Noise improvement: kTC noise virtually eliminated; read noise limited by source follower + column circuits

**Source Follower Gain:**
- Gate-source capacitance: source follower input impedance; sets gain in charge-to-voltage conversion
- Gain < 1: source follower gain typically 0.8-0.95; unity-gain buffer
- Impedance buffering: low output impedance; drives column line capacitance
- Noise contribution: source follower contributes 1/f and thermal noise
- Transconductance: higher transconductance → higher gain and faster settling

**Read Noise Performance:**
- Dominant sources: reset noise (kTC), source follower noise, column amplifier noise
- CDS reduction: reset noise greatly reduced via CDS; SF and column noise remain
- Typical read noise: 2-5 e⁻ RMS for standard CMOS; lower with multiple sampling techniques
- Noise reduction: multiple samples and averaging; temporal and spatial filtering
- Ultra-low noise pixels: specialized architectures (FD-sonorant, fully-differential) achieve <2 e⁻

**Rolling Shutter vs Global Shutter:**
- Rolling shutter: rows exposed and read sequentially; different rows exposed at different times
- Distortion: moving objects show slant/skew; fast motion causes image artifacts
- Efficiency: rolling shutter simpler; high frame rates (>1000 fps) easier
- Global shutter: all rows exposed simultaneously; uniform exposure time
- Synchronized readout: all rows read after synchronized exposure; requires more complex implementation
- Pixel size: global shutter transistors reduce fill factor; more complex architecture
- Application tradeoff: rolling shutter for video/high-speed; global shutter for motion-critical/industrial

**Pixel Size Scaling:**
- Density increase: smaller pixels enable higher resolution on same die area
- Challenges: smaller pixels → lower full-well capacity, higher dark current, increased crosstalk
- Diffraction limit: wavelength ~500 nm; pixels smaller than diffraction limit collect fewer photons
- Design trade-off: pixel pitch 1-5 μm typical; smaller → lower sensitivity
- Resolution scaling: 12 MP → 50 MP achieved via pixel size reduction and better design

**Stacked Sensor Architecture:**
- Logic die + pixel die: pixel die (back-side illuminated) stacked on logic die (signal processing)
- Back-side illumination (BSI): photons incident on rear surface; no front-side metal shading
- QE improvement: near-100% quantum efficiency over visible spectrum; excellent sensitivity
- Signal processing: analog-to-digital conversion, compression, signal processing on logic die
- Integration density: enables higher density via vertical stacking; improved performance

**HDR (High Dynamic Range) Pixel:**
- Multiple exposure integration: simultaneously integrate different exposure times
- Variable integration: different pixel regions exposed for different durations
- Output selection: lower gain branch for bright regions; higher gain for shadows
- Local exposure control: per-pixel or per-region exposure adjustment; mimics human eye
- Processing: tone mapping creates natural-looking image; extended dynamic range

**Shared Readout (Binning):**
- Pixel binning: multiple pixels combined into single output; increases full-well and sensitivity
- Summing pixels: analog or digital combination; reduces resolution
- Noise improvement: binning reduces read noise (√N improvement for N pixels)
- Flexibility: in-pixel or in-read-chain binning; programmable combining
- Trade-off: resolution vs sensitivity/noise; application-dependent optimization

**Column Amplifier Design:**
- Column-level amplification: amplifier per column; drives long column line to ADC
- Noise filtering: column amplifier bandwidth limited; reduces high-frequency noise
- Gain programming: adjustable gain per column; variable sensitivity
- Dynamic range: column amplifier limited dynamic range; determines signal swing
- Offset variation: per-column gain/offset trimming compensates manufacturing variation

**ADC Integration:**
- Per-column ADC: one ADC per column (very-high-speed imaging)
- Shared ADC: multiple columns time-share single ADC (reduced cost/power)
- In-pixel ADC: per-pixel analog-to-digital conversion (radical architecture)
- Bit depth: 8-14 bits typical; higher bits for low-light scenes, lower for video
- ADC noise: column/shared ADC limited resolution; matching architecture to application noise budget

**Photodiode Optimization:**
- Fill factor: fraction of pixel area photosensitive; smaller transistors improve fill factor
- Micro-lenses: on-chip micro-lens focuses light onto photodiode; improves light collection
- Color filters: RGB/Bayer pattern filters enable color imaging; reduces sensitivity via filtering
- AR coating: antireflection coating improves quantum efficiency
- Spectral response: optimization for visible, IR, or specific wavelength; tunable via design

**Crosstalk and Isolation:**
- Optical crosstalk: light from one pixel diffuses to neighbors; blur effect
- Isolation trenches: deep trench isolation reduces crosstalk; improves modulation transfer function
- Electrical crosstalk: charge sharing between neighboring pixels; adjacent-pixel correlation
- Isolation depth: deeper trenches improve isolation; increased process complexity
- Design rules: pixel-to-pixel spacing and isolation structure design critical

**Rolling vs Global Shutter Trade-offs:**
- Speed advantage: rolling shutter enables higher frame rates; global shutter simpler design
- Motion artifacts: rolling shutter causes skew; global shutter eliminates artifacts
- Pixel size: global shutter requires more transistors; reduced fill factor (75% vs 85%)
- Complexity: rolling shutter simpler control; global shutter requires synchronized exposure
- Application choice: video rolling preferred; industrial/automotive global shutter preferred

**CMOS image sensors enable parallel pixel readout with integrated amplification — achieving high frame rates and flexible architecture through source-follower gain and correlated double sampling noise reduction.**

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/cmos-image-sensor-pixel-architecture) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
