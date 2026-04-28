# Mathematical Modeling of Plasma Etching in Semiconductor Manufacturing

**Keywords**: etch plasma modeling,plasma etch modeling,plasma etch physics,plasma sheath,ion bombardment,reactive ion etch,RIE

---

**Mathematical Modeling of Plasma Etching in Semiconductor Manufacturing**


**Introduction**

Plasma etching is a critical process in semiconductor manufacturing where reactive gases are ionized to create a plasma, which selectively removes material from a wafer surface. The mathematical modeling of this process spans multiple physics domains:

- **Electromagnetic theory** — RF power coupling and field distributions
- **Statistical mechanics** — Particle distributions and kinetic theory
- **Reaction kinetics** — Gas-phase and surface chemistry
- **Transport phenomena** — Species diffusion and convection
- **Surface science** — Etch mechanisms and selectivity



**Foundational Plasma Physics**

**Boltzmann Transport Equation**

The most fundamental description of plasma behavior is the **Boltzmann transport equation**, governing the evolution of the particle velocity distribution function $f(\mathbf{r}, \mathbf{v}, t)$:

$$
\frac{\partial f}{\partial t} + \mathbf{v} \cdot 
abla f + \frac{\mathbf{F}}{m} \cdot 
abla_v f = \left(\frac{\partial f}{\partial t}\right)_{\text{collision}}
$$

**Where:**
- $f(\mathbf{r}, \mathbf{v}, t)$ — Velocity distribution function
- $\mathbf{v}$ — Particle velocity
- $\mathbf{F}$ — External force (electromagnetic)
- $m$ — Particle mass
- RHS — Collision integral

**Fluid Moment Equations**

For computational tractability, velocity moments of the Boltzmann equation yield fluid equations:

**Continuity Equation (Mass Conservation)**

$$
\frac{\partial n}{\partial t} + 
abla \cdot (n\mathbf{u}) = S - L
$$

**Where:**
- $n$ — Species number density $[\text{m}^{-3}]$
- $\mathbf{u}$ — Drift velocity $[\text{m/s}]$
- $S$ — Source term (generation rate)
- $L$ — Loss term (consumption rate)

**Momentum Conservation**

$$
\frac{\partial (nm\mathbf{u})}{\partial t} + 
abla \cdot (nm\mathbf{u}\mathbf{u}) + 
abla p = nq(\mathbf{E} + \mathbf{u} \times \mathbf{B}) - nm
u_m \mathbf{u}
$$

**Where:**
- $p = nk_BT$ — Pressure
- $q$ — Particle charge
- $\mathbf{E}$, $\mathbf{B}$ — Electric and magnetic fields
- $
u_m$ — Momentum transfer collision frequency $[\text{s}^{-1}]$

**Energy Conservation**

$$
\frac{\partial}{\partial t}\left(\frac{3}{2}nk_BT\right) + 
abla \cdot \mathbf{q} + p
abla \cdot \mathbf{u} = Q_{\text{heating}} - Q_{\text{loss}}
$$

**Where:**
- $k_B = 1.38 \times 10^{-23}$ J/K — Boltzmann constant
- $\mathbf{q}$ — Heat flux vector
- $Q_{\text{heating}}$ — Power input (Joule heating, stochastic heating)
- $Q_{\text{loss}}$ — Energy losses (collisions, radiation)



**Electromagnetic Field Coupling**

**Maxwell's Equations**

For capacitively coupled plasma (CCP) and inductively coupled plasma (ICP) reactors:

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

**Plasma Conductivity**

The plasma current density couples through the complex conductivity:

$$
\mathbf{J} = \sigma \mathbf{E}
$$

For RF plasmas, the **complex conductivity** is:

$$
\sigma = \frac{n_e e^2}{m_e(
u_m + i\omega)}
$$

**Where:**
- $n_e$ — Electron density
- $e = 1.6 \times 10^{-19}$ C — Elementary charge
- $m_e = 9.1 \times 10^{-31}$ kg — Electron mass
- $\omega$ — RF angular frequency
- $
u_m$ — Electron-neutral collision frequency

