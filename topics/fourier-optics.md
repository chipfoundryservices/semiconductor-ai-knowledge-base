# Computational Lithography Mathematics

**Keywords**: fourier optics, computational lithography, hopkins formulation, transmission cross coefficient, tcc, socs, zernike polynomials, partial coherence, opc, ilt

---

**Computational Lithography Mathematics**

  

Modern semiconductor manufacturing faces a fundamental physical challenge: creating nanoscale features using light with wavelengths much larger than the target dimensions.  Computational lithography  bridges this gap through sophisticated mathematical techniques.



   1. The Core Challenge

  1.1 Resolution Limits

The Rayleigh criterion defines the minimum resolvable feature size:

$$
R = k_1 \cdot \frac{\lambda}{NA}
$$

Where:

- $R$ = minimum resolution
- $k_1$ = process-dependent factor (theoretical limit: 0.25)
- $\lambda$ = wavelength of light (193 nm for ArF, 13.5 nm for EUV)
- $NA$ = numerical aperture of the lens system

  1.2 Depth of Focus

$$
DOF = k_2 \cdot \frac{\lambda}{NA^2}
$$



   2. Wave Optics Fundamentals

  2.1 Partially Coherent Imaging

The aerial image intensity on the wafer is described by Hopkins' equation:

$$
I(x, y) = \iint TCC(f_1, f_2) \cdot M(f_1) \cdot M^*(f_2) \, df_1 \, df_2
$$

Where:

- $I(x, y)$ = intensity at wafer position $(x, y)$
- $TCC(f_1, f_2)$ = Transmission Cross Coefficient
- $M(f)$ = Fourier transform of the mask pattern
- $M^*(f)$ = complex conjugate of $M(f)$

  2.2 Transmission Cross Coefficient

The TCC captures the optical system behavior:

$$
TCC(f_1, f_2) = \iint S(\xi, \eta) \cdot H(f_1 + \xi, \eta) \cdot H^*(f_2 + \xi, \eta) \, d\xi \, d\eta
$$

Where:

- $S(\xi, \eta)$ = source intensity distribution
- $H(f)$ = pupil function of the projection optics



   3. Optical Proximity Correction (OPC)

  3.1 The Inverse Problem

OPC solves the inverse imaging problem:

$$
\min_{M} \sum_{i} \left\| I(x_i, y_i; M) - I_{\text{target}}(x_i, y_i) \right\|^2 + \lambda R(M)
$$

Where:

- $M$ = mask pattern (optimization variable)
- $I_{\text{target}}$ = desired wafer pattern
- $R(M)$ = regularization term for manufacturability
- $\lambda$ = regularization weight

  3.2 Gradient-Based Optimization

The gradient with respect to mask pixels:

$$
\frac{\partial J}{\partial M_k} = \sum_{i} 2 \left( I_i - I_{\text{target},i} \right) \cdot \frac{\partial I_i}{\partial M_k}
$$

  3.3 Key Correction Features

-  Serifs : Corner additions/subtractions to correct corner rounding
-  Hammerheads : Line-end extensions to prevent line shortening
-  Assist features : Sub-resolution features that improve main feature fidelity
-  Scattering bars : Improve depth of focus for isolated features



   4. Inverse Lithography Technology (ILT)

  4.1 Full Pixel-Based Optimization

ILT treats each mask pixel as an independent variable:

$$
\min_{\mathbf{m}} \left\| \mathbf{I}(\mathbf{m}) - \mathbf{I}_{\text{target}} \right\|_2^2 + \alpha \|
abla \mathbf{m}\|_1 + \beta \text{TV}(\mathbf{m})
$$

Where:

- $\mathbf{m} \in [0, 1]^N$ = continuous mask pixel values
- $\text{TV}(\mathbf{m})$ = Total Variation regularization
- $\|
abla \mathbf{m}\|_1$ = sparsity-promoting term

  4.2 Level-Set Formulation

Mask boundaries represented implicitly:

$$
\frac{\partial \phi}{\partial t} = -V \cdot |
abla \phi|
$$

Where:

- $\phi(x, y)$ = level-set function
- Mask region: $\{(x,y) : \phi(x,y) > 0\}$
- $V$ = velocity field derived from optimization gradient



   5. Source Mask Optimization (SMO)

  5.1 Joint Optimization Problem

$$
\min_{S, M} \sum_{i} \left[ I(x_i, y_i; S, M) - I_{\text{target}}(x_i, y_i) \right]^2
$$

Subject to:

- Source constraints: $\int S(\xi, \eta) \, d\xi \, d\eta = 1$, $S \geq 0$
- Mask manufacturability constraints

  5.2 Alternating Optimization

1.  Fix source $S$, optimize mask $M$ 
2.  Fix mask $M$, optimize source $S$ 
3. Repeat until convergence



   6. Rigorous Electromagnetic Simulation

  6.1 Maxwell's Equations

For accurate 3D mask effects:

$$

abla \times \mathbf{E} = -\frac{\partial \mathbf{B}}{\partial t}
$$

$$

abla \times \mathbf{H} = \mathbf{J} + \frac{\partial \mathbf{D}}{\partial t}
$$

$$

abla \cdot \mathbf{D} = \rho
$$

$$

abla \cdot \mathbf{B} = 0
$$

  6.2 Numerical Methods

-  FDTD (Finite-Difference Time-Domain) :
  
  $$
  \frac{\partial E_x}{\partial t} = \frac{1}{\epsilon} \left( \frac{\partial H_z}{\partial y} - \frac{\partial H_y}{\partial z} \right)
  $$

-  RCWA (Rigorous Coupled-Wave Analysis) : Expansion in Fourier harmonics
  
  $$
  \mathbf{E}(x, y, z) = \sum_{m,n} \mathbf{E}_{mn}(z) \cdot e^{i(k_{xm}x + k_{yn}y)}
  $$



   7. Photoresist Modeling

  7.1 Dill Model for Absorption

