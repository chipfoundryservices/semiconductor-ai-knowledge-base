# Epitaxy (Epi) Modeling:

**Keywords**: epitaxy,epi,epitaxial,epitaxial growth,homoepitaxy,heteroepitaxy,MBE,molecular beam epitaxy,MOCVD,metal organic cvd,SiGe,silicon germanium,strain engineering,selective epitaxial growth,SEG,lattice mismatch,critical thickness

---

**Epitaxy (Epi) Modeling:**



  1. Introduction to Epitaxy

Epitaxy is the controlled growth of a crystalline thin film on a crystalline substrate, where the deposited layer inherits the crystallographic orientation of the substrate.

  1.1 Types of Epitaxy

• Homoepitaxy 
  • Same material deposited on substrate
  • Example: Silicon (Si) on Silicon (Si)
  • Maintains perfect lattice matching
  • Used for creating high-purity device layers

• Heteroepitaxy 
  • Different material deposited on substrate
  • Examples:
    • Gallium Arsenide (GaAs) on Silicon (Si)
    • Silicon Germanium (SiGe) on Silicon (Si)
    • Gallium Nitride (GaN) on Sapphire ($\text{Al}_2\text{O}_3$)
  • Introduces lattice mismatch and strain
  • Enables bandgap engineering



  2. Epitaxy Methods

  2.1 Chemical Vapor Deposition (CVD) / Vapor Phase Epitaxy (VPE)

• Characteristics: 
  • Most common method for silicon epitaxy
  • Operates at atmospheric or reduced pressure
  • Temperature range: $900°\text{C} - 1200°\text{C}$

• Common Precursors: 
  • Silane: $\text{SiH}_4$
  • Dichlorosilane: $\text{SiH}_2\text{Cl}_2$ (DCS)
  • Trichlorosilane: $\text{SiHCl}_3$ (TCS)
  • Silicon tetrachloride: $\text{SiCl}_4$

• Key Reactions: 

$$\text{SiH}_4 \xrightarrow{\Delta} \text{Si}_{(s)} + 2\text{H}_2$$

$$\text{SiH}_2\text{Cl}_2 \xrightarrow{\Delta} \text{Si}_{(s)} + 2\text{HCl}$$

  2.2 Molecular Beam Epitaxy (MBE)

• Characteristics: 
  • Ultra-high vacuum environment ($< 10^{-10}$ Torr)
  • Extremely precise thickness control (monolayer accuracy)
  • Lower growth temperatures than CVD
  • Slower growth rates: $\sim 1 \, \mu\text{m/hour}$

• Applications: 
  • III-V compound semiconductors
  • Quantum well structures
  • Superlattices
  • Research and development

  2.3 Metal-Organic CVD (MOCVD)

• Characteristics: 
  • Standard for compound semiconductors
  • Uses metal-organic precursors
  • Higher throughput than MBE

• Common Precursors: 
  • Trimethylgallium: $\text{Ga(CH}_3\text{)}_3$ (TMGa)
  • Trimethylaluminum: $\text{Al(CH}_3\text{)}_3$ (TMAl)
  • Ammonia: $\text{NH}_3$

  2.4 Atomic Layer Epitaxy (ALE)

• Characteristics: 
  • Self-limiting surface reactions
  • Digital control of film thickness
  • Excellent conformality
  • Growth rate: $\sim 1$ Å per cycle



  3. Physics of Epi Modeling

  3.1 Gas-Phase Transport

The transport of precursor gases to the substrate surface involves multiple phenomena:

• Governing Equations: 

  •  Continuity Equation: 
  
  $$\frac{\partial \rho}{\partial t} + 
abla \cdot (\rho \mathbf{v}) = 0$$

  •  Navier-Stokes Equation: 
  
  $$\rho \left( \frac{\partial \mathbf{v}}{\partial t} + \mathbf{v} \cdot 
abla \mathbf{v} \right) = -
abla p + \mu 
abla^2 \mathbf{v} + \rho \mathbf{g}$$

  •  Species Transport Equation: 
  
  $$\frac{\partial C_i}{\partial t} + \mathbf{v} \cdot 
abla C_i = D_i 
abla^2 C_i + R_i$$

  Where:
  • $\rho$ = fluid density
  • $\mathbf{v}$ = velocity vector
  • $p$ = pressure
  • $\mu$ = dynamic viscosity
  • $C_i$ = concentration of species $i$
  • $D_i$ = diffusion coefficient of species $i$
  • $R_i$ = reaction rate term

