# Semiconductor Manufacturing Process: Plasma Physics Mathematical Modeling

**Keywords**: plasma physics, semiconductor plasma, plasma fundamentals, debye length, plasma frequency, electron temperature, ion bombardment, plasma sheath, glow discharge

---

**Semiconductor Manufacturing Process: Plasma Physics Mathematical Modeling**



**1. The Physical Context**

Semiconductor manufacturing relies on **low-temperature, non-equilibrium plasmas** for etching and deposition.

**Key Characteristics**

- **Electron temperature**: $T_e \approx 1\text{–}10 \text{ eV}$ (~10,000–100,000 K)
- **Ion/neutral temperature**: $T_i \approx 0.03 \text{ eV}$ (near room temperature)
- **Non-equilibrium condition**: $T_e \gg T_i$

This disparity is essential—hot electrons drive chemistry while cool heavy particles preserve delicate nanoscale structures.

**Common Reactor Types**

- **CCP (Capacitively Coupled Plasmas)**: Used for reactive ion etching (RIE)
- **ICP (Inductively Coupled Plasmas)**: High-density plasma etching
- **ECR (Electron Cyclotron Resonance)**: Microwave-driven high-density sources
- **Remote plasma sources**: Gentle surface treatment and cleaning



**2. Fundamental Governing Equations**

**2.1 The Boltzmann Equation (Master Kinetic Equation)**

The foundation of plasma kinetic theory:

$$
\frac{\partial f_s}{\partial t} + \mathbf{v} \cdot 
abla_{\mathbf{r}} f_s + \frac{q_s}{m_s}(\mathbf{E} + \mathbf{v} \times \mathbf{B}) \cdot 
abla_{\mathbf{v}} f_s = \left(\frac{\partial f_s}{\partial t}\right)_{\text{coll}}
$$

Where:

- $f_s(\mathbf{r}, \mathbf{v}, t)$ — Distribution function for species $s$ in 6D phase space
- $q_s$ — Particle charge
- $m_s$ — Particle mass
- $\mathbf{E}$, $\mathbf{B}$ — Electric and magnetic fields
- Right-hand side — Collision operator encoding all scattering physics

**2.2 Fluid Approximation (Moment Equations)**

Taking velocity moments of the Boltzmann equation yields the fluid hierarchy:

**Continuity Equation (Zeroth Moment)**

$$
\frac{\partial n_s}{\partial t} + 
abla \cdot (n_s \mathbf{u}_s) = S_s
$$

Where:

- $n_s$ — Number density of species $s$
- $\mathbf{u}_s$ — Mean velocity
- $S_s$ — Source/sink terms from chemical reactions

**Momentum Equation (First Moment)**

$$
m_s n_s \frac{D\mathbf{u}_s}{Dt} = q_s n_s (\mathbf{E} + \mathbf{u}_s \times \mathbf{B}) - 
abla p_s - 
abla \cdot \boldsymbol{\Pi}_s + \mathbf{R}_s
$$

Where:

- $p_s = n_s k_B T_s$ — Scalar pressure
- $\boldsymbol{\Pi}_s$ — Viscous stress tensor
- $\mathbf{R}_s$ — Momentum transfer from collisions

**Energy Equation (Second Moment)**

$$
\frac{\partial}{\partial t}\left(\frac{3}{2}n_s k_B T_s\right) + 
abla \cdot \mathbf{q}_s + p_s 
abla \cdot \mathbf{u}_s = Q_s
$$

Where:

- $\mathbf{q}_s$ — Heat flux vector
- $Q_s$ — Energy source terms (heating, cooling, reactions)

**2.3 Maxwell's Equations**

**Full Electromagnetic Set**

$$

abla \cdot \mathbf{E} = \frac{\rho}{\varepsilon_0} = \frac{e}{\varepsilon_0}\sum_s Z_s n_s
$$

$$

abla \times \mathbf{E} = -\frac{\partial \mathbf{B}}{\partial t}
$$

$$

