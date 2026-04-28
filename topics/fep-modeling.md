# Mathematical Modeling of Epitaxy in Semiconductor Front-End Processing (FEP)

**Keywords**: fep modeling, front end processing, feol, ion implantation, diffusion modeling, oxidation modeling, dopant activation, junction formation, thermal processing, annealing

---

**Mathematical Modeling of Epitaxy in Semiconductor Front-End Processing (FEP)**


**1. Overview**

Epitaxy is a critical **Front-End Process (FEP)** step where crystalline films are grown on crystalline substrates with precise control of:

- Thickness
- Composition
- Doping concentration
- Defect density

Mathematical modeling enables:

- Process optimization
- Defect prediction
- Virtual fabrication
- Equipment design

**1.1 Types of Epitaxy**

- **Homoepitaxy**: Same material as substrate (e.g., Si on Si)
- **Heteroepitaxy**: Different material from substrate (e.g., GaAs on Si, SiGe on Si)

**1.2 Epitaxy Methods**

- **Vapor Phase Epitaxy (VPE)** / Chemical Vapor Deposition (CVD)
  - Atmospheric Pressure CVD (APCVD)
  - Low Pressure CVD (LPCVD)
  - Metal-Organic CVD (MOCVD)
- **Molecular Beam Epitaxy (MBE)**
- **Liquid Phase Epitaxy (LPE)**
- **Solid Phase Epitaxy (SPE)**



**2. Fundamental Thermodynamic Framework**

**2.1 Driving Force for Growth**

The supersaturation provides the thermodynamic driving force:

$$
\Delta \mu = k_B T \ln\left(\frac{P}{P_{eq}}\right)
$$

Where:

- $\Delta \mu$ = chemical potential difference (driving force)
- $k_B$ = Boltzmann's constant ($1.38 \times 10^{-23}$ J/K)
- $T$ = absolute temperature (K)
- $P$ = actual partial pressure of precursor
- $P_{eq}$ = equilibrium vapor pressure

**2.2 Free Energy of Mixing (Multi-component Systems)**

For systems like SiGe alloys:

$$
\Delta G_{mix} = RT\left(x \ln x + (1-x) \ln(1-x)\right) + \Omega x(1-x)
$$

Where:

- $R$ = universal gas constant (8.314 J/mol·K)
- $x$ = mole fraction of component
- $\Omega$ = interaction parameter (regular solution model)

**2.3 Gibbs Free Energy of Formation**

$$
\Delta G = \Delta H - T\Delta S
$$

For spontaneous growth: $\Delta G < 0$



**3. Growth Rate Kinetics**

**3.1 The Two-Regime Model**

Epitaxial growth rate is governed by two competing mechanisms:

**Overall growth rate equation:**

$$
G = \frac{k_s \cdot h_g \cdot C_g}{k_s + h_g}
$$

Where:

- $G$ = growth rate (nm/min or μm/min)
- $k_s$ = surface reaction rate constant
- $h_g$ = gas-phase mass transfer coefficient
- $C_g$ = gas-phase reactant concentration

**3.2 Temperature Dependence**

The surface reaction rate follows Arrhenius behavior:

$$
k_s = A \exp\left(-\frac{E_a}{k_B T}\right)
$$

Where:

- $A$ = pre-exponential factor (frequency factor)
- $E_a$ = activation energy (eV or J/mol)

**3.3 Growth Rate Regimes**

| Temperature Regime | Limiting Factor | Growth Rate Expression | Temperature Dependence |
|:-------------------|:----------------|:-----------------------|:-----------------------|
| **Low T** | Surface reaction | $G \approx k_s \cdot C_g$ | Strong (exponential) |
| **High T** | Mass transport | $G \approx h_g \cdot C_g$ | Weak (~$T^{1.5-2}$) |

**3.4 Boundary Layer Analysis**

For horizontal CVD reactors, the boundary layer thickness evolves as:

$$
\delta(x) = \sqrt{\frac{
u \cdot x}{v_{\infty}}}
$$

Where:

- $\delta(x)$ = boundary layer thickness at position $x$
- $
u$ = kinematic viscosity (m²/s)
- $x$ = distance from gas inlet (m)
- $v_{\infty}$ = free stream gas velocity (m/s)

