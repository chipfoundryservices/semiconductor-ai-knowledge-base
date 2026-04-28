# Device Physics & Mathematical Modeling

**Keywords**: device physics mathematics,device physics math,semiconductor device physics,TCAD modeling,drift diffusion,poisson equation,mosfet physics,quantum effects

---

**Device Physics & Mathematical Modeling**

  1. Fundamental Mathematical Structure

Semiconductor modeling is built on  coupled nonlinear partial differential equations  spanning multiple scales:

| Scale | Methods | Typical Equations |
|:------|:--------|:------------------|
| Quantum (< 1 nm) | DFT, Schrödinger | $H\psi = E\psi$ |
| Atomistic (1–100 nm) | MD, Kinetic Monte Carlo | Newton's equations, master equations |
| Continuum (nm–mm) | Drift-diffusion, FEM | PDEs (Poisson, continuity, heat) |
| Circuit | SPICE | ODEs, compact models |

   Multiscale Hierarchy

The mathematics forms a hierarchy of models through successive averaging:

$$
\boxed{\text{Schrödinger} \xrightarrow{\text{averaging}} \text{Boltzmann} \xrightarrow{\text{moments}} \text{Drift-Diffusion} \xrightarrow{\text{fitting}} \text{Compact Models}}
$$



  2. Process Physics & Models

   2.1 Oxidation: Deal-Grove Model

Thermal oxidation of silicon follows  linear-parabolic kinetics :

$$
\frac{dx_{ox}}{dt} = \frac{B}{A + 2x_{ox}}
$$

where:

- $x_{ox}$ = oxide thickness
- $B/A$ = linear rate constant (surface-reaction limited)
- $B$ = parabolic rate constant (diffusion limited)

 Limiting Cases: 

-  Thin oxide  (reaction-limited):
  
  $$
  x_{ox} \approx \frac{B}{A} \cdot t
  $$

-  Thick oxide  (diffusion-limited):
  
  $$
  x_{ox} \approx \sqrt{B \cdot t}
  $$

 Physical Mechanism: 

1. O₂ transport from gas to oxide surface
2. O₂ diffusion through growing SiO₂ layer
3. Reaction at Si/SiO₂ interface: $\text{Si} + \text{O}_2 \rightarrow \text{SiO}_2$

>  Note:  This is a  Stefan problem  (moving boundary PDE).



   2.2 Diffusion: Fick's Laws

Dopant redistribution follows  Fick's second law :

$$
\frac{\partial C}{\partial t} = 
abla \cdot \left( D(C, T) 
abla C \right)
$$

For constant $D$ in 1D:

$$
\frac{\partial C}{\partial t} = D \frac{\partial^2 C}{\partial x^2}
$$

 Analytical Solutions (1D, constant D): 

-  Constant surface concentration  (infinite source):
  
  $$
  C(x,t) = C_s \cdot \text{erfc}\left( \frac{x}{2\sqrt{Dt}} \right)
  $$

-  Limited source  (e.g., implant drive-in):
  
  $$
  C(x,t) = \frac{Q}{\sqrt{\pi D t}} \exp\left( -\frac{x^2}{4Dt} \right)
  $$

  where $Q$ = dose (atoms/cm²)

 Complications at High Concentrations: 

-  Concentration-dependent diffusivity:  $D = D(C)$
-  Electric field effects:  Charged point defects create internal fields
-  Vacancy/interstitial mechanisms:  Different diffusion pathways

$$
\frac{\partial C}{\partial t} = \frac{\partial}{\partial x}\left[ D(C) \frac{\partial C}{\partial x} \right] + \mu C \frac{\partial \phi}{\partial x}
$$



   2.3 Ion Implantation: Range Theory

The implanted dopant profile is approximately  Gaussian :

$$
C(x) = \frac{\Phi}{\sqrt{2\pi} \Delta R_p} \exp\left( -\frac{(x - R_p)^2}{2 (\Delta R_p)^2} \right)
$$

where:

- $\Phi$ = implant dose (ions/cm²)
- $R_p$ = projected range (mean depth)
- $\Delta R_p$ = straggle (standard deviation)

 LSS Theory  (Lindhard-Scharff-Schiøtt) predicts stopping power:

$$
-\frac{dE}{dx} = N \left[ S_n(E) + S_e(E) \right]
$$

where:

- $S_n(E)$ = nuclear stopping power (dominant at low energy)
- $S_e(E)$ = electronic stopping power (dominant at high energy)
- $N$ = target atomic density

 For asymmetric profiles , the  Pearson IV distribution  is used:

$$
C(x) = \frac{\Phi \cdot K}{\Delta R_p} \left[ 1 + \left( \frac{x - R_p}{a} \right)^2 \right]^{-m} \exp\left[ -
u \arctan\left( \frac{x - R_p}{a} \right) \right]
$$

>  Modern approach:  Monte Carlo codes (SRIM/TRIM) for accurate profiles including channeling effects.



   2.4 Lithography: Optical Imaging

Aerial image formation follows  Hopkins' partially coherent imaging theory :

$$
I(\mathbf{r}) = \iint TCC(f, f') \cdot \tilde{M}(f) \cdot \tilde{M}^*(f') \cdot e^{2\pi i (f - f') \cdot \mathbf{r}} \, df \, df'
$$

where:

- $TCC$ = Transmission Cross-Coefficient
- $\tilde{M}(f)$ = mask spectrum (Fourier transform of mask pattern)
- $\mathbf{r}$ = position in image plane

 Fundamental Limits: 

-  Rayleigh resolution criterion: 
  
  $$
  CD_{\min} = k_1 \frac{\lambda}{NA}
  $$

-  Depth of focus: 
  
  $$
  DOF = k_2 \frac{\lambda}{NA^2}
  $$

where:

- $\lambda$ = wavelength (193 nm for ArF, 13.5 nm for EUV)
- $NA$ = numerical aperture
- $k_1, k_2$ = process-dependent factors

 Resist Modeling — Dill Equations: 

$$
\frac{\partial M}{\partial t} = -C \cdot I(z) \cdot M
$$

$$
\frac{dI}{dz} = -(\alpha M + \beta) I
$$

where $M$ = photoactive compound concentration.



   2.5 Etching & Deposition: Surface Evolution

Topography evolution is modeled with the  level set method :

$$
\frac{\partial \phi}{\partial t} + V |
abla \phi| = 0
$$

where:

- $\phi(\mathbf{r}, t) = 0$ defines the surface
- $V$ = local velocity (etch rate or deposition rate)

 For anisotropic etching: 

$$
V = V(\theta, \phi, \text{ion flux}, \text{chemistry})
$$

 CVD in High Aspect Ratio Features: 

Knudsen diffusion limits step coverage:

$$
\frac{\partial C}{\partial t} = D_K 
abla^2 C - k_s C \cdot \delta_{\text{surface}}
$$

where:

- $D_K = \frac{d}{3}\sqrt{\frac{8k_BT}{\pi m}}$ (Knudsen diffusivity)
- $d$ = feature width
- $k_s$ = surface reaction rate

 ALD (Atomic Layer Deposition): 

Self-limiting surface reactions follow Langmuir kinetics:

$$
\theta = \frac{K \cdot P}{1 + K \cdot P}
$$

where $\theta$ = surface coverage, $P$ = precursor partial pressure.



  3. Device Physics: Semiconductor Equations

The core mathematical framework for device simulation consists of  three coupled PDEs :

   3.1 Poisson's Equation (Electrostatics)

$$

abla \cdot (\varepsilon 
abla \psi) = -q \left( p - n + N_D^+ - N_A^- \right)
$$

where:

- $\psi$ = electrostatic potential
- $n, p$ = electron and hole concentrations
- $N_D^+, N_A^-$ = ionized donor and acceptor concentrations

   3.2 Continuity Equations (Carrier Conservation)

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

where:

- $G$ = generation rate
- $R$ = recombination rate

   3.3 Current Density Equations (Transport)

 Drift-Diffusion Model: 

$$
\mathbf{J}_n = q \mu_n n \mathbf{E} + q D_n 
abla n
$$

$$
\mathbf{J}_p = q \mu_p p \mathbf{E} - q D_p 
abla p
$$

 Einstein Relation: 

$$
\frac{D_n}{\mu_n} = \frac{D_p}{\mu_p} = \frac{k_B T}{q} = V_T
$$

   3.4 Recombination Models

 Shockley-Read-Hall (SRH) Recombination: 

$$
R_{SRH} = \frac{np - n_i^2}{\tau_p (n + n_1) + \tau_n (p + p_1)}
$$

 Auger Recombination: 

$$
R_{Auger} = C_n n (np - n_i^2) + C_p p (np - n_i^2)
$$

 Radiative Recombination: 

$$
R_{rad} = B (np - n_i^2)
$$

   3.5 MOSFET Physics

 Threshold Voltage: 

$$
V_T = V_{FB} + 2\phi_B + \frac{\sqrt{2 \varepsilon_{Si} q N_A (2\phi_B)}}{C_{ox}}
$$

where:

- $V_{FB}$ = flat-band voltage
- $\phi_B = \frac{k_BT}{q} \ln\left(\frac{N_A}{n_i}\right)$ = bulk potential
- $C_{ox} = \frac{\varepsilon_{ox}}{t_{ox}}$ = oxide capacitance

 Drain Current (Gradual Channel Approximation): 

-  Linear region  ($V_{DS} < V_{GS} - V_T$):
  
  $$
  I_D = \frac{W}{L} \mu_n C_{ox} \left[ (V_{GS} - V_T) V_{DS} - \frac{V_{DS}^2}{2} \right]
  $$

-  Saturation region  ($V_{DS} \geq V_{GS} - V_T$):
  
  $$
  I_D = \frac{W}{2L} \mu_n C_{ox} (V_{GS} - V_T)^2
  $$



  4. Quantum Effects at Nanoscale

For modern devices with gate lengths $L_g < 10$ nm, classical models fail.

   4.1 Quantum Confinement

In thin silicon channels, carrier energy becomes  quantized :

$$
E_n = \frac{\hbar^2 \pi^2 n^2}{2 m^* t_{Si}^2}
$$

where:

- $n$ = quantum number (1, 2, 3, ...)
- $m^*$ = effective mass
- $t_{Si}$ = silicon body thickness

 Effects: 

- Increased threshold voltage
- Modified density of states: $g_{2D}(E) = \frac{m^*}{\pi \hbar^2}$ (step function)

   4.2 Quantum Tunneling

 Gate Leakage (Direct Tunneling): 

WKB approximation:

$$
T \approx \exp\left( -2 \int_0^{t_{ox}} \kappa(x) \, dx \right)
$$

where $\kappa = \sqrt{\frac{2m^*(\Phi_B - E)}{\hbar^2}}$

 Source-Drain Tunneling: 

Limits OFF-state current in ultra-short channels.

 Band-to-Band Tunneling: 

Enables Tunnel FETs (TFETs):

$$
I_{BTBT} \propto \exp\left( -\frac{4\sqrt{2m^*} E_g^{3/2}}{3q\hbar |\mathbf{E}|} \right)
$$

   4.3 Ballistic Transport

When channel length $L < \lambda_{mfp}$ (mean free path), the  Landauer formalism  applies:

$$
I = \frac{2q}{h} \int T(E) \left[ f_S(E) - f_D(E) \right] dE
$$

where:

- $T(E)$ = transmission probability
- $f_S, f_D$ = source and drain Fermi functions

 Ballistic Conductance Quantum: 

$$
G_0 = \frac{2q^2}{h} \approx 77.5 \, \mu\text{S}
$$

   4.4 NEGF Formalism

The  Non-Equilibrium Green's Function  method is the gold standard for quantum transport:

$$
G^R = \left[ EI - H - \Sigma_1 - \Sigma_2 \right]^{-1}
$$

where:

- $H$ = device Hamiltonian
- $\Sigma_1, \Sigma_2$ = contact self-energies
- $G^R$ = retarded Green's function

 Observables: 

- Electron density: $n(\mathbf{r}) = -\frac{1}{\pi} \text{Im}[G^<(\mathbf{r}, \mathbf{r}; E)]$
- Current: $I = \frac{q}{h} \text{Tr}[\Gamma_1 G^R \Gamma_2 G^A]$



  5. Numerical Methods

   5.1 Discretization: Scharfetter-Gummel Scheme

The drift-diffusion current requires special treatment to avoid numerical instability:

$$
J_{n,i+1/2} = \frac{q D_n}{h} \left[ n_{i+1} B\left( -\frac{\Delta \psi}{V_T} \right) - n_i B\left( \frac{\Delta \psi}{V_T} \right) \right]
$$

where the  Bernoulli function  is:

$$
B(x) = \frac{x}{e^x - 1}
$$

 Properties: 

- $B(0) = 1$
- $B(x) \to 0$ as $x \to \infty$
- $B(-x) = x + B(x)$

   5.2 Solution Strategies

 Gummel Iteration (Decoupled): 

1. Solve Poisson for $\psi$ (fixed $n$, $p$)
2. Solve electron continuity for $n$ (fixed $\psi$, $p$)
3. Solve hole continuity for $p$ (fixed $\psi$, $n$)
4. Repeat until convergence

 Newton-Raphson (Fully Coupled): 

Solve the Jacobian system:

$$
\begin{pmatrix}
\frac{\partial F_\psi}{\partial \psi} & \frac{\partial F_\psi}{\partial n} & \frac{\partial F_\psi}{\partial p} \\
\frac{\partial F_n}{\partial \psi} & \frac{\partial F_n}{\partial n} & \frac{\partial F_n}{\partial p} \\
\frac{\partial F_p}{\partial \psi} & \frac{\partial F_p}{\partial n} & \frac{\partial F_p}{\partial p}
\end{pmatrix}
\begin{pmatrix}
\delta \psi \\
\delta n \\
\delta p
\end{pmatrix}
= -
\begin{pmatrix}
F_\psi \\
F_n \\
F_p
\end{pmatrix}
$$

   5.3 Time Integration

 Stiffness Problem: 

Time scales span ~15 orders of magnitude:

| Process | Time Scale |
|:--------|:-----------|
| Carrier relaxation | ~ps |
| Thermal response | ~μs–ms |
| Dopant diffusion | min–hours |

 Solution:  Use  implicit methods  (Backward Euler, BDF).

   5.4 Mesh Requirements

 Debye Length Constraint: 

The mesh must resolve the Debye length:

$$
\lambda_D = \sqrt{\frac{\varepsilon k_B T}{q^2 n}}
$$

For $n = 10^{18}$ cm⁻³: $\lambda_D \approx 4$ nm

 Adaptive Mesh Refinement: 

- Refine near junctions, interfaces, corners
- Coarsen in bulk regions
- Use Delaunay triangulation for quality



  6. Compact Models for Circuit Simulation

For SPICE-level simulation, physics is abstracted into algebraic/empirical equations.

   Industry Standard Models

| Model | Device | Key Features |
|:------|:-------|:-------------|
| BSIM4 | Planar MOSFET | ~300 parameters, channel length modulation |
| BSIM-CMG | FinFET | Tri-gate geometry, quantum effects |
| BSIM-GAA | Nanosheet | Stacked channels, sheet width |
| PSP | Bulk MOSFET | Surface-potential-based |

   Key Physics Captured

-  Short-channel effects:  DIBL, $V_T$ roll-off
-  Quantum corrections:  Inversion layer quantization
-  Mobility degradation:  Surface scattering, velocity saturation
-  Parasitic effects:  Series resistance, overlap capacitance
-  Variability:  Statistical mismatch models

   Threshold Voltage Variability (Pelgrom's Law)

$$
\sigma_{V_T} = \frac{A_{VT}}{\sqrt{W \cdot L}}
$$

where $A_{VT}$ is a technology-dependent constant.



  7. TCAD Co-Simulation Workflow

The complete semiconductor design flow:

```text
┌─────────────────────────────────────────────────────────────┐
│  ┌───────────────┐   ┌───────────────┐   ┌───────────────┐  │
│  │   Process     │──▶│    Device     │──▶│   Parameter   │  │
│  │  Simulation   │   │  Simulation   │   │  Extraction   │  │
│  │  (Sentaurus)  │   │  (Sentaurus)  │   │ (BSIM Fit)    │  │
│  └───────────────┘   └───────────────┘   └───────────────┘  │
│         │                   │                   │           │
│         ▼                   ▼                   ▼           │
│  ┌───────────────┐   ┌───────────────┐   ┌───────────────┐  │
│  │• Implantation │   │• I-V, C-V     │   │• BSIM params  │  │
│  │• Diffusion    │   │• Breakdown    │   │• Corner extr. │  │
│  │• Oxidation    │   │• Hot carrier  │   │• Variability  │  │
│  │• Etching      │   │• Noise        │   │  statistics   │  │
│  └───────────────┘   └───────────────┘   └───────────────┘  │
│                                                │            │
│                                                ▼            │
│                                         ┌───────────────┐   │
│                                         │    Circuit    │   │
│                                         │  Simulation   │   │
│                                         │(SPICE,Spectre)│   │
│                                         └───────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

 Key Challenge:  Propagating variability through the entire chain:

- Line Edge Roughness (LER)
- Random Dopant Fluctuation (RDF)
- Work function variation
- Thickness variations



  8. Mathematical Frontiers

   8.1 Machine Learning + Physics

-  Physics-Informed Neural Networks (PINNs): 
  
  $$
  \mathcal{L} = \mathcal{L}_{data} + \lambda \mathcal{L}_{physics}
  $$
  
  where $\mathcal{L}_{physics}$ enforces PDE residuals.

-  Surrogate models  for expensive TCAD simulations
-  Inverse design  and topology optimization
-  Defect prediction  in manufacturing

   8.2 Stochastic Modeling

 Random Dopant Fluctuation: 

$$
\sigma_{V_T} \propto \frac{t_{ox}}{\sqrt{W \cdot L \cdot N_A}}
$$

 Approaches: 

- Atomistic Monte Carlo (place individual dopants)
- Statistical impedance field method
- Compact model statistical extensions

   8.3 Multiphysics Coupling

 Electro-Thermal Self-Heating: 

$$
\rho C_p \frac{\partial T}{\partial t} = 
abla \cdot (\kappa 
abla T) + \mathbf{J} \cdot \mathbf{E}
$$

 Stress Effects on Mobility (Piezoresistance): 

$$
\frac{\Delta \mu}{\mu_0} = \pi_L \sigma_L + \pi_T \sigma_T
$$

 Electromigration in Interconnects: 

$$
\mathbf{J}_{atoms} = \frac{D C}{k_B T} \left( Z^* q \mathbf{E} - \Omega 
abla \sigma \right)
$$

   8.4 Atomistic-Continuum Bridging

 Strategies: 

- Coarse-graining from MD/DFT
- Density gradient quantum corrections:
  
  $$
  V_{QM} = \frac{\gamma \hbar^2}{12 m^*} \frac{
abla^2 \sqrt{n}}{\sqrt{n}}
  $$

- Hybrid methods: atomistic core + continuum far-field



  

The mathematics of semiconductor manufacturing and device physics encompasses:

$$
\boxed{
\begin{aligned}
&\text{Process:} && \text{Stefan problems, diffusion PDEs, reaction kinetics} \\
&\text{Device:} && \text{Coupled Poisson + continuity equations} \\
&\text{Quantum:} && \text{Schrödinger, NEGF, tunneling} \\
&\text{Numerical:} && \text{FEM/FDM, Scharfetter-Gummel, Newton iteration} \\
&\text{Circuit:} && \text{Compact models (BSIM), variability statistics}
\end{aligned}
}
$$

Each level trades  accuracy  for  computational tractability . The art lies in knowing when each approximation breaks down—and modern scaling is pushing us toward the quantum limit where classical continuum models become inadequate.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/device-physics-mathematics) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
