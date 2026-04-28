# Semiconductor Manufacturing Process: Materials Science & Mathematical Modeling

**Keywords**: material science mathematics, materials science mathematics, materials science modeling, semiconductor materials math, crystal growth equations, thin film mathematics, thermodynamics semiconductor, materials modeling

---

**Semiconductor Manufacturing Process: Materials Science & Mathematical Modeling**

A comprehensive guide to the physics, chemistry, and mathematics underlying modern semiconductor fabrication.


**1. Overview**

Modern semiconductor manufacturing is one of the most complex and precise engineering endeavors ever undertaken. Key characteristics include:

- **Feature sizes**: Leading-edge nodes at 3nm, 2nm, and research into sub-nm
- **Precision requirements**: Atomic-level control (angstrom tolerances)
- **Process steps**: Hundreds of sequential operations per chip
- **Yield sensitivity**: Parts-per-billion defect control

**1.1 Core Process Steps**

- **Crystal Growth**
  - Czochralski (CZ) process
  - Float-zone (FZ) refining
  - Epitaxial growth

- **Pattern Definition**
  - Photolithography (DUV, EUV)
  - Electron-beam lithography
  - Nanoimprint lithography

- **Material Addition**
  - Chemical Vapor Deposition (CVD)
  - Physical Vapor Deposition (PVD)
  - Atomic Layer Deposition (ALD)
  - Epitaxy (MBE, MOCVD)

- **Material Removal**
  - Wet etching (isotropic)
  - Dry/plasma etching (anisotropic)
  - Chemical Mechanical Polishing (CMP)

- **Doping**
  - Ion implantation
  - Thermal diffusion
  - Plasma doping

- **Thermal Processing**
  - Oxidation
  - Annealing (RTA, spike, laser)
  - Silicidation



**2. Materials Science Foundations**

**2.1 Silicon Properties**

- **Crystal structure**: Diamond cubic (Fd3m space group)
- **Lattice constant**: $a = 5.431 \text{ Å}$
- **Bandgap**: $E_g = 1.12 \text{ eV}$ (indirect, at 300K)
- **Intrinsic carrier concentration**:

$$n_i = \sqrt{N_c N_v} \exp\left(-\frac{E_g}{2k_B T}\right)$$

At 300K: $n_i \approx 1.0 \times 10^{10} \text{ cm}^{-3}$

**2.2 Crystal Defects**

- **Point Defects**
  - **Vacancies (V)**: Missing lattice atoms
  - **Self-interstitials (I)**: Extra Si atoms in interstitial sites
  - **Substitutional impurities**: Dopants (B, P, As, Sb)
  - **Interstitial impurities**: Fast diffusers (Fe, Cu, Au)

- **Line Defects**
  - **Edge dislocations**: Extra half-plane of atoms
  - **Screw dislocations**: Helical atomic arrangement
  - **Dislocation density target**: $< 100 \text{ cm}^{-2}$ for device wafers

- **Planar Defects**
  - **Stacking faults**: ABCABC → ABCBCABC
  - **Twin boundaries**: Mirror symmetry planes
  - **Grain boundaries**: (avoided in single-crystal wafers)

**2.3 Dielectric Materials**

| Material | Dielectric Constant ($\kappa$) | Bandgap (eV) | Application |
|----------|-------------------------------|--------------|-------------|
| SiO₂ | 3.9 | 9.0 | Traditional gate oxide |
| Si₃N₄ | 7.5 | 5.3 | Spacers, hard masks |
| HfO₂ | ~25 | 5.8 | High-κ gate dielectric |
| Al₂O₃ | 9 | 8.8 | ALD dielectric |
| ZrO₂ | ~25 | 5.8 | High-κ gate dielectric |

**Equivalent Oxide Thickness (EOT)**:

$$\text{EOT} = t_{\text{high-}\kappa} \cdot \frac{\kappa_{\text{SiO}_2}}{\kappa_{\text{high-}\kappa}} = t_{\text{high-}\kappa} \cdot \frac{3.9}{\kappa_{\text{high-}\kappa}}$$

**2.4 Interconnect Materials**

