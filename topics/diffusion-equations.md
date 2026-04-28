# Mathematical Modeling of Diffusion

**Keywords**: diffusion equations,fick laws,fick second law,semiconductor diffusion equations,dopant diffusion equations,arrhenius diffusion,junction depth calculation,transient enhanced diffusion,oxidation enhanced diffusion,numerical methods diffusion,thermal budget

---

**Mathematical Modeling of Diffusion**

  1. Fundamental Governing Equations

   1.1 Fick's Laws of Diffusion

The foundation of diffusion modeling in semiconductor manufacturing rests on  Fick's laws :

   Fick's First Law

The flux is proportional to the concentration gradient:

$$
J = -D \frac{\partial C}{\partial x}
$$

 Where: 

- $J$ = flux (atoms/cm²·s)
- $D$ = diffusion coefficient (cm²/s)
- $C$ = concentration (atoms/cm³)
- $x$ = position (cm)

Note:  The negative sign indicates diffusion occurs from high to low concentration regions.

   Fick's Second Law

Derived from the continuity equation combined with Fick's first law:

$$
\frac{\partial C}{\partial t} = D \frac{\partial^2 C}{\partial x^2}
$$

 Key characteristics: 

- This is a  parabolic partial differential equation 
- Mathematically identical to the heat equation
- Assumes constant diffusion coefficient $D$

   1.2 Temperature Dependence (Arrhenius Relationship)

The diffusion coefficient follows the Arrhenius relationship:

$$
D(T) = D_0 \exp\left(-\frac{E_a}{kT}\right)
$$

 Where: 

- $D_0$ = pre-exponential factor (cm²/s)
- $E_a$ = activation energy (eV)
- $k$ = Boltzmann constant ($8.617 \times 10^{-5}$ eV/K)
- $T$ = absolute temperature (K)

   1.3 Typical Dopant Parameters in Silicon

| Dopant | $D_0$ (cm²/s) | $E_a$ (eV) | $D$ at 1100°C (cm²/s) |
|--------|---------------|------------|------------------------|
| Boron (B) | ~10.5 | ~3.69 | ~$10^{-13}$ |
| Phosphorus (P) | ~10.5 | ~3.69 | ~$10^{-13}$ |
| Arsenic (As) | ~0.32 | ~3.56 | ~$10^{-14}$ |
| Antimony (Sb) | ~5.6 | ~3.95 | ~$10^{-14}$ |



  2. Analytical Solutions for Standard Boundary Conditions

   2.1 Constant Surface Concentration (Predeposition)

   Boundary and Initial Conditions

- $C(0,t) = C_s$ — surface held at solid solubility
- $C(x,0) = 0$ — initially undoped wafer
- $C(\infty,t) = 0$ — semi-infinite substrate

   Solution: Complementary Error Function Profile

$$
C(x,t) = C_s \cdot \text{erfc}\left(\frac{x}{2\sqrt{Dt}}\right)
$$

 Where the complementary error function is defined as: 

$$
\text{erfc}(\eta) = 1 - \text{erf}(\eta) = 1 - \frac{2}{\sqrt{\pi}}\int_0^\eta e^{-u^2} \, du
$$

   Total Dose Introduced

$$
Q = \int_0^\infty C(x,t) \, dx = \frac{2 C_s \sqrt{Dt}}{\sqrt{\pi}} \approx 1.13 \, C_s \sqrt{Dt}
$$

   Key Properties

- Surface concentration remains constant at $C_s$
- Profile penetrates deeper with increasing $\sqrt{Dt}$
- Characteristic diffusion length: $L_D = 2\sqrt{Dt}$

   2.2 Fixed Dose / Gaussian Drive-in

   Boundary and Initial Conditions

- Total dose $Q$ is conserved (no dopant enters or leaves)
- Zero flux at surface: $\left.\frac{\partial C}{\partial x}\right|_{x=0} = 0$
- Delta-function or thin layer initial condition

   Solution: Gaussian Profile

$$
C(x,t) = \frac{Q}{\sqrt{\pi Dt}} \exp\left(-\frac{x^2}{4Dt}\right)
$$

   Time-Dependent Surface Concentration

$$
C_s(t) = C(0,t) = \frac{Q}{\sqrt{\pi Dt}}
$$

 Key characteristics: 

- Surface concentration  decreases  with time as $t^{-1/2}$
- Profile broadens while maintaining total dose
- Peak always at surface ($x = 0$)

   2.3 Junction Depth Calculation

The  junction depth  $x_j$ is the position where dopant concentration equals background concentration $C_B$:

   For erfc Profile

$$
x_j = 2\sqrt{Dt} \cdot \text{erfc}^{-1}\left(\frac{C_B}{C_s}\right)
$$

   For Gaussian Profile