abla \cdot \mathbf{B} = 0
$$

$$

abla \times \mathbf{B} = \mu_0 \mathbf{J} + \mu_0 \varepsilon_0 \frac{\partial \mathbf{E}}{\partial t}
$$

**Electrostatic Approximation (Poisson Equation)**

For most processing plasmas:

$$

abla^2 \phi = -\frac{e}{\varepsilon_0}(n_i - n_e)
$$

Where $\mathbf{E} = -
abla \phi$.



**3. Critical Plasma Parameters**

**3.1 Debye Length**

The characteristic shielding scale:

$$
\lambda_D = \sqrt{\frac{\varepsilon_0 k_B T_e}{n_e e^2}}
$$

Numerical form:

$$
\lambda_D \approx 7.43 \times 10^{3} \sqrt{\frac{T_e[\text{eV}]}{n_e[\text{m}^{-3}]}} \text{ m}
$$

**Typical values**: 10–100 μm in processing plasmas.

**3.2 Plasma Frequency**

The characteristic electron oscillation frequency:

$$
\omega_{pe} = \sqrt{\frac{n_e e^2}{m_e \varepsilon_0}}
$$

Numerical form:

$$
\omega_{pe} \approx 56.4 \sqrt{n_e[\text{m}^{-3}]} \text{ rad/s}
$$

**3.3 Collision Frequency**

Electron-neutral collision frequency:

$$

u_{en} = n_g \langle \sigma_{en} v_e \rangle \approx n_g \sigma_{en} \bar{v}_e
$$

Where:

- $n_g$ — Neutral gas density
- $\sigma_{en}$ — Collision cross-section
- $\bar{v}_e = \sqrt{8 k_B T_e / \pi m_e}$ — Mean electron speed

**3.4 Knudsen Number**

Determines the validity of fluid vs kinetic models:

$$
\text{Kn} = \frac{\lambda_{\text{mfp}}}{L}
$$

Where:

- $\lambda_{\text{mfp}}$ — Mean free path
- $L$ — Characteristic system length

**Regimes**:

- $\text{Kn} \ll 1$: Fluid models valid (collisional regime)
- $\text{Kn} \gg 1$: Kinetic treatment required (collisionless regime)
- $\text{Kn} \sim 1$: Transitional regime (most challenging)



**4. Sheath Physics: The Critical Interface**

The **sheath** is the thin, non-neutral region where ions accelerate toward surfaces. This controls ion bombardment energy—the key parameter for anisotropic etching.

**4.1 Bohm Criterion**

Ions must enter the sheath at or above the Bohm velocity:

$$
u_s \geq u_B = \sqrt{\frac{k_B T_e}{m_i}}
$$

This arises from requiring monotonically decreasing potential solutions.

**4.2 Child-Langmuir Law (Collisionless Sheath)**

Space-charge-limited current density:

$$
J = \frac{4\varepsilon_0}{9}\sqrt{\frac{2e}{m_i}}\frac{V_0^{3/2}}{s^2}
$$

Where:

- $J$ — Ion current density
- $V_0$ — Sheath voltage
- $s$ — Sheath thickness

**4.3 Matrix Sheath Thickness**

For high-voltage sheaths:

$$
s = \lambda_D \left(\frac{2V_0}{T_e}\right)^{1/2}
$$

**4.4 RF Sheath Dynamics**

In RF plasmas, the sheath oscillates with the applied voltage, creating:

- **Self-bias**: Time-averaged DC potential due to asymmetric current flow

$$
V_{dc} = -V_{rf} + \frac{T_e}{e}\ln\left(\frac{m_i}{2\pi m_e}\right)^{1/2}
$$

- **Ion Energy Distribution Functions (IEDF)**: Bimodal structure depending on frequency

- **Stochastic heating**: Electrons gain energy from oscillating sheath boundary

**Frequency Dependence of IEDF**

| Condition | IEDF Shape |
|-----------|------------|
| $\omega \ll \omega_{pi}$ (low frequency) | Broad bimodal distribution |
| $\omega \gg \omega_{pi}$ (high frequency) | Narrow peak at average energy |



