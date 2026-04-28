# Mathematics Modeling

**Keywords**: mathematics,mathematical modeling,semiconductor math,crystal growth math,czochralski equations,dopant segregation,heat transfer equations,lithography math

---

**Mathematics Modeling**




   1. Crystal Growth (Czochralski Process)

Growing single-crystal silicon ingots requires coupled models for heat transfer, fluid flow, and mass transport.

    1.1 Heat Transfer Equation

$$
\rho c_p \frac{\partial T}{\partial t} + \rho c_p \mathbf{v} \cdot 
abla T = 
abla \cdot (k 
abla T) + Q
$$

 Variables: 

- $\rho$ — density ($\text{kg/m}^3$)
- $c_p$ — specific heat capacity ($\text{J/(kg·K)}$)
- $T$ — temperature ($\text{K}$)
- $\mathbf{v}$ — velocity vector ($\text{m/s}$)
- $k$ — thermal conductivity ($\text{W/(m·K)}$)
- $Q$ — heat source term ($\text{W/m}^3$)

    1.2 Melt Convection Drivers

-  Buoyancy forces  — thermal and solutal gradients
-  Marangoni flow  — surface tension gradients
-  Forced convection  — crystal and crucible rotation

    1.3 Dopant Segregation

 Equilibrium segregation coefficient: 

$$
k_0 = \frac{C_s}{C_l}
$$

 Effective segregation coefficient (Burton-Prim-Slichter model): 

$$
k_{eff} = \frac{k_0}{k_0 + (1 - k_0) \exp\left(-\frac{v \delta}{D}\right)}
$$

 Variables: 

- $C_s$ — dopant concentration in solid
- $C_l$ — dopant concentration in liquid
- $v$ — crystal growth velocity
- $\delta$ — boundary layer thickness
- $D$ — diffusion coefficient in melt



   2. Thermal Oxidation (Deal-Grove Model)

The foundational model for growing $\text{SiO}_2$ on silicon.

    2.1 General Equation

$$
x_o^2 + A x_o = B(t + \tau)
$$

 Variables: 

- $x_o$ — oxide thickness ($\mu\text{m}$ or $\text{nm}$)
- $A$ — linear rate constant parameter
- $B$ — parabolic rate constant
- $t$ — oxidation time
- $\tau$ — time offset for initial oxide

    2.2 Growth Regimes

-  Linear regime  (thin oxide, surface-reaction limited):
  
  $$
  x_o \approx \frac{B}{A}(t + \tau)
  $$

-  Parabolic regime  (thick oxide, diffusion limited):
  
  $$
  x_o \approx \sqrt{B(t + \tau)}
  $$

    2.3 Extended Model Considerations

