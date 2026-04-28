# Semiconductor Manufacturing: Etch Equipment Mathematical Modeling

**Keywords**: etch equipment, plasma etch equipment, icp etch, ccp etch, reactive ion etch equipment, plasma reactor, etch chamber, etch simulator

---

**Semiconductor Manufacturing: Etch Equipment Mathematical Modeling**



**1. Introduction**

Plasma etching is a critical process in semiconductor manufacturing where material is selectively removed from wafer surfaces using reactive plasmas. Mathematical modeling spans multiple scales and physics domains:

- **Plasma physics** — Generation and transport of reactive species
- **Surface chemistry** — Reaction kinetics at the wafer surface
- **Transport phenomena** — Gas flow, heat transfer, species diffusion
- **Feature evolution** — Nanoscale profile development
- **Process control** — Run-to-run optimization and fault detection

**1.1 Etch Process Types**

| Type | Mechanism | Selectivity | Anisotropy |
|------|-----------|-------------|------------|
| Wet Etch | Chemical dissolution | High | Isotropic |
| Plasma Etch | Ion + radical reactions | Medium-High | Anisotropic |
| RIE | Ion-enhanced chemistry | Medium | High |
| ICP-RIE | High-density plasma | Tunable | Very High |
| ALE | Self-limiting cycles | Very High | Atomic-level |



**2. Plasma Discharge Modeling**

**2.1 Electron Kinetics**

The electron energy distribution function (EEDF) governs ionization and dissociation rates. It is described by the **Boltzmann transport equation**:

$$
\frac{\partial f}{\partial t} + \vec{v} \cdot 
abla_r f + \frac{e\vec{E}}{m_e} \cdot 
abla_v f = C[f]
$$

Where:

- $f(\vec{r}, \vec{v}, t)$ — Electron distribution function
- $\vec{E}$ — Electric field vector
- $m_e$ — Electron mass
- $C[f]$ — Collision integral

**Two-Term Approximation**

For weakly anisotropic distributions:

$$
f(\vec{r}, \vec{v}, t) = f_0(\vec{r}, v, t) + \vec{v} \cdot \vec{f}_1(\vec{r}, v, t)
$$

**2.2 Species Continuity Equations**

For each species $i$ (electrons, ions, neutrals, radicals):

$$
\frac{\partial n_i}{\partial t} + 
abla \cdot \vec{\Gamma}_i = S_i
$$

Where:

- $n_i$ — Number density of species $i$ (m⁻³)
- $\vec{\Gamma}_i$ — Flux vector (m⁻² s⁻¹)
- $S_i$ — Source/sink term from reactions (m⁻³ s⁻¹)

**Flux Expressions**

- **Neutral species (diffusion only):**
  
  $$
  \vec{\Gamma}_n = -D_n 
abla n_n
  $$

- **Charged species (drift-diffusion):**
  
  $$
  \vec{\Gamma}_{\pm} = \pm \mu_{\pm} n_{\pm} \vec{E} - D_{\pm} 
abla n_{\pm}
  $$

Where:

- $D$ — Diffusion coefficient (m² s⁻¹)
- $\mu$ — Mobility (m² V⁻¹ s⁻¹)

**Einstein Relation**

$$
D = \frac{\mu k_B T}{e}
$$

**2.3 Reaction Rate Coefficients**

Rate coefficients are computed by integrating cross-sections over the EEDF:

$$
k = \int_0^{\infty} \sigma(\varepsilon) \cdot v(\varepsilon) \cdot f(\varepsilon) \, d\varepsilon
$$

Where:

- $\sigma(\varepsilon)$ — Energy-dependent cross-section (m²)
- $v(\varepsilon) = \sqrt{2\varepsilon/m_e}$ — Electron velocity
- $f(\varepsilon)$ — Normalized EEDF

**Key Reactions in Fluorine-Based Plasmas**