- **Evolution**: Al/SiO₂ → Cu/low-κ → Cu/air-gap → (future: Ru, Co)
- **Electromigration** - Black's equation for mean time to failure:

$$\text{MTTF} = A \cdot j^{-n} \exp\left(\frac{E_a}{k_B T}\right)$$

Where:
- $j$ = current density
- $n$ ≈ 1-2 (current exponent)
- $E_a$ ≈ 0.7-0.9 eV for Cu



**3. Crystal Growth Modeling**

**3.1 Czochralski Process Physics**

The Czochralski process involves pulling a single crystal from a melt. Key phenomena:

- **Heat transfer** (conduction, convection, radiation)
- **Fluid dynamics** (buoyancy-driven and forced convection)
- **Mass transport** (dopant distribution)
- **Phase change** (solidification at the interface)

**3.2 Heat Transfer Equation**

$$\rho c_p \frac{\partial T}{\partial t} = 
abla \cdot (k 
abla T) + Q$$

Where:
- $\rho$ = density [kg/m³]
- $c_p$ = specific heat capacity [J/(kg·K)]
- $k$ = thermal conductivity [W/(m·K)]
- $Q$ = volumetric heat source [W/m³]

**3.3 Stefan Problem (Phase Change)**

At the solid-liquid interface, the Stefan condition applies:

$$k_s \frac{\partial T_s}{\partial n} - k_\ell \frac{\partial T_\ell}{\partial n} = \rho L v_n$$

Where:
- $k_s$, $k_\ell$ = thermal conductivity of solid and liquid
- $L$ = latent heat of fusion [J/kg]
- $v_n$ = interface velocity normal to the surface [m/s]

**3.4 Melt Convection (Navier-Stokes with Boussinesq Approximation)**

$$\rho \left( \frac{\partial \mathbf{v}}{\partial t} + \mathbf{v} \cdot 
abla \mathbf{v} \right) = -
abla p + \mu 
abla^2 \mathbf{v} + \rho \mathbf{g} \beta (T - T_0)$$

Dimensionless parameters:
- **Grashof number**: $Gr = \frac{g \beta \Delta T L^3}{
u^2}$
- **Prandtl number**: $Pr = \frac{
u}{\alpha}$
- **Rayleigh number**: $Ra = Gr \cdot Pr$

**3.5 Dopant Segregation**

**Equilibrium segregation coefficient**:

$$k_0 = \frac{C_s}{C_\ell}$$

**Effective segregation coefficient** (Burton-Prim-Slichter model):

$$k_{\text{eff}} = \frac{k_0}{k_0 + (1 - k_0) \exp\left(-\frac{v \delta}{D}\right)}$$

Where:
- $v$ = crystal pull rate [m/s]
- $\delta$ = boundary layer thickness [m]
- $D$ = diffusion coefficient in melt [m²/s]

**Dopant concentration along crystal** (normal freezing):

$$C_s(f) = k_{\text{eff}} C_0 (1 - f)^{k_{\text{eff}} - 1}$$

Where $f$ = fraction solidified.



**4. Diffusion Modeling**

**4.1 Fick's Laws**

**First Law** (flux proportional to concentration gradient):

$$\mathbf{J} = -D 
abla C$$

**Second Law** (conservation equation):

$$\frac{\partial C}{\partial t} = 
abla \cdot (D 
abla C)$$

For constant $D$ in 1D:

$$\frac{\partial C}{\partial t} = D \frac{\partial^2 C}{\partial x^2}$$

**4.2 Analytical Solutions**

**Constant surface concentration** (predeposition):

$$C(x,t) = C_s \cdot \text{erfc}\left(\frac{x}{2\sqrt{Dt}}\right)$$

**Fixed total dose** (drive-in):

$$C(x,t) = \frac{Q}{\sqrt{\pi D t}} \exp\left(-\frac{x^2}{4Dt}\right)$$

Where:
- $C_s$ = surface concentration
- $Q$ = total dose [atoms/cm²]
- $\text{erfc}(z) = 1 - \text{erf}(z)$ = complementary error function

**4.3 Temperature Dependence**

Diffusion coefficient follows Arrhenius behavior:

$$D = D_0 \exp\left(-\frac{E_a}{k_B T}\right)$$

| Dopant | $D_0$ (cm²/s) | $E_a$ (eV) |
|--------|---------------|------------|
| B | 0.76 | 3.46 |
| P | 3.85 | 3.66 |
| As | 0.32 | 3.56 |
| Sb | 0.214 | 3.65 |

**4.4 Point-Defect Mediated Diffusion**

Dopants diffuse via interactions with point defects. The total diffusivity:

$$D_{\text{eff}} = D_I \frac{C_I}{C_I^*} + D_V \frac{C_V}{C_V^*}$$

Where:
- $D_I$, $D_V$ = interstitial and vacancy components
- $C_I^*$, $C_V^*$ = equilibrium concentrations

**Coupled defect-dopant equations**:

$$\frac{\partial C_I}{\partial t} = D_I 
abla^2 C_I + G_I - k_{IV} C_I C_V$$

$$\frac{\partial C_V}{\partial t} = D_V 
abla^2 C_V + G_V - k_{IV} C_I C_V$$

Where:
- $G_I$, $G_V$ = generation rates
- $k_{IV}$ = I-V recombination rate constant

**4.5 Transient Enhanced Diffusion (TED)**

After ion implantation, excess interstitials cause enhanced diffusion:

- **"+1" model**: Each implanted ion creates ~1 net interstitial
- **TED factor**: Can enhance diffusion by 10-1000×
- **Decay time**: τ ~ seconds at high T, hours at low T



**5. Ion Implantation**

**5.1 Range Statistics**

**Gaussian approximation** (light ions, amorphous target):

$$n(x) = \frac{\phi}{\sqrt{2\pi} \Delta R_p} \exp\left(-\frac{(x - R_p)^2}{2 \Delta R_p^2}\right)$$

Where:
- $\phi$ = implant dose [ions/cm²]
- $R_p$ = projected range [nm]
- $\Delta R_p$ = range straggle (standard deviation) [nm]

**Pearson IV distribution** (heavier ions, includes skewness and kurtosis):

$$n(x) = \frac{\phi}{\Delta R_p} \cdot f\left(\frac{x - R_p}{\Delta R_p}; \gamma, \beta\right)$$

**5.2 Stopping Power**

**Total stopping power** (LSS theory):

$$S(E) = -\frac{1}{N}\frac{dE}{dx} = S_n(E) + S_e(E)$$

Where:
- $S_n(E)$ = nuclear stopping (elastic collisions with nuclei)
- $S_e(E)$ = electronic stopping (inelastic interactions with electrons)
- $N$ = atomic density of target

**Nuclear stopping** (screened Coulomb potential):

$$S_n(E) = \frac{\pi a^2 \gamma E}{1 + M_2/M_1}$$

Where:
- $a$ = screening length
- $\gamma = 4 M_1 M_2 / (M_1 + M_2)^2$

**Electronic stopping** (velocity-proportional regime):

$$S_e(E) = k_e \sqrt{E}$$

**5.3 Monte Carlo Simulation (BCA)**

The Binary Collision Approximation treats each collision as isolated:

1. **Free flight**: Ion travels until next collision
2. **Collision**: Classical two-body scattering
3. **Energy loss**: Nuclear + electronic contributions
4. **Repeat**: Until ion stops ($E < E_{\text{threshold}}$)

**Scattering angle** (center of mass frame):

$$\theta_{cm} = \pi - 2 \int_{r_{min}}^{\infty} \frac{b \, dr}{r^2 \sqrt{1 - V(r)/E_{cm} - b^2/r^2}}$$

**5.4 Damage Accumulation**

**Kinchin-Pease model** for displacement damage:

$$N_d = \frac{0.8 E_d}{2 E_{th}}$$

Where:
- $N_d$ = number of displaced atoms
- $E_d$ = damage energy deposited
- $E_{th}$ = displacement threshold (~15 eV for Si)

**Amorphization**: Occurs when damage density exceeds ~10% of atomic density



**6. Thermal Oxidation**

**6.1 Deal-Grove Model**

