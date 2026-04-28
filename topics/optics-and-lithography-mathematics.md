# Optics and Lithography Mathematical Modeling

**Keywords**: optics and lithography mathematics,lithography mathematics,optical lithography math,lithography equations,rayleigh equation,fourier optics,hopkins formulation,tcc,zernike polynomials,opc mathematics,ilt mathematics,smo optimization

---

**Optics and Lithography Mathematical Modeling**

A comprehensive guide to the mathematical foundations of semiconductor lithography, covering electromagnetic theory, Fourier optics, optimization mathematics, and stochastic processes.

  1. Fundamental Imaging Theory

   1.1 The Resolution Limits

The Rayleigh equations define the physical limits of optical lithography:

 Resolution: 

$$
R = k_1 \cdot \frac{\lambda}{NA}
$$

 Depth of Focus: 

$$
DOF = k_2 \cdot \frac{\lambda}{NA^2}
$$

 Parameter Definitions: 

- $\lambda$ — Wavelength of light (193nm for ArF immersion, 13.5nm for EUV)
- $NA = n \cdot \sin(\theta)$ — Numerical aperture
- $n$ — Refractive index of immersion medium
- $\theta$ — Half-angle of the lens collection cone
- $k_1, k_2$ — Process-dependent factors (typically $k_1 \geq 0.25$ from Rayleigh criterion; modern processes achieve $k_1 \sim 0.3–0.4$)

 Fundamental Tension: 

- Improving resolution requires:
  - Increasing $NA$, OR
  - Decreasing $\lambda$
- Both degrade depth of focus  quadratically  ($\propto NA^{-2}$)



  2. Fourier Optics Framework

The projection lithography system is modeled as a  linear shift-invariant system  in the Fourier domain.

   2.1 Coherent Imaging

For a perfectly coherent source, the image field is given by convolution:

$$
E_{image}(x,y) = E_{object}(x,y) \otimes h(x,y)
$$

In frequency space (via Fourier transform):

$$
\tilde{E}_{image}(f_x, f_y) = \tilde{E}_{object}(f_x, f_y) \cdot H(f_x, f_y)
$$

 Key Components: 

- $h(x,y)$ — Amplitude Point Spread Function (PSF)
- $H(f_x, f_y)$ — Coherent Transfer Function (pupil function)
- Typically a `circ` function for circular aperture
- Cuts off spatial frequencies beyond $\frac{NA}{\lambda}$

   2.2 Partially Coherent Imaging — The Hopkins Formulation

Real lithography systems operate in the  partially coherent regime :

$$
\sigma = 0.3 - 0.9
$$

where $\sigma$ is the ratio of condenser NA to objective NA.

   Transmission Cross Coefficient (TCC) Integral

The aerial image intensity is:

$$
I(x,y) = \int\!\!\!\int\!\!\!\int\!\!\!\int TCC(f_1,g_1,f_2,g_2) \cdot M(f_1,g_1) \cdot M^*(f_2,g_2) \cdot e^{2\pi i[(f_1-f_2)x + (g_1-g_2)y]} \, df_1 \, dg_1 \, df_2 \, dg_2
$$

The TCC itself is defined as:

$$
TCC(f_1,g_1,f_2,g_2) = \int\!\!\!\int J(f,g) \cdot P(f+f_1, g+g_1) \cdot P^*(f+f_2, g+g_2) \, df \, dg
$$

 Parameter Definitions: 

- $J(f,g)$ — Source intensity distribution (conventional, annular, dipole, quadrupole, or freeform)
- $P$ — Pupil function (including aberrations)
- $M$ — Mask transmission/diffraction spectrum
- $M^*$ — Complex conjugate of mask spectrum

 Computational Note:  This is a 4D integral over frequency space for every image point — computationally expensive but essential for accuracy.



  3. Computational Acceleration: SOCS Decomposition

Direct TCC computation is prohibitive. The  Sum of Coherent Systems (SOCS)  method uses eigendecomposition:

$$
TCC(f_1,g_1,f_2,g_2) \approx \sum_{i=1}^{N} \lambda_i \cdot \phi_i(f_1,g_1) \cdot \phi_i^*(f_2,g_2)
$$

 Decomposition Components: 

- $\lambda_i$ — Eigenvalues (sorted by magnitude)
- $\phi_i$ — Eigenfunctions (kernels)

The image becomes a sum of coherent images:

$$
I(x,y) \approx \sum_{i=1}^{N} \lambda_i \cdot \left| m(x,y) \otimes \phi_i(x,y) \right|^2
$$

 Computational Properties: 

- Typically $N = 10–50$ kernels capture $>99\%$ of imaging behavior
- Each convolution computed via FFT
- Complexity: $O(N \log N)$ per kernel



  4. Vector Electromagnetic Effects at High NA

When $NA > 0.7$ (immersion lithography reaches $NA \sim 1.35$), scalar diffraction theory fails. The  vector nature of light  must be modeled.

   4.1 Richards-Wolf Vector Diffraction

The electric field near focus:

$$
\mathbf{E}(r,\psi,z) = -\frac{ikf}{2\pi} \int_0^{\theta_{max}} \int_0^{2\pi} \mathbf{A}(\theta,\phi) \cdot P(\theta,\phi) \cdot e^{ik[z\cos\theta + r\sin\theta\cos(\phi-\psi)]} \sin\theta \, d\theta \, d\phi
$$

 Variables: 

- $\mathbf{A}(\theta,\phi)$ — Polarization-dependent amplitude vector
- $P(\theta,\phi)$ — Pupil function
- $k = \frac{2\pi}{\lambda}$ — Wave number
- $(r, \psi, z)$ — Cylindrical coordinates at image plane

   4.2 Polarization Effects

For high-NA imaging, polarization significantly affects image contrast:

| Polarization | Description | Behavior |
|:-------------|:------------|:---------|
|  TE (s-polarization)  | Electric field ⊥ to plane of incidence | Interferes constructively |
|  TM (p-polarization)  | Electric field ∥ to plane of incidence | Suffers contrast loss at high angles |

 Consequences: 

- Horizontal vs. vertical features print differently
- Requires illumination polarization control:
  - Tangential polarization
  - Radial polarization
  - Optimized/freeform polarization



  5. Aberration Modeling: Zernike Polynomials

Wavefront aberrations are expanded in  Zernike polynomials  over the unit pupil:

$$
W(\rho,\theta) = \sum_{n,m} Z_n^m \cdot R_n^{|m|}(\rho) \cdot \begin{cases} \cos(m\theta) & m \geq 0 \\ \sin(|m|\theta) & m < 0 \end{cases}
$$

   5.1 Key Aberrations Affecting Lithography

| Zernike Term | Aberration | Effect on Imaging |
|:-------------|:-----------|:------------------|
| $Z_4$ | Defocus | Pattern-dependent CD shift |
| $Z_5, Z_6$ | Astigmatism | H/V feature difference |
| $Z_7, Z_8$ | Coma | Pattern shift, asymmetric printing |
| $Z_9$ | Spherical | Through-pitch CD variation |
| $Z_{10}, Z_{11}$ | Trefoil | Three-fold symmetric distortion |

   5.2 Aberrated Pupil Function

The pupil function with aberrations:

$$
P(\rho,\theta) = P_0(\rho,\theta) \cdot \exp\left[\frac{2\pi i}{\lambda} W(\rho,\theta)\right]
$$

 Engineering Specifications: 

- Modern scanners control Zernikes through adjustable lens elements
- Typical specification: $< 0.5\text{nm}$ RMS wavefront error



  6. Rigorous Mask Modeling

   6.1 Thin Mask (Kirchhoff) Approximation

Assumes the mask is infinitely thin:

$$
M(x,y) = t(x,y) \cdot e^{i\phi(x,y)}
$$

 Limitations: 

- Fails for advanced nodes
- Mask topography (absorber thickness $\sim 50–70\text{nm}$) affects diffraction

   6.2 Rigorous Electromagnetic Field (EMF) Methods

   6.2.1 Rigorous Coupled-Wave Analysis (RCWA)

The mask is treated as a  periodic grating . Fields are expanded in Fourier series:

$$
E(x,z) = \sum_n E_n(z) \cdot e^{i(k_{x0} + nK)x}
$$

 Parameters: 

- $K = \frac{2\pi}{\text{pitch}}$ — Grating vector
- $k_{x0}$ — Incident wave x-component

Substituting into Maxwell's equations yields  coupled ODEs  solved as an eigenvalue problem in each z-layer.

   6.2.2 FDTD (Finite-Difference Time-Domain)

Directly discretizes Maxwell's curl equations on a  Yee grid :

$$
\frac{\partial \mathbf{E}}{\partial t} = \frac{1}{\epsilon} 
abla \times \mathbf{H}
$$

$$
\frac{\partial \mathbf{H}}{\partial t} = -\frac{1}{\mu} 
abla \times \mathbf{E}
$$

 Characteristics: 

- Explicit time-stepping
- Computationally intensive
- Handles arbitrary geometries



  7. Photoresist Modeling

   7.1 Exposure: Dill ABC Model

The photoactive compound (PAC) concentration $M$ evolves as:

$$
\frac{\partial M}{\partial t} = -I(z,t) \cdot [A \cdot M + B] \cdot M
$$

 Parameters: 

- $A$ — Bleachable absorption coefficient
- $B$ — Non-bleachable absorption coefficient
- $I(z,t)$ — Intensity in the resist

Light intensity in the resist follows Beer-Lambert:

$$
\frac{\partial I}{\partial z} = -\alpha(M) \cdot I
$$

where $\alpha = A \cdot M + B$.

   7.2 Post-Exposure Bake: Reaction-Diffusion

For  chemically amplified resists (CAR) :

$$
\frac{\partial m}{\partial t} = D
abla^2 m - k_{amp} \cdot m \cdot [H^+]
$$

 Variables: 

- $m$ — Blocking group concentration
- $D$ — Diffusivity (temperature-dependent, Arrhenius behavior)
- $[H^+]$ — Acid concentration

Acid diffusion and quenching:

$$
\frac{\partial [H^+]}{\partial t} = D_H 
abla^2 [H^+] - k_q [H^+][Q]
$$

where $Q$ is quencher concentration.

   7.3 Development: Mack Model

Development rate as a function of inhibitor concentration $m$:

$$
R(m) = R_{max} \cdot \frac{(a+1)(1-m)^n}{a + (1-m)^n} + R_{min}
$$

 Parameters: 

- $a, n$ — Kinetic parameters
- $R_{max}$ — Maximum development rate
- $R_{min}$ — Minimum development rate (unexposed)

This creates the  nonlinear resist response  that sharpens edges.



  8. Optical Proximity Correction (OPC)

   8.1 The Inverse Problem

Given target pattern $T$, find mask $M$ such that:

$$
\text{Image}(M) \approx T
$$

   8.2 Model-Based OPC

Iterative edge-based correction. Cost function:

$$
\mathcal{L} = \sum_i w_i \cdot (EPE_i)^2 + \lambda \cdot R(M)
$$

 Components: 

- $EPE_i$ — Edge Placement Error (distance from target at evaluation point $i$)
- $w_i$ — Weight for each evaluation point
- $R(M)$ — Regularization term for mask manufacturability

Gradient descent update:

$$
M^{(k+1)} = M^{(k)} - \eta \frac{\partial \mathcal{L}}{\partial M}
$$

 Gradient Computation Methods: 

- Adjoint methods (efficient for many output points)
- Direct differentiation of SOCS kernels

   8.3 Inverse Lithography Technology (ILT)