**Power Deposition**

Time-averaged power density deposited into the plasma:

$$
P = \frac{1}{2}\text{Re}(\mathbf{J} \cdot \mathbf{E}^*)
$$

**Typical values:**
- CCP: $0.1 - 1$ W/cm³
- ICP: $0.5 - 5$ W/cm³



**Plasma Sheath Physics**

The sheath is a thin, non-neutral region at the plasma-wafer interface that accelerates ions toward the surface, enabling anisotropic etching.

**Bohm Criterion**

Minimum ion velocity entering the sheath:

$$
u_i \geq u_B = \sqrt{\frac{k_B T_e}{M_i}}
$$

**Where:**
- $u_B$ — Bohm velocity
- $T_e$ — Electron temperature (typically 2–5 eV)
- $M_i$ — Ion mass

**Example:** For Ar⁺ ions with $T_e = 3$ eV:
$$
u_B = \sqrt{\frac{3 \times 1.6 \times 10^{-19}}{40 \times 1.67 \times 10^{-27}}} \approx 2.7 \text{ km/s}
$$

**Child-Langmuir Law**

For a collisionless sheath, the ion current density is:

$$
J = \frac{4\varepsilon_0}{9}\sqrt{\frac{2e}{M_i}} \cdot \frac{V_s^{3/2}}{d^2}
$$

**Where:**
- $\varepsilon_0 = 8.85 \times 10^{-12}$ F/m — Vacuum permittivity
- $V_s$ — Sheath voltage drop (typically 10–500 V)
- $d$ — Sheath thickness

**Sheath Thickness**

The sheath thickness scales as:

$$
d \approx \lambda_D \left(\frac{2eV_s}{k_BT_e}\right)^{3/4}
$$

**Where** the Debye length is:

$$
\lambda_D = \sqrt{\frac{\varepsilon_0 k_B T_e}{n_e e^2}}
$$

**Ion Angular Distribution**

Ions arrive at the wafer with an angular distribution:

$$
f(\theta) \propto \exp\left(-\frac{\theta^2}{2\sigma^2}\right)
$$

**Where:**

$$
\sigma \approx \arctan\left(\sqrt{\frac{k_B T_i}{eV_s}}\right)
$$

**Typical values:** $\sigma \approx 2°–5°$ for high-bias conditions.



**Electron Energy Distribution Function**

**Non-Maxwellian Distributions**

In low-pressure plasmas (1–100 mTorr), the EEDF deviates from Maxwellian.

**Two-Term Approximation**

The EEDF is expanded as:

$$
f(\varepsilon, \theta) = f_0(\varepsilon) + f_1(\varepsilon)\cos\theta
$$

The isotropic part $f_0$ satisfies:

$$
\frac{d}{d\varepsilon}\left[\varepsilon D \frac{df_0}{d\varepsilon} + \left(V + \frac{\varepsilon
u_{\text{inel}}}{
u_m}\right)f_0\right] = 0
$$

**Common Distribution Functions**

| Distribution | Functional Form | Applicability |
|-------------|-----------------|---------------|
| **Maxwellian** | $f(\varepsilon) \propto \sqrt{\varepsilon} \exp\left(-\frac{\varepsilon}{k_BT_e}\right)$ | High pressure, collisional |
| **Druyvesteyn** | $f(\varepsilon) \propto \sqrt{\varepsilon} \exp\left(-\left(\frac{\varepsilon}{k_BT_e}\right)^2\right)$ | Elastic collisions dominant |
| **Bi-Maxwellian** | Sum of two Maxwellians | Hot tail population |

**Generalized Form**

$$
f(\varepsilon) \propto \sqrt{\varepsilon} \cdot \exp\left[-\left(\frac{\varepsilon}{k_BT_e}\right)^x\right]
$$

- $x = 1$ → Maxwellian
- $x = 2$ → Druyvesteyn



**Plasma Chemistry and Reaction Kinetics**

**Species Balance Equation**

For species $i$:

$$
\frac{\partial n_i}{\partial t} + 
abla \cdot \mathbf{\Gamma}_i = \sum_j R_j
$$

**Where:**
- $\mathbf{\Gamma}_i$ — Species flux
- $R_j$ — Reaction rates

**Electron-Impact Rate Coefficients**

Rate coefficients are calculated by integration over the EEDF:

$$
k = \int_0^\infty \sigma(\varepsilon) v(\varepsilon) f(\varepsilon) \, d\varepsilon = \langle \sigma v \rangle
$$

**Where:**
- $\sigma(\varepsilon)$ — Energy-dependent cross-section $[\text{m}^2]$
- $v(\varepsilon) = \sqrt{2\varepsilon/m_e}$ — Electron velocity
- $f(\varepsilon)$ — Normalized EEDF

**Heavy-Particle Reactions**

Arrhenius kinetics for neutral reactions:

$$
k = A T^n \exp\left(-\frac{E_a}{k_BT}\right)
$$

**Where:**
- $A$ — Pre-exponential factor
- $n$ — Temperature exponent
- $E_a$ — Activation energy

**Example: SF₆/O₂ Plasma Chemistry**

**Electron-Impact Reactions**

| Reaction | Type | Threshold |
|----------|------|-----------|
| $e + \text{SF}_6 \rightarrow \text{SF}_5 + \text{F} + e$ | Dissociation | ~10 eV |
| $e + \text{SF}_6 \rightarrow \text{SF}_6^-$ | Attachment | ~0 eV |
| $e + \text{SF}_6 \rightarrow \text{SF}_5^+ + \text{F} + 2e$ | Ionization | ~16 eV |
| $e + \text{O}_2 \rightarrow \text{O} + \text{O} + e$ | Dissociation | ~6 eV |

**Gas-Phase Reactions**

- $\text{F} + \text{O} \rightarrow \text{FO}$ (reduces F atom density)
- $\text{SF}_5 + \text{F} \rightarrow \text{SF}_6$ (recombination)
- $\text{O} + \text{CF}_3 \rightarrow \text{COF}_2 + \text{F}$ (polymer removal)

**Surface Reactions**

- $\text{F} + \text{Si}(s) \rightarrow \text{SiF}_{(\text{ads})}$
- $\text{SiF}_{(\text{ads})} + 3\text{F} \rightarrow \text{SiF}_4(g)$ (volatile product)



**Transport Phenomena**

**Drift-Diffusion Model**

For charged species, the flux is:

$$
\mathbf{\Gamma} = \pm \mu n \mathbf{E} - D 
abla n
$$

**Where:**
- Upper sign: positive ions
- Lower sign: electrons
- $\mu$ — Mobility $[\text{m}^2/(\text{V}\cdot\text{s})]$
- $D$ — Diffusion coefficient $[\text{m}^2/\text{s}]$

**Einstein Relation**

Connects mobility and diffusion:

$$
D = \frac{\mu k_B T}{e}
$$

**Ambipolar Diffusion**

When quasi-neutrality holds ($n_e \approx n_i$):

$$
D_a = \frac{\mu_i D_e + \mu_e D_i}{\mu_i + \mu_e} \approx D_i\left(1 + \frac{T_e}{T_i}\right)
$$

Since $T_e \gg T_i$ typically: $D_a \approx D_i (1 + T_e/T_i) \approx 100 D_i$

**Neutral Transport**

For reactive neutrals (radicals), Fickian diffusion:

$$
\frac{\partial n}{\partial t} = D
abla^2 n + S - L
$$

**Surface Boundary Condition**

$$
-D\frac{\partial n}{\partial x}\bigg|_{\text{surface}} = \frac{1}{4}\gamma n v_{\text{th}}
$$

**Where:**
- $\gamma$ — Sticking/reaction coefficient (0 to 1)
- $v_{\text{th}} = \sqrt{\frac{8k_BT}{\pi m}}$ — Thermal velocity

**Knudsen Number**

Determines the appropriate transport regime:

