# Semiconductor Manufacturing Process: Multi-Physics Coupling & Mathematical Modeling

**Keywords**: multi physics coupling, multiphysics modeling, coupled simulation, process simulation, transport phenomena, heat transfer plasma coupling, electromagnetic plasma

---

**Semiconductor Manufacturing Process: Multi-Physics Coupling & Mathematical Modeling**



**1. Overview: Why Multi-Physics Coupling Matters**

Semiconductor fabrication involves hundreds of process steps where multiple physical phenomena occur simultaneously and interact nonlinearly. At the 3nm node and below, these couplings become critical—small perturbations propagate across physics domains, affecting yield, uniformity, and device performance.



**2. Key Processes and Their Coupled Physics**

**2.1 Plasma Etching (RIE, ICP, CCP)**

**Coupled domains:**

- Electromagnetics (RF field, power deposition)
- Plasma kinetics (electron/ion transport, sheath dynamics)
- Neutral gas fluid dynamics
- Gas-phase and surface chemistry
- Heat transfer
- Feature-scale transport and profile evolution

**Coupling chain:**

```
RF Power → EM Fields → Electron Heating → Plasma Density → Sheath Voltage
    ↓                                            ↓
Ion Energy Distribution ← ─────────────────────────┘
    ↓
Surface Bombardment + Radical Flux → Etch Rate & Profile
    ↓
Feature Geometry Evolution → Local Field Modification (feedback)
```

**2.2 Chemical Vapor Deposition (CVD/ALD)**

**Coupled domains:**

- Fluid dynamics (often rarefied/transitional flow)
- Heat transfer (convection, conduction, radiation)
- Multi-component mass transfer
- Gas-phase and surface reaction kinetics
- Film stress evolution

**2.3 Thermal Processing (RTP, Annealing)**

**Coupled domains:**

- Radiation heat transfer
- Solid-state diffusion (dopants)
- Defect kinetics
- Thermo-mechanical stress (slip, warpage)

**2.4 EUV Lithography**

**Coupled domains:**

- Wave optics and diffraction
- Photochemistry in resist
- Stochastic photon/electron effects
- Mask/wafer thermal-mechanical deformation



**3. Mathematical Framework: Governing Equations**

**3.1 Electromagnetics (Plasma Systems)**

For RF-driven plasma, the **time-harmonic Maxwell's equations**:

$$

abla \times \left(\mu_r^{-1} 
abla \times \mathbf{E}\right) - k_0^2 \epsilon_r \mathbf{E} = -j\omega\mu_0 \mathbf{J}_{ext}
$$

The **plasma permittivity** encodes the coupling to electron density:

$$
\epsilon_r = 1 - \frac{\omega_{pe}^2}{\omega(\omega + j
u_m)}
$$

Where the **plasma frequency** is:

$$
\omega_{pe} = \sqrt{\frac{n_e e^2}{m_e \epsilon_0}}
$$

**Key parameters:**

- $n_e$ — electron density
- $e$ — electron charge
- $m_e$ — electron mass
- $\epsilon_0$ — permittivity of free space
- $
u_m$ — electron-neutral collision frequency
- $\omega$ — angular frequency of RF excitation

> **Note:** This creates a **strong nonlinear coupling**: the EM field depends on plasma density, which in turn depends on power absorption from the EM field.

**3.2 Plasma Transport (Drift-Diffusion Approximation)**

**Electron continuity equation:**

$$
\frac{\partial n_e}{\partial t} + 
abla \cdot \boldsymbol{\Gamma}_e = S_e
$$

**Electron flux:**

$$
\boldsymbol{\Gamma}_e = -\mu_e n_e \mathbf{E} - D_e 
abla n_e
$$

**Electron energy density equation:**

$$
\frac{\partial n_\epsilon}{\partial t} + 
abla \cdot \boldsymbol{\Gamma}_\epsilon + \mathbf{E} \cdot \boldsymbol{\Gamma}_e = S_\epsilon - \sum_j \varepsilon_j R_j
$$