Full pixel-based mask optimization:

$$
\min_M \left\| I(M) - I_{target} \right\|^2 + \lambda_1 \|M\|_{TV} + \lambda_2 \|
abla^2 M\|^2
$$

 Regularization Terms: 

- $\|M\|_{TV}$ — Total Variation promotes sharp mask edges
- $\|
abla^2 M\|^2$ — Laplacian term controls curvature

 Result:  ILT produces  curvilinear masks  with superior imaging, enabled by multi-beam mask writers.



  9. Source-Mask Optimization (SMO)

Joint optimization of illumination source $J$ and mask $M$:

$$
\min_{J,M} \mathcal{L}(J,M) = \left\| I(J,M) - I_{target} \right\|^2 + \text{process window terms}
$$

   9.1 Constraints

 Source Constraints: 

- Pixelized representation
- Non-negative intensity: $J \geq 0$
- Power constraint: $\int J \, dA = P_0$

 Mask Constraints: 

- Minimum feature size
- Maximum curvature
- Manufacturability rules

   9.2 Mathematical Properties

The problem is  bilinear in $J$ and $M$  (linear in each separately), enabling:

- Alternating optimization
- Joint gradient methods

   9.3 Process Window Co-optimization

Adds robustness across focus and dose variations:

$$
\mathcal{L}_{PW} = \sum_{focus, dose} w_{f,d} \cdot \left\| I_{f,d}(J,M) - I_{target} \right\|^2
$$



  10. EUV-Specific Mathematics

   10.1 Multilayer Reflector

Mo/Si multilayer with  40–50 bilayer pairs . Peak reflectivity from Bragg condition:

$$
2d \cdot \cos\theta = n\lambda
$$

 Parameters: 

- $d \approx 6.9\text{nm}$ — Bilayer period for $\lambda = 13.5\text{nm}$
- Near-normal incidence ($\theta \approx 0°$)

   Transfer Matrix Method

Reflectivity calculation:

$$
\begin{pmatrix} E_{out}^+ \\ E_{out}^- \end{pmatrix} = \prod_{j=1}^{N} M_j \begin{pmatrix} E_{in}^+ \\ E_{in}^- \end{pmatrix}
$$

where $M_j$ is the transfer matrix for layer $j$.

   10.2 Mask 3D Effects

EUV masks are  reflective  with absorber patterns. At 6° chief ray angle:

-  Shadowing:  Different illumination angles see different absorber profiles
-  Best focus shift:  Pattern-dependent focus offsets

Requires  full 3D EMF simulation  (RCWA or FDTD) for accurate modeling.

   10.3 Stochastic Effects

At EUV, photon counts are low enough that  shot noise  matters:

$$
\sigma_{photon} = \sqrt{N_{photon}}
$$

   Line Edge Roughness (LER) Contributions

- Photon shot noise
- Acid shot noise
- Resist molecular granularity

   Power Spectral Density Model

$$
PSD(f) = \frac{A}{1 + (2\pi f \xi)^{2+2H}}
$$

 Parameters: 

- $\xi$ — Correlation length
- $H$ — Hurst exponent (typically $0.5–0.8$)
- $A$ — Amplitude

   Stochastic Simulation via Monte Carlo

- Poisson-distributed photon absorption
- Random acid generation and diffusion
- Development with local rate variations



  11. Process Window Analysis

   11.1 Bossung Curves

CD vs. focus at multiple dose levels:

$$
CD(E, F) = CD_0 + a_1 E + a_2 F + a_3 E^2 + a_4 F^2 + a_5 EF + \cdots
$$

Polynomial expansion fitted to simulation/measurement.

   11.2 Normalized Image Log-Slope (NILS)

$$
NILS = w \cdot \left. \frac{d \ln I}{dx} \right|_{edge}
$$

 Parameters: 