The oxide thickness $x$ as a function of time $t$:

$$x^2 + A x = B(t + \tau)$$

Or solved for thickness:

$$x = \frac{A}{2} \left( \sqrt{1 + \frac{4B(t + \tau)}{A^2}} - 1 \right)$$

**6.2 Rate Constants**

**Parabolic rate constant** (diffusion-limited):

$$B = \frac{2 D C^*}{N_1}$$

Where:
- $D$ = diffusion coefficient of O₂ in SiO₂
- $C^*$ = equilibrium concentration at surface
- $N_1$ = number of oxidant molecules per unit volume of oxide

**Linear rate constant** (reaction-limited):

$$\frac{B}{A} = \frac{k_s C^*}{N_1}$$

Where $k_s$ = surface reaction rate constant

**6.3 Limiting Cases**

**Thin oxide** ($x \ll A$): Linear regime

$$x \approx \frac{B}{A}(t + \tau)$$

**Thick oxide** ($x \gg A$): Parabolic regime

$$x \approx \sqrt{B(t + \tau)}$$

**6.4 Temperature and Pressure Dependence**

$$B = B_0 \exp\left(-\frac{E_B}{k_B T}\right) \cdot \frac{p}{p_0}$$

$$\frac{B}{A} = \left(\frac{B}{A}\right)_0 \exp\left(-\frac{E_{B/A}}{k_B T}\right) \cdot \frac{p}{p_0}$$

| Condition | $E_B$ (eV) | $E_{B/A}$ (eV) |
|-----------|------------|----------------|
| Dry O₂ | 1.23 | 2.0 |
| Wet O₂ (H₂O) | 0.78 | 2.05 |



**7. Chemical Vapor Deposition (CVD)**

**7.1 Reactor Transport Equations**

**Continuity equation**:

$$
abla \cdot (\rho \mathbf{v}) = 0$$

**Momentum equation** (Navier-Stokes):

$$\rho \left( \frac{\partial \mathbf{v}}{\partial t} + \mathbf{v} \cdot 
abla \mathbf{v} \right) = -
abla p + \mu 
abla^2 \mathbf{v} + \rho \mathbf{g}$$

**Energy equation**:

$$\rho c_p \left( \frac{\partial T}{\partial t} + \mathbf{v} \cdot 
abla T \right) = 
abla \cdot (k 
abla T) + \sum_i H_i R_i$$

**Species transport**:

$$\frac{\partial (\rho Y_i)}{\partial t} + 
abla \cdot (\rho \mathbf{v} Y_i) = 
abla \cdot (\rho D_i 
abla Y_i) + M_i \sum_j 
u_{ij} r_j$$

Where:
- $Y_i$ = mass fraction of species $i$
- $D_i$ = diffusion coefficient
- $
u_{ij}$ = stoichiometric coefficient
- $r_j$ = reaction rate of reaction $j$

**7.2 Surface Reaction Kinetics**

**Langmuir-Hinshelwood mechanism**:

$$R_s = \frac{k_s K_1 K_2 p_1 p_2}{(1 + K_1 p_1 + K_2 p_2)^2}$$

**First-order surface reaction**:

$$R_s = k_s C_s = k_s \cdot h_m (C_g - C_s)$$

At steady state:

$$C_s = \frac{h_m C_g}{h_m + k_s}$$

**7.3 Step Coverage**

**Thiele modulus** for feature filling:

$$\Phi = L \sqrt{\frac{k_s}{D_{\text{Kn}}}}$$

Where:
- $L$ = feature depth
- $D_{\text{Kn}}$ = Knudsen diffusion coefficient

**Step coverage behavior**:
- $\Phi \ll 1$: Reaction-limited → conformal deposition
- $\Phi \gg 1$: Transport-limited → poor step coverage

**7.4 Growth Rate**

$$G = \frac{M_f}{\rho_f} \cdot R_s = \frac{M_f}{\rho_f} \cdot \frac{h_m k_s C_g}{h_m + k_s}$$

Where:
- $M_f$ = molecular weight of film
- $\rho_f$ = film density



**8. Atomic Layer Deposition (ALD)**

