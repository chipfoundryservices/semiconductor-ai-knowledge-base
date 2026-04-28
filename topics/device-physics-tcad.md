# Device Physics, TCAD, and Mathematical Modeling

**Keywords**: device physics tcad,tcad,device physics,semiconductor device physics,band theory,drift diffusion,poisson equation,boltzmann transport,carrier transport,mobility models,recombination models,process tcad

---

**Device Physics, TCAD, and Mathematical Modeling**



  1. Physical Foundation

   1.1 Band Theory and Electronic Structure

-  Energy bands  arise from the periodic potential of the crystal lattice
  - Conduction band (empty states available for electron transport)
  - Valence band (filled states; holes represent missing electrons)
  - Bandgap $E_g$ separates these bands (Si: ~1.12 eV at 300K)

-  Effective mass approximation 
  - Electrons and holes behave as quasi-particles with modified mass
  - Electron effective mass: $m_n^*$
  - Hole effective mass: $m_p^*$

-  Carrier statistics  follow Fermi-Dirac distribution:

$$
f(E) = \frac{1}{1 + \exp\left(\frac{E - E_F}{k_B T}\right)}
$$

-  Carrier concentrations  in non-degenerate semiconductors:

$$
n = N_C \exp\left(-\frac{E_C - E_F}{k_B T}\right)
$$

$$
p = N_V \exp\left(-\frac{E_F - E_V}{k_B T}\right)
$$

Where:
- $N_C$, $N_V$ = effective density of states in conduction/valence bands
- $E_C$, $E_V$ = conduction/valence band edges
- $E_F$ = Fermi level

   1.2 Carrier Transport Mechanisms

| Mechanism | Driving Force | Current Density |
|-----------|---------------|-----------------|
| Drift | Electric field $\mathbf{E}$ | $\mathbf{J} = qn\mu\mathbf{E}$ |
| Diffusion | Concentration gradient | $\mathbf{J} = qD
abla n$ |
| Thermionic emission | Thermal energy over barrier | Exponential in $\phi_B/k_BT$ |
| Tunneling | Quantum penetration | Exponential in barrier |

-  Einstein relation  connects mobility and diffusivity:

$$
D = \frac{k_B T}{q} \mu
$$

   1.3 Generation and Recombination

-  Thermal equilibrium condition: 

$$
np = n_i^2
$$

-  Three primary recombination mechanisms: 
  1. Shockley-Read-Hall (SRH) — trap-assisted
  2. Auger — three-particle process (dominant at high injection)
  3. Radiative — photon emission (important in direct bandgap materials)



  2. Mathematical Hierarchy

   2.1 Quantum Mechanical Level (Most Fundamental)

   Time-Independent Schrödinger Equation

$$
\left[-\frac{\hbar^2}{2m^*}
abla^2 + V(\mathbf{r})\right]\psi = E\psi
$$

Where:
- $\hbar$ = reduced Planck constant
- $m^*$ = effective mass
- $V(\mathbf{r})$ = potential energy
- $\psi$ = wavefunction
- $E$ = energy eigenvalue

   Non-Equilibrium Green's Function (NEGF)

For open quantum systems (nanoscale devices, tunneling):

$$
G^R = [EI - H - \Sigma]^{-1}
$$

- $G^R$ = retarded Green's function
- $H$ = device Hamiltonian
- $\Sigma$ = self-energy (encodes contact coupling)

 Applications: 
- Tunnel FETs
- Ultra-scaled MOSFETs ($L_g < 10$ nm)
- Quantum well devices
- Resonant tunneling diodes

   2.2 Boltzmann Transport Level

   Boltzmann Transport Equation (BTE)

$$
\frac{\partial f}{\partial t} + \mathbf{v} \cdot 
abla_{\mathbf{r}} f + \frac{\mathbf{F}}{\hbar} \cdot 
abla_{\mathbf{k}} f = \left(\frac{\partial f}{\partial t}\right)_{\text{coll}}
$$

Where:
- $f(\mathbf{r}, \mathbf{k}, t)$ = distribution function in phase space
- $\mathbf{v}$ = group velocity
- $\mathbf{F}$ = external force
- RHS = collision integral

 Solution Methods: 