| Reaction | Type | Rate Expression |
|----------|------|-----------------|
| $e + SF_6 \rightarrow SF_5^+ + F + 2e$ | Ionization | $k_1(T_e)$ |
| $e + SF_6 \rightarrow SF_5 + F + e$ | Dissociation | $k_2(T_e)$ |
| $e + SF_6 \rightarrow SF_6^- $ | Attachment | $k_3(T_e)$ |
| $F + Si \rightarrow SiF_{(ads)}$ | Adsorption | $s \cdot \Gamma_F$ |

**2.4 Electron Energy Balance**

$$
\frac{\partial}{\partial t}\left(\frac{3}{2} n_e k_B T_e\right) + 
abla \cdot \vec{q}_e = P_{abs} - P_{loss}
$$

Where:

- $P_{abs}$ — Power absorbed from RF field (W m⁻³)
- $P_{loss}$ — Power lost to collisions (W m⁻³)

$$
P_{loss} = \sum_j n_e n_j k_j \varepsilon_j
$$

**2.5 Electromagnetic Field Equations**

**Capacitively Coupled Plasma (CCP)**

Poisson's equation:

$$

abla^2 \phi = -\frac{\rho}{\varepsilon_0} = -\frac{e(n_i - n_e)}{\varepsilon_0}
$$

**Inductively Coupled Plasma (ICP)**

Wave equation for the azimuthal electric field:

$$

abla^2 E_\theta - \frac{1}{c^2}\frac{\partial^2 E_\theta}{\partial t^2} = \mu_0 \frac{\partial J_\theta}{\partial t}
$$

With plasma conductivity:

$$
\sigma_p = \frac{n_e e^2}{m_e(
u_m + i\omega)}
$$



**3. Sheath Physics**

The plasma sheath is a thin, ion-rich region at the wafer surface that accelerates ions for bombardment.

**3.1 Bohm Criterion**

Ions must reach the sheath edge with minimum velocity:

$$
v_B = \sqrt{\frac{k_B T_e}{M_i}}
$$

Where:

- $k_B$ — Boltzmann constant (1.38 × 10⁻²³ J K⁻¹)
- $T_e$ — Electron temperature (K or eV)
- $M_i$ — Ion mass (kg)

**3.2 Child-Langmuir Law**

Maximum ion current density through a collisionless sheath:

$$
J_{CL} = \frac{4\varepsilon_0}{9}\sqrt{\frac{2e}{M_i}} \cdot \frac{V_s^{3/2}}{d^2}
$$

Where:

- $V_s$ — Sheath voltage (V)
- $d$ — Sheath thickness (m)
- $\varepsilon_0$ — Permittivity of free space (8.85 × 10⁻¹² F m⁻¹)

**3.3 Sheath Thickness**

Approximate expression:

$$
d \approx \lambda_D \cdot \left(\frac{2eV_s}{k_B T_e}\right)^{3/4}
$$

Where Debye length:

$$
\lambda_D = \sqrt{\frac{\varepsilon_0 k_B T_e}{n_e e^2}}
$$

**3.4 Ion Energy Distribution Function (IEDF)**

The IEDF depends critically on the ratio:

$$
\xi = \frac{\tau_{ion}}{\tau_{RF}} = \frac{\omega_{RF} \cdot d}{v_B}
$$

Where:

- **$\xi \gg 1$ (high frequency):** Ions see time-averaged sheath voltage → narrow IEDF
- **$\xi \ll 1$ (low frequency):** Ions respond to instantaneous voltage → bimodal IEDF

**Bimodal IEDF Expression**

For RF sheaths:

$$
f(E) \propto \frac{1}{\sqrt{(E - E_{min})(E_{max} - E)}}
$$

With:

- $E_{max} = e(V_{dc} + V_{rf})$
- $E_{min} = e(V_{dc} - V_{rf})$

**3.5 Collisional Sheath Effects**

When $d > \lambda_{mfp}$ (ion mean free path), ion-neutral collisions broaden the IEDF:

$$
f(E) \propto E \cdot \exp\left(-\frac{E}{\bar{E}}\right)
$$



**4. Surface Reaction Kinetics**

**4.1 General Etch Rate Model**

