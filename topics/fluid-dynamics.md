# Fluid Dynamics: Mathematical Modeling

**Keywords**: fluid dynamics, semiconductor fluid dynamics, navier stokes, reynolds number, cfd, wet processing, cmp slurry, gas dynamics

---

**Fluid Dynamics: Mathematical Modeling**



  1. Overview: Where Fluid Dynamics Matters

Fluid dynamics plays a critical role in numerous semiconductor fabrication steps:

-  Chemical Vapor Deposition (CVD)  — Precursor gas transport and reaction
-  Spin Coating  — Photoresist film formation
-  Chemical Mechanical Planarization (CMP)  — Slurry flow and material removal
-  Wet Etching/Cleaning  — Etchant transport to surfaces
-  Immersion Lithography  — Water flow between lens and wafer
-  Electrochemical Deposition  — Electrolyte flow and ion transport

Each process involves distinct physics, but they share a common mathematical foundation.



  2. Fundamental Governing Equations

   2.1 Navier-Stokes Framework

The foundation is the incompressible Navier-Stokes equations.

   Continuity Equation

$$

abla \cdot \mathbf{u} = 0
$$

   Momentum Equation

$$
\rho\left(\frac{\partial \mathbf{u}}{\partial t} + \mathbf{u} \cdot 
abla \mathbf{u}\right) = -
abla p + \mu 
abla^2 \mathbf{u} + \mathbf{F}
$$

Where:

- $\mathbf{u}$ — Velocity field vector
- $p$ — Pressure field
- $\rho$ — Fluid density
- $\mu$ — Dynamic viscosity
- $\mathbf{F}$ — Body forces (gravity, electromagnetic, etc.)

   Species Transport Equation

$$
\frac{\partial C_i}{\partial t} + \mathbf{u} \cdot 
abla C_i = D_i 
abla^2 C_i + R_i
$$

Where:

- $C_i$ — Concentration of species $i$
- $D_i$ — Diffusion coefficient of species $i$
- $R_i$ — Reaction rate (source/sink term)

   Energy Equation

$$
\rho c_p \left(\frac{\partial T}{\partial t} + \mathbf{u} \cdot 
abla T\right) = k 
abla^2 T + Q
$$

Where:

- $c_p$ — Specific heat capacity
- $T$ — Temperature
- $k$ — Thermal conductivity
- $Q$ — Heat source (reaction heat, Joule heating, etc.)



  3. Chemical Vapor Deposition (CVD)

CVD is one of the most mathematically complex processes, coupling gas-phase transport, homogeneous reactions, and heterogeneous surface chemistry.

   3.1 Reactor-Scale Transport

In a typical showerhead reactor, gas enters through distributed holes and flows toward a heated wafer. The classic  stagnation-point flow  solution applies.

   Similarity Solution

For axisymmetric flow toward a disk:

$$
u_r = r f'(\eta), \quad u_z = -\sqrt{
u a} \cdot f(\eta)
$$

Where:

- $\eta = z\sqrt{a/
u}$ — Similarity variable
- $a$ — Strain rate parameter
- $
u$ — Kinematic viscosity

This yields the  Hiemenz equation :