$$
\text{Kn} = \frac{\lambda}{L}
$$

**Where:**
- $\lambda$ — Mean free path
- $L$ — Characteristic length

| Kn Range | Regime | Model |
|----------|--------|-------|
| $< 0.01$ | Continuum | Navier-Stokes |
| $0.01–0.1$ | Slip flow | Modified N-S |
| $0.1–10$ | Transition | DSMC/BGK |
| $> 10$ | Free molecular | Ballistic |



**Surface Reaction Modeling**

**Langmuir Adsorption Kinetics**

For surface coverage $\theta$:

$$
\frac{d\theta}{dt} = k_{\text{ads}}(1-\theta)P - k_{\text{des}}\theta - k_{\text{react}}\theta
$$

**At steady state:**

$$
\theta = \frac{k_{\text{ads}}P}{k_{\text{ads}}P + k_{\text{des}} + k_{\text{react}}}
$$

**Ion-Enhanced Etching**

The total etch rate combines multiple mechanisms:

$$
\text{ER} = Y_{\text{chem}} \Gamma_n + Y_{\text{phys}} \Gamma_i + Y_{\text{syn}} \Gamma_i f(\theta)
$$

**Where:**
- $Y_{\text{chem}}$ — Chemical etch yield (isotropic)
- $Y_{\text{phys}}$ — Physical sputtering yield
- $Y_{\text{syn}}$ — Ion-enhanced (synergistic) yield
- $\Gamma_n$, $\Gamma_i$ — Neutral and ion fluxes
- $f(\theta)$ — Coverage-dependent function

**Ion Sputtering Yield**

**Energy Dependence**

$$
Y(E) = A\left(\sqrt{E} - \sqrt{E_{\text{th}}}\right) \quad \text{for } E > E_{\text{th}}
$$

**Typical threshold energies:**
- Si: $E_{\text{th}} \approx 20$ eV
- SiO₂: $E_{\text{th}} \approx 30$ eV
- Si₃N₄: $E_{\text{th}} \approx 25$ eV

**Angular Dependence**

$$
Y(\theta) = Y(0) \cos^{-f}(\theta) \exp\left[-b\left(\frac{1}{\cos\theta} - 1\right)\right]
$$

**Behavior:**
- Increases from normal incidence
- Peaks at $\theta \approx 60°–70°$
- Decreases at grazing angles (reflection dominates)



**Feature-Scale Profile Evolution**

**Level Set Method**

The surface is represented as the zero contour of $\phi(\mathbf{x}, t)$:

$$
\frac{\partial \phi}{\partial t} + V_n |
abla \phi| = 0
$$

**Where:**
- $\phi > 0$ — Material
- $\phi < 0$ — Void/vacuum
- $\phi = 0$ — Surface
- $V_n$ — Local normal etch velocity

**Local Etch Rate Calculation**

The normal velocity $V_n$ depends on:

1. **Ion flux and angular distribution**
   $$\Gamma_i(\mathbf{x}) = \int f(\theta, E) \, d\Omega \, dE$$

2. **Neutral flux** (with shadowing)
   $$\Gamma_n(\mathbf{x}) = \Gamma_{n,0} \cdot \text{VF}(\mathbf{x})$$
   where VF is the view factor

3. **Surface chemistry state**
   $$V_n = f(\Gamma_i, \Gamma_n, \theta_{\text{coverage}}, T)$$

**Neutral Transport in High-Aspect-Ratio Features**

**Clausing Transmission Factor**

For a tube of aspect ratio AR:

$$
K \approx \frac{1}{1 + 0.5 \cdot \text{AR}}
$$

**View Factor Calculations**

For surface element $dA_1$ seeing $dA_2$:

$$
F_{1 \rightarrow 2} = \frac{1}{\pi} \int \frac{\cos\theta_1 \cos\theta_2}{r^2} \, dA_2
$$



**Monte Carlo Methods**

**Test-Particle Monte Carlo Algorithm**