The mass transfer coefficient:

$$
h_g = \frac{D_{gas}}{\delta}
$$

Where $D_{gas}$ is the gas-phase diffusion coefficient.



**4. Surface Kinetics: BCF Theory**

The **Burton-Cabrera-Frank (BCF) model** describes atomic-scale growth mechanisms.

**4.1 Surface Diffusion Equation**

$$
D_s 
abla^2 n_s - \frac{n_s - n_{eq}}{\tau_s} + J_{ads} = 0
$$

Where:

- $n_s$ = adatom surface density (atoms/cm²)
- $D_s$ = surface diffusion coefficient (cm²/s)
- $n_{eq}$ = equilibrium adatom density
- $\tau_s$ = mean adatom lifetime before desorption (s)
- $J_{ads}$ = adsorption flux (atoms/cm²·s)

**4.2 Characteristic Diffusion Length**

$$
\lambda_s = \sqrt{D_s \tau_s}
$$

This parameter determines the growth mode:

- **Step-flow growth**: $\lambda_s > L$ (terrace width)
- **2D nucleation growth**: $\lambda_s < L$

**4.3 Surface Diffusion Coefficient**

$$
D_s = D_0 \exp\left(-\frac{E_m}{k_B T}\right)
$$

Where:

- $D_0$ = pre-exponential factor (~$10^{-3}$ cm²/s)
- $E_m$ = migration energy barrier (eV)

**4.4 Step Velocity**

$$
v_{step} = \frac{2 D_s (n_s - n_{eq})}{\lambda_s} \tanh\left(\frac{L}{2\lambda_s}\right)
$$

Where $L$ is the inter-step spacing (terrace width).

**4.5 Growth Rate from Step Flow**

$$
G = \frac{v_{step} \cdot h_{step}}{L}
$$

Where $h_{step}$ is the step height (monolayer thickness).



**5. Heteroepitaxy and Strain Modeling**

**5.1 Lattice Mismatch**

$$
f = \frac{a_{film} - a_{substrate}}{a_{substrate}}
$$

Where:

- $f$ = lattice mismatch (dimensionless, often expressed as %)
- $a_{film}$ = lattice constant of film material
- $a_{substrate}$ = lattice constant of substrate

**Example values:**

| System | Lattice Mismatch |
|:-------|:-----------------|
| Si₀.₇Ge₀.₃ on Si | ~1.2% |
| Ge on Si | ~4.2% |
| GaAs on Si | ~4.0% |
| InAs on GaAs | ~7.2% |
| GaN on Sapphire | ~16% |

**5.2 Strain Components**

For biaxial strain in (001) films:

$$
\varepsilon_{xx} = \varepsilon_{yy} = \varepsilon_{\parallel} = \frac{a_s - a_f}{a_f} \approx -f
$$

$$
\varepsilon_{zz} = \varepsilon_{\perp} = -\frac{2C_{12}}{C_{11}} \varepsilon_{\parallel}
$$

Where $C_{11}$ and $C_{12}$ are elastic constants.

**5.3 Elastic Energy**

For a coherently strained film:

$$
E_{elastic} = \frac{2G(1+
u)}{1-
u} f^2 h = M f^2 h
$$

Where:

- $G$ = shear modulus (Pa)
- $
u$ = Poisson's ratio
- $h$ = film thickness
- $M$ = biaxial modulus = $\frac{2G(1+
u)}{1-
u}$

**5.4 Critical Thickness (Matthews-Blakeslee)**

$$
h_c = \frac{b}{8\pi f(1+
u)} \left[\ln\left(\frac{h_c}{b}\right) + 1\right]
$$

Where:

- $h_c$ = critical thickness for dislocation formation
- $b$ = Burgers vector magnitude
- $f$ = lattice mismatch
- $
u$ = Poisson's ratio

**5.5 People-Bean Approximation (for SiGe)**

Empirical formula:

$$
h_c \approx \frac{0.55}{f^2} \text{ (nm, with } f \text{ as a decimal)}
$$

Or equivalently:

$$
h_c \approx \frac{5500}{x^2} \text{ (nm, for Si}_{1-x}\text{Ge}_x\text{)}
$$

**5.6 Threading Dislocation Density**

