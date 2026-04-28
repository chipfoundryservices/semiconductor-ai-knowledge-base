# ASML & EUV Lithography: Technical Overview

**Keywords**: asml,euv machine,lithography equipment,scanner

---

**ASML & EUV Lithography: Technical Overview**

**Table of Contents**

- [1. Introduction to ASML](#1-introduction-to-asml)
- [2. Lithography Fundamentals](#2-lithography-fundamentals)
- [3. EUV Technology](#3-euv-technology)
- [4. Scanner Systems](#4-scanner-systems)
- [5. Technical Specifications](#5-technical-specifications)
- [6. Geopolitical Context](#6-geopolitical-context)

---

**1. Introduction to ASML**

**Company Overview**

- **Full Name:** ASML Holding N.V.
- **Headquarters:** Veldhoven, Netherlands
- **Founded:** 1984 (spin-off from Philips)
- **Market Position:** Sole manufacturer of EUV lithography systems
- **Employees:** ~42,000+ worldwide

**Market Dominance**

- 100% market share in EUV lithography
- ~90% market share in advanced DUV lithography
- Critical supplier to all leading-edge semiconductor fabs

---

**2. Lithography Fundamentals**

**The Rayleigh Criterion**

The fundamental resolution limit in optical lithography is governed by the **Rayleigh Criterion**:

$$
R = k_1 \cdot \frac{\lambda}{NA}
$$

Where:

- $R$ = minimum resolvable feature size (half-pitch)
- $k_1$ = process-dependent factor (theoretical minimum: 0.25)
- $\lambda$ = wavelength of light
- $NA$ = numerical aperture of the optical system

**Depth of Focus (DOF)**

The depth of focus determines process tolerance:

$$
DOF = k_2 \cdot \frac{\lambda}{NA^2}
$$

Where:

- $DOF$ = depth of focus
- $k_2$ = process-dependent constant
- $\lambda$ = wavelength
- $NA$ = numerical aperture

**Resolution Enhancement Techniques (RET)**

1. **Optical Proximity Correction (OPC)**
   - Sub-resolution assist features (SRAFs)
   - Serif additions/subtractions
   - Line-end extensions

2. **Phase-Shift Masks (PSM)**
   - Alternating PSM
   - Attenuated PSM
   - Phase difference: $\Delta\phi = \pi$ (180°)

3. **Multiple Patterning**
   - LELE (Litho-Etch-Litho-Etch)
   - SADP (Self-Aligned Double Patterning)
   - SAQP (Self-Aligned Quadruple Patterning)

---

**3. EUV Technology**

**Wavelength Comparison**

| Technology | Wavelength ($\lambda$) | Relative Resolution |
|------------|------------------------|---------------------|
| i-line     | 365 nm                 | 1.00×               |
| KrF DUV    | 248 nm                 | 1.47×               |
| ArF DUV    | 193 nm                 | 1.89×               |
| ArF Immersion | 193 nm (effective ~134 nm) | 2.72×        |
| **EUV**    | **13.5 nm**            | **27.04×**          |

**EUV Light Generation Process**

The **Laser-Produced Plasma (LPP)** source generates EUV light:

1. **Tin Droplet Generation**
   - Droplet diameter: $\approx 25 \, \mu m$
   - Droplet velocity: $v \approx 70 \, m/s$
   - Droplet frequency: $f = 50,000 \, Hz$

2. **Pre-Pulse Laser**
   - Flattens the tin droplet into a pancake shape
   - Increases target cross-section

3. **Main Pulse Laser**
   - CO₂ laser power: $P \approx 20-30 \, kW$
   - Creates plasma at temperature: $T \approx 500,000 \, K$
   - Plasma emits EUV at $\lambda = 13.5 \, nm$

4. **Conversion Efficiency**
   
   $$
   \eta_{CE} = \frac{P_{EUV}}{P_{laser}} \approx 5-6\%
   $$

**EUV Optical System**

Since EUV is absorbed by all materials, the system uses **reflective optics**:

- **Mirror Material:** Multi-layer Mo/Si (Molybdenum/Silicon)
- **Layer Thickness:** 
  $$
  d = \frac{\lambda}{2} \approx 6.75 \, nm
  $$
- **Number of Layer Pairs:** ~40-50
- **Peak Reflectivity:** $R \approx 67-70\%$
- **Total Optical Path Reflectivity:**
  $$
  R_{total} = R^n \approx (0.67)^{11} \approx 1.2\%
  $$

**EUV Mask Structure**

```
┌─────────────────────────────────────┐
│         Absorber (TaN/TaBN)         │  ← Pattern layer (~60-80 nm)
├─────────────────────────────────────┤
│         Capping Layer (Ru)          │  ← Protective layer (~2.5 nm)
├─────────────────────────────────────┤
│     Multi-Layer Mirror (Mo/Si)      │  ← 40-50 bilayer pairs
│     ~~~~~~~~~~~~~~~~~~~~~~~~        │
│     ~~~~~~~~~~~~~~~~~~~~~~~~        │
├─────────────────────────────────────┤
│      Low Thermal Expansion          │  ← Substrate
│      Material (LTEM)                │
└─────────────────────────────────────┘
```

---

**4. Scanner Systems**

**Scanner vs. Stepper**

| Parameter | Stepper | Scanner |
|-----------|---------|---------|
| Exposure Method | Full-field | Slit scanning |
| Field Size | Limited by lens | Larger effective field |
| Throughput | Lower | Higher |
| Overlay Control | Good | Excellent |

**Scanning Mechanism**

The wafer and reticle move in opposite directions during exposure:

$$
v_{wafer} = \frac{v_{reticle}}{M}
$$

Where:

- $v_{wafer}$ = wafer stage velocity
- $v_{reticle}$ = reticle stage velocity  
- $M$ = demagnification factor (typically 4×)

**Stage Positioning Accuracy**

- **Overlay Requirement:**
  $$
  \sigma_{overlay} < \frac{CD}{4} \approx 1-2 \, nm
  $$
  
- **Stage Position Accuracy:**
  $$
  \Delta x, \Delta y < 0.5 \, nm
  $$

- **Stage Velocity:**
  $$
  v_{stage} \approx 2 \, m/s
  $$

---

**5. Technical Specifications**

**ASML NXE:3600D (Current EUV)**

- **Numerical Aperture:** $NA = 0.33$
- **Wavelength:** $\lambda = 13.5 \, nm$
- **Resolution:** 
  $$
  R_{min} = k_1 \cdot \frac{13.5}{0.33} = k_1 \cdot 40.9 \, nm
  $$
  With $k_1 = 0.3$: $R_{min} \approx 13 \, nm$

- **Throughput:** $> 160$ wafers per hour (WPH)
- **Overlay:** $< 1.4 \, nm$ (machine-to-machine)
- **Source Power:** $> 250 \, W$ at intermediate focus
- **Cost:** ~€150-200 million

**ASML TWINSCAN EXE:5000 (High-NA EUV)**

- **Numerical Aperture:** $NA = 0.55$
- **Wavelength:** $\lambda = 13.5 \, nm$
- **Resolution:**
  $$
  R_{min} = k_1 \cdot \frac{13.5}{0.55} = k_1 \cdot 24.5 \, nm
  $$
  With $k_1 = 0.3$: $R_{min} \approx 8 \, nm$

- **Resolution Improvement:**
  $$
  \frac{R_{0.33}}{R_{0.55}} = \frac{0.55}{0.33} = 1.67\times
  $$

- **Anamorphic Optics:** 4× reduction in X, 8× reduction in Y
- **Cost:** ~€350+ million
- **Weight:** ~250 tons

**Throughput Calculation**

Wafers per hour (WPH) depends on:

$$
WPH = \frac{3600}{t_{expose} + t_{move} + t_{align} + t_{overhead}}
$$

Where typical values are:

- $t_{expose}$ = exposure time per die
- $t_{move}$ = stage movement time
- $t_{align}$ = alignment time
- $t_{overhead}$ = wafer load/unload time

---

**6. Geopolitical Context**

**Export Restrictions**

- **2019:** Netherlands blocks EUV exports to China
- **2023:** DUV restrictions expanded (NXT:2000i and newer)
- **2024:** Further tightening of servicing restrictions

**Technology Nodes by Company**

| Company | Node | EUV Layers |
|---------|------|------------|
| TSMC    | N3   | ~20-25     |
| TSMC    | N2   | ~25-30     |
| Samsung | 3GAE | ~20+       |
| Intel   | Intel 4 | ~5-10   |
| Intel   | Intel 18A | ~20+  |

**Economic Impact**

- **EUV System Cost:** $150-350M per tool
- **Annual Revenue (ASML 2023):** ~€27.6 billion
- **R&D Investment:** ~€4 billion annually
- **Backlog:** >€40 billion

---

**Mathematical Summary**

**Key Equations Reference**

| Equation | Formula | Application |
|----------|---------|-------------|
| Rayleigh Resolution | $R = k_1 \frac{\lambda}{NA}$ | Feature size limit |
| Depth of Focus | $DOF = k_2 \frac{\lambda}{NA^2}$ | Process window |
| Bragg Reflection | $2d\sin\theta = n\lambda$ | Mirror design |
| Conversion Efficiency | $\eta = \frac{P_{out}}{P_{in}}$ | Source efficiency |
| Throughput | $WPH = \frac{3600}{\sum t_i}$ | Productivity |

**Node Roadmap with Resolution Requirements**

| Node | Half-Pitch | EUV Layers | Year |
|------|------------|------------|------|
| 7nm  | ~36 nm     | 5-10       | 2018 |
| 5nm  | ~27 nm     | 10-15      | 2020 |
| 3nm  | ~21 nm     | 20-25      | 2022 |
| 2nm  | ~15 nm     | 25-30      | 2025 |
| A14  | ~10 nm     | High-NA    | 2027+|

---

**Appendix: Physical Constants**

| Constant | Symbol | Value |
|----------|--------|-------|
| EUV Wavelength | $\lambda_{EUV}$ | $13.5 \, nm$ |
| Speed of Light | $c$ | $3 \times 10^8 \, m/s$ |
| Planck's Constant | $h$ | $6.626 \times 10^{-34} \, J \cdot s$ |
| EUV Photon Energy | $E_{EUV}$ | $91.8 \, eV$ |

Photon energy calculation:

$$
E = \frac{hc}{\lambda} = \frac{(6.626 \times 10^{-34})(3 \times 10^8)}{13.5 \times 10^{-9}} = 1.47 \times 10^{-17} \, J = 91.8 \, eV
$$

---

**References**

1. ASML Annual Report 2023
2. SPIE Advanced Lithography Proceedings
3. Mack, C. "Fundamental Principles of Optical Lithography"
4. Bakshi, V. "EUV Lithography"

---

*Document generated: January 2026*
*Format: Markdown with KaTeX/LaTeX math notation*

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/asml) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
