# Mathematical Modeling of Metal Deposition in Semiconductor Manufacturing

**Keywords**: metal deposition, CVD, PVD, ALD, sputtering, electroplating, copper

---

**Mathematical Modeling of Metal Deposition in Semiconductor Manufacturing**



**1. Overview: Metal Deposition Processes**

Metal deposition is a critical step in semiconductor fabrication, creating interconnects, contacts, barrier layers, and various metallic structures. The primary deposition methods require distinct mathematical treatments:

| Process | Physics Domain | Key Mathematics |
|---------|----------------|-----------------|
| **PVD (Sputtering)** | Ballistic transport, plasma physics | Boltzmann transport, Monte Carlo |
| **CVD/PECVD** | Gas-phase transport, surface reactions | Navier-Stokes, reaction-diffusion |
| **ALD** | Self-limiting surface chemistry | Site-balance kinetics |
| **Electroplating (ECD)** | Electrochemistry, mass transport | Butler-Volmer, Nernst-Planck |



**2. Transport Phenomena Models**

**2.1 Gas-Phase Transport (CVD/PECVD)**

The precursor concentration field follows the **convection-diffusion-reaction equation**:

$$
\frac{\partial C}{\partial t} + \mathbf{v} \cdot 
abla C = D 
abla^2 C + R_{gas}
$$

Where:

- $C$ — precursor concentration (mol/m³)
- $\mathbf{v}$ — velocity field vector (m/s)
- $D$ — diffusion coefficient (m²/s)
- $R_{gas}$ — gas-phase reaction source term (mol/m³$\cdot$s)

**2.2 Flow Field Equations**

The **incompressible Navier-Stokes equations** govern the velocity field:

$$
\rho \left( \frac{\partial \mathbf{v}}{\partial t} + \mathbf{v} \cdot 
abla \mathbf{v} \right) = -
abla p + \mu 
abla^2 \mathbf{v}
$$

With continuity equation:

$$

abla \cdot \mathbf{v} = 0
$$

Where:

- $\rho$ — gas density (kg/m³)
- $p$ — pressure (Pa)
- $\mu$ — dynamic viscosity (Pa$\cdot$s)

**2.3 Knudsen Number and Transport Regimes**

At low pressures, the **Knudsen number** determines the transport regime:

$$
Kn = \frac{\lambda}{L} = \frac{k_B T}{\sqrt{2} \pi d^2 p L}
$$

Where:

- $\lambda$ — mean free path (m)
- $L$ — characteristic length (m)
- $k_B$ — Boltzmann constant ($1.38 \times 10^{-23}$ J/K)
- $T$ — temperature (K)
- $d$ — molecular diameter (m)
- $p$ — pressure (Pa)

**Transport regime classification:**

- $Kn < 0.01$ — **Continuum regime** → Navier-Stokes CFD
- $0.01 < Kn < 0.1$ — **Slip flow regime** → Modified NS with slip boundary conditions
- $0.1 < Kn < 10$ — **Transitional regime** → DSMC, Boltzmann equation
- $Kn > 10$ — **Free molecular regime** → Ballistic/Monte Carlo methods



**3. Surface Reaction Kinetics**

**3.1 Langmuir-Hinshelwood Mechanism**

For bimolecular surface reactions (common in CVD):

$$
r = \frac{k \cdot K_A K_B \cdot p_A p_B}{(1 + K_A p_A + K_B p_B)^2}
$$

Where:

- $r$ — reaction rate (mol/m²$\cdot$s)
- $k$ — surface reaction rate constant (mol/m²$\cdot$s)
- $K_A, K_B$ — adsorption equilibrium constants (Pa⁻¹)
- $p_A, p_B$ — partial pressures of reactants A and B (Pa)

**3.2 Sticking Coefficient Model**

The probability that an impinging molecule adsorbs on the surface:

$$
S = S_0 \exp\left( -\frac{E_a}{k_B T} \right) \cdot f(\theta)
$$

Where:

- $S$ — sticking coefficient (dimensionless)
- $S_0$ — pre-exponential sticking factor
- $E_a$ — activation energy (J)
- $f(\theta) = (1 - \theta)^n$ — site blocking function
- $\theta$ — surface coverage (dimensionless, 0 to 1)
- $n$ — order of site blocking

**3.3 Arrhenius Temperature Dependence**

$$
k(T) = A \exp\left( -\frac{E_a}{RT} \right)
$$

Where:

- $A$ — pre-exponential factor (frequency factor)
- $E_a$ — activation energy (J/mol)
- $R$ — universal gas constant (8.314 J/mol$\cdot$K)
- $T$ — absolute temperature (K)



