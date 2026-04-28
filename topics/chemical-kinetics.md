# Semiconductor Manufacturing Process Chemical Kinetics: Mathematics

**Keywords**: chemical kinetics, reaction rates, CVD, ALD, semiconductor processing

---

**Semiconductor Manufacturing Process Chemical Kinetics: Mathematics**



**Introduction**

Semiconductor manufacturing relies heavily on chemical kinetics to control thin film deposition, etching, oxidation, and dopant diffusion. This document provides the mathematical framework underlying these processes.



**Fundamental Kinetic Concepts**

**Reaction Rate Expression**

The general rate expression for a reaction $A + B \rightarrow C$ is:

$$
r = k[A]^m[B]^n
$$

Where:

- $r$ = reaction rate $\left(\frac{\text{mol}}{\text{m}^3 \cdot \text{s}}\right)$
- $k$ = rate constant
- $[A], [B]$ = concentrations $\left(\frac{\text{mol}}{\text{m}^3}\right)$
- $m, n$ = reaction orders (empirically determined)

**Arrhenius Equation**

The temperature dependence of rate constants follows the Arrhenius equation:

$$
k = A \exp\left(-\frac{E_a}{RT}\right)
$$

Where:

- $A$ = pre-exponential factor (frequency factor)
- $E_a$ = activation energy $\left(\frac{\text{J}}{\text{mol}}\right)$
- $R$ = universal gas constant $\left(8.314 \frac{\text{J}}{\text{mol} \cdot \text{K}}\right)$
- $T$ = absolute temperature (K)

**Linearized Form (for Arrhenius plots):**

$$
\ln(k) = \ln(A) - \frac{E_a}{R} \cdot \frac{1}{T}
$$



**Chemical Vapor Deposition (CVD)**

**Overall Rate Model**

CVD involves both gas-phase transport and surface reaction. The overall deposition rate is:

$$
R = \frac{C_g}{\frac{1}{h_g} + \frac{1}{k_s}}
$$

Where:

- $R$ = deposition rate $\left(\frac{\text{mol}}{\text{m}^2 \cdot \text{s}}\right)$
- $C_g$ = gas-phase reactant concentration
- $h_g$ = gas-phase mass transfer coefficient $\left(\frac{\text{m}}{\text{s}}\right)$
- $k_s$ = surface reaction rate constant $\left(\frac{\text{m}}{\text{s}}\right)$

**Regime Analysis**

**Surface-Reaction Limited** (low temperature, $k_s \ll h_g$):

$$
R \approx k_s \cdot C_g = A \exp\left(-\frac{E_a}{RT}\right) \cdot C_g
$$

**Mass-Transport Limited** (high temperature, $h_g \ll k_s$):

$$
R \approx h_g \cdot C_g
$$

**Mass Transfer Coefficient**

For laminar flow over a flat plate:

$$
h_g = \frac{D_{AB}}{L} \cdot 0.664 \cdot Re_L^{1/2} \cdot Sc^{1/3}
$$

Where:

- $D_{AB}$ = binary diffusion coefficient
- $L$ = characteristic length
- $Re_L = \frac{\rho v L}{\mu}$ = Reynolds number
- $Sc = \frac{\mu}{\rho D_{AB}}$ = Schmidt number



**Thermal Oxidation: Deal-Grove Model**

**Governing Equation**

The Deal-Grove model describes silicon oxidation ($\text{Si} + \text{O}_2 \rightarrow \text{SiO}_2$):

$$
x^2 + Ax = B(t + \tau)
$$

Where:

- $x$ = oxide thickness (m)
- $t$ = oxidation time (s)
- $\tau$ = initial time correction (accounts for native oxide)

**Rate Constants**

**Linear Rate Constant:**

$$
\frac{B}{A} = \frac{k_s C^*}{N_{ox}}
$$

**Parabolic Rate Constant:**

$$
B = \frac{2D_{eff} C^*}{N_{ox}}
$$

Where:

- $D_{eff}$ = effective diffusion coefficient of oxidant through oxide
- $C^*$ = equilibrium oxidant concentration in oxide
- $N_{ox}$ = number of oxidant molecules incorporated per unit volume of oxide
- $k_s$ = surface reaction rate constant

**Limiting Cases**

**Thin Oxide Regime** (short times, $x \ll A$):

$$
x \approx \frac{B}{A}(t + \tau)
$$

- Linear growth (surface-reaction controlled)

**Thick Oxide Regime** (long times, $x \gg A$):

$$
x \approx \sqrt{B \cdot t}
$$

- Parabolic growth (diffusion controlled)

**Explicit Solution**

Solving the quadratic equation:

$$
x = \frac{A}{2}\left[\sqrt{1 + \frac{4B(t+\tau)}{A^2}} - 1\right]
$$



**Plasma Etching Kinetics**

**Ion-Enhanced Etching Model**

The etch rate combines thermal and ion-assisted components:

$$
R = k_{thermal} \cdot P \cdot \exp\left(-\frac{E_a}{RT}\right) + k_{ion} \cdot \Gamma_{ion}^\alpha \cdot \theta
$$

Where:

- $k_{thermal}$ = thermal etching rate constant
- $P$ = reactive gas partial pressure
- $\Gamma_{ion}$ = ion flux $\left(\frac{\text{ions}}{\text{m}^2 \cdot \text{s}}\right)$
- $\alpha$ = ion flux exponent (typically 0.5–1.5)
- $\theta$ = surface coverage of reactive species

**Sputter Yield Model**

Physical sputtering rate:

$$
R_{sputter} = Y(\theta, E) \cdot \frac{\Gamma_{ion}}{n}
$$

Where:

- $Y$ = sputter yield (atoms removed per incident ion)
- $E$ = ion energy
- $\theta$ = ion incidence angle
- $n$ = atomic density of target material

**Selectivity**

Selectivity between materials A and B:

$$
S = \frac{R_A}{R_B}
$$



**Surface Reaction Kinetics**

**Langmuir Adsorption Isotherm**

For single-species adsorption at equilibrium:

$$
\theta = \frac{K \cdot P}{1 + K \cdot P}
$$

Where:

- $\theta$ = fractional surface coverage $(0 \leq \theta \leq 1)$
- $K$ = adsorption equilibrium constant
- $P$ = partial pressure

**Temperature Dependence of K:**

$$
K = K_0 \exp\left(\frac{-\Delta H_{ads}}{RT}\right)
$$

**Multi-Species Competitive Adsorption**

For species A and B competing for the same sites:

$$
\theta_A = \frac{K_A P_A}{1 + K_A P_A + K_B P_B}
$$

$$
\theta_B = \frac{K_B P_B}{1 + K_A P_A + K_B P_B}
$$

**Surface Reaction Rate**

**Langmuir-Hinshelwood Mechanism** (both reactants adsorbed):

$$
r = k_s \cdot \theta_A \cdot \theta_B = k_s \cdot \frac{K_A P_A \cdot K_B P_B}{(1 + K_A P_A + K_B P_B)^2}
$$

**Eley-Rideal Mechanism** (one reactant from gas phase):

$$
r = k_s \cdot \theta_A \cdot P_B = k_s \cdot \frac{K_A P_A \cdot P_B}{1 + K_A P_A}
$$

**Limiting Behavior**

| Condition | Rate Expression | Order |
|-----------|-----------------|-------|
| $K \cdot P \ll 1$ | $r \approx k_s K P$ | First-order |
| $K \cdot P \gg 1$ | $r \approx k_s$ | Zero-order |



**Diffusion Processes**

**Fick's Laws**

**First Law** (steady-state flux):

$$
J = -D \frac{\partial C}{\partial x}
$$

**Second Law** (transient diffusion):

$$
\frac{\partial C}{\partial t} = D \frac{\partial^2 C}{\partial x^2}
$$

For 3D:

$$
\frac{\partial C}{\partial t} = D 
abla^2 C = D \left(\frac{\partial^2 C}{\partial x^2} + \frac{\partial^2 C}{\partial y^2} + \frac{\partial^2 C}{\partial z^2}\right)
$$

**Concentration-Dependent Diffusion**

For dopants where $D = D(C)$:

$$
\frac{\partial C}{\partial t} = \frac{\partial}{\partial x}\left[D(C) \frac{\partial C}{\partial x}\right]
$$

**Analytical Solutions**

**Constant Surface Concentration** (semi-infinite medium):

$$
C(x,t) = C_s \cdot \text{erfc}\left(\frac{x}{2\sqrt{Dt}}\right)
$$

Where $\text{erfc}$ is the complementary error function:

$$
\text{erfc}(z) = 1 - \text{erf}(z) = 1 - \frac{2}{\sqrt{\pi}}\int_0^z e^{-u^2} du
$$

**Fixed Total Dose** (Gaussian profile):

$$
C(x,t) = \frac{Q}{\sqrt{\pi D t}} \exp\left(-\frac{x^2}{4Dt}\right)
$$

Where $Q$ = total dose $\left(\frac{\text{atoms}}{\text{m}^2}\right)$

**Diffusion Coefficient Temperature Dependence**

$$
D = D_0 \exp\left(-\frac{E_a}{kT}\right)
$$

Where $k = 8.617 \times 10^{-5} \frac{\text{eV}}{\text{K}}$ (Boltzmann constant)



**Reactor-Scale Modeling**

**Species Conservation Equation**

The convection-diffusion-reaction equation:

$$
\frac{\partial C_i}{\partial t} + 
abla \cdot (\mathbf{v} C_i) = 
abla \cdot (D_i 
abla C_i) + R_i
$$

Expanded form:

$$
\frac{\partial C_i}{\partial t} + \mathbf{v} \cdot 
abla C_i = D_i 
abla^2 C_i + R_i
$$

**Coupled Equations**

**Navier-Stokes (momentum):**

$$
\rho \left(\frac{\partial \mathbf{v}}{\partial t} + \mathbf{v} \cdot 
abla \mathbf{v}\right) = -
abla P + \mu 
abla^2 \mathbf{v} + \rho \mathbf{g}
$$

**Continuity (mass):**

$$
\frac{\partial \rho}{\partial t} + 
abla \cdot (\rho \mathbf{v}) = 0
$$

**Energy:**

$$
\rho c_p \left(\frac{\partial T}{\partial t} + \mathbf{v} \cdot 
abla T\right) = k 
abla^2 T + Q_{rxn}
$$

Where $Q_{rxn} = \sum_j (-\Delta H_j) r_j$ is the heat of reaction.

**Boundary Conditions**

**Surface reaction flux:**

$$
-D_i \frac{\partial C_i}{\partial n}\bigg|_{surface} = R_{s,i}
$$

**Inlet conditions:**

$$
C_i = C_{i,inlet}, \quad T = T_{inlet}, \quad \mathbf{v} = \mathbf{v}_{inlet}
$$



**Dimensionless Analysis**

**Damköhler Number**

$$
Da = \frac{\text{reaction rate}}{\text{transport rate}} = \frac{k_s L}{D}
$$

| Da Value | Regime | Characteristics |
|----------|--------|-----------------|
| $Da \gg 1$ | Reaction-limited | Uniform deposition, strong T dependence |
| $Da \ll 1$ | Transport-limited | Non-uniform, weak T dependence |

**Thiele Modulus**

For reactions in porous structures:

$$
\phi = L \sqrt{\frac{k}{D_{eff}}}
$$

**Effectiveness Factor:**

$$
\eta = \frac{\tanh(\phi)}{\phi}
$$

**Peclet Number**

$$
Pe = \frac{vL}{D} = \frac{\text{convective transport}}{\text{diffusive transport}}
$$

**Stanton Number**

$$
St = \frac{h}{\rho v c_p} = \frac{\text{heat transfer}}{\text{thermal capacity of flow}}
$$



**Advanced Modeling Techniques**

**Microkinetic Modeling**