- Monte Carlo (stochastic particle tracking)
- Spherical Harmonics Expansion (SHE)
- Moments methods → leads to drift-diffusion, hydrodynamic

 Captures: 
- Hot carrier effects
- Velocity overshoot
- Non-equilibrium distributions
- Ballistic transport

   2.3 Hydrodynamic / Energy Balance Level

Derived from moments of BTE with carrier temperature as variable:

$$
\frac{\partial (nw)}{\partial t} + 
abla \cdot \mathbf{S} = \mathbf{J} \cdot \mathbf{E} - \frac{n(w - w_0)}{\tau_w}
$$

- $w$ = carrier energy density
- $\mathbf{S}$ = energy flux
- $\tau_w$ = energy relaxation time
- $w_0$ = equilibrium energy density

 Key feature:  Carrier temperature $T_n 
eq$ lattice temperature $T_L$

   2.4 Drift-Diffusion Level (The Workhorse)

The most widely used TCAD formulation — three coupled PDEs:

   Poisson's Equation (Electrostatics)

$$

abla \cdot (\varepsilon 
abla \psi) = -\rho = -q(p - n + N_D^+ - N_A^-)
$$

- $\psi$ = electrostatic potential
- $\varepsilon$ = permittivity
- $\rho$ = charge density
- $N_D^+$, $N_A^-$ = ionized donor/acceptor concentrations

   Electron Continuity Equation

$$
\frac{\partial n}{\partial t} = \frac{1}{q}
abla \cdot \mathbf{J}_n + G_n - R_n
$$

   Hole Continuity Equation

$$
\frac{\partial p}{\partial t} = -\frac{1}{q}
abla \cdot \mathbf{J}_p + G_p - R_p
$$

   Current Density Equations

 Standard form: 

$$
\mathbf{J}_n = q\mu_n n \mathbf{E} + qD_n 
abla n
$$

$$
\mathbf{J}_p = q\mu_p p \mathbf{E} - qD_p 
abla p
$$

 Quasi-Fermi level formulation: 

$$
\mathbf{J}_n = q\mu_n n 
abla E_{F,n}
$$

$$
\mathbf{J}_p = q\mu_p p 
abla E_{F,p}
$$

 System characteristics: 
- Coupled, nonlinear, elliptic-parabolic PDEs
- Carrier concentrations vary exponentially with potential
- Spans 10+ orders of magnitude across junctions



  3. Numerical Methods

   3.1 Spatial Discretization

   Finite Difference Method (FDM)
- Simple implementation
- Limited to structured (rectangular) grids
- Box integration for conservation

   Finite Element Method (FEM)
- Handles complex geometries
- Basis function expansion
- Weak (variational) formulation

   Finite Volume Method (FVM)
- Ensures local conservation
- Natural for semiconductor equations
- Control volume integration

   3.2 Scharfetter-Gummel Discretization

 Critical for numerical stability  — handles exponential carrier variations:

$$
J_{n,i+\frac{1}{2}} = \frac{qD_n}{h}\left[n_i B\left(\frac{\psi_i - \psi_{i+1}}{V_T}\right) - n_{i+1} B\left(\frac{\psi_{i+1} - \psi_i}{V_T}\right)\right]
$$

Where the  Bernoulli function  is:

$$
B(x) = \frac{x}{e^x - 1}
$$

 Properties: 
- Reduces to central difference for small $\Delta\psi$
- Reduces to upwind for large $\Delta\psi$
- Prevents spurious oscillations
- Thermal voltage: $V_T = k_B T / q \approx 26$ mV at 300K

   3.3 Mesh Generation

-  2D:  Delaunay triangulation
-  3D:  Tetrahedral meshing

 Adaptive refinement criteria: 
- Junction regions (high field gradients)
- Oxide interfaces
- Contact regions
- High current density areas

 Quality metrics: 
- Aspect ratio
- Orthogonality (important for FVM)
- Delaunay property (circumsphere criterion)

   3.4 Nonlinear Solvers

   Gummel Iteration (Decoupled)


