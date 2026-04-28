# Semiconductor Manufacturing Process: Physics-Based Modeling and Differential Equations

**Keywords**: physics based modeling and differential equations, physics modeling, differential equations, semiconductor physics, device physics, transport equations, heat transfer equations, process modeling, pde semiconductor

---

**Semiconductor Manufacturing Process: Physics-Based Modeling and Differential Equations**

A comprehensive reference for the physics and mathematics governing semiconductor fabrication processes.



**1. Thermal Oxidation of Silicon**

**1.1 Deal-Grove Model**

The foundational model for silicon oxidation describes oxide thickness growth through coupled transport and reaction.

**Governing Equation:**

$$
x^2 + Ax = B(t + \tau)
$$

**Parameter Definitions:**

- $x$ — oxide thickness
- $A = \frac{2D_{ox}}{k_s}$ — linear rate constant parameter (related to surface reaction)
- $B = \frac{2D_{ox}C^*}{N_1}$ — parabolic rate constant (related to diffusion)
- $D_{ox}$ — oxidant diffusivity through oxide
- $k_s$ — surface reaction rate constant
- $C^*$ — equilibrium oxidant concentration at gas-oxide interface
- $N_1$ — number of oxidant molecules incorporated per unit volume of oxide
- $\tau$ — time shift accounting for initial oxide

**1.2 Underlying Diffusion Physics**

**Steady-state diffusion through the oxide:**

$$
\frac{\partial C}{\partial t} = D_{ox}\frac{\partial^2 C}{\partial x^2}
$$

**Boundary Conditions:**

- **Gas-oxide interface (flux from gas phase):**
  
  $$
  F_1 = h_g(C^* - C_0)
  $$

- **Si-SiO₂ interface (surface reaction):**
  
  $$
  F_2 = k_s C_i
  $$

**Steady-state flux through the oxide:**

$$
F = \frac{D_{ox}C^*}{1 + \frac{k_s}{h_g} + \frac{k_s x}{D_{ox}}}
$$

**1.3 Limiting Growth Regimes**

| Regime | Condition | Growth Law | Physical Interpretation |
|--------|-----------|------------|------------------------|
| **Linear** | Thin oxide ($x \ll A$) | $x \approx \frac{B}{A}(t + \tau)$ | Reaction-limited |
| **Parabolic** | Thick oxide ($x \gg A$) | $x \approx \sqrt{Bt}$ | Diffusion-limited |



**2. Dopant Diffusion**

**2.1 Fick's Laws of Diffusion**

**First Law (Flux Equation):**

$$
\vec{J} = -D
abla C
$$

**Second Law (Mass Conservation / Continuity):**

$$
\frac{\partial C}{\partial t} = 
abla \cdot (D
abla C)
$$

**For constant diffusivity in 1D:**

$$
\frac{\partial C}{\partial t} = D\frac{\partial^2 C}{\partial x^2}
$$

**2.2 Analytical Solutions**

**Constant Surface Concentration (Predeposition)**

Initial condition: $C(x, 0) = 0$  
Boundary condition: $C(0, t) = C_s$

$$
C(x,t) = C_s \cdot \text{erfc}\left(\frac{x}{2\sqrt{Dt}}\right)
$$

where the complementary error function is:

$$
\text{erfc}(z) = 1 - \text{erf}(z) = 1 - \frac{2}{\sqrt{\pi}}\int_0^z e^{-u^2} du
$$

**Fixed Dose / Drive-in (Gaussian Distribution)**

Initial condition: Delta function at surface with dose $Q$

$$
C(x,t) = \frac{Q}{\sqrt{\pi Dt}} \exp\left(-\frac{x^2}{4Dt}\right)
$$

**Key Parameters:**

- $Q$ — total dose per unit area (atoms/cm²)
- $\sqrt{Dt}$ — diffusion length
- Peak concentration: $C_{max} = \frac{Q}{\sqrt{\pi Dt}}$

**2.3 Concentration-Dependent Diffusion**

At high doping concentrations, diffusivity becomes concentration-dependent:

$$
\frac{\partial C}{\partial t} = \frac{\partial}{\partial x}\left[D(C)\frac{\partial C}{\partial x}\right]
$$

**Fair-Tsai Model for Diffusivity:**

$$
D = D_i + D^-\frac{n}{n_i} + D^+\frac{p}{n_i} + D^{++}\left(\frac{p}{n_i}\right)^2
$$