**5. Electron Energy Distribution Functions (EEDF)**

**5.1 Non-Maxwellian Distributions**

The EEDF is generally **not Maxwellian** in low-pressure plasmas. The two-term Boltzmann equation:

$$
-\frac{d}{d\varepsilon}\left[A(\varepsilon)\frac{df}{d\varepsilon} + B(\varepsilon)f\right] = C_{\text{inel}}(f)
$$

Where:

- $A(\varepsilon)$, $B(\varepsilon)$ — Coefficients depending on E-field and cross-sections
- $C_{\text{inel}}$ — Inelastic collision operator

**5.2 Common Distribution Types**

**Maxwellian Distribution**

$$
f_M(\varepsilon) = \frac{2\sqrt{\varepsilon}}{\sqrt{\pi}(k_B T_e)^{3/2}} \exp\left(-\frac{\varepsilon}{k_B T_e}\right)
$$

**Druyvesteyn Distribution (Elastic-Dominated)**

$$
f_D(\varepsilon) \propto \exp\left(-c\varepsilon^2\right)
$$

**Bi-Maxwellian Distribution**

$$
f_{bi}(\varepsilon) = \alpha f_M(\varepsilon; T_{e1}) + (1-\alpha) f_M(\varepsilon; T_{e2})
$$

**5.3 Rate Coefficient Calculation**

Reaction rates depend on the EEDF:

$$
k = \langle \sigma v \rangle = \int_0^\infty \sigma(\varepsilon) v(\varepsilon) f(\varepsilon) \, d\varepsilon
$$

For electron-impact reactions:

$$
k_e = \sqrt{\frac{2}{m_e}} \int_0^\infty \varepsilon \, \sigma(\varepsilon) f(\varepsilon) \, d\varepsilon
$$



**6. Plasma Chemistry Modeling**

**6.1 Species Rate Equations**

General form:

$$
\frac{dn_i}{dt} = \sum_j k_j \prod_l n_l^{
u_{jl}} - n_i 
u_{\text{loss}}
$$

Where:

- $k_j$ — Rate coefficient for reaction $j$
- $
u_{jl}$ — Stoichiometric coefficient
- $
u_{\text{loss}}$ — Total loss frequency

**6.2 Arrhenius Rate Coefficients**

For thermal reactions:

$$
k(T) = A T^n \exp\left(-\frac{E_a}{k_B T}\right)
$$

Where:

- $A$ — Pre-exponential factor
- $n$ — Temperature exponent
- $E_a$ — Activation energy

**6.3 Example: Chlorine Plasma Chemistry**

Simplified Cl₂ plasma reaction set:

| Reaction | Type | Threshold |
|----------|------|-----------|
| $e + \text{Cl}_2 \rightarrow 2\text{Cl} + e$ | Dissociation | ~2.5 eV |
| $e + \text{Cl}_2 \rightarrow \text{Cl}_2^+ + 2e$ | Ionization | ~11.5 eV |
| $e + \text{Cl} \rightarrow \text{Cl}^+ + 2e$ | Ionization | ~13 eV |
| $e + \text{Cl}^- \rightarrow \text{Cl} + 2e$ | Detachment | — |
| $\text{Cl}_2^+ + e \rightarrow 2\text{Cl}$ | Dissociative recombination | — |
| $\text{Cl} + \text{wall} \rightarrow \frac{1}{2}\text{Cl}_2$ | Surface recombination | — |

Full models include 50+ reactions with rate constants spanning 10+ orders of magnitude.



**7. Transport Models**

**7.1 Drift-Diffusion Approximation**

Standard flux expression:

$$
\boldsymbol{\Gamma}_s = \text{sgn}(q_s) \mu_s n_s \mathbf{E} - D_s 
abla n_s
$$

Where:

- $\mu_s$ — Mobility
- $D_s$ — Diffusion coefficient