**8.1 Self-Limiting Surface Reactions**

ALD relies on sequential, self-saturating surface reactions.

**Surface site model**:

$$\frac{d\theta}{dt} = k_{\text{ads}} p (1 - \theta) - k_{\text{des}} \theta$$

At steady state:

$$\theta_{eq} = \frac{K p}{1 + K p}$$

Where $K = k_{\text{ads}} / k_{\text{des}}$ = equilibrium constant

**8.2 Growth Per Cycle (GPC)**

$$\text{GPC} = \Gamma_{\text{max}} \cdot \theta \cdot \frac{M_f}{\rho_f N_A}$$

Where:
- $\Gamma_{\text{max}}$ = maximum surface site density [sites/cm²]
- $\theta$ = surface coverage (0 to 1)
- $N_A$ = Avogadro's number

**Typical GPC values**:
- Al₂O₃ (TMA/H₂O): ~1.1 Å/cycle
- HfO₂ (HfCl₄/H₂O): ~1.0 Å/cycle
- TiN (TiCl₄/NH₃): ~0.4 Å/cycle

**8.3 Conformality in High Aspect Ratio Features**

**Penetration depth**:

$$\Lambda = \sqrt{\frac{D_{\text{Kn}}}{k_s \Gamma_{\text{max}}}}$$

**Conformality factor**:

$$\text{CF} = \frac{1}{\sqrt{1 + (L/\Lambda)^2}}$$

For 100% conformality: Require $L \ll \Lambda$



**9. Plasma Etching**

**9.1 Plasma Fundamentals**

**Electron energy balance**:

$$n_e \frac{\partial}{\partial t}\left(\frac{3}{2} k_B T_e\right) = 
abla \cdot (\kappa_e 
abla T_e) + P_{\text{abs}} - P_{\text{loss}}$$

**Debye length** (shielding distance):

$$\lambda_D = \sqrt{\frac{\epsilon_0 k_B T_e}{n_e e^2}}$$

**Plasma frequency**:

$$\omega_{pe} = \sqrt{\frac{n_e e^2}{\epsilon_0 m_e}}$$

**9.2 Sheath Physics**

**Child-Langmuir law** (collisionless sheath):

$$J_i = \frac{4 \epsilon_0}{9} \sqrt{\frac{2e}{M_i}} \frac{V_s^{3/2}}{d^2}$$

Where:
- $J_i$ = ion current density
- $V_s$ = sheath voltage
- $d$ = sheath thickness
- $M_i$ = ion mass

**Bohm criterion** (ion velocity at sheath edge):

$$v_B = \sqrt{\frac{k_B T_e}{M_i}}$$

**9.3 Etch Rate Modeling**

**Ion-enhanced etching**:

$$R = R_{\text{chem}} + R_{\text{ion}} = k_n n_{\text{neutral}} + Y \cdot \Gamma_{\text{ion}}$$

Where:
- $R_{\text{chem}}$ = chemical (isotropic) component
- $R_{\text{ion}}$ = ion-enhanced (directional) component
- $Y$ = sputter yield
- $\Gamma_{\text{ion}}$ = ion flux

**Anisotropy**:

$$A = 1 - \frac{R_{\text{lateral}}}{R_{\text{vertical}}}$$

- $A = 0$: Isotropic
- $A = 1$: Perfectly anisotropic

**9.4 Feature-Scale Modeling**

**Level set equation** for surface evolution:

$$\frac{\partial \phi}{\partial t} + F |
abla \phi| = 0$$

Where:
- $\phi(\mathbf{x}, t)$ = level set function
- $F$ = local velocity (etch or deposition rate)
- Surface defined by $\phi = 0$



**10. Lithography**

**10.1 Resolution Limits**

**Rayleigh criterion**:

$$R = k_1 \frac{\lambda}{NA}$$

**Depth of focus**:

$$DOF = k_2 \frac{\lambda}{NA^2}$$

Where:
- $\lambda$ = wavelength (193 nm DUV, 13.5 nm EUV)
- $NA$ = numerical aperture
- $k_1$, $k_2$ = process-dependent factors