**Where:**

- $n_e$ — electron density
- $\boldsymbol{\Gamma}_e$ — electron flux vector
- $\mu_e$ — electron mobility
- $D_e$ — electron diffusion coefficient
- $S_e$ — electron source term (ionization, attachment, recombination)
- $n_\epsilon$ — electron energy density
- $\varepsilon_j$ — energy loss per reaction $j$
- $R_j$ — reaction rate for process $j$

**Ion transport** (for multiple species $i$):

$$
\frac{\partial n_i}{\partial t} + 
abla \cdot \boldsymbol{\Gamma}_i = S_i
$$

**3.3 Neutral Gas Flow (Navier-Stokes Equations)**

**Continuity equation:**

$$
\frac{\partial \rho}{\partial t} + 
abla \cdot (\rho \mathbf{u}) = 0
$$

**Momentum equation:**

$$
\rho \frac{D\mathbf{u}}{Dt} = -
abla p + 
abla \cdot \boldsymbol{\tau} + \mathbf{F}_{body}
$$

**Where:**

- $\rho$ — gas density
- $\mathbf{u}$ — velocity vector
- $p$ — pressure
- $\boldsymbol{\tau}$ — viscous stress tensor
- $\mathbf{F}_{body}$ — body forces

**Low-pressure corrections (Knudsen effects):**

At low pressures where Knudsen number $Kn = \lambda/L > 0.01$, slip boundary conditions are required:

$$
u_{slip} = \frac{2-\sigma}{\sigma} \lambda \left.\frac{\partial u}{\partial n}\right|_{wall}
$$

Where:

- $\lambda$ — mean free path
- $L$ — characteristic length
- $\sigma$ — tangential momentum accommodation coefficient

**3.4 Species Transport and Chemistry**

**Convection-diffusion-reaction equation:**

$$
\frac{\partial c_k}{\partial t} + 
abla \cdot (c_k \mathbf{u}) = 
abla \cdot (D_k 
abla c_k) + R_k
$$

**Gas-phase reaction rates:**

$$
R_k = \sum_j 
u_{kj} \, k_j(T) \prod_l c_l^{a_{lj}}
$$

**Where:**

- $c_k$ — concentration of species $k$
- $D_k$ — diffusion coefficient
- $R_k$ — net production rate
- $
u_{kj}$ — stoichiometric coefficient
- $k_j(T)$ — temperature-dependent rate constant
- $a_{lj}$ — reaction order

**Surface reactions (Langmuir-Hinshelwood kinetics):**

$$
r_s = k_s \theta_A \theta_B
$$

**Surface coverage:**

$$
\theta_i = \frac{K_i c_i}{1 + \sum_j K_j c_j}
$$

**3.5 Heat Transfer**

**Energy equation:**

$$
\rho c_p \frac{\partial T}{\partial t} + \rho c_p \mathbf{u} \cdot 
abla T = 
abla \cdot (k 
abla T) + Q
$$

**Heat sources in plasma systems:**

$$
Q = Q_{Joule} + Q_{ion} + Q_{reaction} + Q_{radiation}
$$

**Joule heating (time-averaged):**

$$
Q_{Joule} = \frac{1}{2} \text{Re}(\mathbf{J}^* \cdot \mathbf{E})
$$

**Where:**

- $\rho$ — density
- $c_p$ — specific heat capacity
- $k$ — thermal conductivity
- $Q$ — volumetric heat source
- $\mathbf{J}^*$ — complex conjugate of current density

**3.6 Solid Mechanics (Film Stress)**

**Equilibrium equation:**

$$

abla \cdot \boldsymbol{\sigma} = 0
$$

**Constitutive relation with thermal strain:**

$$
\boldsymbol{\sigma} = \mathbf{C} : (\boldsymbol{\epsilon} - \boldsymbol{\epsilon}_{th} - \boldsymbol{\epsilon}_{intrinsic})
$$

