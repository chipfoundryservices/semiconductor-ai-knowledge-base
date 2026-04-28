# Semiconductor Manufacturing Process: Lithography Mathematical Modeling

**Keywords**: photolithography, what is photolithography, lithography process, semiconductor lithography, photoresist, euv lithography, duv lithography, stepper, scanner, patterning

---

**Semiconductor Manufacturing Process: Lithography Mathematical Modeling**


**1. Introduction**

Lithography is the critical patterning step in semiconductor manufacturing that transfers circuit designs onto silicon wafers. It is essentially the "printing press" of chip making and determines the minimum feature sizes achievable.

**1.1 Basic Process Flow**

1. Coat wafer with photoresist
2. Expose photoresist to light through a mask/reticle
3. Develop the photoresist (remove exposed or unexposed regions)
4. Etch or deposit through the patterned resist
5. Strip the remaining resist

**1.2 Types of Lithography**

- **Optical lithography:** DUV at 193nm, EUV at 13.5nm
- **Electron beam lithography:** Direct-write, maskless
- **Nanoimprint lithography:** Mechanical pattern transfer
- **X-ray lithography:** Short wavelength exposure



**2. Optical Image Formation**

The foundation of lithography modeling is **partially coherent imaging theory**, formalized through the Hopkins integral.

**2.1 Hopkins Integral**

The intensity distribution at the image plane is given by:

$$
I(x,y) = \iiint\!\!\!\int TCC(f_1,g_1;f_2,g_2) \cdot \tilde{M}(f_1,g_1) \cdot \tilde{M}^*(f_2,g_2) \cdot e^{2\pi i[(f_1-f_2)x + (g_1-g_2)y]} \, df_1\,dg_1\,df_2\,dg_2
$$

Where:

- $I(x,y)$ — Intensity at image plane coordinates $(x,y)$
- $\tilde{M}(f,g)$ — Fourier transform of the mask transmission function
- $TCC$ — Transmission Cross Coefficient

**2.2 Transmission Cross Coefficient (TCC)**

The TCC encodes both the illumination source and lens pupil:

$$
TCC(f_1,g_1;f_2,g_2) = \iint S(f,g) \cdot P(f+f_1,g+g_1) \cdot P^*(f+f_2,g+g_2) \, df\,dg
$$

Where:

- $S(f,g)$ — Source intensity distribution
- $P(f,g)$ — Pupil function (encodes aberrations, NA cutoff)
- $P^*$ — Complex conjugate of the pupil function

**2.3 Sum of Coherent Systems (SOCS)**

To accelerate computation, the TCC is decomposed using eigendecomposition:

$$
TCC(f_1,g_1;f_2,g_2) = \sum_{k=1}^{N} \lambda_k \cdot \phi_k(f_1,g_1) \cdot \phi_k^*(f_2,g_2)
$$

The image becomes a weighted sum of coherent images:

$$
I(x,y) = \sum_{k=1}^{N} \lambda_k \left| \mathcal{F}^{-1}\{\phi_k \cdot \tilde{M}\} \right|^2
$$

**2.4 Coherence Factor**

The partial coherence factor $\sigma$ is defined as:

$$
\sigma = \frac{NA_{source}}{NA_{lens}}
$$

- $\sigma = 0$ — Fully coherent illumination
- $\sigma = 1$ — Matched illumination
- $\sigma > 1$ — Overfilled illumination



**3. Resolution Limits and Scaling Laws**

**3.1 Rayleigh Criterion**

The minimum resolvable feature size:

$$
R = k_1 \frac{\lambda}{NA}
$$

Where:

- $R$ — Minimum resolvable feature
- $k_1$ — Process factor (theoretical limit $\approx 0.25$, practical $\approx 0.3\text{--}0.4$)
- $\lambda$ — Wavelength of light
- $NA$ — Numerical aperture $= n \sin\theta$

**3.2 Depth of Focus**

$$
DOF = k_2 \frac{\lambda}{NA^2}
$$

Where:

- $DOF$ — Depth of focus
- $k_2$ — Process-dependent constant

**3.3 Technology Comparison**

| Technology | $\lambda$ (nm) | NA | Min. Feature | DOF |
|:-----------|:---------------|:-----|:-------------|:----|
| DUV ArF | 193 | 1.35 | ~38 nm | ~100 nm |
| EUV | 13.5 | 0.33 | ~13 nm | ~120 nm |
| High-NA EUV | 13.5 | 0.55 | ~8 nm | ~45 nm |

