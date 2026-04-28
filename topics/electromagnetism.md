# Electromagnetism Mathematics Modeling

**Keywords**: electromagnetism,electromagnetism mathematics,maxwell equations,drift diffusion,semiconductor electromagnetism,poisson equation,boltzmann transport,negf,quantum transport,optoelectronics

---

**Electromagnetism Mathematics Modeling**

A comprehensive guide to the mathematical frameworks used in semiconductor device simulation, covering electromagnetic theory, carrier transport, and quantum effects.





  1. The Core Problem

Semiconductor device modeling requires solving coupled systems that describe:

- How electromagnetic fields propagate in and interact with semiconductor materials
- How charge carriers (electrons and holes) move in response to fields
- How quantum effects modify classical behavior at nanoscales

 Key Variables: 

| Symbol | Description | Units |
|--------|-------------|-------|
| $\phi$ | Electrostatic potential | V |
| $n$ | Electron concentration | cm⁻³ |
| $p$ | Hole concentration | cm⁻³ |
| $\mathbf{E}$ | Electric field | V/cm |
| $\mathbf{J}_n, \mathbf{J}_p$ | Current densities | A/cm² |



  2. Fundamental Mathematical Frameworks

   2.1 Drift-Diffusion System

The workhorse of semiconductor device simulation couples three fundamental equations.

   2.1.1 Poisson's Equation (Electrostatics)

$$

abla \cdot (\varepsilon 
abla \phi) = -q(p - n + N_D^+ - N_A^-)
$$

 Where: 

- $\varepsilon$ — Permittivity of the semiconductor
- $\phi$ — Electrostatic potential
- $q$ — Elementary charge ($1.602 \times 10^{-19}$ C)
- $n, p$ — Electron and hole concentrations
- $N_D^+$ — Ionized donor concentration
- $N_A^-$ — Ionized acceptor concentration

   2.1.2 Continuity Equations (Carrier Conservation)

 For electrons: 

$$
\frac{\partial n}{\partial t} = \frac{1}{q}
abla \cdot \mathbf{J}_n - R + G
$$

 For holes: 

$$
\frac{\partial p}{\partial t} = -\frac{1}{q}
abla \cdot \mathbf{J}_p - R + G
$$

 Where: 

- $R$ — Recombination rate (cm⁻³s⁻¹)
- $G$ — Generation rate (cm⁻³s⁻¹)

   2.1.3 Current Density Relations

 Electron current (drift + diffusion): 

$$
\mathbf{J}_n = q\mu_n n \mathbf{E} + qD_n 
abla n
$$

 Hole current (drift + diffusion): 

$$
\mathbf{J}_p = q\mu_p p \mathbf{E} - qD_p 
abla p
$$

 Einstein Relations: 

$$
D_n = \frac{k_B T}{q} \mu_n \quad \text{and} \quad D_p = \frac{k_B T}{q} \mu_p
$$

   2.1.4 Recombination Models

-  Shockley-Read-Hall (SRH): 

$$
R_{SRH} = \frac{np - n_i^2}{\tau_p(n + n_1) + \tau_n(p + p_1)}
$$

-  Auger Recombination: 

$$
R_{Auger} = (C_n n + C_p p)(np - n_i^2)
$$

-  Radiative Recombination: 

$$
R_{rad} = B(np - n_i^2)
$$



   2.2 Maxwell's Equations in Semiconductors

For optoelectronics and high-frequency devices, the full electromagnetic treatment is necessary.

   2.2.1 Maxwell's Equations

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

   2.2.2 Constitutive Relations

 Displacement field: 

$$
\mathbf{D} = \varepsilon_0 \varepsilon_r(\omega) \mathbf{E}
$$

 Current density: 

$$
\mathbf{J} = \sigma(\omega) \mathbf{E}
$$

   2.2.3 Frequency-Dependent Dielectric Function

$$
\varepsilon(\omega) = \varepsilon_\infty - \frac{\omega_p^2}{\omega^2 + i\gamma\omega} + \sum_j \frac{f_j}{\omega_j^2 - \omega^2 - i\Gamma_j\omega}
$$

 Components: 