**Parameter Definitions:**

- $D_i$ — intrinsic diffusivity (via neutral defects)
- $D^-$ — diffusivity via negatively charged defects
- $D^+$ — diffusivity via singly positive charged defects
- $D^{++}$ — diffusivity via doubly positive charged defects
- $n, p$ — electron and hole concentrations
- $n_i$ — intrinsic carrier concentration

**2.4 Point Defect Coupled Diffusion**

Modern TCAD uses coupled equations for dopants and point defects (vacancies $V$ and interstitials $I$):

**Vacancy Continuity:**

$$
\frac{\partial C_V}{\partial t} = D_V
abla^2 C_V - k_{IV}C_V C_I + G_V - \frac{C_V - C_V^*}{\tau_V}
$$

**Interstitial Continuity:**

$$
\frac{\partial C_I}{\partial t} = D_I
abla^2 C_I - k_{IV}C_V C_I + G_I - \frac{C_I - C_I^*}{\tau_I}
$$

**Term Definitions:**

- $D_V, D_I$ — diffusion coefficients for vacancies and interstitials
- $k_{IV}$ — recombination rate constant for $V$-$I$ annihilation
- $G_V, G_I$ — generation rates
- $C_V^*, C_I^*$ — equilibrium concentrations
- $\tau_V, \tau_I$ — lifetimes at sinks (surfaces, dislocations)

**Effective Dopant Diffusivity:**

$$
D_{eff} = f_I D_I \frac{C_I}{C_I^*} + f_V D_V \frac{C_V}{C_V^*}
$$

where $f_I$ and $f_V$ are the interstitial and vacancy fractions for the specific dopant species.



**3. Ion Implantation**

**3.1 Range Distribution (LSS Theory)**

The implanted dopant profile follows approximately a Gaussian distribution:

$$
C(x) = \frac{\Phi}{\sqrt{2\pi}\Delta R_p} \exp\left[-\frac{(x - R_p)^2}{2\Delta R_p^2}\right]
$$

**Parameters:**

- $\Phi$ — dose (ions/cm²)
- $R_p$ — projected range (mean implant depth)
- $\Delta R_p$ — straggle (standard deviation of range distribution)

**Higher-Order Moments (Pearson IV Distribution):**

- $\gamma$ — skewness (asymmetry)
- $\beta$ — kurtosis (peakedness)

**3.2 Stopping Power (Energy Loss)**

The rate of energy loss as ions traverse the target:

$$
\frac{dE}{dx} = -N[S_n(E) + S_e(E)]
$$

**Components:**

- $S_n(E)$ — nuclear stopping power (elastic collisions with target nuclei)
- $S_e(E)$ — electronic stopping power (inelastic interactions with electrons)
- $N$ — atomic density of target material (atoms/cm³)

**LSS Electronic Stopping (Low Energy):**

$$
S_e \propto \sqrt{E}
$$

**Nuclear Stopping:** Uses screened Coulomb potentials with Thomas-Fermi or ZBL (Ziegler-Biersack-Littmark) universal screening functions.

**3.3 Boltzmann Transport Equation**

For rigorous treatment (typically solved via Monte Carlo methods):

$$
\frac{\partial f}{\partial t} + \vec{v} \cdot 
abla_r f + \frac{\vec{F}}{m} \cdot 
abla_v f = \left(\frac{\partial f}{\partial t}\right)_{coll}
$$

**Variables:**

- $f(\vec{r}, \vec{v}, t)$ — particle distribution function
- $\vec{F}$ — external force
- Right-hand side — collision integral

**3.4 Damage Accumulation**

**Kinchin-Pease Model:**

$$
N_d = \frac{E_{damage}}{2E_d}
$$

**Parameters:**

- $N_d$ — number of displaced atoms
- $E_{damage}$ — energy available for displacement
- $E_d$ — displacement threshold energy ($\approx 15$ eV for silicon)



**4. Chemical Vapor Deposition (CVD)**

**4.1 Coupled Transport Equations**

**Species Transport (Convection-Diffusion-Reaction):**

$$
\frac{\partial C_i}{\partial t} + \vec{u} \cdot 
abla C_i = D_i
abla^2 C_i + R_i
$$

**Navier-Stokes Equations (Momentum):**