**4. Film Growth Models**

**4.1 Continuum Surface Evolution**

**Edwards-Wilkinson Equation (Linear Growth)**

$$
\frac{\partial h}{\partial t} = 
u 
abla^2 h + F + \eta(\mathbf{x}, t)
$$

**Kardar-Parisi-Zhang (KPZ) Equation (Nonlinear Growth)**

$$
\frac{\partial h}{\partial t} = 
u 
abla^2 h + \frac{\lambda}{2} |
abla h|^2 + F + \eta
$$

Where:

- $h(\mathbf{x}, t)$ — surface height at position $\mathbf{x}$ and time $t$
- $
u$ — surface diffusion coefficient (m²/s)
- $\lambda$ — nonlinear growth parameter
- $F$ — mean deposition flux (m/s)
- $\eta$ — stochastic noise term (Gaussian white noise)

**4.2 Scaling Relations**

Surface roughness evolves according to:

$$
W(L, t) = L^\alpha f\left( \frac{t}{L^z} \right)
$$

Where:

- $W$ — interface width (roughness)
- $L$ — system size
- $\alpha$ — roughness exponent
- $z$ — dynamic exponent
- $f$ — scaling function



**5. Step Coverage and Conformality**

**5.1 Thiele Modulus**

For high-aspect-ratio features, the **Thiele modulus** determines conformality:

$$
\phi = L \sqrt{\frac{k_s}{D_{eff}}}
$$

Where:

- $\phi$ — Thiele modulus (dimensionless)
- $L$ — feature depth (m)
- $k_s$ — surface reaction rate constant (m/s)
- $D_{eff}$ — effective diffusivity (m²/s)

**Step coverage regimes:**

- $\phi \ll 1$ — **Reaction-limited** → Excellent conformality
- $\phi \gg 1$ — **Transport-limited** → Poor step coverage (bread-loafing)

**5.2 Knudsen Diffusion in Trenches**

$$
D_K = \frac{w}{3} \sqrt{\frac{8 R T}{\pi M}}
$$

Where:

- $D_K$ — Knudsen diffusion coefficient (m²/s)
- $w$ — trench width (m)
- $R$ — universal gas constant (J/mol$\cdot$K)
- $T$ — temperature (K)
- $M$ — molecular weight (kg/mol)

**5.3 Feature-Scale Concentration Profile**

Solving for concentration in a trench with reactive walls:

$$
D_{eff} \frac{d^2 C}{dy^2} = \frac{2 k_s C}{w}
$$

General solution:

$$
C(y) = C_0 \frac{\cosh\left( \phi \frac{L - y}{L} \right)}{\cosh(\phi)}
$$



**6. Atomic Layer Deposition (ALD) Models**

**6.1 Self-Limiting Surface Kinetics**

Surface site balance equation:

$$
\frac{d\theta}{dt} = k_a C (1 - \theta) - k_d \theta
$$

Where:

- $\theta$ — fractional surface coverage
- $k_a$ — adsorption rate constant (m³/mol$\cdot$s)
- $k_d$ — desorption rate constant (s⁻¹)
- $C$ — gas-phase precursor concentration (mol/m³)

At equilibrium saturation:

$$
\theta_{eq} = \frac{k_a C}{k_a C + k_d} \approx 1 \quad \text{(for strong chemisorption)}
$$

**6.2 Growth Per Cycle (GPC)**

$$
\text{GPC} = \Gamma_0 \cdot \Omega \cdot \eta
$$

Where:

- $\Gamma_0$ — surface site density (sites/m²)
- $\Omega$ — volume per deposited atom (m³)
- $\eta$ — reaction efficiency (dimensionless)

**6.3 Saturation Dose-Time Relationship**

$$
\theta(t) = 1 - \exp\left( -\frac{S \cdot \Phi \cdot t}{\Gamma_0} \right)
$$

**Impingement flux** from kinetic theory:

$$
\Phi = \frac{p}{\sqrt{2 \pi m k_B T}}
$$

Where:

- $\Phi$ — molecular impingement flux (molecules/m²$\cdot$s)
- $p$ — precursor partial pressure (Pa)
- $m$ — molecular mass (kg)



**7. Plasma Modeling (PVD/PECVD)**

**7.1 Plasma Sheath Physics**

**Child-Langmuir law** for ion current density:

$$
J_{ion} = \frac{4 \varepsilon_0}{9} \sqrt{\frac{2e}{M_i}} \frac{V_s^{3/2}}{d_s^2}
$$