Above critical thickness, dislocation density evolves:

$$
\rho_{TD}(h) = \rho_0 \exp\left(-\frac{h}{h_0}\right) + \rho_{\infty}
$$

Where:

- $\rho_{TD}$ = threading dislocation density (cm⁻²)
- $\rho_0$ = initial density
- $h_0$ = characteristic decay length
- $\rho_{\infty}$ = residual density



**6. Reactor-Scale Modeling**

**6.1 Coupled Transport Equations**

**6.1.1 Momentum Conservation (Navier-Stokes)**

$$
\rho\left(\frac{\partial \mathbf{v}}{\partial t} + \mathbf{v} \cdot 
abla \mathbf{v}\right) = -
abla p + \mu 
abla^2 \mathbf{v} + \rho \mathbf{g}
$$

Where:

- $\rho$ = gas density (kg/m³)
- $\mathbf{v}$ = velocity vector (m/s)
- $p$ = pressure (Pa)
- $\mu$ = dynamic viscosity (Pa·s)
- $\mathbf{g}$ = gravitational acceleration

**6.1.2 Continuity Equation**

$$
\frac{\partial \rho}{\partial t} + 
abla \cdot (\rho \mathbf{v}) = 0
$$

**6.1.3 Species Transport**

$$
\frac{\partial C_i}{\partial t} + \mathbf{v} \cdot 
abla C_i = D_i 
abla^2 C_i + R_i
$$

Where:

- $C_i$ = concentration of species $i$ (mol/m³)
- $D_i$ = diffusion coefficient of species $i$ (m²/s)
- $R_i$ = net reaction rate (mol/m³·s)

**6.1.4 Energy Conservation**

$$
\rho c_p \left(\frac{\partial T}{\partial t} + \mathbf{v} \cdot 
abla T\right) = k 
abla^2 T + \sum_j \Delta H_j r_j
$$

Where:

- $c_p$ = specific heat capacity (J/kg·K)
- $k$ = thermal conductivity (W/m·K)
- $\Delta H_j$ = enthalpy of reaction $j$ (J/mol)
- $r_j$ = rate of reaction $j$ (mol/m³·s)

**6.2 Silicon CVD Chemistry**

**6.2.1 From Silane (SiH₄)**

**Gas phase decomposition:**

$$
\text{SiH}_4 \xrightarrow{k_1} \text{SiH}_2 + \text{H}_2
$$

**Surface reaction:**

$$
\text{SiH}_2(g) + * \xrightarrow{k_2} \text{Si}(s) + \text{H}_2(g)
$$

Where $*$ denotes a surface site.

**6.2.2 From Dichlorosilane (DCS)**

$$
\text{SiH}_2\text{Cl}_2 \rightarrow \text{SiCl}_2 + \text{H}_2
$$

$$
\text{SiCl}_2 + \text{H}_2 \rightarrow \text{Si}(s) + 2\text{HCl}
$$

**6.2.3 Rate Law**

$$
r_{dep} = k_2 P_{SiH_2} (1 - \theta)
$$

Where:

- $P_{SiH_2}$ = partial pressure of SiH₂
- $\theta$ = surface site coverage

**6.3 Dimensionless Numbers**

| Number | Definition | Physical Meaning |
|:-------|:-----------|:-----------------|
| Reynolds | $Re = \frac{\rho v L}{\mu}$ | Inertia vs. viscous forces |
| Prandtl | $Pr = \frac{\mu c_p}{k}$ | Momentum vs. thermal diffusivity |
| Schmidt | $Sc = \frac{\mu}{\rho D}$ | Momentum vs. mass diffusivity |
| Damköhler | $Da = \frac{k_s L}{D}$ | Reaction rate vs. diffusion rate |
| Grashof | $Gr = \frac{g \beta \Delta T L^3}{
u^2}$ | Buoyancy vs. viscous forces |



**7. Selective Epitaxial Growth (SEG) Modeling**

**7.1 Overview**

In SEG, growth occurs on exposed Si but **not** on dielectric (SiO₂/Si₃N₄).

**7.2 Loading Effect Model**

$$
G_{local} = G_0 \left(1 + \alpha \cdot \frac{A_{mask}}{A_{Si}}\right)
$$

Where:

- $G_{local}$ = local growth rate
- $G_0$ = baseline growth rate
- $\alpha$ = pattern sensitivity factor
- $A_{mask}$ = dielectric (mask) area
- $A_{Si}$ = exposed silicon area

**7.3 Pattern-Dependent Growth**

Sources of non-uniformity:

- Local depletion of reactants over Si regions
- Species reflected/desorbed from mask contribute to nearby Si
- Gas-phase diffusion length effects

**7.4 Selectivity Condition**

For selective growth on Si vs. oxide:

$$
r_{deposition,Si} > 0 \quad \text{and} \quad r_{deposition,oxide} < r_{etching,oxide}
$$

**Achieved by adding HCl:**

$$
\text{Si}(nuclei) + 2\text{HCl} \rightarrow \text{SiCl}_2 + \text{H}_2
$$

Nuclei on oxide are etched before they can grow, maintaining selectivity.

**7.5 Faceting Model**

Growth rate depends on crystallographic orientation:

$$
G_{(hkl)} = G_0 \cdot f(hkl) \cdot \exp\left(-\frac{E_{a,(hkl)}}{k_B T}\right)
$$

Typical growth rate hierarchy:

$$
G_{(100)} > G_{(110)} > G_{(111)}
$$



**8. Dopant Incorporation**

**8.1 Segregation Coefficient**

**Equilibrium segregation coefficient:**

$$
k_0 = \frac{C_{solid}}{C_{liquid/gas}}
$$

**Effective segregation coefficient:**

$$
k_{eff} = \frac{k_0}{k_0 + (1-k_0)\exp\left(-\frac{G\delta}{D_l}\right)}
$$

Where:

- $k_0$ = equilibrium segregation coefficient
- $G$ = growth rate
- $\delta$ = boundary layer thickness
- $D_l$ = diffusivity in liquid/gas phase

**8.2 Dopant Concentration in Film**

$$
C_{film} = k_{eff} \cdot C_{gas}
$$

**8.3 Dopant Profile Abruptness**

The transition width is limited by:

- **Surface segregation length**: $\lambda_{seg}$
- **Diffusion during growth**: $L_D = \sqrt{D \cdot t}$
- **Autodoping** from substrate

$$
\Delta z_{transition} \approx \sqrt{\lambda_{seg}^2 + L_D^2}
$$

**8.4 Common Dopants for Si Epitaxy**

| Dopant | Type | Precursor | Segregation Behavior |
|:-------|:-----|:----------|:---------------------|
| B | p-type | B₂H₆, BCl₃ | Low segregation |
| P | n-type | PH₃, PCl₃ | Moderate segregation |
| As | n-type | AsH₃ | Strong segregation |
| Sb | n-type | SbH₃ | Very strong segregation |



**9. Atomistic Simulation Methods**

**9.1 Kinetic Monte Carlo (KMC)**

**9.1.1 Event Rates**

Each atomic event has a rate following Arrhenius:

$$
\Gamma_i = 
u_0 \exp\left(-\frac{E_i}{k_B T}\right)
$$

Where:

- $\Gamma_i$ = rate of event $i$ (s⁻¹)
- $
u_0$ = attempt frequency (~10¹²-10¹³ s⁻¹)
- $E_i$ = activation energy for event $i$

**9.1.2 Events Modeled**

- **Adsorption**: $\Gamma_{ads} = \frac{P}{\sqrt{2\pi m k_B T}} \cdot s$
- **Desorption**: $\Gamma_{des} = 
u_0 \exp(-E_{des}/k_B T)$
- **Surface diffusion**: $\Gamma_{diff} = 
u_0 \exp(-E_m/k_B T)$
- **Step attachment**: $\Gamma_{attach}$
- **Step detachment**: $\Gamma_{detach}$

**9.1.3 Time Advancement**

$$
\Delta t = -\frac{\ln(r)}{\Gamma_{total}} = -\frac{\ln(r)}{\sum_i \Gamma_i}
$$

Where $r$ is a uniform random number in $(0,1]$.

**9.2 Density Functional Theory (DFT)**

Provides input parameters for KMC:

- Adsorption energies
- Migration barriers
- Surface reconstruction energetics
- Reaction pathways

**Kohn-Sham equation:**