repeat:
    1. Solve Poisson equation → ψ
    2. Solve electron continuity → n  
    3. Solve hole continuity → p
until convergence


 Pros: 
- Simple implementation
- Robust for moderate bias
- Each subproblem is smaller

 Cons: 
- Poor convergence at high injection
- Slow for strongly coupled systems

   Newton-Raphson (Fully Coupled)

Solve the linearized system:

$$
\mathbf{J} \cdot \delta\mathbf{x} = -\mathbf{F}(\mathbf{x})
$$

Where:
- $\mathbf{J}$ = Jacobian matrix $\partial \mathbf{F}/\partial \mathbf{x}$
- $\mathbf{F}$ = residual vector
- $\delta\mathbf{x}$ = update vector

 Pros: 
- Quadratic convergence near solution
- Handles strong coupling

 Cons: 
- Requires good initial guess
- Expensive Jacobian assembly
- Larger linear systems

   Hybrid Methods
- Start with Gummel to get close
- Switch to Newton for fast final convergence

   3.5 Linear Solvers

For large, sparse, ill-conditioned Jacobian systems:

| Method | Type | Characteristics |
|--------|------|-----------------|
| LU (PARDISO, UMFPACK) | Direct | Robust, memory-intensive |
| GMRES | Iterative | Krylov subspace, needs preconditioning |
| BiCGSTAB | Iterative | Non-symmetric systems |
| Multigrid | Iterative | Optimal for Poisson-like equations |



  4. Physical Models in TCAD

   4.1 Mobility Models

   Matthiessen's Rule

Combines independent scattering mechanisms:

$$
\frac{1}{\mu} = \frac{1}{\mu_{\text{lattice}}} + \frac{1}{\mu_{\text{impurity}}} + \frac{1}{\mu_{\text{surface}}} + \cdots
$$

   Lattice Scattering

$$
\mu_L = \mu_0 \left(\frac{T}{300}\right)^{-\alpha}
$$

- Si electrons: $\alpha \approx 2.4$
- Si holes: $\alpha \approx 2.2$

   Ionized Impurity Scattering

Brooks-Herring model:

$$
\mu_I \propto \frac{T^{3/2}}{N_I \cdot \ln(1 + b^2) - b^2/(1+b^2)}
$$

   High-Field Saturation (Caughey-Thomas)

$$
\mu(E) = \frac{\mu_0}{\left[1 + \left(\frac{\mu_0 E}{v_{\text{sat}}}\right)^\beta\right]^{1/\beta}}
$$

- $v_{\text{sat}}$ = saturation velocity (~$10^7$ cm/s for Si)
- $\beta$ = fitting parameter (~2 for electrons, ~1 for holes)

   4.2 Recombination Models

   Shockley-Read-Hall (SRH)

$$
R_{\text{SRH}} = \frac{np - n_i^2}{\tau_p(n + n_1) + \tau_n(p + p_1)}
$$

Where:
- $\tau_n$, $\tau_p$ = carrier lifetimes
- $n_1 = n_i \exp[(E_t - E_i)/k_BT]$
- $p_1 = n_i \exp[(E_i - E_t)/k_BT]$
- $E_t$ = trap energy level

   Auger Recombination

$$
R_{\text{Auger}} = (C_n n + C_p p)(np - n_i^2)
$$

- $C_n$, $C_p$ = Auger coefficients (~$10^{-31}$ cm$^6$/s for Si)
- Dominant at high carrier densities ($>10^{18}$ cm$^{-3}$)

   Radiative Recombination

$$
R_{\text{rad}} = B(np - n_i^2)
$$

- $B$ = radiative coefficient
- Important in direct bandgap materials (GaAs, InP)

   4.3 Band-to-Band Tunneling

For tunnel FETs, Zener diodes:

$$
G_{\text{BTBT}} = A \cdot E^2 \exp\left(-\frac{B}{E}\right)
$$

- $A$, $B$ = material-dependent parameters
- $E$ = electric field magnitude

   4.4 Quantum Corrections

   Density Gradient Method