• Boundary Layer: 
  • Stagnant gas layer above substrate
  • Thickness $\delta$ depends on flow conditions:
  
  $$\delta \propto \sqrt{\frac{
u x}{u_\infty}}$$
  
  Where:
  • $
u$ = kinematic viscosity
  • $x$ = distance from leading edge
  • $u_\infty$ = free stream velocity

  3.2 Surface Kinetics

• Adsorption Process: 
  • Physisorption (weak van der Waals forces)
  • Chemisorption (chemical bonding)
  
• Langmuir Adsorption Isotherm: 

$$\theta = \frac{K \cdot P}{1 + K \cdot P}$$

Where:
- $\theta$ = fractional surface coverage
- $K$ = equilibrium constant
- $P$ = partial pressure

• Surface Diffusion: 

$$D_s = D_0 \exp\left(-\frac{E_d}{k_B T}\right)$$

Where:
- $D_s$ = surface diffusion coefficient
- $D_0$ = pre-exponential factor
- $E_d$ = diffusion activation energy
- $k_B$ = Boltzmann constant ($1.38 \times 10^{-23}$ J/K)
- $T$ = absolute temperature

  3.3 Crystal Growth Mechanisms

• Step-Flow Growth (BCF Theory): 
  • Atoms attach at step edges
  • Steps advance across terraces
  • Dominant at high temperatures

• 2D Nucleation: 
  • New layers nucleate on terraces
  • Occurs when step density is low
  • Creates rougher surfaces

• Terrace-Ledge-Kink (TLK) Model: 
  • Terrace: flat regions between steps
  • Ledge: step edges
  • Kink: incorporation sites at step edges



  4. Mathematical Framework

  4.1 Growth Rate Models

 4.1.1 Reaction-Limited Regime

At lower temperatures, surface reaction kinetics dominate:

$$G = k_s \cdot C_s$$

Where the rate constant follows Arrhenius behavior:

$$k_s = k_0 \exp\left(-\frac{E_a}{k_B T}\right)$$

 Parameters: 
- $G$ = growth rate (nm/min or μm/hr)
- $k_s$ = surface reaction rate constant
- $C_s$ = surface concentration
- $k_0$ = pre-exponential factor
- $E_a$ = activation energy

 4.1.2 Mass-Transport Limited Regime

At higher temperatures, diffusion through the boundary layer limits growth:

$$G = \frac{h_g}{N_s} \cdot (C_g - C_s)$$

Where:

$$h_g = \frac{D}{\delta}$$

 Parameters: 
- $h_g$ = mass transfer coefficient
- $N_s$ = atomic density of solid ($\sim 5 \times 10^{22}$ atoms/cm³ for Si)
- $C_g$ = gas phase concentration
- $D$ = gas phase diffusivity
- $\delta$ = boundary layer thickness

 4.1.3 Combined Model (Grove Model)

For the general case combining both regimes:

$$G = \frac{h_g \cdot k_s}{N_s (h_g + k_s)} \cdot C_g$$

Or equivalently:

$$\frac{1}{G} = \frac{N_s}{k_s \cdot C_g} + \frac{N_s}{h_g \cdot C_g}$$

  4.2 Strain in Heteroepitaxy

 4.2.1 Lattice Mismatch

$$f = \frac{a_s - a_f}{a_f}$$

Where:
- $f$ = lattice mismatch (dimensionless)
- $a_s$ = substrate lattice constant
- $a_f$ = film lattice constant (relaxed)

 Example Values: 

| System | $a_f$ (Å) | $a_s$ (Å) | Mismatch $f$ |
|--------|-----------|-----------|--------------|
| Si on Si | 5.431 | 5.431 | 0% |
| Ge on Si | 5.658 | 5.431 | -4.2% |
| GaAs on Si | 5.653 | 5.431 | -4.1% |
| InAs on GaAs | 6.058 | 5.653 | -7.2% |

 4.2.2 In-Plane Strain

For a coherently strained film:

$$\epsilon_{\parallel} = \frac{a_s - a_f}{a_f} = f$$

The out-of-plane strain (for cubic materials):

$$\epsilon_{\perp} = -\frac{2
u}{1-
u} \epsilon_{\parallel}$$

Where $
u$ = Poisson's ratio

 4.2.3 Critical Thickness (Matthews-Blakeslee)

The critical thickness above which misfit dislocations form:

$$h_c = \frac{b}{8\pi f (1+
u)} \left[ \ln\left(\frac{h_c}{b}\right) + 1 \right]$$

Where:
- $h_c$ = critical thickness
- $b$ = Burgers vector magnitude ($\approx \frac{a}{\sqrt{2}}$ for 60° dislocations)
- $f$ = lattice mismatch
- $
u$ = Poisson's ratio

 Approximate Solution: 

For small mismatch:

$$h_c \approx \frac{b}{8\pi |f|}$$

  4.3 Dopant Incorporation

 4.3.1 Segregation Model

$$C_{film} = \frac{C_{gas}}{1 + k_{seg} \cdot (G/G_0)}$$

Where:
- $C_{film}$ = dopant concentration in film
- $C_{gas}$ = dopant concentration in gas phase
- $k_{seg}$ = segregation coefficient
- $G$ = growth rate
- $G_0$ = reference growth rate

 4.3.2 Dopant Profile with Segregation

The surface concentration evolves as:

$$C_s(t) = C_s^{eq} + (C_s(0) - C_s^{eq}) \exp\left(-\frac{G \cdot t}{\lambda}\right)$$

Where:
- $\lambda$ = segregation length
- $C_s^{eq}$ = equilibrium surface concentration



  5. Modeling Approaches

  5.1 Continuum Models

• Scope: 
  • Reactor-scale simulations
  • Temperature and flow field prediction
  • Species concentration profiles

• Methods: 
  • Computational Fluid Dynamics (CFD)
  • Finite Element Method (FEM)
  • Finite Volume Method (FVM)

• Governing Physics: 
  • Coupled heat, mass, and momentum transfer
  • Homogeneous and heterogeneous reactions
  • Radiation heat transfer

  5.2 Feature-Scale Models

• Applications: 
  • Selective epitaxial growth (SEG)
  • Trench filling
  • Facet evolution

• Key Phenomena: 
  • Local loading effects:
  
  $$G_{local} = G_0 \cdot \left(1 - \alpha \cdot \frac{A_{exposed}}{A_{total}}\right)$$
  
  • Orientation-dependent growth rates:
  
  $$\frac{G_{(110)}}{G_{(100)}} \approx 1.5 - 2.0$$

• Methods: 
  • Level set methods
  • String methods
  • Cellular automata

  5.3 Atomistic Models

 5.3.1 Kinetic Monte Carlo (KMC)

• Process Events: 
  • Adsorption: rate $\propto P \cdot \exp(-E_{ads}/k_BT)$
  • Surface diffusion: rate $\propto \exp(-E_{diff}/k_BT)$
  • Desorption: rate $\propto \exp(-E_{des}/k_BT)$
  • Incorporation: rate $\propto \exp(-E_{inc}/k_BT)$

• Master Equation: 

$$\frac{dP_i}{dt} = \sum_j \left( W_{ji} P_j - W_{ij} P_i \right)$$

Where:
- $P_i$ = probability of state $i$
- $W_{ij}$ = transition rate from state $i$ to $j$

 5.3.2 Molecular Dynamics (MD)

• Newton's Equations: 

$$m_i \frac{d^2 \mathbf{r}_i}{dt^2} = -
abla_i U(\mathbf{r}_1, \mathbf{r}_2, ..., \mathbf{r}_N)$$

• Interatomic Potentials: 
  • Tersoff potential (Si, C, Ge)
  • Stillinger-Weber potential (Si)
  • MEAM (metals and alloys)

 5.3.3 Ab Initio / DFT

• Kohn-Sham Equations: 

$$\left[ -\frac{\hbar^2}{2m} 
abla^2 + V_{eff}(\mathbf{r}) \right] \psi_i(\mathbf{r}) = \epsilon_i \psi_i(\mathbf{r})$$

• Applications: 
  • Surface energies
  • Reaction barriers
  • Adsorption energies
  • Electronic structure



  6. Specific Modeling Challenges

  6.1 SiGe Epitaxy

• Composition Control: 

$$x_{Ge} = \frac{R_{Ge}}{R_{Si} + R_{Ge}}$$

Where $R_{Si}$ and $R_{Ge}$ are partial growth rates

• Strain Engineering: 
  • Compressive strain in SiGe on Si
  • Enhances hole mobility
  • Critical thickness depends on Ge content:
  
  $$h_c(x) \approx \frac{0.5}{0.042 \cdot x} \text{ nm}$$

  6.2 Selective Epitaxy