Where:

- $J_{ion}$ — ion current density (A/m²)
- $\varepsilon_0$ — vacuum permittivity ($8.85 \times 10^{-12}$ F/m)
- $e$ — elementary charge ($1.6 \times 10^{-19}$ C)
- $M_i$ — ion mass (kg)
- $V_s$ — sheath voltage (V)
- $d_s$ — sheath thickness (m)

**7.2 Ion Energy at Substrate**

$$
\varepsilon_{ion} \approx e V_s + \frac{1}{2} M_i v_{Bohm}^2
$$

**Bohm velocity:**

$$
v_{Bohm} = \sqrt{\frac{k_B T_e}{M_i}}
$$

Where:

- $T_e$ — electron temperature (K or eV)

**7.3 Sputtering Yield (Sigmund Formula)**

$$
Y(E) = \frac{3 \alpha}{4 \pi^2} \cdot \frac{4 M_1 M_2}{(M_1 + M_2)^2} \cdot \frac{E}{U_0}
$$

Where:

- $Y$ — sputtering yield (atoms/ion)
- $\alpha$ — dimensionless factor (~0.2–0.4)
- $M_1$ — incident ion mass
- $M_2$ — target atom mass
- $E$ — incident ion energy (eV)
- $U_0$ — surface binding energy (eV)

**7.4 Electron Energy Distribution Function (EEDF)**

The Boltzmann equation in energy space:

$$
\frac{\partial f}{\partial t} + \mathbf{v} \cdot 
abla f + \frac{e \mathbf{E}}{m_e} \cdot 
abla_v f = C[f]
$$

Where:

- $f$ — electron energy distribution function
- $\mathbf{E}$ — electric field
- $m_e$ — electron mass
- $C[f]$ — collision integral



**8. MDP: Markov Decision Process for Process Control**

**8.1 MDP Formulation**

A Markov Decision Process is defined by the tuple:

$$
\mathcal{M} = (S, A, P, R, \gamma)
$$

**Components in semiconductor context:**