$$
\rho\left(\frac{\partial \vec{u}}{\partial t} + \vec{u} \cdot 
abla\vec{u}\right) = -
abla p + \mu
abla^2\vec{u} + \rho\vec{g}
$$

**Continuity Equation (Incompressible Flow):**

$$

abla \cdot \vec{u} = 0
$$

**Energy Equation:**

$$
\rho c_p\left(\frac{\partial T}{\partial t} + \vec{u} \cdot 
abla T\right) = k
abla^2 T + Q_{reaction}
$$

**Variable Definitions:**

- $C_i$ — concentration of species $i$
- $\vec{u}$ — velocity vector
- $D_i$ — diffusion coefficient of species $i$
- $R_i$ — net reaction rate for species $i$
- $\rho$ — density
- $p$ — pressure
- $\mu$ — dynamic viscosity
- $c_p$ — specific heat at constant pressure
- $k$ — thermal conductivity
- $Q_{reaction}$ — heat of reaction

**4.2 Surface Reaction Kinetics**

**Flux Balance at Wafer Surface:**

$$
h_m(C_b - C_s) = k_s C_s
$$

**Deposition Rate:**

$$
G = \frac{k_s h_m C_b}{k_s + h_m}
$$

**Parameters:**

- $h_m$ — mass transfer coefficient
- $k_s$ — surface reaction rate constant
- $C_b$ — bulk gas concentration
- $C_s$ — surface concentration

**Limiting Cases:**

| Regime | Condition | Rate Expression | Control Mechanism |
|--------|-----------|-----------------|-------------------|
| **Reaction-limited** | $k_s \ll h_m$ | $G \approx k_s C_b$ | Surface chemistry |
| **Transport-limited** | $k_s \gg h_m$ | $G \approx h_m C_b$ | Mass transfer |

**4.3 Step Coverage — Knudsen Diffusion**

In high-aspect-ratio features, molecular (Knudsen) flow dominates:

$$
D_K = \frac{d}{3}\sqrt{\frac{8k_B T}{\pi m}}
$$

**Parameters:**

- $d$ — characteristic feature dimension
- $k_B$ — Boltzmann constant
- $T$ — temperature
- $m$ — molecular mass

**Thiele Modulus (Reaction-Diffusion Balance):**

$$
\phi = L\sqrt{\frac{k_s}{D_K}}
$$

**Interpretation:**

- $\phi \ll 1$ — Reaction-limited → Conformal deposition
- $\phi \gg 1$ — Diffusion-limited → Poor step coverage



**5. Atomic Layer Deposition (ALD)**

**5.1 Surface Site Model**

**Precursor A Adsorption Kinetics:**

$$
\frac{d\theta_A}{dt} = s_0 \frac{P_A}{\sqrt{2\pi m_A k_B T}}(1 - \theta_A) - k_{des}\theta_A
$$

**Parameters:**

- $\theta_A$ — fractional surface coverage of precursor A
- $s_0$ — sticking coefficient
- $P_A$ — partial pressure of precursor A
- $m_A$ — molecular mass of precursor A
- $k_{des}$ — desorption rate constant

**5.2 Growth Per Cycle (GPC)**

$$
GPC = n_{sites} \cdot \Omega \cdot \theta_A^{sat}
$$

**Parameters:**

- $n_{sites}$ — surface site density (sites/cm²)
- $\Omega$ — atomic volume (volume per deposited atom)
- $\theta_A^{sat}$ — saturation coverage achieved during half-cycle


**6. Plasma Etching**

**6.1 Plasma Fluid Equations**

**Electron Continuity:**

$$
\frac{\partial n_e}{\partial t} + 
abla \cdot \vec{\Gamma}_e = S_{ionization} - S_{recomb}
$$

**Ion Continuity:**

$$
\frac{\partial n_i}{\partial t} + 
abla \cdot \vec{\Gamma}_i = S_{ionization} - S_{recomb}
$$

**Drift-Diffusion Flux (Electrons):**

$$
\vec{\Gamma}_e = -n_e\mu_e\vec{E} - D_e
abla n_e
$$

**Drift-Diffusion Flux (Ions):**

$$
\vec{\Gamma}_i = n_i\mu_i\vec{E} - D_i
abla n_i
$$

**Poisson's Equation (Self-Consistent Field):**

$$

abla^2\phi = -\frac{e}{\varepsilon_0}(n_i - n_e)
$$

**Electron Energy Balance:**