**Einstein Relation**:

$$
\frac{D_s}{\mu_s} = \frac{k_B T_s}{|q_s|}
$$

**7.2 Ambipolar Diffusion**

In quasi-neutral bulk plasma, electrons and ions diffuse together:

$$
D_a = \frac{\mu_i D_e + \mu_e D_i}{\mu_e + \mu_i}
$$

Since $\mu_e \gg \mu_i$:

$$
D_a \approx D_i \left(1 + \frac{T_e}{T_i}\right)
$$

**7.3 Tensor Transport (Magnetized Plasmas)**

In magnetic fields, transport becomes anisotropic:

$$
\boldsymbol{\Gamma} = -\mathbf{D} \cdot 
abla n + n \boldsymbol{\mu} \cdot \mathbf{E}
$$

The diffusion tensor has components:

- **Parallel**: $D_\parallel = D_0$
- **Perpendicular**: $D_\perp = \frac{D_0}{1 + \omega_c^2 \tau^2}$
- **Hall**: $D_H = \frac{\omega_c \tau D_0}{1 + \omega_c^2 \tau^2}$

Where $\omega_c = qB/m$ is the cyclotron frequency.



**8. Computational Approaches**

**8.1 Hierarchy of Models**

| Model | Dimensions | Physics Captured | Typical Runtime |
|-------|------------|------------------|-----------------|
| Global (0D) | Volume-averaged | Detailed chemistry | Seconds |
| Fluid (1D-3D) | Spatial resolution | Transport + chemistry | Minutes–Hours |
| PIC-MCC | Full phase space | Kinetic ions/electrons | Days–Weeks |
| Hybrid | Mixed | Fluid electrons + kinetic ions | Hours–Days |

**8.2 Fluid Model Implementation**

Solve the coupled system:

1. **Species continuity equations** (one per species)
2. **Electron energy equation**
3. **Poisson equation**
4. **Momentum equations** (often drift-diffusion limit)

**Numerical Challenges**

- **Nonlinear coupling**: Exponential dependence of source terms on $T_e$
- **Disparate timescales**: 
  - Electron dynamics: ~ns
  - Ion dynamics: ~μs
  - Chemistry: ~ms
- **Spatial scales**: Sheath ($\lambda_D \sim 100$ μm) vs reactor (~0.1 m)

**Common Numerical Techniques**

- Semi-implicit time stepping
- Scharfetter-Gummel discretization for drift-diffusion fluxes
- Multigrid Poisson solvers
- Adaptive mesh refinement near sheaths

**8.3 Particle-in-Cell with Monte Carlo Collisions (PIC-MCC)**

**Algorithm Steps**

1. **Push particles** using equations of motion:

$$
\frac{d\mathbf{x}}{dt} = \mathbf{v}, \quad m\frac{d\mathbf{v}}{dt} = q(\mathbf{E} + \mathbf{v} \times \mathbf{B})
$$

2. **Deposit charge** onto computational grid
3. **Solve Poisson** equation for electric field
4. **Interpolate field** back to particle positions
5. **Monte Carlo collisions** based on cross-sections

**Applications**

- Low-pressure kinetic regimes
- IEDF predictions
- Non-local electron kinetics
- Detailed sheath physics

**Computational Cost**

Scales as $O(N_p \log N_p)$ per timestep, with $N_p \sim 10^6\text{–}10^8$ superparticles.



**9. Multi-Scale Coupling: The Grand Challenge**

**9.1 Scale Hierarchy**

| Scale | Phenomenon | Typical Model |
|-------|------------|---------------|
| Å–nm | Surface reactions, damage | MD, DFT |
| nm–μm | Feature evolution | Level-set, Monte Carlo |
| μm–mm | Sheath, transport | Fluid/kinetic plasma |
| mm–m | Reactor, gas flow | CFD + plasma |

**9.2 Feature-Scale Modeling**

**Level-Set Method**

Track the evolving surface $\phi = 0$:

$$
\frac{\partial \phi}{\partial t} + V_n |
abla \phi| = 0
$$

Where $V_n$ is the local etch/deposition rate depending on:

- Ion flux $\Gamma_i$ and energy $\varepsilon_i$ from plasma model
- Neutral radical flux $\Gamma_n$
- Surface composition and local geometry
- Angle-dependent yields $Y(\theta, \varepsilon)$

**Etch Rate Model**

$$
R = Y_0 \Gamma_i f(\varepsilon) + k_s \Gamma_n \theta_s
$$

Where:

- $Y_0$ — Base sputter yield
- $f(\varepsilon)$ — Energy-dependent yield function
- $k_s$ — Surface reaction rate
- $\theta_s$ — Surface coverage

**9.3 Aspect Ratio Dependent Etching (ARDE)**

$$
\frac{R_{\text{bottom}}}{R_{\text{top}}} = f(\text{AR})
$$

**Physical Mechanisms**

- Ion angular distribution effects (Knudsen diffusion in feature)
- Neutral transport limitations
- Differential charging in high-aspect-ratio features
- Sidewall passivation dynamics



**10. Electromagnetic Effects in High-Density Sources**

**10.1 ICP Power Deposition**

The RF magnetic field induces an electric field:

$$

abla \times \mathbf{E} = -i\omega \mathbf{B}
$$

Power deposition density:

$$
P = \frac{1}{2}\text{Re}(\mathbf{J}^* \cdot \mathbf{E}) = \frac{1}{2}\text{Re}(\sigma_p)|\mathbf{E}|^2
$$

**10.2 Plasma Conductivity**

$$
\sigma_p = \frac{n_e e^2}{m_e(
u_m + i\omega)}
$$

Where:

- $
u_m$ — Electron momentum transfer collision frequency
- $\omega$ — RF angular frequency

**10.3 Skin Depth**

Electromagnetic field penetration depth:

$$
\delta = \sqrt{\frac{2}{\omega \mu_0 \text{Re}(\sigma_p)}}
$$

**Typical values**: $\delta \approx 1\text{–}3$ cm, creating non-uniform power deposition.

**10.4 E-to-H Mode Transition**

ICPs exhibit hysteresis behavior:

- **E-mode** (low power): Capacitive coupling, low plasma density
- **H-mode** (high power): Inductive coupling, high plasma density

The transition involves bifurcation in the coupled power-density equations.



**11. Surface Reaction Modeling**

**11.1 Surface Reaction Mechanisms**

**Langmuir-Hinshelwood Mechanism**

Both reactants adsorbed:

$$
R = k \theta_A \theta_B
$$

**Eley-Rideal Mechanism**

One reactant from gas phase:

$$
R = k P_A \theta_B
$$

**Surface Coverage Dynamics**

$$
\frac{d\theta}{dt} = k_{\text{ads}}P(1-\theta) - k_{\text{des}}\theta - k_{\text{react}}\theta
$$

**11.2 Kinetic Monte Carlo (KMC)**

For atomic-scale surface evolution:

1. Catalog all possible events with rates $\{k_i\}$
2. Calculate total rate: $k_{\text{tot}} = \sum_i k_i$
3. Time advance: $\Delta t = -\ln(r_1)/k_{\text{tot}}$
4. Select event $j$ probabilistically
5. Execute event and update configuration

**11.3 Molecular Dynamics for Ion-Surface Interactions**

Newton's equations with empirical potentials:

$$
m_i \frac{d^2 \mathbf{r}_i}{dt^2} = -
abla_i U(\{\mathbf{r}\})
$$

**Potentials used**:

- Stillinger-Weber (Si)
- Tersoff (C, Si, Ge)
- ReaxFF (reactive systems)

**Outputs**:

- Sputter yields $Y(\varepsilon, \theta)$
- Damage depth profiles
- Reaction probabilities



**12. Emerging Mathematical Methods**

**12.1 Machine Learning in Plasma Modeling**

