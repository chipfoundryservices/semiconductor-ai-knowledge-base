# Semiconductor Manufacturing Process Geometry and Computational Geometry Mathematical Modeling

**Keywords**: geometry, computational geometry, semiconductor geometry, polygon operations, level set, minkowski, opc geometry, design rule checking, drc, cmp modeling, resist modeling

---

**Semiconductor Manufacturing Process Geometry and Computational Geometry Mathematical Modeling**



**1. The Fundamental Geometric Challenge**

Modern semiconductor manufacturing operates at scales where the features being printed (3–7 nm effective dimensions) are far smaller than the wavelength of light used to pattern them (193 nm for DUV, 13.5 nm for EUV). This creates a regime where **diffraction physics dominates**, and the relationship between the designed geometry and the printed geometry becomes highly nonlinear.

**Resolution and Depth-of-Focus Equations**

The governing resolution relationship:

$$
R = k_1 \cdot \frac{\lambda}{NA}
$$

$$
DOF = k_2 \cdot \frac{\lambda}{NA^2}
$$

Where:

- $R$ — minimum resolvable feature size
- $DOF$ — depth of focus
- $\lambda$ — exposure wavelength
- $NA$ — numerical aperture of the projection lens
- $k_1, k_2$ — process-dependent factors (typically $k_1 \approx 0.25$ for advanced nodes)

The tension between resolution and depth-of-focus defines much of the geometric problem space.



**2. Computational Geometry in Layout and Verification**

**2.1 Polygon Representations**

Semiconductor layouts are fundamentally **rectilinear polygon problems** (Manhattan geometry). The core data structure represents billions of polygons across hierarchical cells.

**Key algorithms employed:**

| Problem | Algorithm | Complexity |
|---------|-----------|------------|
| Polygon Boolean operations | Vatti clipping, Greiner-Hormann | $O(n \log n)$ |
| Design rule checking | Sweep-line with interval trees | $O(n \log n)$ |
| Spatial queries | R-trees, quad-trees | $O(\log n)$ query |
| Nearest-neighbor | Voronoi diagrams | $O(n \log n)$ construction |
| Polygon sizing/offsetting | Minkowski sum/difference | $O(n^2)$ worst case |

**2.2 Design Rule Checking as Geometric Constraint Satisfaction**

Design rules translate to geometric predicates:

- **Minimum width**: polygon thinning check
  - Constraint: $w_{feature} \geq w_{min}$
- **Minimum spacing**: Minkowski sum expansion + intersection test
  - Constraint: $d(P_1, P_2) \geq s_{min}$
- **Enclosure**: polygon containment
  - Constraint: $P_{inner} \subseteq P_{outer} \ominus r$
- **Extension**: segment overlap calculations

The computational geometry challenge is performing these checks on $10^{9}$–$10^{11}$ edges efficiently, requiring sophisticated spatial indexing and hierarchical decomposition.

**2.3 Minkowski Operations**

For polygon $A$ and structuring element $B$:

**Dilation (Minkowski Sum):**

$$
A \oplus B = \{a + b \mid a \in A, b \in B\}
$$

**Erosion (Minkowski Difference):**

$$
A \ominus B = \{x \mid B_x \subseteq A\}
$$

These operations are fundamental to:

- Design rule checking (spacing verification)
- Optical proximity correction (edge biasing)
- Manufacturing constraint validation



**3. Optical Lithography Modeling**

**3.1 Hopkins Formulation for Partially Coherent Imaging**

The aerial image intensity at point $\mathbf{x}$:

$$
I(\mathbf{x}) = \iint TCC(\mathbf{f}, \mathbf{f'}) \cdot \tilde{M}(\mathbf{f}) \cdot \tilde{M}^*(\mathbf{f'}) \cdot e^{2\pi i (\mathbf{f} - \mathbf{f'}) \cdot \mathbf{x}} \, d\mathbf{f} \, d\mathbf{f'}
$$

Where:

- $TCC(\mathbf{f}, \mathbf{f'})$ — Transmission Cross-Coefficient (encodes source and pupil)
- $\tilde{M}(\mathbf{f})$ — Fourier transform of the mask transmission function
- $\tilde{M}^*(\mathbf{f'})$ — complex conjugate

**3.2 Eigendecomposition for Efficient Computation**

**Computational approach:** Eigendecomposition of TCC yields "kernels" for efficient simulation:

$$
I(\mathbf{x}) = \sum_{k=1}^{N} \lambda_k \left| \phi_k(\mathbf{x}) \otimes M(\mathbf{x}) \right|^2
$$

Where:

- $\lambda_k$ — eigenvalues (sorted by magnitude)
- $\phi_k(\mathbf{x})$ — eigenfunctions (SOCS kernels)
- $\otimes$ — convolution operator
- $N$ — number of kernels retained (typically 10–30)

This converts a 4D integral to a sum of 2D convolutions, enabling FFT-based computation with complexity $O(N \cdot n^2 \log n)$ for an $n \times n$ image.

**3.3 Coherence Factor and Illumination**

The partial coherence factor $\sigma$ relates to imaging:

$$
\sigma = \frac{NA_{condenser}}{NA_{objective}}
$$

- $\sigma = 0$: Fully coherent illumination
- $\sigma = 1$: Matched illumination
- $\sigma > 1$: Overfilled illumination

**3.4 Mask 3D Effects (EUV-Specific)**

At EUV wavelengths (13.5 nm), the mask is a 3D scattering structure. Rigorous electromagnetic modeling requires:

- **RCWA** (Rigorous Coupled-Wave Analysis)
  - Solves: $
abla \times \mathbf{E} = -\mu_0 \frac{\partial \mathbf{H}}{\partial t}$
- **FDTD** (Finite-Difference Time-Domain)
  - Discretization: $\frac{\partial E_x}{\partial t} = \frac{1}{\epsilon} \left( \frac{\partial H_z}{\partial y} - \frac{\partial H_y}{\partial z} \right)$
- **Waveguide methods**

The mask shadowing effect introduces asymmetry:

$$
\Delta x_{shadow} = d_{absorber} \cdot \tan(\theta_{chief ray})
$$



**4. Inverse Lithography and Computational Optimization**

**4.1 Optical Proximity Correction (OPC)**

**Forward problem:** Mask → Aerial Image → Printed Pattern

**Inverse problem:** Desired Pattern → Optimal Mask

**Mathematical formulation:**

$$
\min_M \sum_{i=1}^{N_{eval}} \left[ I(x_i, y_i; M) - I_{threshold} \right]^2 \cdot W_i
$$

Subject to mask manufacturing constraints:

- Minimum feature size: $w_{mask} \geq w_{min}^{mask}$
- Minimum spacing: $s_{mask} \geq s_{min}^{mask}$
- Corner rounding radius: $r_{corner} \geq r_{min}$

**4.2 Algorithmic Approaches**

**1. Gradient Descent:**

Compute sensitivity and iteratively adjust:

$$
\frac{\partial I}{\partial e_j} = \frac{\partial I}{\partial M} \cdot \frac{\partial M}{\partial e_j}
$$

$$
e_j^{(k+1)} = e_j^{(k)} - \alpha \cdot \frac{\partial \mathcal{L}}{\partial e_j}
$$

Where $e_j$ represents edge segment positions.

**2. Level-Set Methods:**

Represent mask as zero level set of $\phi(x,y)$, evolve via:

$$
\frac{\partial \phi}{\partial t} = -
abla_M \mathcal{L} \cdot |
abla \phi|
$$

The mask boundary is implicitly defined as:

$$
\Gamma = \{(x,y) : \phi(x,y) = 0\}
$$

**3. Inverse Lithography Technology (ILT):**

Pixel-based optimization treating each mask pixel as a continuous variable:

$$
\min_{\{m_{ij}\}} \mathcal{L}(I(\{m_{ij}\}), I_{target}) + \lambda \cdot R(\{m_{ij}\})
$$

Where $m_{ij} \in [0,1]$ and $R$ is a regularization term encouraging binary solutions.

**4.3 Source-Mask Optimization (SMO)**

Joint optimization of illumination source shape $S$ and mask pattern $M$:

$$
\min_{S, M} \mathcal{L}(I(S, M), I_{target}) + \alpha \cdot R_{mask}(M) + \beta \cdot R_{source}(S)
$$

This is a bilinear optimization problem, typically solved by alternating optimization:

1. Fix $S$, optimize $M$ (OPC subproblem)
2. Fix $M$, optimize $S$ (source optimization)
3. Repeat until convergence



**5. Process Simulation: Surface Evolution Mathematics**

**5.1 Level-Set Formulation for Etch/Deposition**

The evolution of a surface during etching or deposition is captured by:

$$
\frac{\partial \phi}{\partial t} + V(\mathbf{x}, t) \cdot |
abla \phi| = 0
$$

Where:

- $\phi(\mathbf{x}, t)$ — level-set function
- $\phi = 0$ — defines the surface implicitly
- $V(\mathbf{x}, t)$ — local velocity (etch rate or deposition rate)

**Advantages of level-set formulation:**

- Natural handling of topology changes (merging, splitting)
- Easy curvature computation:

$$
\kappa = 
abla \cdot \left( \frac{
abla \phi}{|
abla \phi|} \right) = \frac{\phi_{xx}\phi_y^2 - 2\phi_x\phi_y\phi_{xy} + \phi_{yy}\phi_x^2}{(\phi_x^2 + \phi_y^2)^{3/2}}
$$

- Extension to 3D straightforward

**5.2 Velocity Models**

**Isotropic etch:**

$$
V = V_0 = \text{constant}
$$

**Anisotropic (crystallographic) etch:**

$$
V = V(\theta, \phi)
$$

Where $\theta, \phi$ are angles defining crystal orientation relative to surface normal.

**Ion-enhanced reactive ion etch (RIE):**

$$
V = V_{ion} \cdot \Gamma_{ion}(\mathbf{x}) \cdot f(\theta) + V_{chem}
$$

Where:

- $\Gamma_{ion}(\mathbf{x})$ — ion flux at point $\mathbf{x}$
- $f(\theta)$ — angular dependence (typically $\cos^n \theta$)
- $V_{chem}$ — isotropic chemical component

**Deposition with angular distribution:**

$$
V(\theta) = V_0 \cdot \cos^n(\theta) \cdot \mathcal{V}(\mathbf{x})
$$

Where $\mathcal{V}(\mathbf{x}) \in [0,1]$ is the visibility factor.

**5.3 Visibility Calculations**

For physical vapor deposition or directional etch, computing visible solid angle:

$$
\mathcal{V}(\mathbf{x}) = \frac{1}{\pi} \int_{\Omega_{visible}} \cos\theta \, d\omega
$$

For a point source at position $\mathbf{r}_s$:

$$
\mathcal{V}(\mathbf{x}) = \begin{cases} 
\frac{(\mathbf{r}_s - \mathbf{x}) \cdot \mathbf{n}}{|\mathbf{r}_s - \mathbf{x}|^3} & \text{if line of sight clear} \\
0 & \text{otherwise}
\end{cases}
$$

This requires ray-tracing or hemispherical integration at each surface point.

**5.4 Hamilton-Jacobi Formulation**

The level-set equation can be written as a Hamilton-Jacobi equation:

$$
\phi_t + H(
abla \phi) = 0
$$

With Hamiltonian:

$$
H(\mathbf{p}) = V \cdot |\mathbf{p}|
$$

Numerical schemes include:

- Godunov's method
- ENO/WENO schemes for higher accuracy
- Fast marching for monotonic velocities


**6. Resist Modeling: Reaction-Diffusion Systems**

**6.1 Chemically Amplified Resist (CAR) Dynamics**

**Exposure — Generation of photoacid:**

$$
\frac{\partial [PAG]}{\partial t} = -C \cdot I(\mathbf{x}) \cdot [PAG]
$$

Integrated form:

$$
[H^+]_0 = [PAG]_0 \cdot \left(1 - e^{-C \cdot E(\mathbf{x})}\right)
$$

Where:

- $[PAG]$ — photo-acid generator concentration
- $C$ — Dill C parameter (sensitivity)
- $I(\mathbf{x})$ — local intensity
- $E(\mathbf{x})$ — total exposure dose

**Post-Exposure Bake (PEB) — Acid-catalyzed deprotection with diffusion:**

$$
\frac{\partial [H^+]}{\partial t} = D_H 
abla^2 [H^+] - k_q [H^+][Q] - k_{loss}[H^+]
$$

$$
\frac{\partial [Q]}{\partial t} = D_Q 
abla^2 [Q] - k_q [H^+][Q]
$$

$$
\frac{\partial [M]}{\partial t} = -k_{amp} [H^+] [M]
$$

Where:

- $[H^+]$ — acid concentration
- $[Q]$ — quencher concentration
- $[M]$ — protected (blocked) polymer concentration
- $D_H, D_Q$ — diffusion coefficients
- $k_q$ — quenching rate constant
- $k_{amp}$ — amplification rate constant

**6.2 Acid Diffusion Length**

Characteristic blur from diffusion:

$$
\sigma_{diff} = \sqrt{2 D_H t_{PEB}}
$$

This fundamentally limits resolution:

$$
LER \propto \sqrt{\frac{1}{D_0 \cdot \sigma_{diff}}}
$$

Where $D_0$ is photon dose.

**6.3 Development Rate Models**

**Mack Model (Enhanced Notch Model):**

$$
R_{dev}(m) = R_{max} \cdot \frac{(1-m)^n + R_{min}/R_{max}}{(1-m)^n + 1}
$$

Where:

- $R_{dev}$ — development rate
- $m$ — protected fraction (normalized)
- $R_{max}$ — maximum development rate (fully deprotected)
- $R_{min}$ — minimum development rate (fully protected)
- $n$ — dissolution selectivity parameter

**Critical ionization model:**

$$
R_{dev} = R_0 \cdot \left(\frac{[I^-]}{[I^-]_{crit}}\right)^n \cdot H\left([I^-] - [I^-]_{crit}\right)
$$

Where $H$ is the Heaviside function.

**6.4 Stochastic Effects at Small Scales**

At EUV (13.5 nm), photon shot noise becomes significant. The number of photons absorbed per pixel follows Poisson statistics:

$$
P(n; \bar{n}) = \frac{\bar{n}^n e^{-\bar{n}}}{n!}
$$

**Mean absorbed photons:**

$$
\bar{n} = \frac{E \cdot A \cdot \alpha}{h
u}
$$

Where:

- $E$ — dose (mJ/cm²)
- $A$ — pixel area
- $\alpha$ — absorption coefficient
- $h
u$ — photon energy (91.8 eV for EUV)

**Resulting Line Edge Roughness (LER):**

$$
\sigma_{LER}^2 \approx \frac{1}{\bar{n}} \cdot \left(\frac{\partial CD}{\partial E}\right)^2 \cdot \sigma_E^2
$$

Typical values: LER ≈ 1–2 nm (3σ)



**7. CMP (Chemical-Mechanical Planarization) Modeling**

**7.1 Preston Equation Foundation**

$$
\frac{dz}{dt} = K_p \cdot P \cdot V
$$

Where:

- $z$ — removed thickness
- $K_p$ — Preston coefficient (material-dependent)
- $P$ — applied pressure
- $V$ — relative velocity between wafer and pad

**7.2 Pattern-Density Dependent Models**

Real CMP depends on local pattern density. The effective pressure at a point depends on surrounding features.

**Effective pressure model:**

$$
P_{eff}(\mathbf{x}) = P_{nominal} \cdot \frac{1}{\rho(\mathbf{x})}
$$

Where $\rho$ is local pattern density, computed via convolution with a planarization kernel $K$:

$$
\rho(\mathbf{x}) = K(\mathbf{x}) \otimes D(\mathbf{x})
$$

**Kernel form (typically Gaussian or exponential):**

$$
K(r) = \frac{1}{2\pi L^2} e^{-r^2 / (2L^2)}
$$

Where $L$ is the planarization length (~3–10 mm).

**7.3 Multi-Step Evolution**

For oxide CMP over metal (e.g., copper damascene):

**Step 1 — Bulk removal:**

$$
\frac{dz_1}{dt} = K_{p,oxide} \cdot P_{eff}(\mathbf{x}) \cdot V
$$

**Step 2 — Dishing and erosion:**

$$
\text{Dishing} = K_p \cdot P \cdot V \cdot t_{over} \cdot f(w)
$$

$$
\text{Erosion} = K_p \cdot P \cdot V \cdot t_{over} \cdot g(\rho)
$$

Where $f(w)$ depends on line width and $g(\rho)$ depends on local density.



**8. Multi-Scale Modeling Framework**

**8.1 Scale Hierarchy**

| Scale | Domain | Size | Methods |
|-------|--------|------|---------|
| Atomistic | Ion implantation, surface reactions | Å–nm | MD, KMC, BCA |
| Feature | Etch, deposition, litho | nm–μm | Level-set, FEM, ray-tracing |
| Die | CMP, thermal, stress | mm | Continuum mechanics |
| Wafer | Uniformity, thermal | cm | FEM, statistical |

**8.2 Scale Bridging Techniques**

**Homogenization theory:**

$$
\langle \sigma_{ij} \rangle = C_{ijkl}^{eff} \langle \epsilon_{kl} \rangle
$$

**Representative Volume Element (RVE):**

$$
\langle f \rangle_{RVE} = \frac{1}{|V|} \int_V f(\mathbf{x}) \, dV
$$

**Surrogate models:**

$$
y = f_{surrogate}(\mathbf{x}; \theta) \approx f_{physics}(\mathbf{x})
$$

Where $\theta$ are parameters fitted from physics simulations.

**8.3 Ion Implantation: Binary Collision Approximation (BCA)**

Ion trajectory evolution:

$$
\frac{d\mathbf{r}}{dt} = \mathbf{v}
$$

$$
\frac{d\mathbf{v}}{dt} = -
abla U(\mathbf{r}) / m
$$

With screened Coulomb potential:

$$
U(r) = \frac{Z_1 Z_2 e^2}{r} \cdot \Phi\left(\frac{r}{a}\right)
$$

Where $\Phi$ is the screening function (e.g., ZBL universal).

**Resulting concentration profile:**

$$
C(x) = \frac{\Phi}{\sqrt{2\pi} \Delta R_p} \exp\left(-\frac{(x - R_p)^2}{2 \Delta R_p^2}\right)
$$

Where:

- $\Phi$ — dose (ions/cm²)
- $R_p$ — projected range
- $\Delta R_p$ — range straggle



**9. Machine Learning Integration**

**9.1 Forward Modeling Acceleration**

**Neural network surrogate:**

$$
I_{predicted}(\mathbf{x}) = \mathcal{N}_\theta(M, S, \text{process params})
$$

Where $\mathcal{N}_\theta$ is a trained neural network (often CNN).

**Training objective:**

$$
\min_\theta \sum_{i=1}^{N_{train}} \left\| \mathcal{N}_\theta(M_i) - I_{physics}(M_i) \right\|^2
$$

**9.2 Physics-Informed Neural Networks (PINNs)**

For solving PDEs (e.g., diffusion):

$$
\mathcal{L} = \mathcal{L}_{data} + \lambda \cdot \mathcal{L}_{physics}
$$

Where:

$$
\mathcal{L}_{physics} = \left\| \frac{\partial u}{\partial t} - D
abla^2 u \right\|^2
$$

**9.3 Hotspot Detection**

Pattern classification using CNNs:

$$
P(\text{hotspot} | \text{layout clip}) = \sigma(W \cdot \text{features} + b)
$$

Features extracted from:

- Local pattern density
- Edge interactions
- Spatial frequency content



**10. Emerging Geometric Challenges**

**10.1 3D Architectures**

**3D NAND:**

- 200+ vertically stacked layers
- High aspect ratio etching: $AR > 60:1$
- Geometric challenge: $\frac{depth}{width} = \frac{d}{w}$

**CFET (Complementary FET):**

- Stacked nFET over pFET
- 3D transistor geometry optimization

**Backside Power Delivery:**

- Through-silicon vias (TSVs)
- Via geometry: diameter, pitch, depth

**10.2 Curvilinear Masks**

ILT produces non-Manhattan mask shapes:

**Spline representation:**

$$
\mathbf{r}(t) = \sum_{i=0}^{n} P_i \cdot B_{i,k}(t)
$$

Where $B_{i,k}(t)$ are B-spline basis functions.

**Challenges:**

- Fracturing for e-beam mask writing
- DRC for curved features
- Data volume increase

**10.3 Design-Technology Co-Optimization (DTCO)**

**Unified optimization:**

$$
\min_{\text{design}, \text{process}} \mathcal{L}_{performance} + \alpha \cdot \mathcal{L}_{yield} + \beta \cdot \mathcal{L}_{cost}
$$

Subject to:

- Design rules: $\mathcal{G}_{DRC}(\text{layout}) \leq 0$
- Process window: $PW(\text{process}) \geq PW_{min}$
- Electrical constraints: $\mathcal{C}_{elec}(\text{design}) \leq 0$



**11. Mathematical Framework Overview**

The intersection of semiconductor manufacturing and computational geometry involves:

1. **Classical computational geometry**
   - Polygon operations at massive scale ($10^{9}$–$10^{11}$ edges)
   - Spatial queries and indexing
   - Visibility computations

2. **Fourier optics and inverse problems**
   - Aerial image: $I(\mathbf{x}) = \sum_k \lambda_k |\phi_k \otimes M|^2$
   - OPC/ILT: $\min_M \|I(M) - I_{target}\|^2$

3. **Surface evolution PDEs**
   - Level-set: $\phi_t + V|
abla\phi| = 0$
   - Curvature-dependent flow

4. **Reaction-diffusion systems**
   - Resist: $\frac{\partial [H^+]}{\partial t} = D
abla^2[H^+] - k[H^+][Q]$
   - Acid diffusion blur

5. **Stochastic modeling**
   - Photon statistics: $P(n) = \frac{\bar{n}^n e^{-\bar{n}}}{n!}$
   - LER, LCDU, yield

6. **Multi-physics coupling**
   - Thermal-mechanical-electrical-chemical
   - Multi-scale bridging

7. **Optimization theory**
   - Large-scale constrained optimization
   - Bilinear problems (SMO)
   - Regularization and constraints



**Key Notation Reference**

| Symbol | Meaning |
|--------|---------|
| $\lambda$ | Exposure wavelength |
| $NA$ | Numerical aperture |
| $CD$ | Critical dimension |
| $DOF$ | Depth of focus |
| $\phi$ | Level-set function |
| $TCC$ | Transmission cross-coefficient |
| $\sigma$ | Partial coherence factor |
| $R_p$ | Projected range (implant) |
| $K_p$ | Preston coefficient (CMP) |
| $D_H$ | Acid diffusion coefficient |
| $\Gamma$ | Surface boundary |
| $\kappa$ | Surface curvature |

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/geometry) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