$$
\frac{\partial}{\partial t}\left(\frac{3}{2}n_e k_B T_e\right) + 
abla \cdot \vec{q}_e = -e\vec{\Gamma}_e \cdot \vec{E} - \sum_j \epsilon_j R_j
$$

**6.2 Sheath Physics**

**Bohm Criterion (Sheath Edge Condition):**

$$
u_i \geq u_B = \sqrt{\frac{k_B T_e}{M_i}}
$$

**Child-Langmuir Law (Collisionless Sheath Ion Current):**

$$
J = \frac{4\varepsilon_0}{9}\sqrt{\frac{2e}{M_i}}\frac{V_0^{3/2}}{d^2}
$$

**Parameters:**

- $u_i$ — ion velocity at sheath edge
- $u_B$ — Bohm velocity
- $T_e$ — electron temperature
- $M_i$ — ion mass
- $V_0$ — sheath voltage drop
- $d$ — sheath thickness

**6.3 Surface Etch Kinetics**

**Ion-Enhanced Etching Rate:**

$$
R_{etch} = Y_i\Gamma_i + Y_n\Gamma_n(1-\theta) + Y_{syn}\Gamma_i\theta
$$

**Components:**

- $Y_i\Gamma_i$ — physical sputtering contribution
- $Y_n\Gamma_n(1-\theta)$ — spontaneous chemical etching
- $Y_{syn}\Gamma_i\theta$ — ion-enhanced (synergistic) etching

**Yield Parameters:**

- $Y_i$ — physical sputtering yield
- $Y_n$ — spontaneous chemical etch yield
- $Y_{syn}$ — synergistic yield (ion-enhanced chemistry)
- $\Gamma_i, \Gamma_n$ — ion and neutral fluxes
- $\theta$ — fractional surface coverage of reactive species

**Surface Coverage Dynamics:**

$$
\frac{d\theta}{dt} = s\Gamma_n(1-\theta) - Y_{syn}\Gamma_i\theta - k_v\theta
$$

**Terms:**

- $s\Gamma_n(1-\theta)$ — adsorption onto empty sites
- $Y_{syn}\Gamma_i\theta$ — consumption by ion-enhanced reaction
- $k_v\theta$ — thermal desorption/volatilization



**7. Lithography**

**7.1 Aerial Image Formation**

**Hopkins Formulation (Partially Coherent Imaging):**

$$
I(x,y) = \iint TCC(f,g;f',g') \cdot \tilde{M}(f,g) \cdot \tilde{M}^*(f',g') \, df\,dg\,df'\,dg'
$$

**Parameters:**

- $TCC$ — Transmission Cross Coefficient (encapsulates partial coherence)
- $\tilde{M}(f,g)$ — Fourier transform of mask transmission function
- $f, g$ — spatial frequencies

**Rayleigh Resolution Criterion:**

$$
Resolution = k_1 \frac{\lambda}{NA}
$$

**Depth of Focus:**

$$
DOF = k_2 \frac{\lambda}{NA^2}
$$

**Parameters:**

- $k_1, k_2$ — process-dependent factors
- $\lambda$ — exposure wavelength
- $NA$ — numerical aperture

**7.2 Photoresist Exposure — Dill Model**

**Intensity Attenuation with Photobleaching:**

$$
\frac{\partial I}{\partial z} = -\alpha(M)I
$$

where the absorption coefficient depends on PAC concentration:

$$
\alpha = AM + B
$$

**Photoactive Compound (PAC) Decomposition:**

$$
\frac{\partial M}{\partial t} = -CIM
$$

**Dill Parameters:**

| Parameter | Description | Units |
|-----------|-------------|-------|
| $A$ | Bleachable absorption coefficient | μm⁻¹ |
| $B$ | Non-bleachable absorption coefficient | μm⁻¹ |
| $C$ | Exposure rate constant | cm²/mJ |
| $M$ | Relative PAC concentration | dimensionless (0-1) |

**7.3 Chemically Amplified Resists**

**Photoacid Generation:**

$$
\frac{\partial [H^+]}{\partial t} = C \cdot I \cdot [PAG]
$$

**Post-Exposure Bake — Acid Diffusion and Reaction:**

$$
\frac{\partial [H^+]}{\partial t} = D_{acid}
abla^2[H^+] - k_{loss}[H^+]
$$

**Deprotection Reaction (Catalytic Amplification):**