**3.4 Resolution Enhancement Techniques (RETs)**

Key techniques to reduce effective $k_1$:

- **Off-Axis Illumination (OAI):** Dipole, quadrupole, annular
- **Phase-Shift Masks (PSM):** Alternating, attenuated
- **Optical Proximity Correction (OPC):** Bias, serifs, sub-resolution assist features (SRAFs)
- **Multiple Patterning:** LELE, SADP, SAQP



**4. Rigorous Electromagnetic Mask Modeling**

**4.1 Thin Mask Approximation (Kirchhoff)**

For features much larger than wavelength:

$$
E_{mask}(x,y) = t(x,y) \cdot E_{incident}
$$

Where $t(x,y)$ is the complex transmission function.

**4.2 Maxwell's Equations**

For sub-wavelength features, we must solve Maxwell's equations rigorously:

$$

abla \times \mathbf{E} = -\frac{\partial \mathbf{B}}{\partial t}
$$

$$

abla \times \mathbf{H} = \mathbf{J} + \frac{\partial \mathbf{D}}{\partial t}
$$

**4.3 RCWA (Rigorous Coupled-Wave Analysis)**

For periodic structures with grating period $d$, fields are expanded in Floquet modes:

$$
E(x,z) = \sum_{n=-N}^{N} A_n(z) \cdot e^{i k_{xn} x}
$$

Where the wavevector components are:

$$
k_{xn} = k_0 \sin\theta_0 + \frac{2\pi n}{d}
$$

This yields a matrix eigenvalue problem:

$$
\frac{d^2}{dz^2}\mathbf{A} = \mathbf{K}^2 \mathbf{A}
$$

Where $\mathbf{K}$ couples different diffraction orders through the dielectric tensor.

**4.4 FDTD (Finite-Difference Time-Domain)**

Discretizing Maxwell's equations on a Yee grid:

$$
\frac{\partial H_y}{\partial t} = \frac{1}{\mu}\left(\frac{\partial E_x}{\partial z} - \frac{\partial E_z}{\partial x}\right)
$$

$$
\frac{\partial E_x}{\partial t} = \frac{1}{\epsilon}\left(\frac{\partial H_y}{\partial z} - J_x\right)
$$

**4.5 EUV Mask 3D Effects**

Shadowing from absorber thickness $h$ at angle $\theta$:

$$
\Delta x = h \tan\theta
$$

For EUV at 6° chief ray angle:

$$
\Delta x \approx 0.105 \cdot h
$$



**5. Photoresist Modeling**

**5.1 Dill ABC Model (Exposure)**

The photoactive compound (PAC) concentration evolves as:

$$
\frac{\partial M(z,t)}{\partial t} = -I(z,t) \cdot M(z,t) \cdot C
$$

Light absorption follows Beer-Lambert law:

$$
\frac{dI}{dz} = -\alpha(M) \cdot I
$$

$$
\alpha(M) = A \cdot M + B
$$

Where:

- $A$ — Bleachable absorption coefficient
- $B$ — Non-bleachable absorption coefficient
- $C$ — Exposure rate constant (quantum efficiency)
- $M$ — Normalized PAC concentration

**5.2 Post-Exposure Bake (PEB) — Reaction-Diffusion**

For chemically amplified resists (CARs):

$$
\frac{\partial h}{\partial t} = D 
abla^2 h + k \cdot h \cdot M_{blocking}
$$

Where:

- $h$ — Acid concentration
- $D$ — Diffusion coefficient
- $k$ — Reaction rate constant
- $M_{blocking}$ — Blocking group concentration

The blocking group deprotection:

$$
\frac{\partial M_{blocking}}{\partial t} = -k_{amp} \cdot h \cdot M_{blocking}
$$

**5.3 Mack Development Rate Model**

$$
r(m) = r_{max} \cdot \frac{(a+1)(1-m)^n}{a + (1-m)^n} + r_{min}
$$

Where:

- $r$ — Development rate
- $m$ — Normalized PAC concentration remaining
- $n$ — Contrast (dissolution selectivity)
- $a$ — Inhibition depth
- $r_{max}$ — Maximum development rate (fully exposed)
- $r_{min}$ — Minimum development rate (unexposed)

**5.4 Enhanced Mack Model**

Including surface inhibition:

$$
r(m,z) = r_{max} \cdot \frac{(a+1)(1-m)^n}{a + (1-m)^n} \cdot \left(1 - e^{-z/l}\right) + r_{min}
$$

Where $l$ is the surface inhibition depth.



**6. Optical Proximity Correction (OPC)**

**6.1 Forward Problem**

Given mask $M$, compute the printed wafer image:

$$
I = F(M)
$$

Where $F$ represents the complete optical and resist model.

**6.2 Inverse Problem**

Given target pattern $T$, find mask $M$ such that:

$$
F(M) \approx T
$$

**6.3 Edge Placement Error (EPE)**

$$
EPE_i = x_{printed,i} - x_{target,i}
$$

**6.4 OPC Optimization Formulation**

Minimize the cost function:

$$
\mathcal{L}(M) = \sum_{i=1}^{N} w_i \cdot EPE_i^2 + \lambda \cdot R(M)
$$

Where:

- $w_i$ — Weight for evaluation point $i$
- $R(M)$ — Regularization term for mask manufacturability
- $\lambda$ — Regularization strength

**6.5 Gradient-Based OPC**

Using gradient descent:

$$
M_{n+1} = M_n - \eta \frac{\partial \mathcal{L}}{\partial M}
$$

The gradient requires computing:

$$
\frac{\partial \mathcal{L}}{\partial M} = \sum_i 2 w_i \cdot EPE_i \cdot \frac{\partial EPE_i}{\partial M} + \lambda \frac{\partial R}{\partial M}
$$

**6.6 Adjoint Method for Gradient Computation**

The sensitivity $\frac{\partial I}{\partial M}$ is computed efficiently using the adjoint formulation:

$$
\frac{\partial \mathcal{L}}{\partial M} = \text{Re}\left\{ \tilde{M}^* \cdot \mathcal{F}\left\{ \sum_k \lambda_k \phi_k^* \cdot \mathcal{F}^{-1}\left\{ \phi_k \cdot \frac{\partial \mathcal{L}}{\partial I} \right\} \right\} \right\}
$$

This avoids computing individual sensitivities for each mask pixel.

**6.7 Mask Manufacturability Constraints**

Common regularization terms:

- **Minimum feature size:** $R_1(M) = \sum \max(0, w_{min} - w_i)^2$
- **Minimum space:** $R_2(M) = \sum \max(0, s_{min} - s_i)^2$
- **Edge curvature:** $R_3(M) = \int |\kappa(s)|^2 ds$
- **Shot count:** $R_4(M) = N_{vertices}$



**7. Source-Mask Optimization (SMO)**

**7.1 Joint Optimization Formulation**

$$
\min_{S,M} \sum_{\text{patterns}} \|I(S,M) - T\|^2 + \lambda_S R_S(S) + \lambda_M R_M(M)
$$

Where:

- $S$ — Source intensity distribution
- $M$ — Mask transmission function
- $T$ — Target pattern
- $R_S(S)$ — Source manufacturability regularization
- $R_M(M)$ — Mask manufacturability regularization

**7.2 Source Parameterization**

Pixelated source with constraints:

$$
S(f,g) = \sum_{i,j} s_{ij} \cdot \text{rect}\left(\frac{f - f_i}{\Delta f}\right) \cdot \text{rect}\left(\frac{g - g_j}{\Delta g}\right)
$$

Subject to:

$$
0 \leq s_{ij} \leq 1 \quad \forall i,j
$$

$$
\sum_{i,j} s_{ij} = S_{total}
$$

**7.3 Alternating Optimization**

**Algorithm:**

1. Initialize $S_0$, $M_0$
2. For iteration $n = 1, 2, \ldots$:
   - Fix $S_n$, optimize $M_{n+1} = \arg\min_M \mathcal{L}(S_n, M)$
   - Fix $M_{n+1}$, optimize $S_{n+1} = \arg\min_S \mathcal{L}(S, M_{n+1})$
3. Repeat until convergence

**7.4 Gradient Computation for SMO**

Source gradient:

$$
\frac{\partial I}{\partial S}(x,y) = \left| \mathcal{F}^{-1}\{P \cdot \tilde{M}\}(x,y) \right|^2
$$

Mask gradient uses the adjoint method as in OPC.



**8. Stochastic Effects and EUV**

**8.1 Photon Shot Noise**

Photon counts follow a Poisson distribution:

$$
P(n) = \frac{\bar{n}^n e^{-\bar{n}}}{n!}
$$

For EUV at 13.5 nm, photon energy is:

$$
E_{photon} = \frac{hc}{\lambda} = \frac{1240 \text{ eV} \cdot \text{nm}}{13.5 \text{ nm}} \approx 92 \text{ eV}
$$

Mean photons per pixel:

$$
\bar{n} = \frac{\text{Dose} \cdot A_{pixel}}{E_{photon}}
$$

**8.2 Relative Shot Noise**

$$
\frac{\sigma_n}{\bar{n}} = \frac{1}{\sqrt{\bar{n}}}
$$

For 30 mJ/cm² dose and 10 nm pixel:

$$
\bar{n} \approx 200 \text{ photons} \implies \sigma/\bar{n} \approx 7\%
$$

**8.3 Line Edge Roughness (LER)**

Characterized by power spectral density:

$$
PSD(f) = \frac{LER^2 \cdot \xi}{1 + (2\pi f \xi)^{2(1+H)}}
$$

Where:

- $LER$ — RMS line edge roughness (3σ value)
- $\xi$ — Correlation length
- $H$ — Hurst exponent (0 < H < 1)
- $f$ — Spatial frequency

**8.4 LER Decomposition**

$$
LER^2 = LWR^2/2 + \sigma_{placement}^2
$$

Where:

- $LWR$ — Line width roughness
- $\sigma_{placement}$ — Line placement error

**8.5 Stochastic Defectivity**

Probability of printing failure (e.g., missing contact):

$$
P_{fail} = 1 - \prod_{i} \left(1 - P_{fail,i}\right)
$$

For a chip with $10^{10}$ contacts at 99.9999999% yield per contact:

$$
P_{chip,fail} \approx 1\%
$$

**8.6 Monte Carlo Simulation Steps**

1. **Photon absorption:** Generate random events $\sim \text{Poisson}(\bar{n})$
2. **Acid generation:** Each photon generates acid at random location
3. **Diffusion:** Brownian motion during PEB: $\langle r^2 \rangle = 6Dt$
4. **Deprotection:** Local reaction based on acid concentration
5. **Development:** Cellular automata or level-set method



**9. Multiple Patterning Mathematics**

**9.1 Graph Coloring Formulation**

When pitch $< \lambda/(2NA)$, single-exposure patterning fails.

**Graph construction:**

- Nodes $V$ = features (polygons)
- Edges $E$ = spacing conflicts (features too close for one mask)
- Colors $C$ = different masks

**9.2 k-Colorability Problem**

Find assignment $c: V \rightarrow \{1, 2, \ldots, k\}$ such that:

$$
c(u) 
eq c(v) \quad \forall (u,v) \in E
$$

This is **NP-complete** for $k \geq 3$.

**9.3 Integer Linear Programming (ILP) Formulation**

Binary variables: $x_{v,c} \in \{0,1\}$ (node $v$ assigned color $c$)

**Objective:**

$$
\min \sum_{(u,v) \in E} \sum_c x_{u,c} \cdot x_{v,c} \cdot w_{uv}
$$

**Constraints:**

$$
\sum_{c=1}^{k} x_{v,c} = 1 \quad \forall v \in V
$$

$$
x_{u,c} + x_{v,c} \leq 1 \quad \forall (u,v) \in E, \forall c
$$

**9.4 Self-Aligned Multiple Patterning (SADP)**

Spacer pitch after $n$ iterations:

$$
p_n = \frac{p_0}{2^n}
$$

Where $p_0$ is the initial (lithographic) pitch.



**10. Process Control Mathematics**

**10.1 Overlay Control**

Polynomial model across the wafer:

$$
OVL_x(x,y) = a_0 + a_1 x + a_2 y + a_3 xy + a_4 x^2 + a_5 y^2 + \ldots
$$

**Physical interpretation:**

| Coefficient | Physical Effect |
|:------------|:----------------|
| $a_0$ | Translation |
| $a_1$, $a_2$ | Scale (magnification) |
| $a_3$ | Rotation |
| $a_4$, $a_5$ | Non-orthogonality |

**10.2 Overlay Correction**

Least squares fitting:

$$
\mathbf{a} = (\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T \mathbf{y}
$$

Where $\mathbf{X}$ is the design matrix and $\mathbf{y}$ is measured overlay.

**10.3 Run-to-Run Control — EWMA**

Exponentially Weighted Moving Average:

$$
\hat{y}_{n+1} = \lambda y_n + (1-\lambda)\hat{y}_n
$$

Where:

- $\hat{y}_{n+1}$ — Predicted output
- $y_n$ — Measured output at step $n$
- $\lambda$ — Smoothing factor $(0 < \lambda < 1)$

**10.4 CDU Variance Decomposition**

$$
\sigma^2_{total} = \sigma^2_{local} + \sigma^2_{field} + \sigma^2_{wafer} + \sigma^2_{lot}
$$

**Sources:**

- **Local:** Shot noise, LER, resist
- **Field:** Lens aberrations, mask
- **Wafer:** Focus/dose uniformity
- **Lot:** Tool-to-tool variation

**10.5 Process Capability Index**

$$
C_{pk} = \min\left(\frac{USL - \mu}{3\sigma}, \frac{\mu - LSL}{3\sigma}\right)
$$

Where:

- $USL$, $LSL$ — Upper/lower specification limits
- $\mu$ — Process mean
- $\sigma$ — Process standard deviation



**11. Machine Learning Integration**

**11.1 Applications Overview**

| Application | Method | Purpose |
|:------------|:-------|:--------|
| Hotspot detection | CNNs | Predict yield-limiting patterns |
| OPC acceleration | Neural surrogates | Replace expensive physics sims |
| Metrology | Regression models | Virtual measurements |
| Defect classification | Image classifiers | Automated inspection |
| Etch prediction | Physics-informed NN | Predict etch profiles |

**11.2 Neural Network Surrogate Model**

A neural network approximates the forward model:

$$
\hat{I}(x,y) = f_{NN}(\text{mask}, \text{source}, \text{focus}, \text{dose}; \theta)
$$

Training objective:

$$
\theta^* = \arg\min_\theta \sum_{i=1}^{N} \|f_{NN}(M_i; \theta) - I_i^{rigorous}\|^2
$$

**11.3 Hotspot Detection with CNNs**

Binary classification:

$$
P(\text{hotspot} | \text{pattern}) = \sigma(\mathbf{W} \cdot \mathbf{features} + b)
$$

Where $\sigma$ is the sigmoid function and features are extracted by convolutional layers.

**11.4 Inverse Lithography with Deep Learning**

Generator network $G$ maps target to mask:

$$
\hat{M} = G(T; \theta_G)
$$

Training with physics-based loss:

$$
\mathcal{L} = \|F(G(T)) - T\|^2 + \lambda \cdot R(G(T))
$$



**12. Mathematical Disciplines**

| Mathematical Domain | Application in Lithography |
|:--------------------|:---------------------------|
| **Fourier Optics** | Image formation, aberrations, frequency analysis |
| **Electromagnetic Theory** | RCWA, FDTD, rigorous mask simulation |
| **Partial Differential Equations** | Resist diffusion, development, reaction kinetics |
| **Optimization Theory** | OPC, SMO, inverse problems, gradient descent |
| **Probability & Statistics** | Shot noise, LER, SPC, process control |
| **Linear Algebra** | Matrix methods, eigendecomposition, least squares |
| **Graph Theory** | Multiple patterning decomposition, routing |
| **Numerical Methods** | FEM, finite differences, Monte Carlo |
| **Machine Learning** | Surrogate models, pattern recognition, CNNs |
| **Signal Processing** | Image analysis, metrology, filtering |




**Key Equations Quick Reference**

**Imaging**

$$
I(x,y) = \sum_{k} \lambda_k \left| \mathcal{F}^{-1}\{\phi_k \cdot \tilde{M}\} \right|^2
$$

**Resolution**

$$
R = k_1 \frac{\lambda}{NA}
$$

**Depth of Focus**

$$
DOF = k_2 \frac{\lambda}{NA^2}
$$

**Development Rate**

$$
r(m) = r_{max} \cdot \frac{(a+1)(1-m)^n}{a + (1-m)^n} + r_{min}
$$

**LER Power Spectrum**

$$
PSD(f) = \frac{LER^2 \cdot \xi}{1 + (2\pi f \xi)^{2(1+H)}}
$$

**OPC Cost Function**

$$
\mathcal{L}(M) = \sum_{i} w_i \cdot EPE_i^2 + \lambda \cdot R(M)
$$

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/photolithography) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