**Thermal strain tensor:**

$$
\boldsymbol{\epsilon}_{th} = \alpha(T - T_0)\mathbf{I}
$$

**Where:**

- $\boldsymbol{\sigma}$ — stress tensor
- $\mathbf{C}$ — stiffness tensor
- $\boldsymbol{\epsilon}$ — total strain tensor
- $\alpha$ — coefficient of thermal expansion
- $T_0$ — reference temperature
- $\mathbf{I}$ — identity tensor

**Stoney equation** (wafer curvature from film stress):

$$
\sigma_f = \frac{E_s h_s^2}{6(1-
u_s)h_f}\kappa
$$

**Where:**

- $\sigma_f$ — film stress
- $E_s$ — substrate Young's modulus
- $
u_s$ — substrate Poisson's ratio
- $h_s$ — substrate thickness
- $h_f$ — film thickness
- $\kappa$ — wafer curvature



**4. Feature-Scale Modeling**

At the nanometer scale within etched features, continuum assumptions break down.

**4.1 Profile Evolution (Level Set Method)**

The etch front $\phi(\mathbf{x},t) = 0$ evolves according to:

$$
\frac{\partial \phi}{\partial t} + V_n |
abla \phi| = 0
$$

**Local etch rate** depends on coupled physics:

$$
V_n = \Gamma_{ion}(E,\theta) \cdot Y_{phys}(E,\theta) + \Gamma_{rad} \cdot Y_{chem}(T) + \Gamma_{ion} \cdot \Gamma_{rad} \cdot Y_{synergy}
$$

**Where:**

- $\phi$ — level set function (zero at interface)
- $V_n$ — normal velocity of interface
- $\Gamma_{ion}$ — ion flux (from sheath model)
- $\Gamma_{rad}$ — radical flux (from feature-scale transport)
- $Y_{phys}$ — physical sputtering yield
- $Y_{chem}$ — chemical etch yield
- $Y_{synergy}$ — ion-enhanced chemical yield
- $\theta$ — local incidence angle
- $E$ — ion energy

**4.2 Feature-Scale Transport**

Within high-aspect-ratio features, **Knudsen diffusion** dominates:

$$
D_{Kn} = \frac{d}{3}\sqrt{\frac{8k_BT}{\pi m}}
$$

**Where:**

- $d$ — feature diameter/width
- $k_B$ — Boltzmann constant
- $T$ — temperature
- $m$ — molecular mass

**View factor calculations** for flux at the bottom of features:

$$
\Gamma_{bottom} = \Gamma_{top} \cdot \int_{\Omega} f(\theta) \cos\theta \, d\Omega
$$

**4.3 Ion Angular and Energy Distribution**

At the sheath-feature interface:

$$
f(E, \theta) = f_E(E) \cdot f_\theta(\theta)
$$

**Angular distribution** (from sheath collisionality):

$$
f_\theta(\theta) \propto \cos^n(\theta) \exp\left(-\frac{\theta^2}{2\sigma_\theta^2}\right)
$$

**Where:**

- $f_E(E)$ — ion energy distribution function
- $f_\theta(\theta)$ — ion angular distribution function
- $n$ — exponent (depends on sheath collisionality)
- $\sigma_\theta$ — angular spread parameter



**5. Multi-Scale Coupling Strategy**

```
┌─────────────────────────────────────────────────────────────┐
│                    REACTOR SCALE (cm–m)                     │
│    Continuum: Navier-Stokes, Maxwell, Drift-Diffusion       │
│    Methods: FEM, FVM                                        │
└─────────────────────┬───────────────────────────────────────┘
                      │ Boundary fluxes, plasma parameters
                      ▼
┌─────────────────────────────────────────────────────────────┐
│                    FEATURE SCALE (nm–μm)                    │
│    Kinetic transport: DSMC, Angular distribution            │
│    Profile evolution: Level set, Cell-based methods         │
└─────────────────────┬───────────────────────────────────────┘
                      │ Sticking coefficients, reaction rates
                      ▼
┌─────────────────────────────────────────────────────────────┐
│                    ATOMIC SCALE (Å–nm)                      │
│    DFT: Reaction barriers, surface energies                 │
│    MD: Sputtering yields, sticking probabilities            │
│    KMC: Surface evolution, roughness                        │
└─────────────────────────────────────────────────────────────┘
```