$$
\frac{\partial [Protected]}{\partial t} = -k_{cat}[H^+][Protected]
$$

**Parameters:**

- $[PAG]$ — photoacid generator concentration
- $D_{acid}$ — acid diffusion coefficient
- $k_{loss}$ — acid loss rate (neutralization, evaporation)
- $k_{cat}$ — catalytic deprotection rate constant

**7.4 Development Rate — Mack Model**

$$
R = R_{max}\frac{(a+1)(1-M)^n}{a + (1-M)^n} + R_{min}
$$

**Parameters:**

- $R_{max}$ — maximum development rate (fully exposed)
- $R_{min}$ — minimum development rate (unexposed)
- $a$ — selectivity parameter
- $n$ — contrast parameter
- $M$ — normalized PAC concentration after exposure



**8. Epitaxy**

**8.1 Burton-Cabrera-Frank (BCF) Theory**

**Adatom Diffusion on Terraces:**

$$
\frac{\partial n}{\partial t} = D_s
abla^2 n + F - \frac{n}{\tau}
$$

**Parameters:**

- $n$ — adatom density on terrace
- $D_s$ — surface diffusion coefficient
- $F$ — deposition flux (atoms/cm²·s)
- $\tau$ — adatom lifetime before desorption

**Step Velocity:**

$$
v_{step} = \Omega D_s\left[\left(\frac{\partial n}{\partial x}\right)_+ - \left(\frac{\partial n}{\partial x}\right)_-\right]
$$

**Steady-State Solution for Step Flow:**

$$
v_{step} = \frac{2D_s \lambda_s F}{l} \cdot \tanh\left(\frac{l}{2\lambda_s}\right)
$$

**Parameters:**

- $\Omega$ — atomic volume
- $\lambda_s = \sqrt{D_s \tau}$ — surface diffusion length
- $l$ — terrace width

**8.2 Rate Equations for Island Nucleation**

**Monomer (Single Adatom) Density:**

$$
\frac{dn_1}{dt} = F - 2\sigma_1 D_s n_1^2 - \sum_{j>1}\sigma_j D_s n_1 n_j - \frac{n_1}{\tau}
$$

**Cluster of Size $j$:**

$$
\frac{dn_j}{dt} = \sigma_{j-1}D_s n_1 n_{j-1} - \sigma_j D_s n_1 n_j
$$

**Parameters:**

- $n_j$ — density of clusters containing $j$ atoms
- $\sigma_j$ — capture cross-section for clusters of size $j$


**9. Chemical Mechanical Polishing (CMP)**

**9.1 Preston Equation**

$$
MRR = K_p \cdot P \cdot V
$$

**Parameters:**

- $MRR$ — material removal rate (nm/min)
- $K_p$ — Preston coefficient (material/process dependent)
- $P$ — applied pressure
- $V$ — relative velocity between pad and wafer

**9.2 Contact Mechanics — Greenwood-Williamson Model**

**Real Contact Area:**

$$
A_r = \pi \eta A_n R_p \int_d^\infty (z-d)\phi(z)dz
$$

**Parameters:**

- $\eta$ — asperity density
- $A_n$ — nominal contact area
- $R_p$ — asperity radius
- $d$ — separation distance
- $\phi(z)$ — asperity height distribution

**9.3 Slurry Hydrodynamics — Reynolds Equation**

$$
\frac{\partial}{\partial x}\left(h^3\frac{\partial p}{\partial x}\right) + \frac{\partial}{\partial y}\left(h^3\frac{\partial p}{\partial y}\right) = 6\mu U\frac{\partial h}{\partial x}
$$

**Parameters:**

- $h$ — film thickness
- $p$ — pressure
- $\mu$ — dynamic viscosity
- $U$ — sliding velocity



**10. Thin Film Stress**

**10.1 Stoney Equation**

**Film Stress from Wafer Curvature:**

$$
\sigma_f = \frac{E_s h_s^2}{6(1-
u_s)h_f R}
$$

**Parameters:**

- $\sigma_f$ — film stress
- $E_s$ — substrate Young's modulus
- $
u_s$ — substrate Poisson's ratio
- $h_s$ — substrate thickness
- $h_f$ — film thickness
- $R$ — radius of curvature

**10.2 Thermal Stress**

$$
\sigma_{th} = \frac{E_f}{1-
u_f}(\alpha_s - \alpha_f)\Delta T
$$

**Parameters:**