$$
x_j = 2\sqrt{Dt \cdot \ln\left(\frac{Q}{C_B \sqrt{\pi Dt}}\right)}
$$



  3. Green's Function Method

   3.1 General Solution for Arbitrary Initial Conditions

For an arbitrary initial profile $C_0(x')$, the solution is a  convolution  with the Gaussian kernel (Green's function):

$$
C(x,t) = \int_{-\infty}^{\infty} C_0(x') \cdot \frac{1}{2\sqrt{\pi Dt}} \exp\left(-\frac{(x-x')^2}{4Dt}\right) dx'
$$

 Physical interpretation: 

- Each point in the initial distribution spreads as a Gaussian
- The final profile is the superposition of all spreading contributions

   3.2 Application: Ion-Implanted Gaussian Profile

   Initial Implant Profile

$$
C_0(x) = \frac{Q}{\sqrt{2\pi} \, \Delta R_p} \exp\left(-\frac{(x - R_p)^2}{2 \Delta R_p^2}\right)
$$

 Where: 

- $Q$ = implanted dose (atoms/cm²)
- $R_p$ = projected range (mean depth)
- $\Delta R_p$ = straggle (standard deviation)

   Profile After Diffusion

$$
C(x,t) = \frac{Q}{\sqrt{2\pi \, \sigma_{eff}^2}} \exp\left(-\frac{(x - R_p)^2}{2 \sigma_{eff}^2}\right)
$$

   Effective Straggle

$$
\sigma_{eff} = \sqrt{\Delta R_p^2 + 2Dt}
$$

 Key observations: 

- Peak remains at $R_p$ (no shift in position)
- Peak concentration decreases
- Profile broadens symmetrically



  4. Concentration-Dependent Diffusion

   4.1 Nonlinear Diffusion Equation

At high dopant concentrations (above intrinsic carrier concentration $n_i$), diffusion becomes  concentration-dependent :

$$
\frac{\partial C}{\partial t} = \frac{\partial}{\partial x}\left(D(C) \frac{\partial C}{\partial x}\right)
$$

   4.2 Concentration-Dependent Diffusivity Models

   Simple Power Law Model