- $w$ — Feature width
- Evaluated at the edge position

 Design Rule:  $NILS > 2$ generally required for acceptable process latitude.

 Relationship to Exposure Latitude: 

$$
EL \propto NILS
$$

   11.3 Depth of Focus (DOF) and Exposure Latitude (EL) Trade-off

Visualized as overlapping process windows across pattern types — the  common process window  must satisfy all critical features.



  12. Multi-Patterning Mathematics

   12.1 SADP (Self-Aligned Double Patterning)

$$
\text{Spacer pitch} = \frac{\text{Mandrel pitch}}{2}
$$

 Design Rule Constraints: 

- Mandrel CD and pitch
- Spacer thickness uniformity
- Cut pattern overlay

   12.2 LELE (Litho-Etch-Litho-Etch) Decomposition

 Graph coloring problem:  Assign features to masks such that:

- Features on same mask satisfy minimum spacing
- Total mask count minimized (typically 2)

 Computational Properties: 

- For 1D patterns: Equivalent to 2-colorable graph (bipartite)
- For 2D:  NP-complete  in general

 Solution Methods: 

- Integer Linear Programming (ILP)
- SAT solvers
- Heuristic algorithms

 Conflict Graph Edge Weight: 

$$
w_{ij} = \begin{cases} \infty & \text{if } d_{ij} < d_{min,same} \\ 0 & \text{otherwise} \end{cases}
$$



  13. Machine Learning Integration

   13.1 Surrogate Models

Neural networks approximate aerial image or resist profile:

$$
I_{NN}(x; M) \approx I_{physics}(x; M)
$$

 Benefits: 

- Training on physics simulation data
- Inference 100–1000× faster

   13.2 OPC with ML

-  CNNs:  Predict edge corrections
-  GANs:  Generate mask patterns
-  Reinforcement Learning:  Iterative OPC optimization

   13.3 Hotspot Detection

Classification of lithographic failure sites:

$$
P(\text{hotspot} \mid \text{pattern}) = \sigma(W \cdot \phi(\text{pattern}) + b)
$$

where $\sigma$ is the sigmoid function and $\phi$ extracts pattern features.



  14. Mathematical Optimization Framework

   14.1 Constrained Optimization Formulation

$$
\min f(x) \quad \text{subject to} \quad g(x) \leq 0, \quad h(x) = 0
$$

 Solution Methods: 

- Sequential Quadratic Programming (SQP)
- Interior Point Methods
- Augmented Lagrangian

   14.2 Regularization Techniques

| Regularization | Formula | Effect |
|:---------------|:--------|:-------|
| L1 (Sparsity) | $\|
abla M\|_1$ | Promotes sparse gradients |
| L2 (Smoothness) | $\|
abla M\|_2^2$ | Promotes smooth transitions |
| Total Variation | $\int |
abla M| \, dx$ | Preserves edges while smoothing |



  15. Mathematical Stack:

| Layer | Mathematics |
|:------|:------------|
| Electromagnetic Propagation | Maxwell's equations, RCWA, FDTD |
| Image Formation | Fourier optics, TCC, Hopkins, vector diffraction |
| Aberrations | Zernike polynomials, wavefront phase |
| Photoresist | Coupled PDEs (reaction-diffusion) |
| Correction (OPC/ILT) | Inverse problems, constrained optimization |
| SMO | Bilinear optimization, gradient methods |
| Stochastics (EUV) | Poisson processes, Monte Carlo |
| Multi-Patterning | Graph theory, combinatorial optimization |
| Machine Learning | Neural networks, surrogate models |



  Formulas:

   Core Equations


Resolution:           R = k₁ × λ / NA
Depth of Focus:       DOF = k₂ × λ / NA²
Numerical Aperture:   NA = n × sin(θ)
NILS:                 NILS = w × (d ln I / dx)|edge
Bragg Condition:      2d × cos(θ) = nλ
Shot Noise:           σ = √N

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/optics-and-lithography-mathematics) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