- $E_f$ — film Young's modulus
- $
u_f$ — film Poisson's ratio
- $\alpha_s, \alpha_f$ — thermal expansion coefficients (substrate, film)
- $\Delta T$ — temperature change from deposition



**11. Electromigration (Reliability)**

**11.1 Black's Equation (Empirical MTTF)**

$$
MTTF = A \cdot j^{-n} \cdot \exp\left(\frac{E_a}{k_B T}\right)
$$

**Parameters:**

- $MTTF$ — mean time to failure
- $j$ — current density
- $n$ — current density exponent (typically 1-2)
- $E_a$ — activation energy
- $A$ — material/geometry constant

**11.2 Drift-Diffusion Model**

$$
\frac{\partial C}{\partial t} = 
abla \cdot \left[D\left(
abla C - C\frac{Z^*e\rho \vec{j}}{k_B T}\right)\right]
$$

**Parameters:**

- $C$ — atomic concentration
- $D$ — diffusion coefficient
- $Z^*$ — effective charge number (wind force parameter)
- $\rho$ — electrical resistivity
- $\vec{j}$ — current density vector

**11.3 Stress Evolution — Korhonen Model**

$$
\frac{\partial \sigma}{\partial t} = \frac{\partial}{\partial x}\left[\frac{D_a B\Omega}{k_B T}\left(\frac{\partial\sigma}{\partial x} + \frac{Z^*e\rho j}{\Omega}\right)\right]
$$

**Parameters:**

- $\sigma$ — hydrostatic stress
- $D_a$ — atomic diffusivity
- $B$ — effective bulk modulus
- $\Omega$ — atomic volume



**12. Numerical Solution Methods**

**12.1 Common Numerical Techniques**

| Method | Application | Strengths |
|--------|-------------|-----------|
| **Finite Difference (FDM)** | Regular grids, 1D/2D problems | Simple implementation, efficient |
| **Finite Element (FEM)** | Complex geometries, stress analysis | Flexible meshing, boundary conditions |
| **Monte Carlo** | Ion implantation, plasma kinetics | Statistical accuracy, handles randomness |
| **Level Set** | Topography evolution (etch/deposition) | Handles topology changes |
| **Kinetic Monte Carlo (KMC)** | Atomic-scale diffusion, nucleation | Captures rare events, atomic detail |

**12.2 Discretization Examples**

**Explicit Forward Euler (1D Diffusion):**

$$
C_i^{n+1} = C_i^n + \frac{D\Delta t}{(\Delta x)^2}\left(C_{i+1}^n - 2C_i^n + C_{i-1}^n\right)
$$

**Stability Criterion:**

$$
\frac{D\Delta t}{(\Delta x)^2} \leq \frac{1}{2}
$$

**Implicit Backward Euler:**

$$
C_i^{n+1} - \frac{D\Delta t}{(\Delta x)^2}\left(C_{i+1}^{n+1} - 2C_i^{n+1} + C_{i-1}^{n+1}\right) = C_i^n
$$

**12.3 Major TCAD Software Tools**

- **Synopsys Sentaurus** — comprehensive process and device simulation
- **Silvaco ATHENA/ATLAS** — process and device modeling
- **COMSOL Multiphysics** — general multiphysics platform
- **SRIM/TRIM** — ion implantation Monte Carlo
- **PROLITH** — lithography simulation



**Processes and Governing Equations**

| Process | Primary Physics | Key Equation |
|---------|-----------------|--------------|
| **Oxidation** | Diffusion + Reaction | $x^2 + Ax = Bt$ |
| **Diffusion** | Mass Transport | $\frac{\partial C}{\partial t} = D
abla^2 C$ |
| **Implantation** | Ballistic + Stopping | $\frac{dE}{dx} = -N(S_n + S_e)$ |
| **CVD** | Transport + Kinetics | Navier-Stokes + Species |
| **ALD** | Self-limiting Adsorption | Langmuir kinetics |
| **Plasma Etch** | Plasma + Surface | Poisson + Drift-Diffusion |
| **Lithography** | Wave Optics + Chemistry | Dill ABC model |
| **Epitaxy** | Surface Diffusion | BCF theory |
| **CMP** | Tribology + Chemistry | Preston equation |
| **Stress** | Elasticity | Stoney equation |
| **Electromigration** | Mass transport under current | Korhonen model |

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/physics-based-modeling-and-differential-equations) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