-  First term  ($\varepsilon_\infty$): High-frequency (background) permittivity
-  Second term  (Drude): Free carrier response
  - $\omega_p = \sqrt{\frac{nq^2}{\varepsilon_0 m^*}}$ — Plasma frequency
  - $\gamma$ — Damping rate
-  Third term  (Lorentz oscillators): Interband transitions
  - $\omega_j$ — Resonance frequencies
  - $\Gamma_j$ — Linewidths
  - $f_j$ — Oscillator strengths

   2.2.4 Complex Refractive Index

$$
\tilde{n}(\omega) = n(\omega) + i\kappa(\omega) = \sqrt{\varepsilon(\omega)}
$$

 Optical properties: 

-  Refractive index:  $n = \text{Re}(\tilde{n})$
-  Extinction coefficient:  $\kappa = \text{Im}(\tilde{n})$
-  Absorption coefficient:  $\alpha = \frac{2\omega\kappa}{c} = \frac{4\pi\kappa}{\lambda}$



   2.3 Boltzmann Transport Equation

When drift-diffusion is insufficient (hot carriers, high fields, ultrafast phenomena):

$$
\frac{\partial f}{\partial t} + \mathbf{v} \cdot 
abla_\mathbf{r} f + \frac{\mathbf{F}}{\hbar} \cdot 
abla_\mathbf{k} f = \left(\frac{\partial f}{\partial t}\right)_{\text{coll}}
$$

 Where: 

- $f(\mathbf{r}, \mathbf{k}, t)$ — Distribution function in 6D phase space
- $\mathbf{v} = \frac{1}{\hbar}
abla_\mathbf{k} E(\mathbf{k})$ — Group velocity
- $\mathbf{F}$ — External force (e.g., $q\mathbf{E}$)

   2.3.1 Collision Integral (Relaxation Time Approximation)

$$
\left(\frac{\partial f}{\partial t}\right)_{\text{coll}} \approx -\frac{f - f_0}{\tau}
$$

   2.3.2 Scattering Mechanisms

-  Acoustic phonon scattering: 

$$
\frac{1}{\tau_{ac}} \propto T \cdot E^{1/2}
$$

-  Optical phonon scattering: 

$$
\frac{1}{\tau_{op}} \propto \left(N_{op} + \frac{1}{2} \mp \frac{1}{2}\right)
$$

-  Ionized impurity scattering (Brooks-Herring): 

$$
\frac{1}{\tau_{ii}} \propto \frac{N_I}{E^{3/2}}
$$

   2.3.3 Solution Approaches

-  Monte Carlo methods:  Stochastically simulate individual carrier trajectories
-  Moment expansions:  Derive hydrodynamic equations from velocity moments
-  Spherical harmonic expansion:  Expand angular dependence in k-space



   2.4 Quantum Transport

For nanoscale devices where quantum effects dominate.

   2.4.1 Schrödinger Equation (Effective Mass Approximation)

$$
\left[-\frac{\hbar^2}{2m^*}
abla^2 + V(\mathbf{r})\right]\psi = E\psi
$$

   2.4.2 Schrödinger-Poisson Self-Consistent Loop


┌─────────────────────────────────────────────────┐
│                                                 │
│    Initial guess: V(r)                          │
│           │                                     │
│           ▼                                     │
│    Solve Schrodinger: H*psi = E*psi             │
│           │                                     │
│           ▼                                     │
│    Calculate charge density:                    │
│    rho(r) = q * sum |psi_i(r)|^2 * f(E_i)       │
│           │                                     │
│           ▼                                     │
│    Solve Poisson: div(grad V) = -rho/eps        │
│           │                                     │
│           ▼                                     │
│    Check convergence ──► If not, iterate        │
│                                                 │
└─────────────────────────────────────────────────┘


   2.4.3 Non-Equilibrium Green's Function (NEGF)

 Retarded Green's function: 

$$
[EI - H - \Sigma^R]G^R = I
$$

 Lesser Green's function (for electron density): 