$$
D(C) = D^i \left(1 + \left(\frac{C}{n_i}\right)^r\right)
$$

   Charged Defect Model (Fair's Equation)

$$
D = D^0 + D^- \frac{n}{n_i} + D^{=} \left(\frac{n}{n_i}\right)^2 + D^+ \frac{p}{n_i}
$$

 Where: 

- $D^0$ = neutral defect contribution
- $D^-$ = singly negative defect contribution
- $D^{=}$ = doubly negative defect contribution
- $D^+$ = positive defect contribution
- $n, p$ = electron and hole concentrations

   4.3 Electric Field Enhancement

High concentration gradients create internal electric fields that enhance diffusion:

$$
J = -D \frac{\partial C}{\partial x} - \mu C \mathcal{E}
$$

For extrinsic conditions with a single dopant species:

$$
J = -hD \frac{\partial C}{\partial x}
$$

 Field enhancement factor: 

$$
h = 1 + \frac{C}{n + p}
$$

- For fully ionized n-type dopant at high concentration: $h \approx 2$
- Results in approximately 2× faster effective diffusion

   4.4 Resulting Profile Shapes

-  Phosphorus:  "Kink-and-tail" profile at high concentrations
-  Arsenic:  Box-like profiles due to clustering
-  Boron:  Enhanced tail diffusion in oxidizing ambient



  5. Point Defect-Mediated Diffusion

   5.1 Diffusion Mechanisms

Dopants don't diffuse as isolated atoms—they move via  defect complexes :

   Vacancy Mechanism

$$
A + V \rightleftharpoons AV \quad \text{(dopant-vacancy pair forms, diffuses, dissociates)}
$$

   Interstitial Mechanism

$$
A + I \rightleftharpoons AI \quad \text{(dopant-interstitial pair)}
$$

   Kick-out Mechanism

$$
A_s + I \rightleftharpoons A_i \quad \text{(substitutional ↔ interstitial)}
$$

   5.2 Effective Diffusivity

$$
D_{eff} = D_V \frac{C_V}{C_V^*} + D_I \frac{C_I}{C_I^*}
$$

 Where: 

- $D_V, D_I$ = diffusivity via vacancy/interstitial mechanism
- $C_V, C_I$ = actual vacancy/interstitial concentrations
- $C_V^*, C_I^*$ = equilibrium concentrations

 Fractional interstitialcy: 

$$
f_I = \frac{D_I}{D_V + D_I}
$$

| Dopant | $f_I$ | Dominant Mechanism |
|--------|-------|-------------------|
| Boron | ~1.0 | Interstitial |
| Phosphorus | ~0.9 | Interstitial |
| Arsenic | ~0.4 | Mixed |
| Antimony | ~0.02 | Vacancy |

   5.3 Coupled Reaction-Diffusion System

The full model requires solving  coupled PDEs :

   Dopant Equation

$$
\frac{\partial C_A}{\partial t} = 
abla \cdot \left(D_A \frac{C_I}{C_I^*} 
abla C_A\right)
$$

   Interstitial Balance

$$
\frac{\partial C_I}{\partial t} = D_I 
abla^2 C_I + G - k_{IV}\left(C_I C_V - C_I^* C_V^*\right)
$$

   Vacancy Balance

$$
\frac{\partial C_V}{\partial t} = D_V 
abla^2 C_V + G - k_{IV}\left(C_I C_V - C_I^* C_V^*\right)
$$

 Where: 

- $G$ = defect generation rate
- $k_{IV}$ = bulk recombination rate constant

   5.4 Transient Enhanced Diffusion (TED)

After ion implantation, excess interstitials cause  anomalously rapid diffusion :

 The "+1" Model: 

$$
\int_0^\infty (C_I - C_I^*) \, dx \approx \Phi \quad \text{(implant dose)}
$$

 Enhancement factor: 

$$
\frac{D_{eff}}{D^*} = \frac{C_I}{C_I^*} \gg 1 \quad \text{(transient)}
$$

 Key characteristics: 

- Enhancement decays as interstitials recombine
- Time constant: typically 10-100 seconds at 1000°C
- Critical for shallow junction formation



  6. Oxidation Effects

   6.1 Oxidation-Enhanced Diffusion (OED)

During thermal oxidation, silicon interstitials are  injected  into the substrate:

$$
\frac{C_I}{C_I^*} = 1 + A \left(\frac{dx_{ox}}{dt}\right)^n
$$

 Effective diffusivity: 

$$
D_{eff} = D^* \left[1 + f_I \left(\frac{C_I}{C_I^*} - 1\right)\right]
$$

 Dopants enhanced by oxidation: 

- Boron (high $f_I$)
- Phosphorus (high $f_I$)

   6.2 Oxidation-Retarded Diffusion (ORD)

Growing oxide  absorbs vacancies , reducing vacancy concentration:

$$
\frac{C_V}{C_V^*} < 1
$$

 Dopants retarded by oxidation: 

- Antimony (low $f_I$, primarily vacancy-mediated)

   6.3 Segregation at SiO₂/Si Interface

Dopants redistribute at the interface according to the  segregation coefficient :

$$
m = \frac{C_{Si}}{C_{SiO_2}}\bigg|_{\text{interface}}
$$

| Dopant | Segregation Coefficient $m$ | Behavior |
|--------|----------------------------|----------|
| Boron | ~0.3 | Pile-down (into oxide) |
| Phosphorus | ~10 | Pile-up (into silicon) |
| Arsenic | ~10 | Pile-up |



  7. Numerical Methods

   7.1 Finite Difference Method

Discretize space and time on grid $(x_i, t^n)$:

   Explicit Scheme (FTCS)

$$
\frac{C_i^{n+1} - C_i^n}{\Delta t} = D \frac{C_{i+1}^n - 2C_i^n + C_{i-1}^n}{(\Delta x)^2}
$$

 Rearranged: 

$$
C_i^{n+1} = C_i^n + \alpha \left(C_{i+1}^n - 2C_i^n + C_{i-1}^n\right)
$$

 Where Fourier number: 

$$
\alpha = \frac{D \Delta t}{(\Delta x)^2}
$$

 Stability requirement (von Neumann analysis): 

$$
\alpha \leq \frac{1}{2}
$$

   Implicit Scheme (BTCS)

$$
\frac{C_i^{n+1} - C_i^n}{\Delta t} = D \frac{C_{i+1}^{n+1} - 2C_i^{n+1} + C_{i-1}^{n+1}}{(\Delta x)^2}
$$

-  Unconditionally stable  (no restriction on $\alpha$)
- Requires solving tridiagonal system at each time step

   Crank-Nicolson Scheme (Second-Order Accurate)

$$
C_i^{n+1} - C_i^n = \frac{\alpha}{2}\left[(C_{i+1}^{n+1} - 2C_i^{n+1} + C_{i-1}^{n+1}) + (C_{i+1}^n - 2C_i^n + C_{i-1}^n)\right]
$$

 Properties: 

- Unconditionally stable
- Second-order accurate in both space and time
- Results in tridiagonal system: solved by  Thomas algorithm 

   7.2 Handling Concentration-Dependent Diffusion

Use iterative methods:

1. Estimate $D^{(k)}$ from current concentration $C^{(k)}$
2. Solve linear diffusion equation for $C^{(k+1)}$
3. Update diffusivity: $D^{(k+1)} = D(C^{(k+1)})$
4. Iterate until $\|C^{(k+1)} - C^{(k)}\| < \epsilon$

   7.3 Moving Boundary Problems

For oxidation with moving Si/SiO₂ interface:

 Approaches: 

-  Coordinate transformation:  Map to fixed domain via $\xi = x/s(t)$
-  Front-tracking methods:  Explicitly track interface position
-  Level-set methods:  Implicit interface representation
-  Phase-field methods:  Diffuse interface approximation



  8. Thermal Budget Concept

   8.1 The Dt Product

Diffusion profiles scale with $\sqrt{Dt}$. The  thermal budget  quantifies total diffusion:

$$
(Dt)_{total} = \sum_i D(T_i) \cdot t_i
$$

   8.2 Continuous Temperature Profile

For time-varying temperature:

$$
(Dt)_{eff} = \int_0^{t_{total}} D(T(\tau)) \, d\tau
$$

   8.3 Equivalent Time at Reference Temperature

$$
t_{eq} = \sum_i t_i \exp\left(\frac{E_a}{k}\left(\frac{1}{T_{ref}} - \frac{1}{T_i}\right)\right)
$$

   8.4 Combining Multiple Diffusion Steps

For sequential Gaussian redistributions:

$$
\sigma_{final} = \sqrt{\sum_i 2D_i t_i}
$$

For erfc profiles, use effective $(Dt)_{total}$:

$$
C(x) = C_s \cdot \text{erfc}\left(\frac{x}{2\sqrt{(Dt)_{total}}}\right)
$$



  9. Key Dimensionless Parameters

| Parameter | Definition | Physical Meaning |
|-----------|------------|------------------|
|  Fourier Number  | $Fo = \dfrac{Dt}{L^2}$ | Diffusion time vs. characteristic length |
|  Damköhler Number  | $Da = \dfrac{kL^2}{D}$ | Reaction rate vs. diffusion rate |
|  Péclet Number  | $Pe = \dfrac{vL}{D}$ | Advection (drift) vs. diffusion |
|  Biot Number  | $Bi = \dfrac{hL}{D}$ | Surface transfer vs. bulk diffusion |



  10. Process Simulation Software

   10.1 Commercial and Research Tools

| Simulator | Developer | Key Capabilities |
|-----------|-----------|------------------|
|  Sentaurus Process  | Synopsys | Full 3D, atomistic KMC, advanced models |
|  Athena  | Silvaco | Integrated with device simulation (Atlas) |
|  SUPREM-IV  | Stanford | Classic 1D/2D, widely validated |
|  FLOOPS  | U. Florida | Research-oriented, extensible |
|  Victory Process  | Silvaco | Modern 3D process simulation |

   10.2 Physical Models Incorporated

- Multiple coupled dopant species
- Full point-defect dynamics (I, V, clusters)
- Stress-dependent diffusion
- Cluster nucleation and dissolution
- Atomistic kinetic Monte Carlo (KMC) options
- Quantum corrections for ultra-shallow junctions



  Mathematical Modeling Hierarchy:

   Level 1: Simple Analytical Models

$$
\frac{\partial C}{\partial t} = D \frac{\partial^2 C}{\partial x^2}
$$

- Constant $D$
- erfc and Gaussian solutions
- Junction depth calculations

   Level 2: Intermediate Complexity

$$
\frac{\partial C}{\partial t} = \frac{\partial}{\partial x}\left(D(C) \frac{\partial C}{\partial x}\right)
$$

- Concentration-dependent $D$
- Electric field effects
- Nonlinear PDEs requiring numerical methods

   Level 3: Advanced Coupled Models

$$
\begin{aligned}
\frac{\partial C_A}{\partial t} &= 
abla \cdot \left(D_A \frac{C_I}{C_I^*} 
abla C_A\right) \\[6pt]
\frac{\partial C_I}{\partial t} &= D_I 
abla^2 C_I + G - k_{IV}(C_I C_V - C_I^* C_V^*)
\end{aligned}
$$

- Coupled dopant-defect systems
- TED, OED/ORD effects
- Process simulators required

   Level 4: State-of-the-Art

- Atomistic kinetic Monte Carlo
- Molecular dynamics for interface phenomena
- Ab initio calculations for defect properties
- Essential for sub-10nm technology nodes



  Key Insight

The fundamental scaling of semiconductor diffusion is governed by $\sqrt{Dt}$, but the effective diffusion coefficient $D$ depends on:
- Temperature (Arrhenius)
- Concentration (charged defects)
- Point defect supersaturation (TED)
- Processing ambient (oxidation)
- Mechanical stress

 This complexity requires sophisticated physical models for modern nanometer-scale devices.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/diffusion-equations) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