$$
I(z) = I_0 \exp\left( -\int_0^z \alpha(z') \, dz' \right)
$$

Where absorption coefficient:

$$
\alpha = A \cdot M + B
$$

- $A$ = bleachable absorption
- $B$ = non-bleachable absorption
- $M$ = photoactive compound concentration

  7.2 Exposure Kinetics

$$
\frac{dM}{dt} = -C \cdot I \cdot M
$$

- $C$ = exposure rate constant

  7.3 Acid Diffusion (Post-Exposure Bake)

Reaction-diffusion equation:

$$
\frac{\partial [H^+]}{\partial t} = D 
abla^2 [H^+] - k_{\text{loss}} [H^+]
$$

Where:

- $D$ = diffusion coefficient (temperature-dependent)
- $k_{\text{loss}}$ = acid loss rate

  7.4 Development Rate

Mack model:

$$
r = r_{\max} \cdot \frac{(a+1)(1-m)^n}{a + (1-m)^n} + r_{\min}
$$

Where $m$ = normalized remaining PAC concentration.



   8. Stochastic Effects

  8.1 Photon Shot Noise

Photon count follows Poisson distribution:

$$
P(n) = \frac{\lambda^n e^{-\lambda}}{n!}
$$

Standard deviation:

$$
\sigma_n = \sqrt{\bar{n}}
$$

  8.2 Line Edge Roughness (LER)

Power spectral density:

$$
PSD(f) = \frac{A}{1 + (2\pi f \xi)^{2\alpha}}
$$

Where:

- $\xi$ = correlation length
- $\alpha$ = roughness exponent
- $A$ = amplitude

  8.3 Stochastic Defect Probability

For extreme ultraviolet (EUV):

$$
P_{\text{defect}} = 1 - \exp\left( -\frac{A_{\text{pixel}}}{N_{\text{photons}} \cdot \eta} \right)
$$



   9. Multi-Patterning Mathematics

  9.1 Graph Coloring Formulation

Given conflict graph $G = (V, E)$:

- $V$ = features
- $E$ = edges connecting features with spacing $< \text{min}_{\text{space}}$

Find $k$-coloring $c: V \rightarrow \{1, 2, \ldots, k\}$ such that:

$$
\forall (u, v) \in E: c(u) 
eq c(v)
$$

  9.2 Integer Linear Programming Formulation

$$
\min \sum_{(i,j) \in E} w_{ij} \cdot y_{ij}
$$

Subject to:

$$
\sum_{k=1}^{K} x_{ik} = 1 \quad \forall i \in V
$$

$$
x_{ik} + x_{jk} - y_{ij} \leq 1 \quad \forall (i,j) \in E, \forall k
$$

$$
x_{ik}, y_{ij} \in \{0, 1\}
$$



   10. EUV Lithography Specific Mathematics

  10.1 Multilayer Mirror Reflectivity

Bragg condition for Mo/Si multilayers:

$$
2d \sin\theta = n\lambda
$$

Reflectivity at each interface:

$$
r = \frac{n_1 - n_2}{n_1 + n_2}
$$

Total reflectivity (matrix method):

$$
\mathbf{M}_{\text{total}} = \prod_{j=1}^{N} \mathbf{M}_j
$$

  10.2 Mask 3D Effects

Shadow effect for off-axis illumination:

$$
\Delta x = h_{\text{absorber}} \cdot \tan(\theta_{\text{chief ray}})
$$



   11. Machine Learning in Computational Lithography

  11.1 Neural Network as Fast Surrogate Model

$$
I_{\text{predicted}} = f_{\theta}(M)
$$

Where $f_{\theta}$ is a trained CNN, training minimizes:

$$
\mathcal{L} = \sum_{i} \left\| f_{\theta}(M_i) - I_{\text{rigorous}}(M_i) \right\|^2
$$

  11.2 Physics-Informed Neural Networks

Loss function incorporating physics:

$$
\mathcal{L} = \mathcal{L}_{\text{data}} + \lambda_{\text{physics}} \mathcal{L}_{\text{physics}}
$$

Where:

$$
\mathcal{L}_{\text{physics}} = \left\| 
abla^2 E + k^2 \epsilon E \right\|^2
$$



   12. Key Mathematical Techniques Summary

| Technique | Application |
|-----------|-------------|
| Fourier Analysis | Optical imaging, frequency domain calculations |
| Inverse Problems | OPC, ILT, metrology |
| Non-convex Optimization | Mask optimization, SMO |
| Partial Differential Equations | EM simulation, resist diffusion |
| Graph Theory | Multi-patterning decomposition |
| Stochastic Processes | Shot noise, LER modeling |
| Linear Algebra | Large sparse system solutions |
| Machine Learning | Fast surrogate models, pattern recognition |



   13. Computational Complexity

  13.1 Full-Chip OPC Scale

-  Features : $\sim 10^{12}$ polygon edges
-  Variables : $\sim 10^8$ optimization parameters
-  Compute time : hours to days on $1000+$ CPU cores
-  Memory : terabytes of working data

  13.2 Complexity Classes

| Operation | Complexity |
|-----------|------------|
| FFT for imaging | $O(N \log N)$ |
| RCWA per wavelength | $O(M^3)$ where $M$ = harmonics |
| ILT optimization | $O(N \cdot k)$ where $k$ = iterations |
| Graph coloring | NP-complete (general case) |


  



   Notation:

| Symbol | Meaning |
|--------|---------|
| $\lambda$ | Wavelength |
| $NA$ | Numerical Aperture |
| $TCC$ | Transmission Cross Coefficient |
| $M(f)$ | Mask Fourier transform |
| $I(x,y)$ | Intensity at wafer |
| $\phi$ | Level-set function |
| $D$ | Diffusion coefficient |
| $\sigma$ | Standard deviation |
| $PSD$ | Power Spectral Density |

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/fourier-optics) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