$$
G^< = G^R \Sigma^< G^A
$$

 Current formula (Landauer-Büttiker type): 

$$
I = \frac{2q}{h}\int \text{Tr}\left[\Sigma^< G^> - \Sigma^> G^<\right] dE
$$

 Transmission function: 

$$
T(E) = \text{Tr}\left[\Gamma_L G^R \Gamma_R G^A\right]
$$

where $\Gamma_{L,R} = i(\Sigma_{L,R}^R - \Sigma_{L,R}^A)$ are the broadening matrices.

   2.4.4 Wigner Function Formalism

Quantum analog of the Boltzmann distribution:

$$
f_W(\mathbf{r}, \mathbf{p}, t) = \frac{1}{(\pi\hbar)^3}\int \psi^*\left(\mathbf{r}+\mathbf{s}\right)\psi\left(\mathbf{r}-\mathbf{s}\right) e^{2i\mathbf{p}\cdot\mathbf{s}/\hbar} d^3s
$$



  3. Coupled Optoelectronic Modeling

For solar cells, LEDs, and lasers, optical and electrical physics must be solved self-consistently.

   3.1 Self-Consistent Loop


┌─────────────────────────────────────────────────────────────┐
│                                                             │
│    Maxwell's Equations  ──────► Optical field E(r,w)        │
│           │                                                 │
│           ▼                                                 │
│    Generation rate: G(r) = alpha*|E|^2/(hbar*w)             │
│           │                                                 │
│           ▼                                                 │
│    Drift-Diffusion ──────► Carrier densities n(r), p(r)     │
│           │                                                 │
│           ▼                                                 │
│    Update eps(w,n,p) ──────► Free carrier absorption,       │
│           │                  plasma effects, band filling   │
│           │                                                 │
│           └──────────────── iterate ────────────────────┘   │
│                                                             │
└─────────────────────────────────────────────────────────────┘


   3.2 Key Coupling Equations

 Optical generation rate: 

$$
G(\mathbf{r}) = \frac{\alpha(\mathbf{r})|\mathbf{E}(\mathbf{r})|^2}{2\hbar\omega}
$$

 Free carrier absorption (modifies permittivity): 

$$
\Delta\alpha_{fc} = \sigma_n n + \sigma_p p
$$

 Band gap narrowing (high injection): 

$$
\Delta E_g = -A\left(\ln\frac{n}{n_0} + \ln\frac{p}{p_0}\right)
$$

   3.3 Laser Rate Equations

 Carrier density: 

$$
\frac{dn}{dt} = \frac{\eta I}{qV} - \frac{n}{\tau} - g(n)S
$$

 Photon density: 

$$
\frac{dS}{dt} = \Gamma g(n)S - \frac{S}{\tau_p} + \Gamma\beta\frac{n}{\tau}
$$

 Gain function (linear approximation): 

$$
g(n) = g_0(n - n_{tr})
$$



  4. Numerical Methods

   4.1 Method Comparison

| Method | Best For | Key Features | Computational Cost |
|--------|----------|--------------|-------------------|
|  Finite Element (FEM)  | Complex geometries | Adaptive meshing, handles interfaces | Medium-High |
|  Finite Difference (FDM)  | Regular grids | Simpler implementation | Low-Medium |
|  FDTD  | Time-domain EM | Explicit time stepping, broadband | High |
|  Transfer Matrix (TMM)  | Multilayer thin films | Analytical for 1D, very fast | Very Low |
|  RCWA  | Periodic structures | Fourier expansion | Medium |
|  Monte Carlo  | High-field transport | Stochastic, parallelizable | Very High |

   4.2 Scharfetter-Gummel Discretization

Essential for numerical stability in drift-diffusion. For electron current between nodes $i$ and $i+1$:

$$
J_{n,i+1/2} = \frac{qD_n}{h}\left[n_i B\left(\frac{\phi_i - \phi_{i+1}}{V_T}\right) - n_{i+1} B\left(\frac{\phi_{i+1} - \phi_i}{V_T}\right)\right]
$$

 Bernoulli function: 