| Technology | λ (nm) | NA | Minimum k₁ | Resolution (nm) |
|------------|--------|-----|------------|-----------------|
| DUV (ArF) | 193 | 1.35 | 0.25 | ~36 |
| EUV | 13.5 | 0.33 | 0.25 | ~10 |
| High-NA EUV | 13.5 | 0.55 | 0.25 | ~6 |

**10.2 Aerial Image Formation**

**Coherent illumination**:

$$I(x,y) = \left| \mathcal{F}^{-1} \left\{ \tilde{M}(f_x, f_y) \cdot H(f_x, f_y) \right\} \right|^2$$

Where:
- $\tilde{M}$ = Fourier transform of mask transmission
- $H$ = optical transfer function (pupil function)

**Partially coherent illumination** (Hopkins formulation):

$$I(x,y) = \iint \iint TCC(f_1, g_1, f_2, g_2) \cdot \tilde{M}(f_1, g_1) \cdot \tilde{M}^*(f_2, g_2) \cdot e^{2\pi i [(f_1 - f_2)x + (g_1 - g_2)y]} \, df_1 \, dg_1 \, df_2 \, dg_2$$

Where $TCC$ = transmission cross coefficient

**10.3 Photoresist Chemistry**

**Chemically Amplified Resists (CARs)**:

**Photoacid generation**:

$$\frac{\partial [\text{PAG}]}{\partial t} = -C \cdot I \cdot [\text{PAG}]$$

**Acid diffusion and reaction**:

$$\frac{\partial [H^+]}{\partial t} = D_H 
abla^2 [H^+] + k_{\text{gen}} - k_{\text{neut}}[H^+][Q]$$

**Deprotection kinetics**:

$$\frac{\partial [M]}{\partial t} = -k_{\text{amp}} [H^+] [M]$$

Where:
- $[\text{PAG}]$ = photoacid generator concentration
- $[H^+]$ = acid concentration
- $[Q]$ = quencher concentration
- $[M]$ = protected site concentration

**10.4 Stochastic Effects in EUV**

**Photon shot noise**:

$$\sigma_N = \sqrt{N}$$

**Line Edge Roughness (LER)**:

$$\sigma_{\text{LER}} \propto \frac{1}{\sqrt{\text{dose}}} \propto \frac{1}{\sqrt{N_{\text{photons}}}}$$

**Stochastic defect probability**:

$$P_{\text{defect}} = 1 - \exp(-\lambda A)$$

Where $\lambda$ = defect density, $A$ = feature area



**11. Chemical Mechanical Polishing (CMP)**

**11.1 Preston Equation**

$$\frac{dh}{dt} = K_p \cdot P \cdot v$$

Where:
- $dh/dt$ = material removal rate [nm/s]
- $K_p$ = Preston coefficient [nm/(Pa·m)]
- $P$ = applied pressure [Pa]
- $v$ = relative velocity [m/s]

**11.2 Contact Mechanics**

**Greenwood-Williamson model** for asperity contact:

$$A_{\text{real}} = \pi n \beta \sigma \int_{d}^{\infty} (z - d) \phi(z) \, dz$$

$$F = \frac{4}{3} n E^* \sqrt{\beta} \int_{d}^{\infty} (z - d)^{3/2} \phi(z) \, dz$$

Where:
- $n$ = asperity density
- $\beta$ = asperity radius
- $\sigma$ = RMS roughness
- $\phi(z)$ = height distribution
- $E^*$ = effective elastic modulus

**11.3 Pattern-Dependent Effects**

**Dishing** (in metal features):

$$\Delta h_{\text{dish}} \propto w^2$$

Where $w$ = line width

**Erosion** (in dielectric):

$$\Delta h_{\text{erosion}} \propto \rho_{\text{metal}}$$

Where $\rho_{\text{metal}}$ = local metal pattern density


**12. Device Simulation (TCAD)**

**12.1 Poisson Equation**

$$
abla \cdot (\epsilon 
abla \psi) = -q(p - n + N_D^+ - N_A^-)$$

Where:
- $\psi$ = electrostatic potential [V]
- $\epsilon$ = permittivity
- $n$, $p$ = electron and hole concentrations
- $N_D^+$, $N_A^-$ = ionized donor and acceptor concentrations