```
1. SAMPLE incident particle from flux distribution at feature opening
   - Ion: from IEDF and IADF
   - Neutral: from Maxwellian
   
2. TRACE trajectory through feature
   - Ion: ballistic, solve equation of motion
   - Neutral: random walk with wall collisions
   
3. DETERMINE reaction at surface impact
   - Sample from probability distribution
   - Update surface coverage if adsorption
   
4. UPDATE surface geometry
   - Remove material (etching)
   - Add material (deposition)
   
5. REPEAT for statistically significant sample
```

**Ion Trajectory Integration**

Through the sheath/feature:

$$
m\frac{d^2\mathbf{r}}{dt^2} = q\mathbf{E}(\mathbf{r})
$$

**Numerical integration:** Velocity-Verlet or Boris algorithm

**Collision Sampling**

Null-collision method for efficiency:

$$
P_{\text{collision}} = 1 - \exp(-
u_{\text{max}} \Delta t)
$$

**Where** $
u_{\text{max}}$ is the maximum possible collision frequency.



**Multi-Scale Modeling Framework**

**Scale Hierarchy**

| Scale | Length | Time | Physics | Method |
|-------|--------|------|---------|--------|
| **Reactor** | cm–m | ms–s | Plasma transport, EM fields | Fluid PDE |
| **Sheath** | µm–mm | µs–ms | Ion acceleration, EEDF | Kinetic/Fluid |
| **Feature** | nm–µm | ns–ms | Profile evolution | Level set/MC |
| **Atomic** | Å–nm | ps–ns | Reaction mechanisms | MD/DFT |

**Coupling Approaches**

**Hierarchical (One-Way)**

```
Atomic scale → Surface parameters
     ↓
Feature scale ← Fluxes from reactor scale
     ↓
Reactor scale → Process outputs
```

**Concurrent (Two-Way)**

- Feature-scale results feed back to reactor scale
- Requires iterative solution
- Computationally expensive



**Numerical Methods and Challenges**

**Stiff ODE Systems**

Plasma chemistry involves timescales spanning many orders of magnitude:

| Process | Timescale |
|---------|-----------|
| Electron attachment | $\sim 10^{-10}$ s |
| Ion-molecule reactions | $\sim 10^{-6}$ s |
| Metastable decay | $\sim 10^{-3}$ s |
| Surface diffusion | $\sim 10^{-1}$ s |

**Implicit Methods Required**

**Backward Differentiation Formula (BDF):**

$$
y_{n+1} = \sum_{j=0}^{k-1} \alpha_j y_{n-j} + h\beta f(t_{n+1}, y_{n+1})
$$

**Spatial Discretization**

**Finite Volume Method**

Ensures mass conservation:

$$
\int_V \frac{\partial n}{\partial t} dV + \oint_S \mathbf{\Gamma} \cdot d\mathbf{S} = \int_V S \, dV
$$

**Mesh Requirements**

- Sheath resolution: $\Delta x < \lambda_D$
- RF skin depth: $\Delta x < \delta$
- Adaptive mesh refinement (AMR) common

**EM-Plasma Coupling**

**Iterative scheme:**

1. Solve Maxwell's equations for $\mathbf{E}$, $\mathbf{B}$
2. Update plasma transport (density, temperature)
3. Recalculate $\sigma$, $\varepsilon_{\text{plasma}}$
4. Repeat until convergence



**Advanced Topics**

**Atomic Layer Etching (ALE)**

Self-limiting reactions for atomic precision:

$$
\text{EPC} = \Theta \cdot d_{\text{ML}}
$$

**Where:**
- EPC — Etch per cycle
- $\Theta$ — Modified layer coverage fraction
- $d_{\text{ML}}$ — Monolayer thickness

**ALE Cycle**

1. **Modification step:** Reactive gas creates modified surface layer
   $$\frac{d\Theta}{dt} = k_{\text{mod}}(1-\Theta)P_{\text{gas}}$$

2. **Removal step:** Ion bombardment removes modified layer only
   $$\text{ER} = Y_{\text{mod}}\Gamma_i\Theta$$

**Pulsed Plasma Dynamics**