Adds quantum potential to classical equations:

$$
V_Q = -\frac{\hbar^2}{6m^*} \frac{
abla^2\sqrt{n}}{\sqrt{n}}
$$

Or equivalently, the quantum potential term:

$$
\Lambda_n = \frac{\hbar^2}{12 m_n^* k_B T} 
abla^2 \ln(n)
$$

 Applications: 
- Inversion layer quantization in MOSFETs
- Thin body SOI devices
- FinFETs, nanowires

   1D Schrödinger-Poisson

For stronger quantum confinement:
1. Solve 1D Schrödinger in confinement direction → subbands $E_i$, $\psi_i$
2. Calculate 2D density of states
3. Compute carrier density from subband occupation
4. Solve 2D Poisson with quantum charge
5. Iterate to self-consistency

   4.5 Bandgap Narrowing

At high doping ($N > 10^{17}$ cm$^{-3}$):

$$
\Delta E_g = A \cdot N^{1/3} + B \cdot \ln\left(\frac{N}{N_{\text{ref}}}\right)
$$

 Effect:  Increases $n_i^2$ → affects recombination and device characteristics

   4.6 Interface Models

-  Interface trap density:  $D_{it}(E)$ — states per cm$^2$·eV
-  Oxide charges: 
  - Fixed oxide charge $Q_f$
  - Mobile ionic charge $Q_m$
  - Oxide trapped charge $Q_{ot}$
  - Interface trapped charge $Q_{it}$



  5. Process TCAD

   5.1 Ion Implantation

   Monte Carlo Method
- Track individual ion trajectories
- Binary collision approximation
- Accurate for low doses, complex geometries

   Analytical Profiles

 Gaussian: 

$$
N(x) = \frac{\Phi}{\sqrt{2\pi}\Delta R_p} \exp\left[-\frac{(x - R_p)^2}{2\Delta R_p^2}\right]
$$

- $\Phi$ = dose (ions/cm$^2$)
- $R_p$ = projected range
- $\Delta R_p$ = straggle

 Pearson IV:  Adds skewness and kurtosis for better accuracy

   5.2 Diffusion

 Fick's First Law: 

$$
\mathbf{J} = -D 
abla C
$$

 Fick's Second Law: 

$$
\frac{\partial C}{\partial t} = 
abla \cdot (D 
abla C)
$$

 Concentration-dependent diffusion: 

$$
D = D_i \left(\frac{n}{n_i}\right)^2 + D_v + D_x \left(\frac{n}{n_i}\right)
$$

(Accounts for charged point defects)

   5.3 Oxidation

 Deal-Grove Model: 

$$
x_{ox}^2 + A \cdot x_{ox} = B(t + \tau)
$$

- $x_{ox}$ = oxide thickness
- $A$, $B$ = temperature-dependent parameters
- Linear regime: $x_{ox} \approx (B/A) \cdot t$ (thin oxide)
- Parabolic regime: $x_{ox} \approx \sqrt{B \cdot t}$ (thick oxide)

   5.4 Etching and Deposition

 Level-set method  for surface evolution:

$$
\frac{\partial \phi}{\partial t} + v_n |
abla \phi| = 0
$$

- $\phi$ = level-set function (zero contour = surface)
- $v_n$ = normal velocity (etch/deposition rate)



  6. Multiphysics and Advanced Topics

   6.1 Electrothermal Coupling

 Heat equation: 

$$
\rho c_p \frac{\partial T}{\partial t} = 
abla \cdot (\kappa 
abla T) + H
$$

 Heat generation: 

$$
H = \mathbf{J} \cdot \mathbf{E} + (R - G)(E_g + 3k_BT)
$$

- First term: Joule heating
- Second term: recombination heating

 Thermoelectric effects: 
- Seebeck effect
- Peltier effect
- Thomson effect

   6.2 Electromechanical Coupling

 Strain effects on mobility: 

$$
\mu_{\text{strained}} = \mu_0 (1 + \Pi \cdot \sigma)
$$

- $\Pi$ = piezoresistance coefficient
- $\sigma$ = mechanical stress

 Applications:  Strained Si, SiGe channels

   6.3 Statistical Variability