$$
B(x) = \frac{x}{e^x - 1}
$$

   4.3 FDTD Yee Grid

 Update equations (1D example): 

$$
E_x^{n+1}(k) = E_x^n(k) + \frac{\Delta t}{\varepsilon \Delta z}\left[H_y^{n+1/2}(k+1/2) - H_y^{n+1/2}(k-1/2)\right]
$$

$$
H_y^{n+1/2}(k+1/2) = H_y^{n-1/2}(k+1/2) + \frac{\Delta t}{\mu \Delta z}\left[E_x^n(k+1) - E_x^n(k)\right]
$$

 Courant stability condition: 

$$
\Delta t \leq \frac{\Delta x}{c\sqrt{d}}
$$

where $d$ is the number of spatial dimensions.

   4.4 Newton-Raphson for Coupled System

For the coupled Poisson-continuity system, solve:

$$
\begin{pmatrix}
\frac{\partial F_\phi}{\partial \phi} & \frac{\partial F_\phi}{\partial n} & \frac{\partial F_\phi}{\partial p} \\
\frac{\partial F_n}{\partial \phi} & \frac{\partial F_n}{\partial n} & \frac{\partial F_n}{\partial p} \\
\frac{\partial F_p}{\partial \phi} & \frac{\partial F_p}{\partial n} & \frac{\partial F_p}{\partial p}
\end{pmatrix}
\begin{pmatrix}
\delta\phi \\ \delta n \\ \delta p
\end{pmatrix}
= -
\begin{pmatrix}
F_\phi \\ F_n \\ F_p
\end{pmatrix}
$$



  5. Multiscale Challenge

   5.1 Hierarchy of Scales

| Scale | Size | Method | Physics Captured |
|-------|------|--------|------------------|
|  Atomic  | 0.1–1 nm | DFT, tight-binding | Band structure, material parameters |
|  Quantum  | 1–100 nm | NEGF, Wigner function | Tunneling, confinement |
|  Mesoscale  | 10–1000 nm | Boltzmann, Monte Carlo | Hot carriers, non-equilibrium |
|  Device  | 100 nm–μm | Drift-diffusion | Classical transport |
|  Circuit  | μm–mm | Compact models (SPICE) | Lumped elements |

   5.2 Scale-Bridging Techniques

-  Parameter extraction:  DFT → effective masses, band gaps → drift-diffusion parameters
-  Quantum corrections to drift-diffusion: 

$$
n = N_c F_{1/2}\left(\frac{E_F - E_c - \Lambda_n}{k_B T}\right)
$$

where $\Lambda_n$ is the quantum potential from density-gradient theory:

$$
\Lambda_n = -\frac{\hbar^2}{12m^*}\frac{
abla^2 \sqrt{n}}{\sqrt{n}}
$$

-  Machine learning surrogates:  Train neural networks on expensive quantum simulations



  6. Key Mathematical Difficulties

   6.1 Extreme Nonlinearity

Carrier concentrations depend exponentially on potential:

$$
n = n_i \exp\left(\frac{E_F - E_i}{k_B T}\right) = n_i \exp\left(\frac{q\phi}{k_B T}\right)
$$

At room temperature, $k_B T/q \approx 26$ mV, so small potential changes cause huge concentration swings.

 Solutions: 

- Gummel iteration (decouple and solve sequentially)
- Newton-Raphson with damping
- Continuation methods

   6.2 Numerical Stiffness

- Doping varies by $10^{10}$ or more (from intrinsic to heavily doped)
- Depletion regions: nm-scale features in μm-scale devices
- Time scales: fs (optical) to ms (thermal)

 Solutions: 

- Adaptive mesh refinement
- Implicit time stepping
- Logarithmic variable transformations: $u = \ln(n/n_i)$

   6.3 High Dimensionality

- Full Boltzmann: 7D (3 position + 3 momentum + time)
- NEGF: Large matrix inversions per energy point

 Solutions: 

- Mode-space approximation
- Hierarchical matrix methods
- GPU acceleration

   6.4 Multiphysics Coupling