- **Surrogate models**: Neural networks for real-time prediction
- **Reduced-order models**: POD/DMD for parametric studies
- **Inverse problems**: Inferring plasma parameters from sensor data

**12.2 Uncertainty Quantification**

Given uncertainties in input parameters:

- Cross-section data (~20–50% uncertainty)
- Surface reaction coefficients
- Boundary conditions

**Propagation methods**:

- Polynomial chaos expansions
- Monte Carlo sampling
- Sensitivity analysis (Sobol indices)

**12.3 Data-Driven Closures**

Learning moment closures from kinetic data:

$$
\mathbf{q} = \mathcal{F}_\theta(n, \mathbf{u}, T, 
abla T, \ldots)
$$

Where $\mathcal{F}_\theta$ is a neural network trained on PIC simulation data.



**13. Key Dimensionless Groups**

| Parameter | Definition | Significance |
|-----------|------------|--------------|
| $\Lambda = L/\lambda_D$ | System size / Debye length | Plasma character ($\gg 1$ for quasi-neutrality) |
| $\omega/
u_m$ | Frequency / collision rate | Collisional vs collisionless |
| $\omega/\omega_{pe}$ | Frequency / plasma frequency | Wave propagation regime |
| $r_L/L$ | Larmor radius / system size | Degree of magnetization |
| $\text{Kn} = \lambda/L$ | Mean free path / system size | Fluid vs kinetic regime |
| $\text{Re}_m$ | Magnetic Reynolds number | Magnetic field diffusion |



**14. Example: Complete CCP Model**

**14.1 Governing Equations (1D)**

**Electron Continuity**

$$
\frac{\partial n_e}{\partial t} + \frac{\partial \Gamma_e}{\partial x} = k_{\text{iz}} n_e n_g - k_{\text{att}} n_e n_g
$$

**Electron Flux**

$$
\Gamma_e = -\mu_e n_e E - D_e \frac{\partial n_e}{\partial x}
$$

**Ion Continuity**

$$
\frac{\partial n_i}{\partial t} + \frac{\partial \Gamma_i}{\partial x} = k_{\text{iz}} n_e n_g
$$

**Electron Energy Density**

$$
\frac{\partial n_\varepsilon}{\partial t} + \frac{\partial \Gamma_\varepsilon}{\partial x} + e\Gamma_e E = -\sum_j n_e n_g k_j \varepsilon_j
$$

**Poisson Equation**

$$
\frac{\partial^2 \phi}{\partial x^2} = -\frac{e}{\varepsilon_0}(n_i - n_e)
$$

**14.2 Boundary Conditions**

At electrodes ($x = 0, L$):

- **Potential**: $\phi(0,t) = V_{\text{rf}}\sin(\omega t)$, $\phi(L,t) = 0$
- **Secondary emission**: $\Gamma_e = \gamma \Gamma_i$ (with $\gamma \approx 0.1$)
- **Kinetic fluxes**: Derived from distribution function at boundary

**14.3 Numerical Parameters**

| Parameter | Typical Value |
|-----------|---------------|
| Grid points | ~1000 |
| Species | ~10 |
| RF cycles to steady state | $10^5\text{–}10^6$ |
| Time step | $\Delta t < 0.1/\omega_{pe}$ |



**Summary**

The mathematical modeling of plasmas in semiconductor manufacturing represents a magnificent multi-physics, multi-scale scientific endeavor requiring:

1. **Kinetic theory** for non-equilibrium particle distributions
2. **Fluid mechanics** for macroscopic transport
3. **Electromagnetism** for field and power coupling
4. **Chemical kinetics** for reactive processes
5. **Surface science** for etch/deposition mechanisms
6. **Numerical analysis** for efficient computation
7. **Uncertainty quantification** for predictive capability

The field continues to advance with machine learning integration, exascale computing enabling full 3D kinetic simulations, and tighter coupling between atomic-scale and reactor-scale models—driven by the relentless progression toward smaller feature sizes and novel materials in semiconductor technology.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/plasma-physics-10704) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