System of coupled ODEs for surface species:

$$
\frac{d\theta_i}{dt} = \sum_j \left[
u_{ij}^+ r_j^+ - 
u_{ij}^- r_j^-\right]
$$

Where:

- $\theta_i$ = coverage of species $i$
- $
u_{ij}$ = stoichiometric coefficient
- $r_j^+, r_j^-$ = forward and reverse rates of reaction $j$

**Example: Adsorption-Desorption-Reaction:**

$$
\frac{d\theta_A}{dt} = k_{ads} P_A (1-\theta_A-\theta_B) - k_{des} \theta_A - k_{rxn} \theta_A \theta_B
$$

**Stochastic Methods**

**Kinetic Monte Carlo (KMC):**

Transition rates:

$$
W_i = 
u_i \exp\left(-\frac{E_i}{kT}\right)
$$

Time step:

$$
\Delta t = -\frac{\ln(r)}{\sum_i W_i}
$$

Where $r \in (0,1]$ is a random number.

**Master Equation:**

$$
\frac{dP_n}{dt} = \sum_m \left[W_{mn} P_m - W_{nm} P_n\right]
$$

**Multi-Scale Coupling**

| Scale | Size | Method | Output |
|-------|------|--------|--------|
| Quantum | ~Å | DFT | Reaction barriers, adsorption energies |
| Atomic | ~nm | MD, KMC | Surface morphology, growth modes |
| Feature | ~$\mu$m | Level-set, FEM | Profile evolution |
| Reactor | ~cm | CFD | Uniformity, gas dynamics |



**Computational Methods**

**Numerical Discretization**

**Finite Difference (1D diffusion):**

$$
\frac{C_i^{n+1} - C_i^n}{\Delta t} = D \frac{C_{i+1}^n - 2C_i^n + C_{i-1}^n}{(\Delta x)^2}
$$

**Stability Criterion (explicit method):**

$$
\frac{D \Delta t}{(\Delta x)^2} \leq \frac{1}{2}
$$

**Operator Splitting**

For stiff reaction-diffusion systems:

1. **Diffusion step:** Solve $\frac{\partial C}{\partial t} = D 
abla^2 C$ for $\Delta t/2$
2. **Reaction step:** Solve $\frac{dC}{dt} = R(C)$ for $\Delta t$
3. **Diffusion step:** Solve $\frac{\partial C}{\partial t} = D 
abla^2 C$ for $\Delta t/2$

**Newton-Raphson for Nonlinear Systems**

$$
\mathbf{x}^{(k+1)} = \mathbf{x}^{(k)} - \mathbf{J}^{-1}(\mathbf{x}^{(k)}) \cdot \mathbf{F}(\mathbf{x}^{(k)})
$$

Where $\mathbf{J}$ is the Jacobian matrix:

$$
J_{ij} = \frac{\partial F_i}{\partial x_j}
$$



**Key Equations Summary**

**Rate Expressions**

| Process | Equation |
|---------|----------|
| Arrhenius | $k = A \exp\left(-\frac{E_a}{RT}\right)$ |
| CVD Rate | $R = \frac{C_g}{1/h_g + 1/k_s}$ |
| Deal-Grove | $x^2 + Ax = B(t + \tau)$ |
| Langmuir | $\theta = \frac{KP}{1+KP}$ |
| Fick's 2nd Law | $\frac{\partial C}{\partial t} = D \frac{\partial^2 C}{\partial x^2}$ |

**Dimensionless Numbers**

| Number | Definition | Physical Meaning |
|--------|------------|------------------|
| Damköhler ($Da$) | $\frac{k_s L}{D}$ | Reaction vs. transport rate |
| Thiele ($\phi$) | $L\sqrt{k/D_{eff}}$ | Reaction-diffusion penetration |
| Peclet ($Pe$) | $\frac{vL}{D}$ | Convection vs. diffusion |
| Reynolds ($Re$) | $\frac{\rho vL}{\mu}$ | Inertial vs. viscous forces |

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/chemical-kinetics) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