Interacting effects:

-  Electro-thermal:  $\mu(T)$, $\kappa(T)$, Joule heating
-  Opto-electrical:  Generation, free-carrier absorption
-  Electro-mechanical:  Piezoelectric effects, strain-modified bands



  7. Emerging Frontiers

   7.1 Topological Effects

 Berry curvature: 

$$
\mathbf{\Omega}_n(\mathbf{k}) = i\langle
abla_\mathbf{k} u_n| \times |
abla_\mathbf{k} u_n\rangle
$$

 Anomalous velocity contribution: 

$$
\dot{\mathbf{r}} = \frac{1}{\hbar}
abla_\mathbf{k} E_n - \dot{\mathbf{k}} \times \mathbf{\Omega}_n
$$

 Applications:  Topological insulators, quantum Hall effect, valley-selective transport

   7.2 2D Materials

 Graphene (Dirac equation): 

$$
H = v_F \begin{pmatrix} 0 & p_x - ip_y \\ p_x + ip_y & 0 \end{pmatrix} = v_F \boldsymbol{\sigma} \cdot \mathbf{p}
$$

 Linear dispersion: 

$$
E = \pm \hbar v_F |\mathbf{k}|
$$

 TMDCs (valley physics): 

$$
H = at(\tau k_x \sigma_x + k_y \sigma_y) + \frac{\Delta}{2}\sigma_z + \lambda\tau\frac{\sigma_z - 1}{2}s_z
$$

   7.3 Spintronics

 Spin drift-diffusion: 

$$
\frac{\partial \mathbf{s}}{\partial t} = D_s 
abla^2 \mathbf{s} - \frac{\mathbf{s}}{\tau_s} + \mathbf{s} \times \boldsymbol{\omega}
$$

 Landau-Lifshitz-Gilbert (magnetization dynamics): 

$$
\frac{d\mathbf{M}}{dt} = -\gamma \mathbf{M} \times \mathbf{H}_{eff} + \frac{\alpha}{M_s}\mathbf{M} \times \frac{d\mathbf{M}}{dt}
$$

   7.4 Plasmonics in Semiconductors

 Nonlocal dielectric response: 

$$
\varepsilon(\omega, \mathbf{k}) = \varepsilon_\infty - \frac{\omega_p^2}{\omega^2 + i\gamma\omega - \beta^2 k^2}
$$

where $\beta^2 = \frac{3}{5}v_F^2$ accounts for spatial dispersion.

 Quantum corrections (Feibelman parameters): 

$$
d_\perp(\omega) = \frac{\int z \delta n(z) dz}{\int \delta n(z) dz}
$$



  Constants:

| Constant | Symbol | Value |
|----------|--------|-------|
| Elementary charge | $q$ | $1.602 \times 10^{-19}$ C |
| Planck's constant | $h$ | $6.626 \times 10^{-34}$ J·s |
| Reduced Planck's constant | $\hbar$ | $1.055 \times 10^{-34}$ J·s |
| Boltzmann constant | $k_B$ | $1.381 \times 10^{-23}$ J/K |
| Vacuum permittivity | $\varepsilon_0$ | $8.854 \times 10^{-12}$ F/m |
| Electron mass | $m_0$ | $9.109 \times 10^{-31}$ kg |
| Speed of light | $c$ | $2.998 \times 10^{8}$ m/s |

  Material Parameters (Silicon @ 300K):

| Parameter | Symbol | Value |
|-----------|--------|-------|
| Band gap | $E_g$ | 1.12 eV |
| Intrinsic carrier concentration | $n_i$ | $1.0 \times 10^{10}$ cm⁻³ |
| Electron mobility | $\mu_n$ | 1400 cm²/V·s |
| Hole mobility | $\mu_p$ | 450 cm²/V·s |
| Relative permittivity | $\varepsilon_r$ | 11.7 |
| Electron effective mass | $m_n^*/m_0$ | 0.26 |
| Hole effective mass | $m_p^*/m_0$ | 0.39 |

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/electromagnetism) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