- **State space $S$**: Film thickness, resistivity, uniformity, equipment state, wafer position
- **Action space $A$**: Temperature, pressure, flow rates, RF power, deposition time
- **Transition probability $P(s' | s, a)$**: Stochastic process model
- **Reward function $R(s, a)$**: Yield, uniformity, throughput, quality metrics
- **Discount factor $\gamma$**: Time preference (typically 0.9–0.99)

**8.2 Bellman Optimality Equation**

$$
V^*(s) = \max_{a \in A} \left[ R(s, a) + \gamma \sum_{s'} P(s' | s, a) V^*(s') \right]
$$

**Q-function formulation:**

$$
Q^*(s, a) = R(s, a) + \gamma \sum_{s'} P(s' | s, a) \max_{a'} Q^*(s', a')
$$

**8.3 Run-to-Run (R2R) Control**

Optimal recipe adjustment after each wafer:

$$
\mathbf{u}_{k+1} = \mathbf{u}_k + \mathbf{K} (\mathbf{y}_{target} - \mathbf{y}_k)
$$

Where:

- $\mathbf{u}_k$ — process recipe parameters at run $k$
- $\mathbf{y}_k$ — measured output at run $k$
- $\mathbf{K}$ — controller gain matrix (from MDP policy optimization)

**8.4 Reinforcement Learning Approaches**

| Method | Application | Characteristics |
|--------|-------------|-----------------|
| **Q-Learning** | Discrete parameter optimization | Model-free, tabular |
| **Deep Q-Network (DQN)** | High-dimensional state spaces | Neural network approximation |
| **Policy Gradient** | Continuous process control | Direct policy optimization |
| **Actor-Critic (A2C/PPO)** | Complex control tasks | Combined value and policy |
| **Model-Based RL** | Physics-informed control | Sample efficient |



**9. Electrochemical Deposition (Copper Damascene)**

**9.1 Butler-Volmer Equation**

$$
i = i_0 \left[ \exp\left( \frac{\alpha_a F \eta}{RT} \right) - \exp\left( -\frac{\alpha_c F \eta}{RT} \right) \right]
$$

Where:

- $i$ — current density (A/m²)
- $i_0$ — exchange current density (A/m²)
- $\alpha_a, \alpha_c$ — anodic and cathodic transfer coefficients
- $F$ — Faraday constant (96,485 C/mol)
- $\eta = E - E_{eq}$ — overpotential (V)
- $R$ — gas constant (J/mol$\cdot$K)
- $T$ — temperature (K)

**9.2 Mass Transport Limited Current**

$$
i_L = \frac{n F D C_b}{\delta}
$$

Where:

- $i_L$ — limiting current density (A/m²)
- $n$ — number of electrons transferred
- $D$ — diffusion coefficient of Cu²⁺ (m²/s)
- $C_b$ — bulk concentration (mol/m³)
- $\delta$ — diffusion layer thickness (m)

**9.3 Nernst-Planck Equation**

$$
\mathbf{J}_i = -D_i 
abla C_i - \frac{z_i F D_i}{RT} C_i 
abla \phi + C_i \mathbf{v}
$$

Where:

- $\mathbf{J}_i$ — flux of species $i$
- $z_i$ — charge number
- $\phi$ — electric potential

**9.4 Superfilling (Bottom-Up Fill)**

The curvature-enhanced accelerator mechanism:

$$
v_n = v_0 (1 + \kappa \cdot \Gamma_{acc})
$$

Where:

- $v_n$ — local growth velocity normal to surface
- $v_0$ — baseline growth velocity
- $\kappa$ — local surface curvature (1/m)
- $\Gamma_{acc}$ — accelerator surface concentration



**10. Multiscale Modeling Framework**

**10.1 Hierarchical Scale Integration**

```
-
┌──────────────────────────────────────────────────────────────┐
│                     REACTOR SCALE                            │
│            CFD: Flow, temperature, concentration             │
│                   Time: seconds | Length: cm                 │
└─────────────────────────┬────────────────────────────────────┘
                          │ Boundary fluxes
                          ▼
┌──────────────────────────────────────────────────────────────┐
│                     FEATURE SCALE                            │
│        Level-set / String method for surface evolution       │
│                   Time: seconds | Length: $\mu$m             │
└─────────────────────────┬────────────────────────────────────┘
                          │ Local rates
                          ▼
┌──────────────────────────────────────────────────────────────┐
│                    MESOSCALE (kMC)                           │
│          Kinetic Monte Carlo: nucleation, island growth      │
│                    Time: ms | Length: nm                     │
└─────────────────────────┬────────────────────────────────────┘
                          │ Rate parameters
                          ▼
┌──────────────────────────────────────────────────────────────┐
│                  ATOMISTIC (MD/DFT)                          │
│       Molecular dynamics, ab initio: binding energies,       │
│              diffusion barriers, reaction paths              │
│                    Time: ps | Length: Å                      │
└──────────────────────────────────────────────────────────────┘
```

**10.2 Kinetic Monte Carlo (kMC)**

Event rate from transition state theory:

$$
k_i = 
u_0 \exp\left( -\frac{E_{a,i}}{k_B T} \right)
$$

Total rate and time step:

$$
k_{total} = \sum_i k_i, \quad \Delta t = -\frac{\ln(r)}{k_{total}}
$$

Where $r \in (0, 1]$ is a uniform random number.

**10.3 Molecular Dynamics**

Newton's equations of motion:

$$
m_i \frac{d^2 \mathbf{r}_i}{dt^2} = -
abla_i U(\mathbf{r}_1, \mathbf{r}_2, \ldots, \mathbf{r}_N)
$$

**Lennard-Jones potential:**

$$
U_{LJ}(r) = 4\varepsilon \left[ \left( \frac{\sigma}{r} \right)^{12} - \left( \frac{\sigma}{r} \right)^6 \right]
$$

**Embedded Atom Method (EAM) for metals:**

$$
U = \sum_i F_i(\rho_i) + \frac{1}{2} \sum_{i 
eq j} \phi_{ij}(r_{ij})
$$

Where $\rho_i = \sum_{j 
eq i} f_j(r_{ij})$ is the electron density at atom $i$.



**11. Uniformity Modeling**

**11.1 Wafer-Scale Thickness Distribution (Sputtering)**

For a circular magnetron target:

$$
t(r) = \int_{target} \frac{Y \cdot J_{ion} \cdot \cos\theta_t \cdot \cos\theta_w}{\pi R^2} \, dA
$$

Where:

- $t(r)$ — thickness at radial position $r$
- $\theta_t$ — emission angle from target
- $\theta_w$ — incidence angle at wafer

**11.2 Uniformity Metrics**

**Within-Wafer Uniformity (WIW):**

$$
\sigma_{WIW} = \frac{1}{\bar{t}} \sqrt{\frac{1}{N} \sum_{i=1}^{N} (t_i - \bar{t})^2} \times 100\%
$$

**Wafer-to-Wafer Uniformity (WTW):**

$$
\sigma_{WTW} = \frac{1}{\bar{t}_{avg}} \sqrt{\frac{1}{M} \sum_{j=1}^{M} (\bar{t}_j - \bar{t}_{avg})^2} \times 100\%
$$

**Target specifications:**

- $\sigma_{WIW} < 1\%$ for advanced nodes (≤7 nm)
- $\sigma_{WTW} < 0.5\%$ for high-volume manufacturing



**12. Virtual Metrology and Statistical Models**

**12.1 Gaussian Process Regression (GPR)**

$$
f(\mathbf{x}) \sim \mathcal{GP}(m(\mathbf{x}), k(\mathbf{x}, \mathbf{x}'))
$$

**Squared exponential (RBF) kernel:**

$$
k(\mathbf{x}, \mathbf{x}') = \sigma_f^2 \exp\left( -\frac{|\mathbf{x} - \mathbf{x}'|^2}{2\ell^2} \right)
$$

**Predictive distribution:**

$$
f_* | \mathbf{X}, \mathbf{y}, \mathbf{x}_* \sim \mathcal{N}(\bar{f}_*, \text{var}(f_*))
$$

**12.2 Partial Least Squares (PLS)**

$$
\mathbf{Y} = \mathbf{X} \mathbf{B} + \mathbf{E}
$$

Where:

- $\mathbf{X}$ — process parameter matrix
- $\mathbf{Y}$ — quality outcome matrix
- $\mathbf{B}$ — regression coefficient matrix
- $\mathbf{E}$ — residual matrix

**12.3 Principal Component Analysis (PCA)**

$$
\mathbf{X} = \mathbf{T} \mathbf{P}^T + \mathbf{E}
$$

**Hotelling's $T^2$ statistic for fault detection:**

$$
T^2 = \sum_{i=1}^{k} \frac{t_i^2}{\lambda_i}
$$



**13. Process Optimization**

**13.1 Response Surface Methodology (RSM)**

**Second-order polynomial model:**

$$
y = \beta_0 + \sum_{i=1}^{k} \beta_i x_i + \sum_{i=1}^{k} \beta_{ii} x_i^2 + \sum_{i < j} \beta_{ij} x_i x_j + \varepsilon
$$

**13.2 Constrained Optimization**

$$
\min_{\mathbf{x}} f(\mathbf{x}) \quad \text{subject to} \quad g_i(\mathbf{x}) \leq 0, \quad h_j(\mathbf{x}) = 0
$$

**Example constraints:**

- $g_1$: Non-uniformity ≤ 3%
- $g_2$: Resistivity within spec
- $g_3$: Throughput ≥ target
- $h_1$: Total film thickness = target

**13.3 Pareto Multi-Objective Optimization**

$$
\min_{\mathbf{x}} \left[ f_1(\mathbf{x}), f_2(\mathbf{x}), \ldots, f_m(\mathbf{x}) \right]
$$

Common trade-offs:

- Uniformity vs. throughput
- Film quality vs. cost
- Conformality vs. deposition rate



**14. Mathematical Toolkit**

| Domain | Key Equations | Application |
|--------|---------------|-------------|
| **Transport** | Navier-Stokes, Convection-Diffusion | Gas flow, precursor delivery |
| **Kinetics** | Arrhenius, Langmuir-Hinshelwood | Reaction rates |
| **Surface Evolution** | KPZ, Level-set, Edwards-Wilkinson | Film morphology |
| **Plasma** | Boltzmann, Child-Langmuir | Ion/electron dynamics |
| **Electrochemistry** | Butler-Volmer, Nernst-Planck | Copper plating |
| **Control** | Bellman, MDP, RL algorithms | Recipe optimization |
| **Statistics** | GPR, PLS, PCA | Virtual metrology |
| **Multiscale** | MD, kMC, Continuum | Integrated simulation |



**15. Physical Constants**

| Constant | Symbol | Value | Units |
|----------|--------|-------|-------|
| Boltzmann constant | $k_B$ | $1.38 \times 10^{-23}$ | J/K |
| Gas constant | $R$ | $8.314$ | J/(mol$\cdot$K) |
| Faraday constant | $F$ | $96,485$ | C/mol |
| Elementary charge | $e$ | $1.60 \times 10^{-19}$ | C |
| Vacuum permittivity | $\varepsilon_0$ | $8.85 \times 10^{-12}$ | F/m |
| Avogadro's number | $N_A$ | $6.02 \times 10^{23}$ | mol⁻¹ |
| Electron mass | $m_e$ | $9.11 \times 10^{-31}$ | kg |

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/metal-deposition) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