$$
\left[-\frac{\hbar^2}{2m}
abla^2 + V_{eff}(\mathbf{r})\right]\psi_i(\mathbf{r}) = \varepsilon_i \psi_i(\mathbf{r})
$$

**9.3 Molecular Dynamics (MD)**

**Newton's equations:**

$$
m_i \frac{d^2 \mathbf{r}_i}{dt^2} = -
abla_i U(\mathbf{r}_1, \mathbf{r}_2, ..., \mathbf{r}_N)
$$

Where $U$ is the interatomic potential (e.g., Stillinger-Weber, Tersoff for Si).



**10. Nucleation Theory**

**10.1 Classical Nucleation Theory (CNT)**

**10.1.1 Gibbs Free Energy Change**

$$
\Delta G(r) = -\frac{4}{3}\pi r^3 \cdot \frac{\Delta \mu}{\Omega} + 4\pi r^2 \gamma
$$

Where:

- $r$ = nucleus radius
- $\Delta \mu$ = supersaturation (driving force)
- $\Omega$ = atomic volume
- $\gamma$ = surface energy

**10.1.2 Critical Nucleus Radius**

Setting $\frac{d(\Delta G)}{dr} = 0$:

$$
r^* = \frac{2\gamma \Omega}{\Delta \mu}
$$

**10.1.3 Free Energy Barrier**

$$
\Delta G^* = \frac{16 \pi \gamma^3 \Omega^2}{3 (\Delta \mu)^2}
$$

**10.1.4 Nucleation Rate**

$$
J = Z \beta^* N_s \exp\left(-\frac{\Delta G^*}{k_B T}\right)
$$

Where:

- $J$ = nucleation rate (nuclei/cm²·s)
- $Z$ = Zeldovich factor (~0.01-0.1)
- $\beta^*$ = attachment rate to critical nucleus
- $N_s$ = surface site density

**10.2 Growth Modes**

| Mode | Surface Energy Condition | Growth Behavior | Example |
|:-----|:-------------------------|:----------------|:--------|
| **Frank-van der Merwe** | $\gamma_s \geq \gamma_f + \gamma_{int}$ | Layer-by-layer (2D) | Si on Si |
| **Volmer-Weber** | $\gamma_s < \gamma_f + \gamma_{int}$ | Island (3D) | Metals on oxides |
| **Stranski-Krastanov** | Intermediate | 2D then 3D islands | InAs/GaAs QDs |

**10.3 2D Nucleation**

Critical island size (atoms):

$$
i^* = \frac{\pi \gamma_{step}^2 \Omega}{(\Delta \mu)^2 k_B T}
$$



**11. TCAD Process Simulation**

**11.1 Overview**

Tools: Synopsys Sentaurus Process, Silvaco Victory Process

**11.2 Diffusion-Reaction System**

$$
\frac{\partial C_i}{\partial t} = 
abla \cdot (D_i 
abla C_i - \mu_i C_i 
abla \phi) + G_i - R_i
$$

Where:

- First term: Fickian diffusion
- Second term: Drift in electric field (for charged species)
- $G_i$ = generation rate
- $R_i$ = recombination rate

**11.3 Point Defect Dynamics**

**Vacancy concentration:**

$$
\frac{\partial C_V}{\partial t} = D_V 
abla^2 C_V + G_V - k_{IV} C_I C_V
$$

**Interstitial concentration:**

$$
\frac{\partial C_I}{\partial t} = D_I 
abla^2 C_I + G_I - k_{IV} C_I C_V
$$

Where $k_{IV}$ is the recombination rate constant.

**11.4 Stress Evolution**

**Equilibrium equation:**

$$

abla \cdot \boldsymbol{\sigma} = 0
$$

**Constitutive relation:**

$$
\boldsymbol{\sigma} = \mathbf{C} : (\boldsymbol{\varepsilon} - \boldsymbol{\varepsilon}^{thermal} - \boldsymbol{\varepsilon}^{intrinsic})
$$

Where:

- $\boldsymbol{\sigma}$ = stress tensor
- $\mathbf{C}$ = elastic stiffness tensor
- $\boldsymbol{\varepsilon}$ = total strain
- $\boldsymbol{\varepsilon}^{thermal}$ = thermal strain = $\alpha \Delta T$
- $\boldsymbol{\varepsilon}^{intrinsic}$ = intrinsic strain (lattice mismatch)