**12.2 Drift-Diffusion Equations**

**Current densities**:

$$\mathbf{J}_n = q \mu_n n \mathbf{E} + q D_n 
abla n$$

$$\mathbf{J}_p = q \mu_p p \mathbf{E} - q D_p 
abla p$$

**Einstein relation**:

$$D_n = \frac{k_B T}{q} \mu_n, \quad D_p = \frac{k_B T}{q} \mu_p$$

**Continuity equations**:

$$\frac{\partial n}{\partial t} = \frac{1}{q} 
abla \cdot \mathbf{J}_n + G - R$$

$$\frac{\partial p}{\partial t} = -\frac{1}{q} 
abla \cdot \mathbf{J}_p + G - R$$

**12.3 Carrier Statistics**

**Boltzmann approximation**:

$$n = N_c \exp\left(\frac{E_F - E_c}{k_B T}\right)$$

$$p = N_v \exp\left(\frac{E_v - E_F}{k_B T}\right)$$

**Fermi-Dirac (degenerate regime)**:

$$n = N_c \mathcal{F}_{1/2}\left(\frac{E_F - E_c}{k_B T}\right)$$

Where $\mathcal{F}_{1/2}$ = Fermi-Dirac integral of order 1/2

**12.4 Recombination Models**

**Shockley-Read-Hall (SRH)**:

$$R_{\text{SRH}} = \frac{pn - n_i^2}{\tau_p(n + n_1) + \tau_n(p + p_1)}$$

**Auger recombination**:

$$R_{\text{Auger}} = (C_n n + C_p p)(pn - n_i^2)$$

**Radiative recombination**:

$$R_{\text{rad}} = B(pn - n_i^2)$$



**13. Advanced Mathematical Methods**

**13.1 Level Set Methods**

**Evolution equation**:

$$\frac{\partial \phi}{\partial t} + F |
abla \phi| = 0$$

**Reinitialization** (maintain signed distance function):

$$\frac{\partial \phi}{\partial \tau} = \text{sign}(\phi_0)(1 - |
abla \phi|)$$

**Curvature**:

$$\kappa = 
abla \cdot \left( \frac{
abla \phi}{|
abla \phi|} \right)$$

**13.2 Kinetic Monte Carlo (KMC)**

**Rate catalog**:

$$r_i = 
u_0 \exp\left(-\frac{E_i}{k_B T}\right)$$

**Event selection** (Bortz-Kalos-Lebowitz algorithm):

1. Calculate total rate: $R_{\text{tot}} = \sum_i r_i$
2. Generate random $u \in (0,1)$
3. Select event $j$ where $\sum_{i=1}^{j-1} r_i < u \cdot R_{\text{tot}} \leq \sum_{i=1}^{j} r_i$

**Time advancement**:

$$\Delta t = -\frac{\ln(u')}{R_{\text{tot}}}$$

**13.3 Phase Field Methods**

**Free energy functional**:

$$F[\phi] = \int \left[ f(\phi) + \frac{\epsilon^2}{2} |
abla \phi|^2 \right] dV$$

**Allen-Cahn equation** (non-conserved order parameter):

$$\frac{\partial \phi}{\partial t} = -M \frac{\delta F}{\delta \phi} = M \left[ \epsilon^2 
abla^2 \phi - f'(\phi) \right]$$

**Cahn-Hilliard equation** (conserved order parameter):

$$\frac{\partial \phi}{\partial t} = 
abla \cdot \left( M 
abla \frac{\delta F}{\delta \phi} \right)$$

**13.4 Density Functional Theory (DFT)**

**Kohn-Sham equations**:

$$\left[ -\frac{\hbar^2}{2m} 
abla^2 + V_{\text{eff}}(\mathbf{r}) \right] \psi_i(\mathbf{r}) = \epsilon_i \psi_i(\mathbf{r})$$

**Effective potential**:

$$V_{\text{eff}}(\mathbf{r}) = V_{\text{ext}}(\mathbf{r}) + V_H(\mathbf{r}) + V_{xc}(\mathbf{r})$$

Where:
- $V_{\text{ext}}$ = external (ionic) potential
- $V_H = e^2 \int \frac{n(\mathbf{r}')}{|\mathbf{r} - \mathbf{r}'|} d\mathbf{r}'$ = Hartree potential
- $V_{xc} = \frac{\delta E_{xc}[n]}{\delta n}$ = exchange-correlation potential

**Electron density**:

$$n(\mathbf{r}) = \sum_i f_i |\psi_i(\mathbf{r})|^2$$



**14. Current Frontiers**

**14.1 Extreme Ultraviolet (EUV) Lithography**

- **Challenges**:
  - Stochastic effects at low photon counts
  - Mask defectivity and pellicle development
  - Resist trade-offs (sensitivity vs. resolution vs. LER)
  - Source power and productivity

- **High-NA EUV**:
  - NA = 0.55 (vs. 0.33 current)
  - Anamorphic optics (4× magnification in one direction)
  - Sub-8nm half-pitch capability

**14.2 3D Integration**

- **Through-Silicon Vias (TSVs)**:
  - Via-first, via-middle, via-last approaches
  - Cu filling and barrier requirements
  - Thermal-mechanical stress modeling

- **Hybrid Bonding**:
  - Cu-Cu direct bonding
  - Sub-micron alignment requirements
  - Surface preparation and activation

**14.3 New Materials**

- **2D Materials**:
  - Graphene (zero bandgap)
  - Transition metal dichalcogenides (MoS₂, WS₂, WSe₂)
  - Hexagonal boron nitride (hBN)

- **Wide Bandgap Semiconductors**:
  - GaN: $E_g = 3.4$ eV
  - SiC: $E_g = 3.3$ eV (4H-SiC)
  - Ga₂O₃: $E_g = 4.8$ eV

**14.4 Novel Device Architectures**

- **Gate-All-Around (GAA) FETs**:
  - Nanosheet and nanowire channels
  - Superior electrostatic control
  - Samsung 3nm, Intel 20A/18A

- **Complementary FET (CFET)**:
  - Vertically stacked NMOS/PMOS
  - Reduced footprint
  - Complex fabrication

- **Backside Power Delivery (BSPD)**:
  - Power rails on wafer backside
  - Reduced IR drop
  - Intel PowerVia

**14.5 Machine Learning in Semiconductor Manufacturing**

- **Virtual Metrology**: Predict wafer properties from tool sensor data
- **Defect Detection**: CNN-based wafer map classification
- **Process Optimization**: Bayesian optimization, reinforcement learning
- **Surrogate Models**: Neural networks replacing expensive simulations
- **OPC (Optical Proximity Correction)**: ML-accelerated mask design



**Physical Constants**

| Constant | Symbol | Value |
|----------|--------|-------|
| Boltzmann constant | $k_B$ | $1.381 \times 10^{-23}$ J/K |
| Elementary charge | $e$ | $1.602 \times 10^{-19}$ C |
| Planck constant | $h$ | $6.626 \times 10^{-34}$ J·s |
| Electron mass | $m_e$ | $9.109 \times 10^{-31}$ kg |
| Permittivity of free space | $\epsilon_0$ | $8.854 \times 10^{-12}$ F/m |
| Avogadro's number | $N_A$ | $6.022 \times 10^{23}$ mol⁻¹ |
| Thermal voltage (300K) | $k_B T/q$ | 25.85 mV |



**Multiscale Modeling Hierarchy**

| Level | Method | Length Scale | Time Scale | Application |
|-------|--------|--------------|------------|-------------|
| 1 | Ab initio (DFT) | Å | fs | Reaction mechanisms, band structure |
| 2 | Molecular Dynamics | nm | ps-ns | Defect dynamics, interfaces |
| 3 | Kinetic Monte Carlo | nm-μm | ns-s | Growth, etching, diffusion |
| 4 | Continuum (PDE) | μm-mm | s-hr | Process simulation (TCAD) |
| 5 | Compact Models | Device | — | Circuit simulation |
| 6 | Statistical | Die/Wafer | — | Yield prediction |

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/material-science-mathematics) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