$$
f''' + ff'' - (f')^2 + 1 = 0
$$

With boundary conditions:

- $f(0) = f'(0) = 0$ (no-slip at surface)
- $f'(\infty) = 1$ (far-field condition)

   3.2 Key Dimensionless Groups

   Damköhler Number

$$
\text{Da} = \frac{k_s L}{D}
$$

Physical meaning: Ratio of surface reaction rate to diffusive transport rate.

| Regime | Condition | Implication |
|--------|-----------|-------------|
| Transport-limited | $\text{Da} \gg 1$ | Uniformity controlled by flow |
| Reaction-limited | $\text{Da} \ll 1$ | Uniformity controlled by temperature |

   Péclet Number

$$
\text{Pe} = \frac{UL}{D}
$$

Physical meaning: Ratio of convective to diffusive transport.

   Grashof Number

$$
\text{Gr} = \frac{g\beta \Delta T L^3}{
u^2}
$$

Physical meaning: Ratio of buoyancy to viscous forces (important in horizontal reactors).

Where:

- $g$ — Gravitational acceleration
- $\beta$ — Thermal expansion coefficient
- $\Delta T$ — Temperature difference

   3.3 Surface Boundary Conditions

The critical coupling between transport and chemistry at the wafer surface:

$$
-D \left.\frac{\partial C}{\partial n}\right|_{\text{surface}} = k_s \cdot f(C, T, \theta)
$$

This is a  Robin boundary condition  linking diffusive flux to surface kinetics.

   Langmuir-Hinshelwood Kinetics

$$
R = \frac{k C}{1 + KC}
$$

Features:

- First-order at low concentration ($C \ll 1/K$)
- Zero-order (saturated) at high concentration ($C \gg 1/K$)

   Sticking Coefficient Model

$$
s = s_0 \cdot f(T) \cdot (1 - \theta)
$$

Where:

- $s_0$ — Base sticking coefficient
- $\theta$ — Surface coverage fraction

   3.4 Multi-Scale Challenge

CVD spans enormous length scales:

| Scale | Dimension | Physics |
|-------|-----------|---------|
| Reactor chamber | 0.1–1 m | Continuum CFD |
| Boundary layer | 1–10 mm | Convection-diffusion |
| Surface features | 10–100 nm | Ballistic/Knudsen transport |
| Molecular mean free path | 0.1–10 μm | Molecular dynamics |

   Knudsen Number

$$
\text{Kn} = \frac{\lambda}{L}
$$

Where $\lambda$ is the molecular mean free path.

| Regime | Condition | Modeling Approach |
|--------|-----------|-------------------|
| Continuum | $\text{Kn} < 0.01$ | Navier-Stokes |
| Slip flow | $0.01 < \text{Kn} < 0.1$ | Navier-Stokes + slip BC |
| Transition | $0.1 < \text{Kn} < 10$ | DSMC, Boltzmann |
| Free molecular | $\text{Kn} > 10$ | Ballistic transport |



  4. Spin Coating

Spin coating deposits thin photoresist films through centrifugal spreading and solvent evaporation.

   4.1 Thin Film Lubrication Theory

For a thin viscous layer ($h \ll R$) on a rotating disk, the  lubrication approximation  applies:

$$
\frac{\partial h}{\partial t} + \frac{1}{r}\frac{\partial}{\partial r}(r h \bar{u}_r) = -E
$$

Where:

- $h(r,t)$ — Film thickness
- $\bar{u}_r$ — Depth-averaged radial velocity
- $E$ — Evaporation rate

   4.2 Velocity Profile

Integrating the momentum equation with:

- No-slip at substrate ($u_r = 0$ at $z = 0$)
- Zero shear at free surface ($\partial u_r / \partial z = 0$ at $z = h$)

Yields:

$$
u_r(z) = \frac{\rho \omega^2 r}{2\mu}(2hz - z^2)
$$

Depth-averaged velocity:

$$
\bar{u}_r = \frac{\rho \omega^2 r h^2}{3\mu}
$$

   4.3 Emslie-Bonner-Peck Solution

For a Newtonian fluid without evaporation:

$$
\frac{\partial h}{\partial t} = -\frac{\rho \omega^2}{3\mu} \cdot \frac{1}{r}\frac{\partial (r h^3)}{\partial r}
$$

For  uniform initial thickness  $h_0$:

$$
h(t) = \frac{h_0}{\sqrt{1 + \dfrac{4\rho \omega^2 h_0^2 t}{3\mu}}}
$$

Asymptotic behavior:

- Short time: $h \approx h_0$
- Long time: $h \propto t^{-1/2}$

   4.4 Non-Newtonian Photoresists

Real photoresists are shear-thinning. Using a  power-law model :

$$
\tau = K\left(\frac{\partial u}{\partial z}\right)^n
$$

Where:

- $K$ — Consistency index
- $n$ — Power-law index ($n < 1$ for shear-thinning)

The governing equation becomes:

$$
\frac{\partial h}{\partial t} = -\frac{n}{2n+1}\left(\frac{\rho \omega^2}{K}\right)^{1/n} \frac{1}{r}\frac{\partial}{\partial r}\left(r h^{(2n+1)/n}\right)
$$

   4.5 Evaporation and Marangoni Effects

   Coupled Concentration Equation

$$
\frac{\partial(h x_s)}{\partial t} + \frac{1}{r}\frac{\partial}{\partial r}(r h x_s \bar{u}_r) = -\frac{e}{\rho_s}
$$

Where:

- $x_s$ — Solvent mass fraction
- $e$ — Evaporation mass flux
- $\rho_s$ — Solvent density

   Marangoni Stress

Surface tension gradients drive Marangoni flows:

$$
\tau_{\text{surface}} = \frac{\partial \sigma}{\partial r} = \frac{d\sigma}{dC}\frac{\partial C}{\partial r}
$$

   Marangoni Number

$$
\text{Ma} = \frac{\Delta\sigma \cdot L}{\mu \alpha}
$$

Where $\alpha$ is thermal diffusivity.



  5. Chemical Mechanical Planarization (CMP)

CMP combines chemical etching with mechanical abrasion, mediated by slurry flow between pad and wafer.

   5.1 Reynolds Lubrication Equation

For the thin fluid film:

$$
\frac{\partial}{\partial x}\left(h^3 \frac{\partial p}{\partial x}\right) + \frac{\partial}{\partial y}\left(h^3 \frac{\partial p}{\partial y}\right) = 6\mu U \frac{\partial h}{\partial x} + 12\mu \frac{\partial h}{\partial t}
$$

Terms:

- Left side: Pressure-driven (Poiseuille) flow
- First term on right: Shear-driven (Couette) flow (wedge effect)
- Second term on right: Squeeze film effect

   5.2 Slurry as Suspension

CMP slurries contain abrasive particles exhibiting complex rheology.

   Shear-Induced Migration (Leighton-Acrivos)

$$
\mathbf{J}_{\text{shear}} = -K_c a^2 \phi 
abla(\dot{\gamma} \phi) - K_\eta a^2 \dot{\gamma} \phi^2 
abla(\ln \eta)
$$

Where:

- $a$ — Particle radius
- $\phi$ — Particle volume fraction
- $\dot{\gamma}$ — Shear rate
- $K_c, K_\eta$ — Empirical constants

Physical effect: Particles migrate from high-shear to low-shear regions.

   Effective Viscosity (Krieger-Dougherty)

$$
\eta_{\text{eff}} = \eta_0 \left(1 - \frac{\phi}{\phi_m}\right)^{-[\eta]\phi_m}
$$

Where:

- $\phi_m$ — Maximum packing fraction (~0.64)
- $[\eta]$ — Intrinsic viscosity (~2.5 for spheres)

   5.3 Material Removal Models

   Classical Preston Equation

$$
\text{MRR} = K_p \cdot p \cdot V
$$

Where:

- MRR — Material removal rate
- $K_p$ — Preston coefficient
- $p$ — Applied pressure
- $V$ — Relative velocity

   Enhanced Models

$$
\text{MRR} = f(\tau_{\text{shear}}, \phi_{\text{particle}}, k_{\text{chem}}, T)
$$

Incorporating:

- Fluid shear stress: $\tau = \mu \left.\dfrac{\partial u}{\partial z}\right|_{\text{surface}}$
- Local particle flux
- Chemical reaction rate
- Temperature-dependent kinetics

   5.4 Contact Mechanics

When pad asperities contact wafer:

   Greenwood-Williamson Model

$$
p_{\text{contact}} = \frac{4}{3} E^* n \int_d^\infty (z-d)^{3/2} \phi(z) \, dz
$$

Where:

- $E^*$ — Effective elastic modulus
- $n$ — Asperity density
- $\phi(z)$ — Asperity height distribution
- $d$ — Separation distance

   Force Balance

$$
p_{\text{fluid}} + p_{\text{contact}} = P_{\text{applied}}
$$



  6. Wet Etching: Mass Transfer Limited Processes

   6.1 Convective-Diffusion Equation

$$
\frac{\partial C}{\partial t} + \mathbf{u} \cdot 
abla C = D 
abla^2 C
$$

At the reactive surface (fast reaction limit):

$$
C|_{\text{surface}} = 0
$$

Etch rate:

$$
\text{ER} \propto D \left.\frac{\partial C}{\partial n}\right|_{\text{surface}}
$$

   6.2 Rotating Disk Solution (Levich)

For a wafer rotating in etchant:

   Velocity Components

$$
u_r = r\omega F(\zeta), \quad u_\theta = r\omega G(\zeta), \quad u_z = \sqrt{
u\omega} H(\zeta)
$$

Where $\zeta = z\sqrt{\omega/
u}$.

   Boundary Layer Thickness

$$
\delta = 1.61 D^{1/3} 
u^{1/6} \omega^{-1/2}
$$

   Mass Flux (Levich Equation)

$$
j = 0.62 D^{2/3} 
u^{-1/6} \omega^{1/2} C_\infty
$$

 Key insight : The etch rate is  uniform across an infinite disk . This explains why rotating processes achieve excellent uniformity.

   6.3 Feature-Scale Transport

In high-aspect-ratio trenches:

   Knudsen Diffusion

$$
D_{\text{Kn}} = \frac{d}{3}\sqrt{\frac{8RT}{\pi M}}
$$

Where:

- $d$ — Trench width
- $M$ — Molecular weight

   Concentration Profile in Trench

For a trench of depth $L$ with reactive bottom:

$$
\frac{d^2 C}{dz^2} = 0 \quad \text{(diffusion only)}
$$

With boundary conditions:

- $C(0) = C_{\text{top}}$ (top of trench)
- $-D\dfrac{dC}{dz}\big|_{z=L} = k_s C(L)$ (reactive bottom)

Solution:

$$
\frac{C(z)}{C_{\text{top}}} = 1 - \frac{z}{L} \cdot \frac{1}{1 + D/(k_s L)}
$$

   Thiele Modulus

$$
\phi = L\sqrt{\frac{k_s}{D}}
$$

- $\phi \ll 1$: Reaction-limited (uniform etch in feature)
- $\phi \gg 1$: Transport-limited (RIE lag)



  7. Immersion Lithography

At 193 nm wavelength, water ($n \approx 1.44$) fills the gap between lens and wafer, increasing numerical aperture.

   7.1 Free Surface Dynamics

   Capillary Number

$$
\text{Ca} = \frac{\mu U}{\sigma}
$$

Where $\sigma$ is surface tension.

- $\text{Ca} < \text{Ca}_{\text{crit}} \approx 0.1$: Stable meniscus
- $\text{Ca} > \text{Ca}_{\text{crit}}$: Bubble entrainment risk

   Young-Laplace Equation

$$
\Delta p = \sigma \kappa = \sigma \left(\frac{1}{R_1} + \frac{1}{R_2}\right)
$$

Where $\kappa$ is the interface curvature.

   7.2 Interface Tracking Methods

   Level Set Method

$$
\frac{\partial \phi}{\partial t} + \mathbf{u} \cdot 
abla \phi = 0
$$

Where:

- $\phi > 0$: Liquid phase
- $\phi < 0$: Gas phase
- $\phi = 0$: Interface

   Volume of Fluid (VOF)

$$
\frac{\partial \alpha}{\partial t} + 
abla \cdot (\alpha \mathbf{u}) = 0
$$

Where $\alpha$ is the volume fraction.

   7.3 Thermal Management

Light absorption heats the water:

$$
\rho c_p \left(\frac{\partial T}{\partial t} + \mathbf{u} \cdot 
abla T\right) = k
abla^2 T + Q_{\text{abs}}
$$

   Refractive Index Sensitivity

$$
\frac{dn}{dT} \approx -1 \times 10^{-4} \text{ K}^{-1}
$$

Temperature variations cause refractive index changes, introducing imaging errors (aberrations).



  8. Numerical Methods

   8.1 Finite Volume Method (FVM)

The workhorse for semiconductor CFD. Starting from integral form:

$$
\frac{\partial}{\partial t}\int_V \rho \phi \, dV + \oint_S \rho \phi \mathbf{u} \cdot \mathbf{n} \, dS = \oint_S \Gamma 
abla \phi \cdot \mathbf{n} \, dS + \int_V S_\phi \, dV
$$

   Discretization

$$
\frac{(\rho \phi)_P^{n+1} - (\rho \phi)_P^n}{\Delta t} V_P + \sum_f F_f \phi_f = \sum_f \Gamma_f (
abla \phi)_f \cdot \mathbf{A}_f + S_\phi V_P
$$

Where:

- $P$ — Cell center
- $f$ — Face index
- $F_f = \rho \mathbf{u}_f \cdot \mathbf{A}_f$ — Face flux

   8.2 Advection Schemes

| Scheme | Order | Properties |
|--------|-------|------------|
| Upwind | 1st | Stable, diffusive |
| Central | 2nd | Unstable for high Pe |
| QUICK | 3rd | Good accuracy, bounded |
| MUSCL | 2nd | TVD, shock-capturing |

   8.3 Pressure-Velocity Coupling

   SIMPLE Algorithm

1. Guess pressure field $p^*$
2. Solve momentum for $\mathbf{u}^*$
3. Solve pressure correction: $
abla \cdot (D 
abla p') = 
abla \cdot \mathbf{u}^*$
4. Correct: $p = p^* + \alpha_p p'$, $\mathbf{u} = \mathbf{u}^* - D 
abla p'$
5. Iterate until convergence

   8.4 Moving Boundary Problems

For etching/deposition where geometry evolves:

   Arbitrary Lagrangian-Eulerian (ALE)

$$
\left.\frac{\partial \phi}{\partial t}\right|_{\chi} + (\mathbf{u} - \mathbf{u}_{\text{mesh}}) \cdot 
abla \phi = \text{RHS}
$$

Where $\mathbf{u}_{\text{mesh}}$ is mesh velocity.

   Level Set Velocity Extension

$$
\frac{\partial d}{\partial \tau} + \text{sign}(\phi)(|
abla d| - 1) = 0
$$

Reinitializes the level set to a signed distance function.

   8.5 Stiff Chemistry

CVD with multiple reactions has time scales from ns (gas reactions) to s (deposition).

   Operator Splitting

1. Solve transport: $\dfrac{\partial C}{\partial t} + \mathbf{u} \cdot 
abla C = D
abla^2 C$
2. Solve chemistry: $\dfrac{dC}{dt} = R(C)$ (using stiff ODE solver)

   Implicit Methods

For stiff systems:

$$
\mathbf{C}^{n+1} = \mathbf{C}^n + \Delta t \cdot \mathbf{R}(\mathbf{C}^{n+1})
$$

Requires Newton iteration with Jacobian $\partial R_i / \partial C_j$.



  9. Dimensionless

| Group | Definition | Physical Meaning |
|-------|------------|------------------|
| Reynolds (Re) | $\dfrac{\rho UL}{\mu}$ | Inertia / Viscosity |
| Péclet (Pe) | $\dfrac{UL}{D}$ | Convection / Diffusion |
| Damköhler (Da) | $\dfrac{k_s L}{D}$ | Reaction / Transport |
| Knudsen (Kn) | $\dfrac{\lambda}{L}$ | Mean free path / Length |
| Capillary (Ca) | $\dfrac{\mu U}{\sigma}$ | Viscous / Surface tension |
| Marangoni (Ma) | $\dfrac{\Delta\sigma \cdot L}{\mu \alpha}$ | Marangoni / Viscous |
| Grashof (Gr) | $\dfrac{g\beta \Delta T L^3}{
u^2}$ | Buoyancy / Viscous |
| Schmidt (Sc) | $\dfrac{
u}{D}$ | Momentum / Mass diffusivity |
| Sherwood (Sh) | $\dfrac{k_m L}{D}$ | Convective / Diffusive mass transfer |
| Thiele ($\phi$) | $L\sqrt{\dfrac{k_s}{D}}$ | Reaction / Diffusion in pores |



  10. Current Research Frontiers

   10.1 Machine Learning Integration

-  Surrogate models  replacing expensive CFD for real-time process control
-  Physics-informed neural networks (PINNs)  for solving PDEs
-  Digital twins  for predictive maintenance and optimization

   10.2 Atomic Layer Processes (ALD/ALE)

- Highly transient, surface-reaction-dominated
- Requires time-dependent modeling of pulse/purge cycles
- Surface coverage evolution:

$$
\frac{d\theta}{dt} = k_{\text{ads}} C (1-\theta) - k_{\text{des}} \theta
$$

   10.3 Extreme Aspect Ratios

- 3D NAND with aspect ratios > 100
- Transition to molecular flow ($\text{Kn} > 0.1$)
-  Transmission probability methods :

$$
P = \frac{1}{1 + 3L/(8r)}
$$

   10.4 EUV-Related Flows

- Hydrogen buffer gas flow for debris mitigation
- Tin droplet dynamics in source
- Molecular outgassing and mask contamination

   10.5 Plasma-Flow Coupling

Low-pressure plasma processes require multi-physics:

$$

abla \cdot \mathbf{J}_e = S_e - R_e \quad \text{(electron continuity)}
$$

$$

abla \cdot \mathbf{J}_i = S_i - R_i \quad \text{(ion continuity)}
$$

$$

abla \cdot (\epsilon 
abla \phi) = -e(n_i - n_e) \quad \text{(Poisson)}
$$

Coupled to neutral gas Navier-Stokes equations.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/fluid-dynamics) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