$$
ER = \underbrace{Y_{phys}(E,\theta) \cdot \Gamma_{ion}}_{\text{Physical sputtering}} + \underbrace{Y_{chem} \cdot \Gamma_R \cdot \theta_{ads} \cdot f(E_{ion})}_{\text{Ion-enhanced chemistry}}
$$

Where:

- $ER$ — Etch rate (nm min⁻¹ or Å s⁻¹)
- $Y_{phys}$ — Physical sputtering yield (atoms/ion)
- $Y_{chem}$ — Chemical etch yield coefficient
- $\Gamma$ — Flux (m⁻² s⁻¹)
- $\theta_{ads}$ — Surface coverage fraction (0–1)
- $f(E_{ion})$ — Ion enhancement function

**4.2 Physical Sputtering Yield**

**Sigmund Theory**

For normal incidence:

$$
Y_0(E) = \frac{3\alpha}{4\pi^2 U_s} \cdot \frac{4M_1 M_2}{(M_1 + M_2)^2} \cdot E
$$

Where:

- $U_s$ — Surface binding energy (eV)
- $M_1$, $M_2$ — Ion and target atom masses
- $\alpha$ — Dimensionless parameter (~0.2–0.4)

**Threshold Energy**

Sputtering occurs only above threshold:

$$
E_{th} \approx \frac{(M_1 + M_2)^2}{4M_1 M_2} \cdot U_s
$$

**4.3 Angular Dependence of Sputtering**

Yamamura formula:

$$
Y(\theta) = Y_0 \cdot \cos^{-f}(\theta) \cdot \exp\left[-b\left(\frac{1}{\cos\theta} - 1\right)\right]
$$

Where:

- $\theta$ — Ion incidence angle from surface normal
- $f$ — Fitting parameter (~1.5–2.5)
- $b$ — Fitting parameter (~0.1–0.5)

**Physical interpretation:**

- $\cos^{-f}(\theta)$ term: Enhanced yield at grazing angles (energy deposited closer to surface)
- $\exp[-b(\cdot)]$ term: Suppression at very grazing angles (reflection)

**4.4 Surface Coverage Dynamics**

Langmuir adsorption kinetics:

$$
\frac{d\theta}{dt} = \underbrace{s \cdot \Gamma_R (1-\theta)}_{\text{Adsorption}} - \underbrace{k_d \cdot \theta}_{\text{Thermal desorption}} - \underbrace{k_{react} \cdot \theta \cdot \Gamma_{ion}}_{\text{Ion-induced reaction}}
$$

Where:

- $s$ — Sticking coefficient (0–1)
- $k_d = 
u_0 \exp(-E_d/k_B T)$ — Desorption rate
- $
u_0$ — Attempt frequency (~10¹³ s⁻¹)
- $E_d$ — Desorption activation energy (eV)

**Steady-State Coverage**

$$
\theta_{ss} = \frac{s \cdot \Gamma_R}{s \cdot \Gamma_R + k_d + k_{react} \cdot \Gamma_{ion}}
$$

**4.5 Ion-Enhanced Etching Mechanisms**

**Damage Model**

Ion bombardment creates reactive sites:

$$
ER = k \cdot [\text{Damage}] \cdot \Gamma_R
$$

$$
[\text{Damage}] = \frac{Y_d \cdot \Gamma_{ion}}{k_{anneal} + k_{react} \cdot \Gamma_R}
$$

**Chemically Enhanced Physical Sputtering**

Product species have lower binding energy:

$$
Y_{eff} = Y_{substrate} \cdot (1 - \theta) + Y_{product} \cdot \theta
$$

Where typically $Y_{product} > Y_{substrate}$.

**4.6 Silicon Etching in Fluorine Plasmas**

Simplified mechanism:

1. **Adsorption:** $F_{(g)} + Si^* \rightarrow SiF_{(ads)}$
2. **Fluorination:** $SiF_{(ads)} + F \rightarrow SiF_2 \rightarrow SiF_3 \rightarrow SiF_4$
3. **Desorption:** $SiF_4 \xrightarrow{ion} SiF_4 (g)\uparrow$