Sources of random variation:
-  Random Dopant Fluctuations (RDF)  — discrete dopant positions
-  Line Edge Roughness (LER)  — gate patterning variation
-  Metal Gate Granularity (MGG)  — work function variation
-  Oxide Thickness Variation (OTV) 

 Simulation approach: 
- Monte Carlo sampling over device instances
- Statistical TCAD → threshold voltage distributions

   6.4 Reliability Modeling

 Bias Temperature Instability (BTI): 
- Defect generation at Si/SiO$_2$ interface
- Reaction-diffusion models

 Hot Carrier Injection (HCI): 
- High-energy carriers damage interface
- Coupled with energy transport

   6.5 Noise Modeling

 Noise sources: 
- Thermal noise: $S_I = 4k_BT/R$
- Shot noise: $S_I = 2qI$
- 1/f noise (flicker): $S_I \propto I^2/(f \cdot N)$

 Impedance field method  for spatial correlation



  7. Computational Architecture

   7.1 Model Hierarchy Comparison

| Level | Physics | Math | Cost | Accuracy |
|-------|---------|------|------|----------|
| NEGF | Quantum coherence | $G = [E-H-\Sigma]^{-1}$ | &#36;&#36;&#36;&#36;&#36; | Highest |
| Monte Carlo | Distribution function | Stochastic DEs | &#36;&#36;&#36;&#36; | High |
| Hydrodynamic | Carrier temperature | Hyperbolic-parabolic PDEs | &#36;&#36;&#36; | Good |
| Drift-Diffusion | Continuum transport | Elliptic-parabolic PDEs | &#36;&#36; | Moderate |
| Compact Models | Empirical | Algebraic | &#36; | Calibrated |

   7.2 Software Architecture

```text
┌─────────────────────────────────────────┐
│           User Interface (GUI)          │
├─────────────────────────────────────────┤
│         Structure Definition            │
│    (Geometry, Mesh, Materials)          │
├─────────────────────────────────────────┤
│          Physical Models                │
│  (Mobility, Recombination, Quantum)     │
├─────────────────────────────────────────┤
│         Numerical Engine                │
│  (Discretization, Solvers, Linear Alg)  │
├─────────────────────────────────────────┤
│        Post-Processing                  │
│   (Visualization, Parameter Extraction) │
└─────────────────────────────────────────┘
```

   7.3 TCAD ↔ Compact Model Flow

```text
┌──────────┐    calibrate    ┌──────────────┐
│   TCAD   │ ──────────────► │ Compact Model│
│(Physics) │                 │   (BSIM,PSP) │
└──────────┘                 └──────────────┘
      │                             │
      │ validate                    │ enable
      ▼                             ▼
┌──────────┐                 ┌──────────────┐
│ Silicon  │                 │   Circuit    │
│   Data   │                 │  Simulation  │
└──────────┘                 └──────────────┘
```



  Equations: 

   Fundamental Constants

| Symbol | Name | Value |
|--------|------|-------|
| $q$ | Elementary charge | $1.602 \times 10^{-19}$ C |
| $k_B$ | Boltzmann constant | $1.381 \times 10^{-23}$ J/K |
| $\hbar$ | Reduced Planck | $1.055 \times 10^{-34}$ J·s |
| $\varepsilon_0$ | Vacuum permittivity | $8.854 \times 10^{-12}$ F/m |
| $V_T$ | Thermal voltage (300K) | 25.9 mV |

   Silicon Properties (300K)

| Property | Value |
|----------|-------|
| Bandgap $E_g$ | 1.12 eV |
| Intrinsic carrier density $n_i$ | $1.0 \times 10^{10}$ cm$^{-3}$ |
| Electron mobility $\mu_n$ | 1450 cm$^2$/V·s |
| Hole mobility $\mu_p$ | 500 cm$^2$/V·s |
| Electron saturation velocity | $1.0 \times 10^7$ cm/s |
| Relative permittivity $\varepsilon_r$ | 11.7 |

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/device-physics-tcad) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