**Scale hierarchy:**

1. **Reactor scale (cm–m)**
   - Continuum fluid dynamics
   - Maxwell's equations for EM fields
   - Drift-diffusion for charged species
   - Numerical methods: FEM, FVM

2. **Feature scale (nm–μm)**
   - Knudsen transport in high-aspect-ratio structures
   - Direct Simulation Monte Carlo (DSMC)
   - Level set methods for profile evolution

3. **Atomic scale (Å–nm)**
   - Density Functional Theory (DFT) for reaction barriers
   - Molecular Dynamics (MD) for sputtering yields
   - Kinetic Monte Carlo (KMC) for surface evolution



**6. Coupled System Structure**

The full system can be written abstractly as:

$$
\mathbf{M}(\mathbf{u})\frac{\partial \mathbf{u}}{\partial t} = \mathbf{F}(\mathbf{u}, 
abla\mathbf{u}, 
abla^2\mathbf{u}, t)
$$

**State vector:**

$$
\mathbf{u} = \begin{bmatrix} n_e \\ n_\epsilon \\ n_{i,k} \\ c_j \\ T \\ \mathbf{E} \\ \mathbf{u}_{gas} \\ p \\ \boldsymbol{\sigma} \\ \phi_{profile} \\ \vdots \end{bmatrix}
$$

**Jacobian structure reveals coupling:**

$$
\mathbf{J} = \frac{\partial \mathbf{F}}{\partial \mathbf{u}} = \begin{pmatrix} 
J_{ee} & J_{e\epsilon} & J_{ei} & J_{ec} & \cdots \\
J_{\epsilon e} & J_{\epsilon\epsilon} & J_{\epsilon i} & & \\
J_{ie} & J_{i\epsilon} & J_{ii} & & \\
J_{ce} & & & J_{cc} & \\
\vdots & & & & \ddots 
\end{pmatrix}
$$

**Off-diagonal blocks** represent inter-physics coupling strengths.



**7. Numerical Solution Strategies**

**7.1 Coupling Approaches**

**Monolithic (fully coupled):**

- Solve all physics simultaneously
- Newton iteration on full Jacobian
- Robust but computationally expensive
- Required for strongly coupled physics (plasma + EM)

**Partitioned (sequential):**

- Solve each physics domain separately
- Iterate between domains until convergence
- More efficient for weakly coupled physics
- Risk of convergence issues

**Hybrid approach:**

- Group strongly coupled physics into blocks
- Sequential coupling between blocks

**7.2 Spatial Discretization**

**Finite Element Method (FEM)** — weak form for species transport:

$$
\int_\Omega w \frac{\partial c}{\partial t} \, d\Omega + \int_\Omega w (\mathbf{u} \cdot 
abla c) \, d\Omega + \int_\Omega 
abla w \cdot (D
abla c) \, d\Omega = \int_\Omega w R \, d\Omega
$$

**SUPG Stabilization** for convection-dominated problems:

$$
w \rightarrow w + \tau_{SUPG} \, \mathbf{u} \cdot 
abla w
$$

**Where:**

- $w$ — test function
- $c$ — concentration field
- $\tau_{SUPG}$ — stabilization parameter

**7.3 Time Integration**

**Stiff systems** require implicit methods:

- **BDF** (Backward Differentiation Formulas)
- **ESDIRK** (Explicit Singly Diagonally Implicit Runge-Kutta)

**Operator splitting** for multi-physics:

$$
\mathbf{u}^{n+1} = \mathcal{L}_1(\Delta t) \circ \mathcal{L}_2(\Delta t) \circ \mathcal{L}_3(\Delta t) \, \mathbf{u}^n
$$

**Where:**

- $\mathcal{L}_i$ — solution operator for physics domain $i$
- $\Delta t$ — time step
- $\circ$ — composition of operators



**8. Specific Application: ICP Etch Model**

**Complete coupled system summary:**

| Physics Domain | Governing Equations | Key Coupling Variables |
|----------------|---------------------|------------------------|
| EM (inductive) | $
abla \times (
abla \times \mathbf{E}) + k^2\epsilon_p \mathbf{E} = 0$ | $n_e \rightarrow \epsilon_p$ |
| Electron transport | $
abla \cdot \Gamma_e = S_e$ | $\mathbf{E}_{dc}, n_e, T_e$ |
| Electron energy | $
abla \cdot \Gamma_\epsilon = Q_{EM} - Q_{loss}$ | $T_e \rightarrow$ rate coefficients |
| Ion transport | $
abla \cdot \Gamma_i = S_i$ | $n_e, \mathbf{E}_{dc}$ |
| Neutral chemistry | $
abla \cdot (c_k \mathbf{u} - D_k
abla c_k) = R_k$ | $T_e \rightarrow k_{diss}$ |
| Gas flow | Navier-Stokes | $T_{gas}$ |
| Heat transfer | $
abla \cdot (k
abla T) + Q = 0$ | $Q_{plasma}$ |
| Sheath | Child-Langmuir / PIC | $n_e, T_e, V_{dc}$ |
| Feature transport | Knudsen + angular | $\Gamma_{ion}, \Gamma_{rad}$ from reactor |
| Profile evolution | Level set | $V_n$ from surface kinetics |



**9. EUV Lithography: Stochastic Multi-Physics**

At EUV wavelength (13.5 nm), photon shot noise becomes significant.

**9.1 Aerial Image Formation**

$$
I(\mathbf{r}) = \left|\mathcal{F}^{-1}\left[\tilde{M}(\mathbf{f}) \cdot H(\mathbf{f})\right]\right|^2
$$

**Where:**

- $I(\mathbf{r})$ — intensity at position $\mathbf{r}$
- $\tilde{M}(\mathbf{f})$ — mask spectrum (Fourier transform of mask pattern)
- $H(\mathbf{f})$ — pupil function (includes aberrations, partial coherence)
- $\mathcal{F}^{-1}$ — inverse Fourier transform

**9.2 Photon Statistics**

$$
N \sim \text{Poisson}(\bar{N})
$$

$$
\sigma_N = \sqrt{\bar{N}}
$$

**Where:**

- $N$ — number of photons absorbed
- $\bar{N}$ — expected number of photons
- $\sigma_N$ — standard deviation (shot noise)

**9.3 Resist Exposure (Stochastic Dill Model)**

$$
\frac{\partial [PAG]}{\partial t} = -C \cdot I \cdot [PAG] + \xi(t)
$$

**Where:**

- $[PAG]$ — photoactive compound concentration
- $C$ — exposure rate constant
- $I$ — local intensity
- $\xi(t)$ — stochastic noise term

**9.4 Line Edge Roughness (LER)**

$$
\sigma_{LER} \propto \sqrt{\frac{1}{\text{dose}}} \cdot \frac{1}{\text{image contrast}}
$$

> **Note:** This requires **Kinetic Monte Carlo** or **Gillespie algorithm** rather than continuum PDEs.



**10. Process Optimization (Inverse Problem)**

**10.1 Problem Formulation**

**Objective:** Minimize profile deviation from target

$$
\min_{\mathbf{p}} J = \int_\Gamma \left|\phi(\mathbf{x}; \mathbf{p}) - \phi_{target}\right|^2 \, d\Gamma
$$

**Subject to physics constraints:**

$$
\mathbf{F}(\mathbf{u}, \mathbf{p}) = 0
$$

