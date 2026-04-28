# Semiconductor Manufacturing Etch Endpoint Process

**Keywords**: endpoint detection, etch endpoint, optical emission spectroscopy, OES, interferometry, endpoint monitoring, process control

---

**Semiconductor Manufacturing Etch Endpoint Process**

**Overview**

In semiconductor fabrication, **etching** selectively removes material from wafers to create circuit patterns. The **endpoint detection problem** is determining precisely when to stop etching.

$$
\text{Endpoint} = f(\text{target layer removal}, \text{underlayer preservation})
$$



**The Core Challenge**

**Why Endpoint Detection Matters**

- **Under-etching**: Leaves residual material → defects, shorts, incomplete patterns
- **Over-etching**: Damages underlying layers → profile degradation, reliability issues

At advanced nodes (3nm, 5nm), tolerances are measured in angstroms:

$$
\Delta d_{\text{tolerance}} \approx 1-5 \text{ Å}
$$



**Primary Endpoint Detection Techniques**

**1. Optical Emission Spectroscopy (OES)**

The most widely used technique for plasma (dry) etching.

**Principle**

During plasma etching, reactive species and etch byproducts emit characteristic photons. The emission intensity $I(\lambda)$ at wavelength $\lambda$ follows:

$$
I(\lambda) \propto n_{\text{species}} \cdot \sigma_{\text{emission}}(\lambda) \cdot E_{\text{plasma}}
$$

Where:

- $n_{\text{species}}$ = density of emitting species
- $\sigma_{\text{emission}}$ = emission cross-section
- $E_{\text{plasma}}$ = plasma excitation energy

**Key Wavelengths for Common Etch Chemistries**

| Species | Wavelength (nm) | Application |
|---------|-----------------|-------------|
| CO | 483.5, 519.8 | SiO₂ etch indicator |
| F | 685.6, 703.7 | Fluorine radical monitoring |
| Si | 288.2 | Silicon exposure detection |
| Cl | 837.6 | Chlorine-based etch |
| O | 777.4 | Oxygen monitoring |

**Signal Processing**

The endpoint is typically detected using derivative methods:

$$
\frac{dI}{dt} = \lim_{\Delta t \to 0} \frac{I(t + \Delta t) - I(t)}{\Delta t}
$$

Endpoint trigger condition:

$$
\left| \frac{dI}{dt} \right| > \theta_{\text{threshold}}
$$

**Advantages**

- Non-contact, non-destructive measurement
- Real-time monitoring capability
- Works across entire wafer surface

**Limitations**

- Weak signals for very thin films ($d < 10$ nm)
- Pattern density affects signal intensity
- Requires optical access to plasma chamber



**2. Laser Interferometry**

**Principle**

A monochromatic laser beam reflects from the wafer surface. As etching progresses, film thickness changes alter the interference pattern.

The reflected intensity follows:

$$
I_{\text{reflected}} = I_1 + I_2 + 2\sqrt{I_1 I_2} \cos\left(\frac{4\pi n d}{\lambda} + \phi_0\right)
$$

Where:

- $I_1, I_2$ = intensities from top surface and interface reflections
- $n$ = refractive index of the film
- $d$ = film thickness
- $\lambda$ = laser wavelength
- $\phi_0$ = initial phase offset

**Fringe Analysis**

Each complete oscillation (fringe) corresponds to:

$$
\Delta d_{\text{per fringe}} = \frac{\lambda}{2n}
$$

**Example calculation** for SiO₂ with HeNe laser ($\lambda = 632.8$ nm):

$$
\Delta d = \frac{632.8 \text{ nm}}{2 \times 1.46} \approx 216.7 \text{ nm/fringe}
$$

**Etch Rate Determination**

$$
\text{Etch Rate} = \frac{\lambda}{2n} \cdot \frac{1}{T_{\text{fringe}}}
$$

Where $T_{\text{fringe}}$ is the period of one complete oscillation.

**Advantages**

- Quantitative thickness measurement
- Real-time etch rate monitoring
- High precision for transparent films

**Limitations**

- Requires optically transparent or semi-transparent films
- Pattern density complicates signal interpretation
- Multiple interfaces create complex interference



**3. Residual Gas Analysis (Mass Spectrometry)**

**Principle**

Analyze exhaust gas composition. Different materials produce different volatile byproducts:

$$
\text{Material}_{\text{solid}} + \text{Etchant}_{\text{gas}} \rightarrow \text{Byproduct}_{\text{volatile}}
$$

**Example Reactions**

**Silicon etching with fluorine:**

$$
\text{Si} + 4\text{F} \rightarrow \text{SiF}_4 \uparrow
$$

**Oxide etching with fluorine:**

$$
\text{SiO}_2 + 4\text{F} \rightarrow \text{SiF}_4 + \text{O}_2 \uparrow
$$