**11.5 Level Set Method for Interface Tracking**

$$
\frac{\partial \phi}{\partial t} + v_n |
abla \phi| = 0
$$

Where:

- $\phi$ = level set function (interface at $\phi = 0$)
- $v_n$ = interface normal velocity



**12. Advanced Topics**

**12.1 Atomic Layer Epitaxy (ALE) / Atomic Layer Deposition (ALD)**

Self-limiting surface reactions modeled as Langmuir kinetics:

$$
\theta = \frac{K \cdot P \cdot t}{1 + K \cdot P \cdot t} \rightarrow 1 \quad \text{as } t \rightarrow \infty
$$

**Growth per cycle (GPC):**

$$
GPC = \theta_{sat} \cdot d_{monolayer}
$$

Typical GPC values: 0.5-1.5 Å/cycle

**12.2 III-V on Silicon Integration**

Challenges and models:

- **Anti-phase boundaries (APBs)**: Form at single-step terraces
- **Threading dislocations**: $\rho_{TD} \propto f^2$ initially
- **Thermal mismatch stress**: $\sigma_{thermal} = \frac{E \Delta \alpha \Delta T}{1-
u}$

**12.3 Quantum Dot Formation (Stranski-Krastanov)**

**Critical thickness for islanding:**

$$
h_{SK} \approx \frac{\gamma}{M f^2}
$$

**Island density:**

$$
n_{island} \propto \exp\left(-\frac{E_{island}}{k_B T}\right) \cdot F^{1/3}
$$

Where $F$ is the deposition flux.

**12.4 Machine Learning in Epitaxy Modeling**

**Physics-Informed Neural Networks (PINNs):**

$$
\mathcal{L}_{total} = \mathcal{L}_{data} + \lambda_{PDE}\mathcal{L}_{physics} + \lambda_{BC}\mathcal{L}_{boundary}
$$

Where:

- $\mathcal{L}_{data}$ = data fitting loss
- $\mathcal{L}_{physics}$ = PDE residual loss
- $\mathcal{L}_{boundary}$ = boundary condition loss
- $\lambda$ = weighting parameters

**Applications:**

- Surrogate models for reactor optimization
- Inverse problems (parameter extraction)
- Process window optimization
- Defect prediction



**13. Key Equations**

| Phenomenon | Key Equation | Primary Parameters |
|:-----------|:-------------|:-------------------|
| Growth rate (dual regime) | $G = \frac{k_s h_g C_g}{k_s + h_g}$ | Temperature, pressure, flow |
| Surface diffusion length | $\lambda_s = \sqrt{D_s \tau_s}$ | Temperature |
| Lattice mismatch | $f = \frac{a_f - a_s}{a_s}$ | Material system |
| Critical thickness | $h_c = \frac{b}{8\pi f(1+
u)}\left[\ln\frac{h_c}{b}+1\right]$ | Mismatch, Burgers vector |
| Elastic strain energy | $E = M f^2 h$ | Mismatch, thickness, modulus |
| Nucleation rate | $J \propto \exp(-\Delta G^*/k_BT)$ | Supersaturation, surface energy |
| Species transport | $\frac{\partial C}{\partial t} + \mathbf{v}\cdot
abla C = D
abla^2 C + R$ | Diffusivity, velocity, reactions |
| KMC event rate | $\Gamma = 
u_0 \exp(-E_a/k_BT)$ | Activation energy, temperature |





**Physical Constants**

| Constant | Symbol | Value |
|:---------|:-------|:------|
| Boltzmann constant | $k_B$ | $1.38 \times 10^{-23}$ J/K |
| Gas constant | $R$ | 8.314 J/mol·K |
| Planck constant | $h$ | $6.63 \times 10^{-34}$ J·s |
| Electron charge | $e$ | $1.60 \times 10^{-19}$ C |
| Si lattice constant | $a_{Si}$ | 5.431 Å |
| Ge lattice constant | $a_{Ge}$ | 5.658 Å |
| GaAs lattice constant | $a_{GaAs}$ | 5.653 Å |

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/fep-modeling) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
