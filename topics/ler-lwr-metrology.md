# LER/LWR Metrology

**Keywords**: ler/lwr metrology, ler/lwr, metrology

---

**LER/LWR Metrology** combines **Line Edge Roughness and Line Width Roughness characterization** — measuring nanometer-scale variations in patterned feature edges and widths that impact transistor performance, yield, and reliability, critical for advanced lithography process control and EUV patterning quality assessment.

**What Is LER/LWR Metrology?**

- **LER (Line Edge Roughness)**: Edge position variation along a single feature edge (3σ, nm).
- **LWR (Line Width Roughness)**: Line width variation along feature length (3σ, nm).
- **Relationship**: LWR combines both edge variations: LWR² ≈ 2×LER² (if uncorrelated).
- **Critical Metric**: Key indicator of patterning quality and process control.

**Why LER/LWR Matters**

- **Transistor Variability**: Edge roughness causes threshold voltage variation.
- **Performance Impact**: Increased delay variation, reduced circuit speed.
- **Yield Loss**: Severe roughness can cause shorts or opens.
- **EUV Lithography**: Stochastic effects make LER/LWR critical challenge.
- **Scaling Limit**: May limit continued feature size reduction.

**Measurement Techniques**

**CD-SEM (Critical Dimension Scanning Electron Microscope)**:
- **Method**: High-resolution SEM imaging of feature edges.
- **Process**: Multiple measurements along feature length.
- **Analysis**: Statistical analysis of edge position variations.
- **Advantages**: High resolution, direct edge visualization.
- **Typical Use**: Primary method for LER/LWR characterization.

**AFM (Atomic Force Microscopy)**:
- **Method**: 3D surface profile measurement.
- **Advantages**: True 3D profile, sidewall angle information.
- **Limitations**: Slower than SEM, tip convolution effects.
- **Typical Use**: Reference metrology, sidewall roughness.

**Scatterometry (Optical CD)**:
- **Method**: Optical diffraction pattern analysis.
- **Advantages**: Fast, non-destructive, inline capable.
- **Limitations**: Average values, less spatial detail than SEM.
- **Typical Use**: High-throughput monitoring, trend tracking.

**LER/LWR Specifications**

**Advanced Node Targets**:
- **7nm/5nm**: LER < 2nm (3σ) typical requirement.
- **3nm and Below**: LER < 1.5nm increasingly critical.
- **EUV Patterning**: Tighter specs due to stochastic effects.

**Frequency Decomposition**:
- **Low-Frequency (Systematic)**: Long-range edge variations.
- **High-Frequency (Stochastic)**: Short-range random variations.
- **Impact**: Different frequencies affect different failure modes.

**Impact on Device Performance**

**Threshold Voltage Variation**:
- **Mechanism**: Edge roughness modulates channel width.
- **Impact**: ΔVth increases with LWR, affects circuit timing.
- **Scaling**: Relative impact worsens at smaller dimensions.

**Drive Current Variation**:
- **Mechanism**: Width variation directly affects current.
- **Impact**: Performance binning, reduced yield.
- **Statistical**: Must account for in circuit design.

**Leakage Current**:
- **Mechanism**: Narrow regions have higher leakage.
- **Impact**: Increased standby power, thermal issues.
- **Reliability**: Accelerated aging in high-leakage regions.

**Failure Modes**:
- **Shorts**: Severe roughness can cause adjacent line bridging.
- **Opens**: Extreme narrowing can cause line breaks.
- **Reliability**: Weak points accelerate electromigration.

**Sources of LER/LWR**

**Photoresist Effects**:
- **Molecular Size**: Polymer chain dimensions set lower limit.
- **Acid Diffusion**: Chemical amplification creates roughness.
- **Shot Noise**: Photon statistics in exposure.

**Etch Process**:
- **Etch Selectivity**: Non-uniform etch rates amplify roughness.
- **Sidewall Passivation**: Incomplete passivation increases roughness.
- **Plasma Damage**: Ion bombardment creates surface roughness.

**EUV Stochastic Effects**:
- **Photon Shot Noise**: Low photon counts create statistical variation.
- **Resist Stochastics**: Molecular-scale randomness in resist.
- **Secondary Electron Blur**: Electron scattering adds roughness.

**LER/LWR Reduction Strategies**

**Resist Optimization**:
- **High-Performance Resists**: Optimized for low LER.
- **Molecular Design**: Smaller molecules, controlled diffusion.
- **Sensitizer Loading**: Balance sensitivity and roughness.

**Exposure Optimization**:
- **Higher Dose**: Reduces shot noise, improves LER.
- **Optimized Illumination**: Pupil optimization for edge quality.
- **Multiple Patterning**: Pitch division reduces roughness.

**Post-Lithography Treatment**:
- **Thermal Reflow**: Smooths resist edges before etch.
- **Chemical Smoothing**: Selective dissolution of roughness.
- **Plasma Treatment**: Controlled surface modification.

**Etch Optimization**:
- **High Selectivity**: Minimize resist erosion.
- **Sidewall Passivation**: Uniform protective layer.
- **Low Damage**: Reduce ion bombardment energy.

**Measurement & Analysis**

**Power Spectral Density (PSD)**:
- **Method**: Frequency analysis of edge position.
- **Information**: Roughness amplitude vs. spatial frequency.
- **Use**: Identify dominant roughness sources.

**Correlation Length**:
- **Definition**: Distance over which edge positions are correlated.
- **Significance**: Relates to physical roughness mechanisms.
- **Typical Values**: 10-50nm for resist, 20-100nm post-etch.

**Height-Height Correlation**:
- **Method**: Statistical correlation of edge positions.
- **Information**: Roughness scaling behavior.
- **Use**: Characterize roughness growth mechanisms.

**Challenges at Advanced Nodes**

**Measurement Resolution**:
- **Requirement**: Sub-nanometer precision for <2nm LER.
- **SEM Limitations**: Noise floor, edge detection algorithms.
- **Solution**: Advanced SEM, improved image processing.

**Sampling Statistics**:
- **Requirement**: Many measurements for statistical confidence.
- **Challenge**: Balance throughput vs. statistical rigor.
- **Solution**: Automated measurement, smart sampling.

**3D Effects**:
- **Challenge**: Sidewall roughness, not just top-down.
- **Measurement**: Requires 3D metrology (AFM, cross-section).
- **Impact**: 2D measurements may underestimate true roughness.

**Process Control**

**Inline Monitoring**:
- **Frequency**: Every lot or wafer for critical layers.
- **Locations**: Multiple sites across wafer.
- **Action Limits**: Trigger process adjustment or hold.

**Correlation to Electrical**:
- **Method**: Correlate LER/LWR to device parameters.
- **Metrics**: Vth variation, drive current distribution.
- **Use**: Validate metrology, set specifications.

**Tools & Vendors**

- **Hitachi**: High-resolution CD-SEM systems.
- **AMAT (Applied Materials)**: SEMVision for automated LER/LWR.
- **KLA**: eSL10 e-beam metrology.
- **Bruker**: AFM for 3D roughness characterization.

LER/LWR Metrology is **critical for advanced semiconductor manufacturing** — as EUV lithography and stochastic effects make edge roughness a primary challenge, precise measurement and control of LER/LWR becomes essential for maintaining transistor performance, yield, and reliability at 7nm and below.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/ler-lwr-metrology) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