**Aluminum etching with chlorine:**

$$
\text{Al} + 3\text{Cl} \rightarrow \text{AlCl}_3 \uparrow
$$

**Mass-to-Charge Ratios**

| Byproduct | m/z | Parent Material |
|-----------|-----|-----------------|
| SiF₄ | 104 | Si, SiO₂ |
| SiCl₄ | 170 | Si |
| AlCl₃ | 133 | Al |
| CO₂ | 44 | SiO₂, organics |
| TiCl₄ | 190 | Ti, TiN |

**Advantages**

- Works regardless of optical properties
- Chemically specific detection
- Can detect multiple transitions

**Limitations**

- Response time limited by gas transport: $\tau \approx 0.5-2$ s
- Requires differential pumping
- Sensitivity issues at low etch rates



**4. RF Impedance Monitoring**

**Principle**

Plasma impedance changes when material composition changes. The plasma can be modeled as:

$$
Z_{\text{plasma}} = R_{\text{plasma}} + j\omega L_{\text{plasma}} + \frac{1}{j\omega C_{\text{sheath}}}
$$

**Monitored Parameters**

- **Voltage**: $V_{\text{RF}}$
- **Current**: $I_{\text{RF}}$
- **Phase**: $\phi = \arctan\left(\frac{X}{R}\right)$
- **Impedance magnitude**: $|Z| = \sqrt{R^2 + X^2}$

**Advantages**

- Uses existing RF infrastructure
- No additional optical access needed
- Sensitive to plasma chemistry changes

**Limitations**

- Subtle signal changes
- Affected by many process parameters
- Requires sophisticated signal processing



**Advanced Considerations**

**Aspect Ratio Dependent Etching (ARDE)**

High aspect ratio (HAR) features etch slower due to transport limitations:

$$
\text{Etch Rate}(AR) = \text{Etch Rate}_0 \cdot \exp\left(-\frac{AR}{AR_c}\right)
$$

Where:

- $AR = \frac{\text{depth}}{\text{width}}$ = aspect ratio
- $AR_c$ = characteristic aspect ratio (process-dependent)

**Consequence**: Dense arrays reach endpoint before isolated features.

**Pattern Loading Effect**

Local etch rate depends on pattern density $\rho$:

$$
ER(\rho) = ER_{\text{open}} \cdot \frac{1}{1 + K \cdot \rho}
$$

Where $K$ is the loading coefficient.

**Selectivity**

The selectivity $S$ between materials A and B:

$$
S = \frac{ER_A}{ER_B}
$$

**Higher selectivity allows more overetch margin:**

$$
t_{\text{overetch,max}} = \frac{d_{\text{underlayer}} \cdot S}{ER_A}
$$



**Practical Endpoint Strategy**

**Overetch Calculation**

Total etch time:

$$
t_{\text{total}} = t_{\text{endpoint}} + t_{\text{overetch}}
$$

Overetch percentage:

$$
\text{Overetch \%} = \frac{t_{\text{overetch}}}{t_{\text{main}}} \times 100
$$

Typical values: 20-50% depending on uniformity and selectivity.

**Statistical Process Control**

Endpoint time follows a distribution:

$$
t_{\text{EP}} \sim \mathcal{N}(\mu_{\text{EP}}, \sigma_{\text{EP}}^2)
$$

Control limits:

$$
\text{UCL} = \mu + 3\sigma, \quad \text{LCL} = \mu - 3\sigma
$$



**Multi-Sensor Fusion**

Modern systems combine multiple techniques:

$$
\text{Endpoint}_{\text{final}} = \sum_{i} w_i \cdot \text{Signal}_i
$$

Where weights $w_i$ are optimized by machine learning algorithms.

**Sensor Contributions**

| Sensor | Primary Detection |
|--------|-------------------|
| OES | Bulk composition change |
| Interferometry | Precise thickness |
| RF monitoring | Plasma state shifts |
| Full-wafer imaging | Spatial uniformity |



**Key Equations Summary**

**Interferometry**

$$
\boxed{\Delta d = \frac{\lambda}{2n}}
$$

**OES Endpoint Trigger**

$$
\boxed{\left| \frac{dI}{dt} \right| > \theta}
$$

**Selectivity**

$$
\boxed{S = \frac{ER_{\text{target}}}{ER_{\text{stop}}}}
$$

**ARDE Model**

$$
\boxed{ER(AR) = ER_0 \cdot e^{-AR/AR_c}}
$$



**Conclusion**

Etch endpoint detection is critical for:

1. **Yield**: Complete clearing without damage
2. **Uniformity**: Consistent results across wafer
3. **Reliability**: Device performance and longevity

The combination of OES, interferometry, mass spectrometry, and RF monitoring—enhanced by machine learning—enables the precision required for sub-10nm semiconductor manufacturing.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/endpoint-detection) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