- Stress-dependent oxidation rates
- Point defect injection into silicon
- 2D/3D geometries (LOCOS bird's beak)
- High-pressure oxidation kinetics
- Thin oxide regime anomalies (<20 nm)



   3. Diffusion and Dopant Transport

    3.1 Fick's Laws

 First Law (flux equation): 

$$
\mathbf{J} = -D 
abla C
$$

 Second Law (continuity equation): 

$$
\frac{\partial C}{\partial t} = 
abla \cdot (D 
abla C)
$$

For constant $D$:

$$
\frac{\partial C}{\partial t} = D 
abla^2 C
$$

    3.2 Concentration-Dependent Diffusivity

$$
D(C) = D_i + D^{-} \frac{n}{n_i} + D^{2-} \left(\frac{n}{n_i}\right)^2 + D^{+} \frac{p}{n_i} + D^{2+} \left(\frac{p}{n_i}\right)^2
$$

 Variables: 

- $D_i$ — intrinsic diffusivity
- $D^{-}, D^{2-}$ — diffusivity via negatively charged defects
- $D^{+}, D^{2+}$ — diffusivity via positively charged defects
- $n, p$ — electron and hole concentrations
- $n_i$ — intrinsic carrier concentration

    3.3 Point-Defect Mediated Diffusion

 Effective diffusivity: 

$$
D_{eff} = D_I \frac{C_I}{C_I^*} + D_V \frac{C_V}{C_V^*}
$$

 Point defect continuity equations: 

$$
\frac{\partial C_I}{\partial t} = D_I 
abla^2 C_I + G_I - R_{IV}
$$

$$
\frac{\partial C_V}{\partial t} = D_V 
abla^2 C_V + G_V - R_{IV}
$$

 Recombination rate: 

$$
R_{IV} = k_{IV} \left( C_I C_V - C_I^* C_V^* \right)
$$

 Variables: 

- $C_I, C_V$ — interstitial and vacancy concentrations
- $C_I^*, C_V^*$ — equilibrium concentrations
- $G_I, G_V$ — generation rates
- $R_{IV}$ — interstitial-vacancy recombination rate

    3.4 Transient Enhanced Diffusion (TED)

Ion implantation creates excess interstitials causing:

- "+1" model: each implanted ion creates one net interstitial
- Enhanced diffusion persists until excess defects anneal out
- Critical for ultra-shallow junction formation



   4. Ion Implantation

    4.1 Gaussian Profile Model

$$
N(x) = \frac{\phi}{\sqrt{2\pi} \Delta R_p} \exp\left[ -\frac{(x - R_p)^2}{2 (\Delta R_p)^2} \right]
$$

 Variables: 

- $N(x)$ — dopant concentration at depth $x$ ($\text{cm}^{-3}$)
- $\phi$ — implant dose ($\text{ions/cm}^2$)
- $R_p$ — projected range (mean depth)
- $\Delta R_p$ — straggle (standard deviation)

    4.2 Pearson IV Distribution

For asymmetric profiles using four moments:

-  First moment:  $R_p$ (projected range)
-  Second moment:  $\Delta R_p$ (straggle)
-  Third moment:  $\gamma$ (skewness)
-  Fourth moment:  $\beta$ (kurtosis)

    4.3 Monte Carlo Methods (TRIM/SRIM)

 Stopping power: 

$$
\frac{dE}{dx} = S_n(E) + S_e(E)
$$

- $S_n(E)$ — nuclear stopping power
- $S_e(E)$ — electronic stopping power

 Key outputs: 

- Ion trajectories via binary collision approximation (BCA)
- Damage cascade distribution
- Sputtering yield
- Vacancy and interstitial generation profiles

    4.4 Channeling Effects

For crystalline targets, ions aligned with crystal axes experience:

- Reduced stopping power
- Deeper penetration
- Modified range distributions
- Requires dual-Pearson or Monte Carlo models



   5. Plasma Etching

    5.1 Surface Kinetics Model

$$
\frac{\partial \theta}{\partial t} = J_i s_i (1 - \theta) - k_r \theta
$$

 Variables: 

- $\theta$ — fractional surface coverage of reactive species
- $J_i$ — incident ion/radical flux
- $s_i$ — sticking coefficient
- $k_r$ — surface reaction rate constant

    5.2 Etching Yield

$$
Y = \frac{\text{atoms removed}}{\text{incident ion}}
$$

 Dependence factors: 

- Ion energy ($E_{ion}$)
- Ion incidence angle ($\theta$)
- Ion-to-neutral flux ratio
- Surface chemistry and temperature

    5.3 Profile Evolution (Level Set Method)

$$
\frac{\partial \phi}{\partial t} + V |
abla \phi| = 0
$$

 Variables: 

- $\phi(\mathbf{x}, t)$ — level set function (surface defined by $\phi = 0$)
- $V$ — local etch rate (normal velocity)

    5.4 Knudsen Transport in High Aspect Ratio Features

For molecular flow regime ($Kn > 1$):

$$
\frac{1}{\lambda} \frac{dI}{dx} = -I + \int K(x, x') I(x') dx'
$$

 Key effects: 

- Aspect ratio dependent etching (ARDE)
- Reactive ion angular distribution (RIAD)
- Neutral shadowing



   6. Chemical Vapor Deposition (CVD)

    6.1 Transport-Reaction Equation

$$
\frac{\partial C}{\partial t} + \mathbf{v} \cdot 
abla C = D 
abla^2 C - k C^n
$$

 Variables: 

- $C$ — reactant concentration
- $\mathbf{v}$ — gas velocity
- $D$ — gas-phase diffusivity
- $k$ — reaction rate constant
- $n$ — reaction order

    6.2 Thiele Modulus

$$
\phi = L \sqrt{\frac{k}{D}}
$$

 Regimes: 

- $\phi \ll 1$ — reaction-limited (uniform deposition)
- $\phi \gg 1$ — transport-limited (poor step coverage)

    6.3 Step Coverage

 Conformality factor: 

$$
S = \frac{\text{thickness at bottom}}{\text{thickness at top}}
$$

 Models: 

- Ballistic transport (line-of-sight)
- Knudsen diffusion
- Surface reaction probability

    6.4 Atomic Layer Deposition (ALD)

 Self-limiting surface coverage: 

$$
\theta(t) = 1 - \exp\left( -\frac{p \cdot t}{\tau} \right)
$$

 Variables: 

- $\theta(t)$ — fractional surface coverage
- $p$ — precursor partial pressure
- $\tau$ — characteristic adsorption time

 Growth per cycle (GPC): 

$$
\text{GPC} = \theta_{sat} \cdot \Gamma_{ML}
$$

where $\Gamma_{ML}$ is the monolayer thickness.



   7. Chemical Mechanical Polishing (CMP)

    7.1 Preston Equation

$$
\frac{dz}{dt} = K_p \cdot P \cdot V
$$

 Variables: 

- $dz/dt$ — material removal rate (MRR)
- $K_p$ — Preston coefficient ($\text{m}^2/\text{N}$)
- $P$ — applied pressure
- $V$ — relative velocity

    7.2 Pattern-Dependent Effects

 Effective pressure: 

$$
P_{eff} = \frac{P_{applied}}{\rho_{pattern}}
$$

where $\rho_{pattern}$ is local pattern density.

 Key phenomena: 

-  Dishing:  over-polishing of soft materials (e.g., Cu)
-  Erosion:  oxide loss in high-density regions
-  Within-die non-uniformity (WIDNU) 

    7.3 Contact Mechanics

 Hertzian contact pressure: 

$$
P(r) = P_0 \sqrt{1 - \left(\frac{r}{a}\right)^2}
$$

 Pad asperity models: 

- Greenwood-Williamson for rough surfaces
- Viscoelastic pad behavior



   8. Lithography

    8.1 Aerial Image Formation

 Hopkins formulation (partially coherent): 

$$
I(\mathbf{x}) = \iint TCC(\mathbf{f}, \mathbf{f}') \, M(\mathbf{f}) \, M^*(\mathbf{f}') \, e^{2\pi i (\mathbf{f} - \mathbf{f}') \cdot \mathbf{x}} \, d\mathbf{f} \, d\mathbf{f}'
$$

 Variables: 

- $I(\mathbf{x})$ — intensity at image plane position $\mathbf{x}$
- $TCC$ — transmission cross-coefficient
- $M(\mathbf{f})$ — mask spectrum at spatial frequency $\mathbf{f}$

    8.2 Resolution and Depth of Focus

 Rayleigh resolution criterion: 

$$
R = k_1 \frac{\lambda}{NA}
$$

 Depth of focus: 

$$
DOF = k_2 \frac{\lambda}{NA^2}
$$

 Variables: 

- $\lambda$ — exposure wavelength (e.g., 193 nm for DUV, 13.5 nm for EUV)
- $NA$ — numerical aperture
- $k_1, k_2$ — process-dependent factors

    8.3 Photoresist Exposure (Dill Model)

 Photoactive compound (PAC) decomposition: 

$$
\frac{\partial m}{\partial t} = -I(z, t) \cdot m \cdot C
$$

 Intensity attenuation: 

$$
I(z, t) = I_0 \exp\left( -\int_0^z [A \cdot m(z', t) + B] \, dz' \right)
$$

 Dill parameters: 

- $A$ — bleachable absorption coefficient
- $B$ — non-bleachable absorption coefficient
- $C$ — exposure rate constant
- $m$ — normalized PAC concentration

    8.4 Development Rate (Mack Model)

$$
r = r_{max} \frac{(a + 1)(1 - m)^n}{a + (1 - m)^n}
$$

 Variables: 

- $r$ — development rate
- $r_{max}$ — maximum development rate
- $m$ — normalized PAC concentration
- $a, n$ — resist contrast parameters

    8.5 Computational Lithography

-  Optical Proximity Correction (OPC):  inverse problem to find mask patterns
-  Source-Mask Optimization (SMO):  co-optimize illumination and mask
-  Inverse Lithography Technology (ILT):  pixel-based mask optimization



   9. Device Simulation (TCAD)

    9.1 Poisson's Equation

$$

abla \cdot (\epsilon 
abla \psi) = -q(p - n + N_D^+ - N_A^-)
$$

 Variables: 

- $\psi$ — electrostatic potential
- $\epsilon$ — permittivity
- $q$ — elementary charge
- $n, p$ — electron and hole concentrations
- $N_D^+, N_A^-$ — ionized donor and acceptor concentrations

    9.2 Carrier Continuity Equations

 Electrons: 

$$
\frac{\partial n}{\partial t} = \frac{1}{q} 
abla \cdot \mathbf{J}_n + G - R
$$

 Holes: 

$$
\frac{\partial p}{\partial t} = -\frac{1}{q} 
abla \cdot \mathbf{J}_p + G - R
$$

 Variables: 

- $\mathbf{J}_n, \mathbf{J}_p$ — electron and hole current densities
- $G$ — carrier generation rate
- $R$ — carrier recombination rate

    9.3 Drift-Diffusion Current Equations

 Electron current: 

$$
\mathbf{J}_n = q n \mu_n \mathbf{E} + q D_n 
abla n
$$

 Hole current: 

$$
\mathbf{J}_p = q p \mu_p \mathbf{E} - q D_p 
abla p
$$

 Einstein relation: 

$$
D = \frac{k_B T}{q} \mu
$$

    9.4 Advanced Transport Models

-  Hydrodynamic model:  includes carrier temperature
-  Monte Carlo:  tracks individual carrier scattering events
-  Quantum corrections:  density gradient, NEGF for tunneling



   10. Yield Modeling

    10.1 Poisson Yield Model

$$
Y = e^{-A D_0}
$$

 Variables: 

- $Y$ — chip yield
- $A$ — chip area
- $D_0$ — defect density ($\text{defects/cm}^2$)

    10.2 Negative Binomial Model (Clustered Defects)

$$
Y = \left(1 + \frac{A D_0}{\alpha}\right)^{-\alpha}
$$

 Variables: 

- $\alpha$ — clustering parameter
- As $\alpha \to \infty$, reduces to Poisson model

    10.3 Critical Area Analysis

$$
Y = \exp\left( -\sum_i D_i \cdot A_{c,i} \right)
$$

 Variables: 

- $D_i$ — defect density for defect type $i$
- $A_{c,i}$ — critical area sensitive to defect type $i$

 Critical area depends on: 

- Defect size distribution
- Layout geometry
- Defect type (shorts, opens, particles)



   11. Statistical and Machine Learning Methods

    11.1 Response Surface Methodology (RSM)

 Second-order model: 

$$
y = \beta_0 + \sum_{i=1}^{k} \beta_i x_i + \sum_{i=1}^{k} \beta_{ii} x_i^2 + \sum_{i<j} \beta_{ij} x_i x_j + \epsilon
$$

    11.2 Design of Experiments (DOE)

| Design Type | Application |
|------------|-------------|
| Full factorial | Complete parameter space exploration |
| Fractional factorial | Screening many factors |
| Central composite | RSM fitting |
| Box-Behnken | Efficient quadratic modeling |
| Taguchi | Robust design optimization |

    11.3 Statistical Process Control (SPC)

 Process capability indices: 

$$
C_p = \frac{USL - LSL}{6\sigma}
$$

$$
C_{pk} = \min\left( \frac{USL - \mu}{3\sigma}, \frac{\mu - LSL}{3\sigma} \right)
$$

    11.4 Machine Learning Applications

| Method | Application |
|--------|-------------|
| Neural Networks | Process-property prediction |
| Gaussian Process Regression | Surrogate modeling |
| Random Forest | Defect classification |
| Bayesian Optimization | Recipe tuning |
| Convolutional Neural Networks | Defect detection in images |
| Recurrent Neural Networks | Time-series process data |



   12. Multi-Scale Modeling

    12.1 Modeling Hierarchy

| Scale | Length | Method | Example |
|-------|--------|--------|---------|
| Quantum | < 1 nm | DFT, ab initio MD | Reaction barriers |
| Atomistic | 1–100 nm | Classical MD | Surface diffusion |
| Mesoscale | 100 nm – 1 μm | Kinetic Monte Carlo | Dopant clustering |
| Continuum | > 1 μm | FEM, FDM | Process simulation |
| System | Wafer/die | Statistical | Yield modeling |

    12.2 Bridging Methods

-  Coarse-graining:  atomistic → mesoscale
-  Parameter extraction:  quantum → continuum
-  Concurrent multiscale:  couple different scales simultaneously



   13. Key Mathematical Toolkit

    13.1 Partial Differential Equations

-  Diffusion equation:  $\frac{\partial u}{\partial t} = D 
abla^2 u$
-  Heat equation:  $\rho c_p \frac{\partial T}{\partial t} = 
abla \cdot (k 
abla T)$
-  Navier-Stokes:  $\rho \frac{D\mathbf{v}}{Dt} = -
abla p + \mu 
abla^2 \mathbf{v} + \mathbf{f}$
-  Poisson:  $
abla^2 \phi = -\rho/\epsilon$
-  Level set:  $\frac{\partial \phi}{\partial t} + \mathbf{v} \cdot 
abla \phi = 0$

    13.2 Numerical Methods

-  Finite Difference Method (FDM):  simple geometries
-  Finite Element Method (FEM):  complex geometries
-  Finite Volume Method (FVM):  conservation laws
-  Monte Carlo:  stochastic processes, particle transport
-  Level Set / Volume of Fluid:  interface tracking

    13.3 Optimization Techniques

- Gradient descent and conjugate gradient
- Newton-Raphson method
- Genetic algorithms
- Simulated annealing
- Bayesian optimization

    13.4 Stochastic Processes

- Random walk (diffusion)
- Poisson processes (defect generation)
- Markov chains (KMC)
- Birth-death processes (nucleation)



   14. Modern Challenges

    14.1 Random Dopant Fluctuation (RDF)

 Threshold voltage variation: 

$$
\sigma_{V_T} \propto \frac{1}{\sqrt{W \cdot L}} \cdot \frac{t_{ox}}{\sqrt{N_A}}
$$

    14.2 Line Edge Roughness (LER)

 Power spectral density: 

$$
PSD(f) = \frac{2\sigma^2 \xi}{1 + (2\pi f \xi)^{2(1+H)}}
$$

 Variables: 

- $\sigma$ — RMS roughness amplitude
- $\xi$ — correlation length
- $H$ — Hurst exponent

    14.3 Stochastic Effects in EUV Lithography

-  Photon shot noise:  $\sigma_N = \sqrt{N}$ where $N$ = absorbed photons
-  Secondary electron blur 
-  Resist stochastics:  acid generation, diffusion, deprotection

    14.4 3D Device Architectures

Modern modeling must handle:

-  FinFET:  3D fin geometry
-  Gate-All-Around (GAA):  nanowire/nanosheet
-  CFET:  stacked complementary FETs
-  3D NAND:  vertical channel, charge trap

    14.5 Emerging Modeling Approaches

-  Physics-Informed Neural Networks (PINNs) 
-  Digital twins  for real-time process control
-  Reduced-order models  for fast simulation
-  Uncertainty quantification  for variability prediction

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/mathematics) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
