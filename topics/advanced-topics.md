# Semiconductor Manufacturing: Advanced Mathematics

**Keywords**: advanced topics, advanced mathematics, semiconductor mathematics, lithography math, plasma physics, diffusion math

---

**Semiconductor Manufacturing: Advanced Mathematics**



**1. Lithography & Optical Physics**

This is arguably the most mathematically demanding area of semiconductor manufacturing.

**1.1 Fourier Optics & Partial Coherence Theory**

The foundation of photolithography treats optical imaging as a spatial frequency filtering problem.

- **Key Concept**: The mask pattern is decomposed into spatial frequency components
- **Optical System**: Acts as a low-pass filter on spatial frequencies
- **Hopkins Formulation**: Describes partially coherent imaging

The aerial image intensity $I(x,y)$ is given by:

$$
I(x,y) = \iint\iint TCC(f_1, g_1, f_2, g_2) \cdot M(f_1, g_1) \cdot M^*(f_2, g_2) \cdot e^{2\pi i[(f_1-f_2)x + (g_1-g_2)y]} \, df_1 \, dg_1 \, df_2 \, dg_2
$$

Where:

- $TCC$ = Transmission Cross-Coefficient
- $M(f,g)$ = Mask spectrum (Fourier transform of mask pattern)
- $M^*$ = Complex conjugate of mask spectrum

**SOCS Decomposition** (Sum of Coherent Systems):

$$
TCC(f_1, g_1, f_2, g_2) = \sum_{k=1}^{N} \lambda_k \phi_k(f_1, g_1) \phi_k^*(f_2, g_2)
$$

- Eigenvalue decomposition makes computation tractable
- $\lambda_k$ are eigenvalues (typically only 10-20 terms needed)
- $\phi_k$ are eigenfunctions

**1.2 Inverse Lithography Technology (ILT)**

Given a desired wafer pattern $T(x,y)$, find the optimal mask $M(x,y)$.

**Mathematical Framework**:

- **Objective Function**:

$$
\min_{M} \left\| I[M](x,y) - T(x,y) \right\|^2 + \alpha R[M]
$$

- **Key Methods**:
  - Variational calculus and gradient descent in function spaces
  - Level-set methods for topology optimization:
    $$
    \frac{\partial \phi}{\partial t} + v|
abla\phi| = 0
    $$
  - Tikhonov regularization: $R[M] = \|
abla M\|^2$
  - Total-variation regularization: $R[M] = \int |
abla M| \, dx \, dy$
  - Adjoint methods for efficient gradient computation

**1.3 EUV & Rigorous Electromagnetics**

At $\lambda = 13.5$ nm, scalar diffraction theory fails. Full vector Maxwell's equations are required.

**Maxwell's Equations** (time-harmonic form):

$$

abla \times \mathbf{E} = -i\omega\mu\mathbf{H}
$$

$$

abla \times \mathbf{H} = i\omega\varepsilon\mathbf{E}
$$

**Numerical Methods**:

- **RCWA** (Rigorous Coupled-Wave Analysis):
  - Eigenvalue problem for each diffraction order
  - Transfer matrix for multilayer stacks:
    $$
    \begin{pmatrix} E^+ \\ E^- \end{pmatrix}_{out} = \mathbf{T} \begin{pmatrix} E^+ \\ E^- \end{pmatrix}_{in}
    $$

- **FDTD** (Finite-Difference Time-Domain):
  - Yee grid discretization
  - Leapfrog time integration:
    $$
    E^{n+1} = E^n + \frac{\Delta t}{\varepsilon} 
abla \times H^{n+1/2}
    $$

- **Multilayer Thin-Film Optics**:
  - Fresnel coefficients at each interface
  - Transfer matrix method for $N$ layers

**1.4 Aberration Theory**

Optical aberrations characterized using **Zernike Polynomials**:

$$
W(\rho, \theta) = \sum_{n,m} Z_n^m R_n^m(\rho) \cdot 
\begin{cases} 
\cos(m\theta) & \text{(even)} \\
\sin(m\theta) & \text{(odd)}
\end{cases}
$$

Where $R_n^m(\rho)$ are radial polynomials:

$$
R_n^m(\rho) = \sum_{k=0}^{(n-m)/2} \frac{(-1)^k (n-k)!}{k! \left(\frac{n+m}{2}-k\right)! \left(\frac{n-m}{2}-k\right)!} \rho^{n-2k}
$$

**Common Aberrations**:

| Zernike Term | Name | Effect |
|--------------|------|--------|
| $Z_4^0$ | Defocus | Uniform blur |
| $Z_3^1$ | Coma | Asymmetric distortion |
| $Z_4^0$ | Spherical | Halo effect |
| $Z_2^2$ | Astigmatism | Directional blur |



**2. Quantum Mechanics & Device Physics**

As transistors reach sub-5nm dimensions, classical models break down.

**2.1 Schrödinger Equation & Quantum Transport**

**Time-Independent Schrödinger Equation**:

$$
\hat{H}\psi = E\psi
$$

$$
\left[-\frac{\hbar^2}{2m}
abla^2 + V(\mathbf{r})\right]\psi(\mathbf{r}) = E\psi(\mathbf{r})
$$

**Non-Equilibrium Green's Function (NEGF) Formalism**:

- Retarded Green's function:
  $$
  G^R(E) = \left[(E + i\eta)I - H - \Sigma_L - \Sigma_R\right]^{-1}
  $$

- Self-energy $\Sigma$ incorporates:
  - Contact coupling
  - Scattering mechanisms
  - Electron-phonon interaction

- Current calculation:
  $$
  I = \frac{2e}{h} \int T(E) [f_L(E) - f_R(E)] \, dE
  $$

- Transmission function:
  $$
  T(E) = \text{Tr}\left[\Gamma_L G^R \Gamma_R G^A\right]
  $$

**Wigner Function** (bridging quantum and semiclassical):

$$
W(x,p) = \frac{1}{2\pi\hbar} \int \psi^*\left(x + \frac{y}{2}\right) \psi\left(x - \frac{y}{2}\right) e^{ipy/\hbar} \, dy
$$

**2.2 Band Structure Theory**

**k·p Perturbation Theory**:

$$
H_{k \cdot p} = \frac{p^2}{2m_0} + V(\mathbf{r}) + \frac{\hbar}{m_0}\mathbf{k} \cdot \mathbf{p} + \frac{\hbar^2 k^2}{2m_0}
$$

**Effective Mass Tensor**:

$$
\frac{1}{m^*_{ij}} = \frac{1}{\hbar^2} \frac{\partial^2 E}{\partial k_i \partial k_j}
$$

**Tight-Binding Hamiltonian**:

$$
H = \sum_i \varepsilon_i |i\rangle\langle i| + \sum_{\langle i,j \rangle} t_{ij} |i\rangle\langle j|
$$

- $\varepsilon_i$ = on-site energy
- $t_{ij}$ = hopping integral (Slater-Koster parameters)

**2.3 Semiclassical Transport**

**Boltzmann Transport Equation**:

$$
\frac{\partial f}{\partial t} + \mathbf{v} \cdot 
abla_r f + \frac{\mathbf{F}}{\hbar} \cdot 
abla_k f = \left(\frac{\partial f}{\partial t}\right)_{coll}
$$