**Control parameters** $\mathbf{p}$:

- RF power
- Chamber pressure
- Gas flow rates
- Substrate temperature
- Process time

**10.2 Adjoint Method for Efficient Gradients**

**Gradient computation:**

$$
\frac{dJ}{d\mathbf{p}} = \frac{\partial J}{\partial \mathbf{p}} - \boldsymbol{\lambda}^T \frac{\partial \mathbf{F}}{\partial \mathbf{p}}
$$

**Adjoint equation:**

$$
\left(\frac{\partial \mathbf{F}}{\partial \mathbf{u}}\right)^T \boldsymbol{\lambda} = \left(\frac{\partial J}{\partial \mathbf{u}}\right)^T
$$

**Where:**

- $\boldsymbol{\lambda}$ — adjoint variable (Lagrange multiplier)
- $\mathbf{u}$ — state variables
- $\mathbf{p}$ — control parameters



**11. Emerging Approaches**

**11.1 Physics-Informed Neural Networks (PINNs)**

**Loss function:**

$$
\mathcal{L} = \mathcal{L}_{data} + \lambda \mathcal{L}_{PDE}
$$

**Where:**

- $\mathcal{L}_{data}$ — data fitting loss
- $\mathcal{L}_{PDE}$ — PDE residual loss at collocation points
- $\lambda$ — regularization parameter

**11.2 Digital Twins**

**Key features:**

- Real-time reduced-order models calibrated to equipment sensors
- Combine physics-based models with ML for fast prediction
- Enable predictive maintenance and process control

**11.3 Uncertainty Quantification**

**Methods:**

- **Polynomial Chaos Expansion (PCE)** — for parametric uncertainty propagation
- **Bayesian Inference** — for model calibration with experimental data
- **Monte Carlo Sampling** — for statistical analysis of outputs



**12. Mathematical Structure**

The semiconductor manufacturing multi-physics problem has a characteristic mathematical structure:

1. **Hierarchy of scales** (atomic → feature → reactor)
   - Requires multi-scale methods
   - Information passing between scales via homogenization

2. **Nonlinear coupling** between physics domains
   - Varying coupling strengths
   - Both explicit and implicit dependencies

3. **Stiff ODEs/DAEs**
   - Disparate time scales (electron dynamics ~ ns, thermal ~ s)
   - Requires implicit time integration

4. **Moving boundaries**
   - Etch/deposition fronts
   - Requires interface tracking (level set, phase field)

5. **Rarefied gas effects**
   - At low pressures ($Kn > 0.01$)
   - Requires kinetic corrections or DSMC

6. **Stochastic effects**
   - At nanometer scales (EUV, atomic-scale roughness)
   - Requires Monte Carlo methods



**Key Physical Constants**

| Symbol | Value | Description |
|--------|-------|-------------|
| $e$ | $1.602 \times 10^{-19}$ C | Elementary charge |
| $m_e$ | $9.109 \times 10^{-31}$ kg | Electron mass |
| $\epsilon_0$ | $8.854 \times 10^{-12}$ F/m | Permittivity of free space |
| $\mu_0$ | $4\pi \times 10^{-7}$ H/m | Permeability of free space |
| $k_B$ | $1.381 \times 10^{-23}$ J/K | Boltzmann constant |
| $N_A$ | $6.022 \times 10^{23}$ mol$^{-1}$ | Avogadro's number |



**Common Dimensionless Numbers**

| Number | Definition | Physical Meaning |
|--------|------------|------------------|
| Knudsen ($Kn$) | $\lambda / L$ | Mean free path / characteristic length |
| Reynolds ($Re$) | $\rho u L / \mu$ | Inertia / viscous forces |
| Péclet ($Pe$) | $u L / D$ | Convection / diffusion |
| Damköhler ($Da$) | $k L / u$ | Reaction / convection rate |
| Biot ($Bi$) | $h L / k$ | Surface / bulk heat transfer |

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/multi-physics-coupling) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