Time-modulated RF introduces:

- **Active glow:** Plasma on, high ion/radical generation
- **Afterglow:** Plasma off, selective chemistry

**Ion Energy Modulation**

By pulsing bias:

$$
\langle E_i \rangle = \frac{1}{T}\left[\int_0^{t_{\text{on}}} E_{\text{high}}dt + \int_{t_{\text{on}}}^{T} E_{\text{low}}dt\right]
$$

**High-Aspect-Ratio Etching (HAR)**

For AR > 50 (memory, 3D NAND):

**Challenges:**
- Ion angular broadening → bowing
- Neutral depletion at bottom
- Feature charging → twisting
- Mask erosion → tapering

**Ion Angular Distribution Broadening:**

$$
\sigma_{\text{effective}} = \sqrt{\sigma_{\text{sheath}}^2 + \sigma_{\text{scattering}}^2}
$$

**Neutral Flux at Bottom:**

$$
\Gamma_{\text{bottom}} \approx \Gamma_{\text{top}} \cdot K(\text{AR})
$$

**Machine Learning Integration**

**Applications:**
- Surrogate models for fast prediction
- Process optimization (Bayesian)
- Virtual metrology
- Anomaly detection

**Physics-Informed Neural Networks (PINNs):**

$$
\mathcal{L} = \mathcal{L}_{\text{data}} + \lambda \mathcal{L}_{\text{physics}}
$$

Where $\mathcal{L}_{\text{physics}}$ enforces governing equations.



**Validation and Experimental Techniques**

**Plasma Diagnostics**

| Technique | Measurement | Typical Values |
|-----------|-------------|----------------|
| **Langmuir probe** | $n_e$, $T_e$, EEDF | $10^{9}–10^{12}$ cm⁻³, 1–5 eV |
| **OES** | Relative species densities | Qualitative/semi-quantitative |
| **APMS** | Ion mass, energy | 1–500 amu, 0–500 eV |
| **LIF** | Absolute radical density | $10^{11}–10^{14}$ cm⁻³ |
| **Microwave interferometry** | $n_e$ (line-averaged) | $10^{10}–10^{12}$ cm⁻³ |

**Etch Characterization**

- **Profilometry:** Etch depth, uniformity
- **SEM/TEM:** Feature profiles, sidewall angle
- **XPS:** Surface composition
- **Ellipsometry:** Film thickness, optical properties

**Model Validation Workflow**

1. **Plasma validation:** Match $n_e$, $T_e$, species densities
2. **Flux validation:** Compare ion/neutral fluxes to wafer
3. **Etch rate validation:** Blanket wafer etch rates
4. **Profile validation:** Patterned feature cross-sections



**Key Dimensionless Numbers Summary**

| Number | Definition | Physical Meaning |
|--------|------------|------------------|
| **Knudsen** | $\text{Kn} = \lambda/L$ | Continuum vs. kinetic |
| **Damköhler** | $\text{Da} = \tau_{\text{transport}}/\tau_{\text{reaction}}$ | Transport vs. reaction limited |
| **Sticking coefficient** | $\gamma = \text{reactions}/\text{collisions}$ | Surface reactivity |
| **Aspect ratio** | $\text{AR} = \text{depth}/\text{width}$ | Feature geometry |
| **Debye number** | $N_D = n\lambda_D^3$ | Plasma ideality |



**Physical Constants**

| Constant | Symbol | Value |
|----------|--------|-------|
| Elementary charge | $e$ | $1.602 \times 10^{-19}$ C |
| Electron mass | $m_e$ | $9.109 \times 10^{-31}$ kg |
| Proton mass | $m_p$ | $1.673 \times 10^{-27}$ kg |
| Boltzmann constant | $k_B$ | $1.381 \times 10^{-23}$ J/K |
| Vacuum permittivity | $\varepsilon_0$ | $8.854 \times 10^{-12}$ F/m |
| Vacuum permeability | $\mu_0$ | $4\pi \times 10^{-7}$ H/m |

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/etch-plasma-modeling) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