Etch rate expression:

$$
ER_{Si} = \frac{N_0}{\rho_{Si}} \left[ k_s \cdot \Gamma_F \cdot \theta_F + Y_{ion} \cdot \Gamma_{ion} \right]
$$

Where:

- $N_0$ — Avogadro's number
- $\rho_{Si}$ — Silicon atomic density (5 × 10²² cm⁻³)

**4.7 Oxide Etching in Fluorocarbon Plasmas**

More complex due to polymer competition:

$$
ER_{ox} = k_{etch} \cdot \Gamma_{ion} \cdot E_{ion}^n \cdot \exp\left(-\frac{t_{poly}}{t_0}\right)
$$

Where:

- $t_{poly}$ — Polymer thickness
- Balance between etching and deposition determines regime

**Regime boundaries:**

- High F/C ratio → Etching dominant
- Low F/C ratio → Deposition dominant (polymerization)



**5. Feature-Scale Modeling**

**5.1 Level Set Method**

The surface is represented implicitly as the zero level set of $\phi(\vec{x}, t)$:

$$
\phi(\vec{x}, t) = \begin{cases}
< 0 & \text{inside material} \\
= 0 & \text{surface} \\
> 0 & \text{outside (plasma/vacuum)}
\end{cases}
$$

**Evolution Equation**

$$
\frac{\partial \phi}{\partial t} + V_n |
abla \phi| = 0
$$

Where $V_n$ is the velocity in the normal direction:

$$
\vec{n} = \frac{
abla \phi}{|
abla \phi|}
$$

**Advantages**

- Handles topological changes naturally (merging, splitting)
- No explicit surface tracking required
- Curvature easily computed: $\kappa = 
abla \cdot \vec{n}$

**5.2 Flux Calculation at Surface Points**

Local etch velocity depends on incident fluxes:

$$
V_n(\vec{x}) = \Omega \cdot \left[ Y_{phys} \cdot \Gamma_{ion}(\vec{x}) + Y_{chem} \cdot \Gamma_R(\vec{x}) \cdot \theta(\vec{x}) \right]
$$

Where $\Omega$ is the atomic volume.

**5.3 Knudsen Transport in High Aspect Ratio Features**

At low pressure, neutral mean free path > feature dimensions → **free molecular flow**.

**View Factor Method**

Flux at surface point P:

$$
\Gamma(P) = \Gamma_0 \cdot \Omega(P) + \int_{\text{visible}} \Gamma(P') \cdot K(P', P) \, dA'
$$

Where:

- $\Gamma_0$ — Flux from plasma (at feature opening)
- $\Omega(P)$ — Solid angle subtended by opening at P
- $K(P', P)$ — Kernel for re-emission from P' to P

**Cosine Re-emission Law**

For diffuse reflection:

$$
K(P', P) = \frac{\cos\theta' \cos\theta}{\pi r^2} \cdot (1 - s)
$$

Where:

- $\theta'$, $\theta$ — Angles from surface normals
- $r$ — Distance between points
- $s$ — Sticking coefficient

**5.4 Clausing Factor for Tubes**

Transmission probability through a cylindrical hole:

$$
W = \frac{1}{1 + \frac{3L}{8r}}
$$

Where $L$ = length, $r$ = radius.

For aspect ratio $AR = L/(2r)$:

$$
W \approx \frac{1}{1 + \frac{3}{4}AR}
$$

**5.5 Aspect Ratio Dependent Etching (ARDE)**

Empirical model:

$$
\frac{ER(AR)}{ER_0} = \frac{1}{1 + \beta \cdot AR^n}
$$

Where:

- $ER_0$ — Etch rate at open area
- $\beta$, $n$ — Fitting parameters (typically $n \approx 1$–2)

**Physical causes:**

- Neutral transport limitation (Knudsen diffusion)
- Ion angular distribution effects
- Charging effects in dielectric etching

**5.6 Ion Angular Distribution Effects**

Ions have finite angular spread due to:

- Thermal velocity at sheath edge
- Collisions in sheath
- Non-vertical electric fields

Distribution often modeled as:

$$
f(\theta_{ion}) = \frac{1}{\sqrt{2\pi}\sigma_\theta} \exp\left(-\frac{\theta_{ion}^2}{2\sigma_\theta^2}\right)
$$

Typical $\sigma_\theta \approx 2°$–5°

**5.7 Monte Carlo Feature-Scale Methods**

**Algorithm:**

1. Launch particle from plasma with appropriate energy/angle distribution
2. Track trajectory to surface
3. Evaluate reaction probability based on local conditions
4. If reaction occurs, remove material; else reflect particle
5. Repeat for statistical convergence
6. Advance surface based on accumulated removal

**Advantages:**

- Naturally handles stochastic effects
- Easy to incorporate complex physics
- Parallelizable



**6. Equipment-Scale Transport**

**6.1 Gas Flow Regimes**

Characterized by Knudsen number:

$$
Kn = \frac{\lambda}{L}
$$

Where $\lambda$ is mean free path, $L$ is characteristic length.

| Kn Range | Regime | Model |
|----------|--------|-------|
| $< 0.01$ | Continuum | Navier-Stokes |
| $0.01$–$0.1$ | Slip flow | Modified N-S |
| $0.1$–$10$ | Transitional | DSMC |
| $> 10$ | Free molecular | Kinetic theory |

**6.2 Navier-Stokes Equations**

**Continuity:**

$$
\frac{\partial \rho}{\partial t} + 
abla \cdot (\rho \vec{v}) = 0
$$

**Momentum:**

$$
\rho \left( \frac{\partial \vec{v}}{\partial t} + \vec{v} \cdot 
abla \vec{v} \right) = -
abla p + \mu 
abla^2 \vec{v} + \frac{\mu}{3} 
abla(
abla \cdot \vec{v})
$$

**Energy:**

$$
\rho c_p \left( \frac{\partial T}{\partial t} + \vec{v} \cdot 
abla T \right) = 
abla \cdot (k 
abla T) + \Phi + Q_{source}
$$

Where $\Phi$ is viscous dissipation.

**6.3 Slip Boundary Conditions**

For Knudsen numbers 0.01–0.1:

$$
v_{slip} = \frac{2 - \sigma_v}{\sigma_v} \lambda \left. \frac{\partial v}{\partial n} \right|_{wall}
$$

$$
T_{slip} - T_{wall} = \frac{2 - \sigma_T}{\sigma_T} \frac{2\gamma}{\gamma + 1} \frac{\lambda}{Pr} \left. \frac{\partial T}{\partial n} \right|_{wall}
$$

Where $\sigma_v$, $\sigma_T$ are accommodation coefficients.

**6.4 Wafer Temperature Model**

Energy balance at wafer surface:

$$
\rho c_p t_w \frac{\partial T_w}{\partial t} = Q_{ion} + Q_{chem} - Q_{rad} - Q_{cond}
$$

Components:

- **Ion bombardment:** $Q_{ion} = \Gamma_{ion} \cdot E_{ion}$

- **Chemical reactions:** $Q_{chem} = \Gamma_{etch} \cdot \Delta H_{rxn}$

- **Radiation:** $Q_{rad} = \varepsilon \sigma (T_w^4 - T_{wall}^4)$

- **Conduction to chuck:** $Q_{cond} = h_c (T_w - T_{chuck})$

The contact conductance $h_c$ depends on:
- Backside gas pressure
- Surface roughness
- Clamping force

**6.5 Uniformity Modeling**

Radial etch rate profile:

$$
ER(r) = ER_0 \cdot \left[ 1 + \sum_{n=1}^{N} a_n \left( \frac{r}{R_w} \right)^{2n} \right]
$$

Where $R_w$ is wafer radius.

**Uniformity metric:**

$$
\text{Uniformity} = \frac{ER_{max} - ER_{min}}{2 \cdot ER_{avg}} \times 100\%
$$

**6.6 Loading Effect**

Etch rate depends on exposed area:

$$
ER = \frac{ER_0}{1 + \beta \cdot A_{exposed}}
$$

Or in terms of pattern density $\rho_p$:

$$
ER(\rho_p) = ER_0 \cdot \frac{1 - \rho_p}{1 - \rho_p + \rho_p \cdot \frac{ER_0}{ER_{max}}}
$$



**7. Multiscale Coupling**

**7.1 Scale Hierarchy**

| Scale | Dimension | Time | Physics |
|-------|-----------|------|---------|
| Equipment | ~0.5 m | ms–s | Gas flow, power |
| Plasma | ~cm | μs–ms | Species transport |
| Sheath | ~100 μm | ns–μs | Ion acceleration |
| Feature | ~10–100 nm | s–min | Profile evolution |
| Surface | ~nm | ps–ns | Adsorption, reaction |

**7.2 Coupling Strategies**

**Hierarchical Approach**

1. Solve equipment-scale flow → boundary conditions for plasma
2. Solve plasma model → fluxes to sheath
3. Solve sheath model → IEDF to surface
4. Solve feature-scale model → local etch rates

**Embedded Multiscale**

Feature-scale model embedded in equipment simulation:
- Sample representative features across wafer
- Compute local etch rates from local plasma conditions
- Interpolate for full wafer prediction

**7.3 Reduced-Order Models**

**Plasma model simplification:**

$$
n_e(P, W) = n_0 \cdot \left( \frac{P}{P_0} \right)^a \cdot \left( \frac{W}{W_0} \right)^b
$$

Where P is pressure, W is power.

**Response surfaces:**

$$
ER = \beta_0 + \sum_i \beta_i x_i + \sum_i \sum_j \beta_{ij} x_i x_j + \sum_i \beta_{ii} x_i^2
$$



**8. Process Control Mathematics**

**8.1 Run-to-Run (R2R) Control**

**EWMA Controller**

$$
u_k = u_{k-1} + K \cdot (y_{target} - y_{k-1})
$$

Where:

- $u_k$ — Recipe parameter at run $k$
- $y_k$ — Measured output at run $k$
- $K$ — Controller gain

**Double EWMA (for drift)**

$$
\hat{y}_{k+1} = \alpha y_k + (1-\alpha)\hat{y}_k
$$

$$
\hat{d}_{k+1} = \beta(\hat{y}_{k+1} - \hat{y}_k) + (1-\beta)\hat{d}_k
$$

$$
u_{k+1} = u_k - G(\hat{y}_{k+1} + \hat{d}_{k+1} - y_{target})
$$

**8.2 Model Predictive Control (MPC)**

Optimize over horizon N:

$$
\min_{u_k, ..., u_{k+N-1}} J = \sum_{i=1}^{N} \left[ \| y_{k+i} - y_{ref} \|_Q^2 + \| \Delta u_{k+i-1} \|_R^2 \right]
$$

Subject to:

- Process model: $y_{k+1} = f(y_k, u_k)$
- Input constraints: $u_{min} \leq u \leq u_{max}$
- Output constraints: $y_{min} \leq y \leq y_{max}$
- Rate constraints: $|\Delta u| \leq \Delta u_{max}$

**8.3 Virtual Metrology**

Predict wafer-level results from equipment data:

$$
\hat{y} = f(\vec{x}_{sensor})
$$

Where $\vec{x}_{sensor}$ includes:

- Optical emission spectroscopy (OES) signals
- RF impedance (voltage, current, phase)
- Pressure, flow rates
- Chamber wall temperature
- Endpoint detection signals

**PLS (Partial Least Squares) Model**

$$
\hat{y} = \vec{x}^T \cdot \vec{\beta}_{PLS}
$$

**Neural Network Model**

$$
\hat{y} = W_2 \cdot \sigma(W_1 \cdot \vec{x} + \vec{b}_1) + b_2
$$

**8.4 Fault Detection and Classification (FDC)**

**Hotelling's T² Statistic**

$$
T^2 = (\vec{x} - \vec{\mu})^T \Sigma^{-1} (\vec{x} - \vec{\mu})
$$

Alarm if $T^2 > T^2_{critical}(\alpha, p, n)$

**Q-Statistic (SPE)**

$$
Q = \|\vec{x} - \hat{\vec{x}}\|^2
$$

Where $\hat{\vec{x}}$ is PCA reconstruction.

**8.5 Endpoint Detection**

**OES Endpoint**

Monitor emission intensity ratio:

$$
R(t) = \frac{I_{\lambda_1}(t)}{I_{\lambda_2}(t)}
$$

Endpoint when:

$$
\left| \frac{dR}{dt} \right| > \text{threshold}
$$



**9. Emerging Frontiers**

**9.1 Atomic Layer Etching (ALE)**

Self-limiting process:

1. **Modification step:** Surface layer modified (e.g., chlorination)
2. **Removal step:** Modified layer removed by low-energy ions

$$
EPC = \Gamma_{sat} \cdot \delta_{modified}
$$

Where:

- $EPC$ — Etch per cycle (typically 0.5–2 Å)
- $\Gamma_{sat}$ — Saturation coverage
- $\delta_{modified}$ — Modified layer thickness

**Synergy Parameter**

$$
S = \frac{EPC_{ALE}}{EPC_{continuous}}
$$

High synergy indicates good self-limiting behavior.

**9.2 Machine Learning Integration**

**Physics-Informed Neural Networks (PINNs)**

Loss function includes physics constraints:

$$
\mathcal{L} = \mathcal{L}_{data} + \lambda \cdot \mathcal{L}_{physics}
$$

Where:

$$
\mathcal{L}_{physics} = \left\| \frac{\partial n}{\partial t} + 
abla \cdot \vec{\Gamma} - S \right\|^2
$$

**Gaussian Process Regression**

For process optimization with uncertainty quantification:

$$
f(\vec{x}) \sim \mathcal{GP}(m(\vec{x}), k(\vec{x}, \vec{x}'))
$$

Posterior mean:

$$
\bar{f}(\vec{x}_*) = \vec{k}_*^T (K + \sigma_n^2 I)^{-1} \vec{y}
$$

**9.3 Stochastic Effects at Nanoscale**

**Line Edge Roughness (LER)**

At sub-10 nm features, discrete nature of reactions matters:

$$
\sigma_{LER}^2 = \frac{a^3}{L} \cdot \left( \frac{1}{\Gamma_{ion}} + \frac{1}{\Gamma_R \cdot s} \right)
$$

Where $a$ is atomic spacing, $L$ is line length.

**Kinetic Monte Carlo (KMC)**

Event selection probability:

$$
P_i = \frac{r_i}{\sum_j r_j}
$$

Time advance:

$$
\Delta t = -\frac{\ln(u)}{\sum_j r_j}
$$

Where $u \in (0,1)$ is uniform random.



**Physical Constants**

| Constant | Symbol | Value |
|----------|--------|-------|
| Boltzmann constant | $k_B$ | $1.38 \times 10^{-23}$ J K⁻¹ |
| Elementary charge | $e$ | $1.60 \times 10^{-19}$ C |
| Electron mass | $m_e$ | $9.11 \times 10^{-31}$ kg |
| Permittivity of free space | $\varepsilon_0$ | $8.85 \times 10^{-12}$ F m⁻¹ |
| Avogadro's number | $N_A$ | $6.02 \times 10^{23}$ mol⁻¹ |
| Stefan-Boltzmann constant | $\sigma$ | $5.67 \times 10^{-8}$ W m⁻² K⁻⁴ |



**Typical Process Parameters**

| Parameter | Typical Range | Units |
|-----------|---------------|-------|
| Pressure | 1–100 | mTorr |
| RF Power | 100–2000 | W |
| Bias Voltage | 50–500 | V |
| Electron Temperature | 2–5 | eV |
| Electron Density | 10⁹–10¹² | cm⁻³ |
| Ion Energy | 50–500 | eV |
| Etch Rate | 50–500 | nm min⁻¹ |

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/etch-equipment) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