- 6D phase space $(x, y, z, k_x, k_y, k_z)$
- Collision integral (scattering):
  $$
  \left(\frac{\partial f}{\partial t}\right)_{coll} = \sum_{k'} [S(k',k)f(k')(1-f(k)) - S(k,k')f(k)(1-f(k'))]
  $$

**Drift-Diffusion Equations** (moment expansion):

$$
\mathbf{J}_n = q\mu_n n\mathbf{E} + qD_n
abla n
$$

$$
\mathbf{J}_p = q\mu_p p\mathbf{E} - qD_p
abla p
$$



**3. Process Simulation PDEs**

**3.1 Dopant Diffusion**

**Fick's Second Law** (concentration-dependent):

$$
\frac{\partial C}{\partial t} = 
abla \cdot (D(C,T) 
abla C) + G - R
$$

**Coupled Point-Defect System**:

$$
\begin{aligned}
\frac{\partial C_A}{\partial t} &= 
abla \cdot (D_A 
abla C_A) + k_{AI}C_AC_I - k_{AV}C_AC_V \\
\frac{\partial C_I}{\partial t} &= 
abla \cdot (D_I 
abla C_I) + G_I - k_{IV}C_IC_V \\
\frac{\partial C_V}{\partial t} &= 
abla \cdot (D_V 
abla C_V) + G_V - k_{IV}C_IC_V
\end{aligned}
$$

Where:

- $C_A$ = dopant concentration
- $C_I$ = interstitial concentration
- $C_V$ = vacancy concentration
- $k_{ij}$ = reaction rate constants

**3.2 Oxidation & Film Growth**

**Deal-Grove Model**:

$$
x_{ox}^2 + Ax_{ox} = B(t + \tau)
$$

- $A$ = linear rate constant (surface reaction limited)
- $B$ = parabolic rate constant (diffusion limited)
- $\tau$ = time offset for initial oxide

**Moving Boundary (Stefan) Problem**:

$$
D\frac{\partial C}{\partial x}\bigg|_{x=s(t)} = C^* \frac{ds}{dt}
$$

**3.3 Ion Implantation**

**Binary Collision Approximation** (Monte Carlo):

- Screened Coulomb potential:
  $$
  V(r) = \frac{Z_1 Z_2 e^2}{r} \phi\left(\frac{r}{a}\right)
  $$

- Scattering angle from two-body collision integral

**As-Implanted Profile** (Pearson IV distribution):

$$
f(x) = f_0 \left[1 + \left(\frac{x-R_p}{b}\right)^2\right]^{-m} \exp\left[-r \tan^{-1}\left(\frac{x-R_p}{b}\right)\right]
$$

Parameters: $R_p$ (projected range), $\Delta R_p$ (straggle), skewness, kurtosis

**3.4 Plasma Etching**

**Electron Energy Distribution** (Boltzmann equation):

$$
\frac{\partial f}{\partial t} + \mathbf{v} \cdot 
abla f - \frac{e\mathbf{E}}{m} \cdot 
abla_v f = C[f]
$$

**Child-Langmuir Law** (sheath ion flux):

$$
J = \frac{4\varepsilon_0}{9} \sqrt{\frac{2e}{M}} \frac{V^{3/2}}{d^2}
$$

**3.5 Chemical-Mechanical Polishing (CMP)**

**Preston Equation**:

$$
\frac{dh}{dt} = K_p \cdot P \cdot V
$$

- $K_p$ = Preston coefficient
- $P$ = local pressure
- $V$ = relative velocity

**Pattern-Density Dependent Model**:

$$
P_{local} = P_{avg} \cdot \frac{A_{total}}{A_{contact}(\rho)}
$$



**4. Electromagnetic Simulation**

**4.1 Interconnect Modeling**

**Capacitance Extraction** (Laplace equation):

$$

abla^2 \phi = 0 \quad \text{(dielectric regions)}
$$

$$

abla \cdot (\varepsilon 
abla \phi) = -\rho \quad \text{(with charges)}
$$

**Boundary Element Method**:

$$
c(\mathbf{r})\phi(\mathbf{r}) = \int_S \left[\phi(\mathbf{r}') \frac{\partial G}{\partial n'} - G(\mathbf{r}, \mathbf{r}') \frac{\partial \phi}{\partial n'}\right] dS'
$$

Where $G(\mathbf{r}, \mathbf{r}') = \frac{1}{4\pi|\mathbf{r} - \mathbf{r}'|}$ (free-space Green's function)

**4.2 Partial Inductance**

**PEEC Method** (Partial Element Equivalent Circuit):

$$
L_{p,ij} = \frac{\mu_0}{4\pi} \frac{1}{a_i a_j} \int_{V_i} \int_{V_j} \frac{d\mathbf{l}_i \cdot d\mathbf{l}_j}{|\mathbf{r}_i - \mathbf{r}_j|}
$$



**5. Statistical & Stochastic Methods**

**5.1 Process Variability**

**Multivariate Gaussian Model**:

$$
p(\mathbf{x}) = \frac{1}{(2\pi)^{n/2}|\Sigma|^{1/2}} \exp\left(-\frac{1}{2}(\mathbf{x}-\boldsymbol{\mu})^T \Sigma^{-1} (\mathbf{x}-\boldsymbol{\mu})\right)
$$

**Principal Component Analysis**:

$$
\mathbf{X} = \mathbf{U}\mathbf{S}\mathbf{V}^T
$$

- Transform to uncorrelated variables
- Dimensionality reduction: retain components with largest singular values

**Polynomial Chaos Expansion**:

$$
Y(\boldsymbol{\xi}) = \sum_{k=0}^{P} y_k \Psi_k(\boldsymbol{\xi})
$$

- $\Psi_k$ = orthogonal polynomial basis (Hermite for Gaussian inputs)
- Enables uncertainty quantification without Monte Carlo

**5.2 Yield Modeling**

**Poisson Defect Model**:

$$
Y = e^{-D \cdot A}
$$

- $D$ = defect density (defects/cm²)
- $A$ = critical area

**Negative Binomial** (clustered defects):

$$
Y = \left(1 + \frac{DA}{\alpha}\right)^{-\alpha}
$$

**5.3 Reliability Physics**

**Weibull Distribution** (lifetime):

$$
F(t) = 1 - \exp\left[-\left(\frac{t}{\eta}\right)^\beta\right]
$$

- $\eta$ = scale parameter (characteristic life)
- $\beta$ = shape parameter (failure mode indicator)

**Black's Equation** (electromigration):

$$
MTTF = A \cdot J^{-n} \cdot \exp\left(\frac{E_a}{k_B T}\right)
$$



**6. Optimization & Inverse Problems**

**6.1 Design of Experiments**

**Response Surface Methodology**:

$$
y = \beta_0 + \sum_i \beta_i x_i + \sum_i \beta_{ii} x_i^2 + \sum_{i<j} \beta_{ij} x_i x_j + \varepsilon
$$

**D-Optimal Design**:

$$
\max_{\xi} \det(\mathbf{X}^T\mathbf{X})
$$

**6.2 Metrology as Inverse Problems**

**Scatterometry / OCD**:

Given measured diffraction intensities $\mathbf{I}_{meas}$, find structure parameters $\boldsymbol{\theta}$:

$$
\min_{\boldsymbol{\theta}} \left\| \mathbf{I}_{meas} - \mathbf{I}_{model}(\boldsymbol{\theta}) \right\|^2 + \lambda \|\boldsymbol{\theta}\|^2
$$

- Forward model: RCWA
- Inversion: Levenberg-Marquardt, trust-region methods

**Spectroscopic Ellipsometry**:

Measured quantities $\Psi$ and $\Delta$ from:

$$
\frac{r_p}{r_s} = \tan(\Psi) e^{i\Delta}
$$

Fit to dispersion models:

**Tauc-Lorentz** (amorphous semiconductors):

$$
\varepsilon_2(E) = \begin{cases}
\frac{AE_0 C (E-E_g)^2}{(E^2-E_0^2)^2 + C^2E^2} \cdot \frac{1}{E} & E > E_g \\
0 & E \leq E_g
\end{cases}
$$



**7. Computational Geometry & Graph Theory**

**7.1 VLSI Physical Design**

**Graph Partitioning** (min-cut):

$$
\min_{P} \sum_{(u,v) \in E : u \in P, v 
otin P} w(u,v)
$$

- Kernighan-Lin algorithm
- Spectral methods using Fiedler vector

**Placement** (quadratic programming):

$$
\min_{\mathbf{x}, \mathbf{y}} \sum_{(i,j) \in E} w_{ij} \left[(x_i - x_j)^2 + (y_i - y_j)^2\right]
$$

**Steiner Tree Problem** (routing):

- Given pins to connect, find minimum-length tree
- NP-hard; use approximation algorithms (RSMT, rectilinear Steiner)

**7.2 Mask Data Preparation**

- **Boolean Operations**: Union, intersection, difference of polygons
- **Polygon Clipping**: Sutherland-Hodgman, Vatti algorithms
- **Fracturing**: Decompose complex shapes into trapezoids for e-beam writing



**8. Thermal & Mechanical Analysis**

**8.1 Heat Transport**

**Fourier Heat Equation**:

$$
\rho c_p \frac{\partial T}{\partial t} = 
abla \cdot (k 
abla T) + Q
$$

**Phonon Boltzmann Transport** (nanoscale):

$$
\frac{\partial f}{\partial t} + \mathbf{v}_g \cdot 
abla f = \frac{f_0 - f}{\tau}
$$

- Required when feature size $<$ phonon mean free path
- Non-Fourier effects: ballistic transport, thermal rectification

**8.2 Thermo-Mechanical Stress**

**Linear Elasticity**:

$$
\sigma_{ij} = C_{ijkl} \varepsilon_{kl}
$$

**Equilibrium**:

$$

abla \cdot \boldsymbol{\sigma} + \mathbf{f} = 0
$$

**Thin Film Stress** (Stoney Equation):

$$
\sigma_f = \frac{E_s h_s^2}{6(1-
u_s) h_f} \cdot \frac{1}{R}
$$

- $R$ = wafer curvature radius
- $h_s$, $h_f$ = substrate and film thickness

**Thermal Stress**:

$$
\varepsilon_{thermal} = \alpha \Delta T
$$

$$
\sigma_{thermal} = E(\alpha_{film} - \alpha_{substrate})\Delta T
$$



**9. Multiscale & Atomistic Methods**

**9.1 Molecular Dynamics**

**Equation of Motion**:

$$
m_i \frac{d^2 \mathbf{r}_i}{dt^2} = -
abla_i U(\{\mathbf{r}\})
$$

**Interatomic Potentials**:

- **Tersoff** (covalent, e.g., Si):
  $$
  V_{ij} = f_c(r_{ij})[f_R(r_{ij}) + b_{ij} f_A(r_{ij})]
  $$

- **Embedded Atom Method** (metals):
  $$
  E_i = F_i(\rho_i) + \frac{1}{2}\sum_{j 
eq i} \phi_{ij}(r_{ij})
  $$

**Velocity Verlet Integration**:

$$
\mathbf{r}(t+\Delta t) = \mathbf{r}(t) + \mathbf{v}(t)\Delta t + \frac{\mathbf{a}(t)}{2}\Delta t^2
$$

$$
\mathbf{v}(t+\Delta t) = \mathbf{v}(t) + \frac{\mathbf{a}(t) + \mathbf{a}(t+\Delta t)}{2}\Delta t
$$

**9.2 Kinetic Monte Carlo**

**Master Equation**:

$$
\frac{dP_i}{dt} = \sum_j (W_{ji} P_j - W_{ij} P_i)
$$

**Transition Rates** (Arrhenius):

$$
W_{ij} = 
u_0 \exp\left(-\frac{E_a}{k_B T}\right)
$$

**BKL Algorithm**:

1. Compute all rates $\{r_i\}$
2. Total rate: $R = \sum_i r_i$
3. Select event $j$ with probability $r_j / R$
4. Advance time: $\Delta t = -\ln(u) / R$ where $u \in (0,1)$

**9.3 Ab Initio Methods**

**Kohn-Sham Equations** (DFT):

$$
\left[-\frac{\hbar^2}{2m}
abla^2 + V_{eff}(\mathbf{r})\right]\psi_i(\mathbf{r}) = \varepsilon_i \psi_i(\mathbf{r})
$$

$$
V_{eff} = V_{ext} + V_H[n] + V_{xc}[n]
$$

Where:

- $V_H[n] = \int \frac{n(\mathbf{r}')}{|\mathbf{r} - \mathbf{r}'|} d\mathbf{r}'$ (Hartree potential)
- $V_{xc}[n] = \frac{\delta E_{xc}[n]}{\delta n}$ (exchange-correlation)



**10. Machine Learning & Data Science**

**10.1 Virtual Metrology**

**Regression Models**:

- Linear: $y = \mathbf{w}^T \mathbf{x} + b$
- Kernel Ridge Regression:
  $$
  \mathbf{w} = (\mathbf{K} + \lambda \mathbf{I})^{-1} \mathbf{y}
  $$
- Neural Networks: $y = f_L \circ f_{L-1} \circ \cdots \circ f_1(\mathbf{x})$

**10.2 Defect Detection**

**Convolutional Neural Networks**:

$$
(f * g)[n] = \sum_m f[m] \cdot g[n-m]
$$

- Feature extraction through learned filters
- Pooling for translation invariance

**Anomaly Detection**:

- Autoencoders: $\text{loss} = \|x - D(E(x))\|^2$
- Isolation Forest: anomaly score based on path length

**10.3 Process Optimization**

**Bayesian Optimization**:

$$
x_{next} = \arg\max_x \alpha(x | \mathcal{D})
$$

**Acquisition Functions**:

- Expected Improvement: $\alpha_{EI}(x) = \mathbb{E}[\max(f(x) - f^*, 0)]$
- Upper Confidence Bound: $\alpha_{UCB}(x) = \mu(x) + \kappa \sigma(x)$



**Summary Table**

| Domain | Key Mathematical Topics |
|--------|-------------------------|
| **Lithography** | Fourier analysis, inverse problems, PDEs, optimization |
| **Device Physics** | Quantum mechanics, functional analysis, group theory |
| **Process Simulation** | Nonlinear PDEs, Monte Carlo, stochastic processes |
| **Electromagnetics** | Maxwell's equations, BEM, PEEC, capacitance/inductance extraction |
| **Statistics** | Multivariate Gaussian, PCA, polynomial chaos, yield models |
| **Optimization** | Response surface, inverse problems, Levenberg-Marquardt |
| **Physical Design** | Graph theory, combinatorial optimization, ILP, Steiner trees |
| **Thermal/Mechanical** | Continuum mechanics, FEM, tensor analysis |
| **Atomistic Modeling** | Statistical mechanics, DFT, KMC, molecular dynamics |
| **Machine Learning** | Neural networks, Bayesian inference, optimization |

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/advanced-topics) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