• Growth Selectivity: 
  • Deposition only on exposed silicon
  • HCl addition for selectivity enhancement
  
• Selectivity Condition: 

$$\frac{\text{Growth on Si}}{\text{Growth on SiO}_2} > 100:1$$

• Loading Effects: 
  • Pattern-dependent growth rate
  • Faceting at mask edges

  6.3 III-V on Silicon

• Major Challenges: 
  • Large lattice mismatch (4-8%)
  • Thermal expansion mismatch
  • Anti-phase domain boundaries (APDs)
  • High threading dislocation density

• Mitigation Strategies: 
  • Aspect ratio trapping (ART)
  • Graded buffer layers
  • Selective area growth
  • Dislocation filtering



  7. Applications and Tools

  7.1 Industrial Applications

| Application | Material System | Key Parameters |
|-------------|-----------------|----------------|
| FinFET/GAA Source/Drain | Embedded SiGe, SiC | Strain, selectivity |
| SiGe HBT | SiGe:C | Profile abruptness |
| Power MOSFETs | SiC epitaxy | Defect density |
| LEDs/Lasers | GaN, InGaN | Composition uniformity |
| RF Devices | GaN on SiC | Buffer quality |

  7.2 Simulation Software

• Reactor-Scale CFD: 
  • ANSYS Fluent
  • COMSOL Multiphysics
  • OpenFOAM

• TCAD Process Simulation: 
  • Synopsys Sentaurus Process
  • Silvaco Victory Process
  • Lumerical (for optoelectronics)

• Atomistic Simulation: 
  • LAMMPS (MD)
  • VASP, Quantum ESPRESSO (DFT)
  • Custom KMC codes

  7.3 Key Metrics for Process Development

• Uniformity: 

$$\text{Uniformity} = \frac{t_{max} - t_{min}}{2 \cdot t_{avg}} \times 100\%$$

• Defect Density: 
  • Threading dislocations: target $< 10^6$ cm$^{-2}$
  • Stacking faults: target $< 10^3$ cm$^{-2}$

• Profile Abruptness: 
  • Dopant transition width $< 3$ nm/decade



  8. Emerging Directions

  8.1 Machine Learning Integration

• Applications: 
  • Surrogate models for process optimization
  • Real-time virtual metrology
  • Defect classification
  • Recipe optimization

• Model Types: 
  • Neural networks for growth rate prediction
  • Gaussian process regression for uncertainty quantification
  • Reinforcement learning for process control

  8.2 Multi-Scale Modeling

• Hierarchical Approach: 

```text
┌─────────────────────────────────────────────┐
│  Ab Initio (DFT)                            │
│      ↓ Reaction rates, energies             │
├─────────────────────────────────────────────┤
│  Kinetic Monte Carlo                        │
│      ↓ Surface kinetics, morphology         │
├─────────────────────────────────────────────┤
│  Feature-Scale Models                       │
│      ↓ Local growth behavior                │
├─────────────────────────────────────────────┤
│  Reactor-Scale CFD                          │
│      ↓ Process conditions                   │
├─────────────────────────────────────────────┤
│  Device Simulation                          │
└─────────────────────────────────────────────┘
```

• Applications: 
  • Surface energies
  • Reaction barriers
  • Adsorption energies
  • Electronic structure

  8.3 Digital Twins

• Components: 
  • Real-time sensor data integration
  • Physics-based + ML hybrid models
  • Predictive maintenance
  • Closed-loop process control

  8.4 New Material Systems

• 2D Materials: 
  • Graphene via CVD
  • Transition metal dichalcogenides (TMDs)
  • Van der Waals epitaxy

• Ultra-Wide Bandgap: 
  • $\beta$-Ga$_2$O$_3$ ($E_g \approx 4.8$ eV)
  • Diamond ($E_g \approx 5.5$ eV)
  • AlN ($E_g \approx 6.2$ eV)





Constants and Conversions

| Constant | Symbol | Value |
|----------|--------|-------|
| Boltzmann constant | $k_B$ | $1.381 \times 10^{-23}$ J/K |
| Planck constant | $h$ | $6.626 \times 10^{-34}$ J·s |
| Avogadro number | $N_A$ | $6.022 \times 10^{23}$ mol$^{-1}$ |
| Si atomic density | $N_{Si}$ | $5.0 \times 10^{22}$ atoms/cm³ |
| Si lattice constant | $a_{Si}$ | 5.431 Å |

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/epitaxy) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
