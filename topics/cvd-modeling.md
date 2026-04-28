# CVD Modeling in Semiconductor Manufacturing

**Keywords**: cvd modeling, chemical vapor deposition, cvd process, lpcvd, pecvd, hdp-cvd, mocvd, ald, thin film deposition, cvd equipment, cvd simulation

---

**CVD Modeling in Semiconductor Manufacturing**



**1. Introduction**

Chemical Vapor Deposition (CVD) is a critical thin-film deposition technique in semiconductor manufacturing. Gaseous precursors are introduced into a reaction chamber where they undergo chemical reactions to deposit solid films on heated substrates.

**1.1 Key Process Steps**

- **Transport** of reactants from bulk gas to the substrate surface
- **Gas-phase chemistry** including precursor decomposition and intermediate formation
- **Surface reactions** involving adsorption, surface diffusion, and reaction
- **Film nucleation and growth** with specific microstructure evolution
- **Byproduct desorption** and transport away from the surface

**1.2 Common CVD Types**

- **APCVD** — Atmospheric Pressure CVD
- **LPCVD** — Low Pressure CVD (0.1–10 Torr)
- **PECVD** — Plasma Enhanced CVD
- **MOCVD** — Metal-Organic CVD
- **ALD** — Atomic Layer Deposition
- **HDPCVD** — High Density Plasma CVD



**2. Governing Equations**

**2.1 Continuity Equation (Mass Conservation)**

$$
\frac{\partial \rho}{\partial t} + 
abla \cdot (\rho \mathbf{u}) = 0
$$

Where:

- $\rho$ — gas density $\left[\text{kg/m}^3\right]$
- $\mathbf{u}$ — velocity vector $\left[\text{m/s}\right]$
- $t$ — time $\left[\text{s}\right]$

**2.2 Momentum Equation (Navier-Stokes)**

$$
\rho \left( \frac{\partial \mathbf{u}}{\partial t} + \mathbf{u} \cdot 
abla \mathbf{u} \right) = -
abla p + \mu 
abla^2 \mathbf{u} + \rho \mathbf{g}
$$

Where:

- $p$ — pressure $\left[\text{Pa}\right]$
- $\mu$ — dynamic viscosity $\left[\text{Pa} \cdot \text{s}\right]$
- $\mathbf{g}$ — gravitational acceleration $\left[\text{m/s}^2\right]$

**2.3 Species Conservation Equation**

$$
\frac{\partial (\rho Y_i)}{\partial t} + 
abla \cdot (\rho \mathbf{u} Y_i) = 
abla \cdot (\rho D_i 
abla Y_i) + R_i
$$

Where:

- $Y_i$ — mass fraction of species $i$ $\left[\text{dimensionless}\right]$
- $D_i$ — diffusion coefficient of species $i$ $\left[\text{m}^2/\text{s}\right]$
- $R_i$ — net production rate from reactions $\left[\text{kg/m}^3 \cdot \text{s}\right]$

**2.4 Energy Conservation Equation**

$$
\rho c_p \left( \frac{\partial T}{\partial t} + \mathbf{u} \cdot 
abla T \right) = 
abla \cdot (k 
abla T) + Q
$$

Where:

- $c_p$ — specific heat capacity $\left[\text{J/kg} \cdot \text{K}\right]$
- $T$ — temperature $\left[\text{K}\right]$
- $k$ — thermal conductivity $\left[\text{W/m} \cdot \text{K}\right]$
- $Q$ — volumetric heat source $\left[\text{W/m}^3\right]$

**2.5 Key Dimensionless Numbers**

| Number | Definition | Physical Meaning |
|--------|------------|------------------|
| Reynolds | $Re = \frac{\rho u L}{\mu}$ | Inertial vs. viscous forces |
| Péclet | $Pe = \frac{u L}{D}$ | Convection vs. diffusion |
| Damköhler | $Da = \frac{k_s L}{D}$ | Reaction rate vs. transport rate |
| Knudsen | $Kn = \frac{\lambda}{L}$ | Mean free path vs. length scale |

Where:

- $L$ — characteristic length $\left[\text{m}\right]$
- $\lambda$ — mean free path $\left[\text{m}\right]$
- $k_s$ — surface reaction rate constant $\left[\text{m/s}\right]$



**3. Chemical Kinetics**

**3.1 Arrhenius Equation**

The temperature dependence of reaction rate constants follows:

$$
k = A \exp\left(-\frac{E_a}{R T}\right)
$$

Where:

- $k$ — rate constant $\left[\text{varies}\right]$
- $A$ — pre-exponential factor $\left[\text{same as } k\right]$
- $E_a$ — activation energy $\left[\text{J/mol}\right]$
- $R$ — universal gas constant $= 8.314 \, \text{J/mol} \cdot \text{K}$

**3.2 Gas-Phase Reactions**

**Example: Silane Pyrolysis**

$$
\text{SiH}_4 \xrightarrow{k_1} \text{SiH}_2 + \text{H}_2
$$

$$
\text{SiH}_2 + \text{SiH}_4 \xrightarrow{k_2} \text{Si}_2\text{H}_6
$$

**General reaction rate expression:**

$$
r_j = k_j \prod_{i} C_i^{
u_{ij}}
$$

Where:

- $r_j$ — rate of reaction $j$ $\left[\text{mol/m}^3 \cdot \text{s}\right]$
- $C_i$ — concentration of species $i$ $\left[\text{mol/m}^3\right]$
- $
u_{ij}$ — stoichiometric coefficient of species $i$ in reaction $j$

**3.3 Surface Reaction Kinetics**

**3.3.1 Hertz-Knudsen Impingement Flux**

$$
J = \frac{p}{\sqrt{2 \pi m k_B T}}
$$

Where:

- $J$ — molecular flux $\left[\text{molecules/m}^2 \cdot \text{s}\right]$
- $p$ — partial pressure $\left[\text{Pa}\right]$
- $m$ — molecular mass $\left[\text{kg}\right]$
- $k_B$ — Boltzmann constant $= 1.381 \times 10^{-23} \, \text{J/K}$

**3.3.2 Surface Reaction Rate**

$$
R_s = s \cdot J = s \cdot \frac{p}{\sqrt{2 \pi m k_B T}}
$$

Where:

- $s$ — sticking coefficient $\left[0 \leq s \leq 1\right]$

**3.3.3 Langmuir-Hinshelwood Kinetics**

For surface reaction between two adsorbed species:

$$
r = \frac{k \, K_A \, K_B \, p_A \, p_B}{(1 + K_A p_A + K_B p_B)^2}
$$

Where:

- $K_A, K_B$ — adsorption equilibrium constants $\left[\text{Pa}^{-1}\right]$
- $p_A, p_B$ — partial pressures of reactants A and B $\left[\text{Pa}\right]$

**3.3.4 Eley-Rideal Mechanism**

For reaction between adsorbed species and gas-phase species:

$$
r = \frac{k \, K_A \, p_A \, p_B}{1 + K_A p_A}
$$

**3.4 Common CVD Reaction Systems**

- **Silicon from Silane:**
  - $\text{SiH}_4 \rightarrow \text{Si}_{(s)} + 2\text{H}_2$

- **Silicon Dioxide from TEOS:**
  - $\text{Si(OC}_2\text{H}_5\text{)}_4 + 12\text{O}_2 \rightarrow \text{SiO}_2 + 8\text{CO}_2 + 10\text{H}_2\text{O}$

- **Silicon Nitride from DCS:**
  - $3\text{SiH}_2\text{Cl}_2 + 4\text{NH}_3 \rightarrow \text{Si}_3\text{N}_4 + 6\text{HCl} + 6\text{H}_2$

- **Tungsten from WF₆:**
  - $\text{WF}_6 + 3\text{H}_2 \rightarrow \text{W}_{(s)} + 6\text{HF}$



**4. Process Regimes**

**4.1 Transport-Limited Regime**

**Characteristics:**

- High Damköhler number: $Da \gg 1$
- Surface reactions are fast
- Deposition rate controlled by mass transport
- Sensitive to:
  - Flow patterns
  - Temperature gradients
  - Reactor geometry

**Deposition rate expression:**

$$
R_{dep} \approx \frac{D \cdot C_{\infty}}{\delta}
$$

Where:

- $C_{\infty}$ — bulk gas concentration $\left[\text{mol/m}^3\right]$
- $\delta$ — boundary layer thickness $\left[\text{m}\right]$

**4.2 Reaction-Limited Regime**

**Characteristics:**

- Low Damköhler number: $Da \ll 1$
- Plenty of reactants at surface
- Rate controlled by surface kinetics
- Strong Arrhenius temperature dependence
- Better step coverage in features

**Deposition rate expression:**

$$
R_{dep} \approx k_s \cdot C_s \approx k_s \cdot C_{\infty}
$$

Where:

- $k_s$ — surface reaction rate constant $\left[\text{m/s}\right]$
- $C_s$ — surface concentration $\approx C_{\infty}$ $\left[\text{mol/m}^3\right]$

**4.3 Regime Transition**

The transition occurs when:

$$
Da = \frac{k_s \delta}{D} \approx 1
$$

**Practical implications:**

- **Transport-limited:** Optimize flow, temperature uniformity
- **Reaction-limited:** Optimize temperature, precursor chemistry
- **Mixed regime:** Most complex to control and model



**5. Multiscale Modeling**

**5.1 Scale Hierarchy**

| Scale | Length | Time | Methods |
|-------|--------|------|---------|
| Reactor | cm – m | s – min | CFD, FEM |
| Feature | nm – μm | ms – s | Level set, Monte Carlo |
| Surface | nm | μs – ms | KMC |
| Atomistic | Å | fs – ps | MD, DFT |

**5.2 Reactor-Scale Modeling**

**Governing physics:**

- Coupled Navier-Stokes + species + energy equations
- Multicomponent diffusion (Stefan-Maxwell)
- Chemical source terms

**Stefan-Maxwell diffusion:**

$$

abla x_i = \sum_{j 
eq i} \frac{x_i x_j}{D_{ij}} (\mathbf{u}_j - \mathbf{u}_i)
$$

Where:

- $x_i$ — mole fraction of species $i$
- $D_{ij}$ — binary diffusion coefficient $\left[\text{m}^2/\text{s}\right]$

**Common software:**

- ANSYS Fluent
- COMSOL Multiphysics
- OpenFOAM (open-source)
- Silvaco Victory Process
- Synopsys Sentaurus

**5.3 Feature-Scale Modeling**

**Key phenomena:**

- Knudsen diffusion in high-aspect-ratio features
- Molecular re-emission and reflection
- Surface reaction probability
- Film profile evolution

**Knudsen diffusion coefficient:**

$$
D_K = \frac{d}{3} \sqrt{\frac{8 k_B T}{\pi m}}
$$

Where:

- $d$ — feature width $\left[\text{m}\right]$

**Effective diffusivity (transition regime):**

$$
\frac{1}{D_{eff}} = \frac{1}{D_{mol}} + \frac{1}{D_K}
$$

**Level set method for surface tracking:**

$$
\frac{\partial \phi}{\partial t} + v_n |
abla \phi| = 0
$$

Where:

- $\phi$ — level set function (zero at surface)
- $v_n$ — surface normal velocity (deposition rate)

**5.4 Atomistic Modeling**

**Density Functional Theory (DFT):**

- Calculate binding energies
- Determine activation barriers
- Predict reaction pathways

**Kinetic Monte Carlo (KMC):**

- Stochastic surface evolution
- Event rates from Arrhenius:

$$
\Gamma_i = 
u_0 \exp\left(-\frac{E_i}{k_B T}\right)
$$

Where:

- $\Gamma_i$ — rate of event $i$ $\left[\text{s}^{-1}\right]$
- $
u_0$ — attempt frequency $\sim 10^{12} - 10^{13} \, \text{s}^{-1}$
- $E_i$ — activation energy for event $i$ $\left[\text{eV}\right]$



**6. CVD Process Variants**

**6.1 LPCVD (Low Pressure CVD)**

**Operating conditions:**

- Pressure: $0.1 - 10 \, \text{Torr}$
- Temperature: $400 - 900 \, °\text{C}$
- Hot-wall reactor design

**Advantages:**

- Better uniformity (longer mean free path)
- Good step coverage
- High purity films

**Applications:**

- Polysilicon gates
- Silicon nitride (Si₃N₄)
- Thermal oxides

**6.2 PECVD (Plasma Enhanced CVD)**

**Additional physics:**

- Electron impact reactions
- Ion bombardment
- Radical chemistry
- Plasma sheath dynamics

**Electron density equation:**

$$
\frac{\partial n_e}{\partial t} + 
abla \cdot \boldsymbol{\Gamma}_e = S_e
$$

Where:

- $n_e$ — electron density $\left[\text{m}^{-3}\right]$
- $\boldsymbol{\Gamma}_e$ — electron flux $\left[\text{m}^{-2} \cdot \text{s}^{-1}\right]$
- $S_e$ — electron source term (ionization - recombination)

**Electron energy distribution:**

Often non-Maxwellian, requiring solution of Boltzmann equation or two-temperature models.

**Advantages:**

- Lower deposition temperatures ($200 - 400 \, °\text{C}$)
- Higher deposition rates
- Tunable film stress

**6.3 ALD (Atomic Layer Deposition)**

**Process characteristics:**

- Self-limiting surface reactions
- Sequential precursor pulses
- Sub-monolayer control

**Growth per cycle:**

$$
\text{GPC} = \frac{\Delta t}{\text{cycle}}
$$

Typically: $\text{GPC} \approx 0.5 - 2 \, \text{Å/cycle}$

**Surface coverage model:**

$$
\theta = \theta_{sat} \left(1 - e^{-\sigma J t}\right)
$$

Where:

- $\theta$ — surface coverage $\left[0 \leq \theta \leq 1\right]$
- $\theta_{sat}$ — saturation coverage
- $\sigma$ — reaction cross-section $\left[\text{m}^2\right]$
- $t$ — exposure time $\left[\text{s}\right]$

**Applications:**

- High-k gate dielectrics (HfO₂, ZrO₂)
- Barrier layers (TaN, TiN)
- Conformal coatings in 3D structures

**6.4 MOCVD (Metal-Organic CVD)**

**Precursors:**

- Metal-organic compounds (e.g., TMGa, TMAl, TMIn)
- Hydrides (AsH₃, PH₃, NH₃)

**Key challenges:**

- Parasitic gas-phase reactions
- Particle formation
- Precise composition control

**Applications:**

- III-V semiconductors (GaAs, InP, GaN)
- LEDs and laser diodes
- High-electron-mobility transistors (HEMTs)



**7. Step Coverage Modeling**

**7.1 Definition**

**Step coverage (SC):**

$$
SC = \frac{t_{bottom}}{t_{top}} \times 100\%
$$

Where:

- $t_{bottom}$ — film thickness at feature bottom
- $t_{top}$ — film thickness at feature top

**Aspect ratio (AR):**

$$
AR = \frac{H}{W}
$$

Where:

- $H$ — feature depth
- $W$ — feature width

**7.2 Ballistic Transport Model**

For molecular flow in features ($Kn > 1$):

**View factor approach:**

$$
F_{i \rightarrow j} = \frac{A_j \cos\theta_i \cos\theta_j}{\pi r_{ij}^2}
$$

**Flux balance at surface element:**

$$
J_i = J_{direct} + \sum_j (1-s) J_j F_{j \rightarrow i}
$$

Where:

- $s$ — sticking coefficient
- $(1-s)$ — re-emission probability

**7.3 Step Coverage Dependencies**

**Sticking coefficient effect:**

$$
SC \approx \frac{1}{1 + \frac{s \cdot AR}{2}}
$$

**Key observations:**

- Low $s$ → better step coverage
- High AR → poorer step coverage
- ALD achieves ~100% SC due to self-limiting chemistry

**7.4 Aspect Ratio Dependent Deposition (ARDD)**

**Local loading effect:**

- Reactant depletion in features
- Aspect ratio dependent etch (ARDE) analog

**Modeling approach:**

$$
R_{dep}(z) = R_0 \cdot \frac{C(z)}{C_0}
$$

Where:

- $z$ — depth into feature
- $C(z)$ — local concentration (decreases with depth)



**8. Thermal Modeling**

**8.1 Heat Transfer Mechanisms**

**Conduction (Fourier's law):**

$$
\mathbf{q}_{cond} = -k 
abla T
$$

**Convection:**

$$
q_{conv} = h (T_s - T_{\infty})
$$

Where:

- $h$ — heat transfer coefficient $\left[\text{W/m}^2 \cdot \text{K}\right]$

**Radiation (Stefan-Boltzmann):**

$$
q_{rad} = \varepsilon \sigma (T_s^4 - T_{surr}^4)
$$

Where:

- $\varepsilon$ — emissivity $\left[0 \leq \varepsilon \leq 1\right]$
- $\sigma$ — Stefan-Boltzmann constant $= 5.67 \times 10^{-8} \, \text{W/m}^2 \cdot \text{K}^4$

**8.2 Wafer Temperature Uniformity**

**Temperature non-uniformity impact:**

For reaction-limited regime:

$$
\frac{\Delta R}{R} \approx \frac{E_a}{R T^2} \Delta T
$$

**Example calculation:**

For $E_a = 1.5 \, \text{eV}$, $T = 900 \, \text{K}$, $\Delta T = 5 \, \text{K}$:

$$
\frac{\Delta R}{R} \approx \frac{1.5 \times 1.6 \times 10^{-19}}{1.38 \times 10^{-23} \times (900)^2} \times 5 \approx 10.7\%
$$

**8.3 Susceptor Design Considerations**

- **Material:** SiC, graphite, quartz
- **Heating:** Resistive, inductive, lamp (RTP)
- **Rotation:** Improves azimuthal uniformity
- **Edge effects:** Guard rings, pocket design



**9. Validation and Calibration**

**9.1 Experimental Characterization Techniques**

| Technique | Measurement | Resolution |
|-----------|-------------|------------|
| Ellipsometry | Thickness, optical constants | ~0.1 nm |
| XRF | Composition, thickness | ~1% |
| RBS | Composition, depth profile | ~10 nm |
| SIMS | Trace impurities | ppb |
| AFM | Surface morphology | ~0.1 nm (z) |
| SEM/TEM | Cross-section profile | ~1 nm |
| XRD | Crystallinity, stress | — |

**9.2 Model Calibration Approach**

**Parameter estimation:**

Minimize objective function:

$$
\chi^2 = \sum_i \left( \frac{y_i^{exp} - y_i^{model}}{\sigma_i} \right)^2
$$

Where:

- $y_i^{exp}$ — experimental measurement
- $y_i^{model}$ — model prediction
- $\sigma_i$ — measurement uncertainty

**Sensitivity analysis:**

$$
S_{ij} = \frac{\partial y_i}{\partial p_j} \cdot \frac{p_j}{y_i}
$$

Where:

- $S_{ij}$ — normalized sensitivity of output $i$ to parameter $j$
- $p_j$ — model parameter

**9.3 Uncertainty Quantification**

**Parameter uncertainty propagation:**

$$
\text{Var}(y) = \sum_j \left( \frac{\partial y}{\partial p_j} \right)^2 \text{Var}(p_j)
$$

**Monte Carlo approach:**

- Sample parameter distributions
- Run multiple model evaluations
- Statistical analysis of outputs



**10. Modern Developments**

**10.1 Machine Learning Integration**

**Applications:**

- **Surrogate models:** Neural networks trained on simulation data
- **Process optimization:** Bayesian optimization, genetic algorithms
- **Virtual metrology:** Predict film properties from process data
- **Defect prediction:** Correlate conditions with yield

**Neural network surrogate:**

$$
\hat{y} = f_{NN}(\mathbf{x}; \mathbf{w})
$$

Where:

- $\mathbf{x}$ — input process parameters
- $\mathbf{w}$ — trained network weights
- $\hat{y}$ — predicted output (rate, uniformity, etc.)

**10.2 Digital Twins**

**Components:**

- Real-time sensor data integration
- Physics-based + data-driven models
- Predictive capabilities

**Applications:**

- Chamber matching
- Predictive maintenance
- Run-to-run control
- Virtual experiments

**10.3 Advanced Materials**

**Emerging challenges:**

- **High-k dielectrics:** HfO₂, ZrO₂ via ALD
- **2D materials:** Graphene, MoS₂, WS₂
- **Selective deposition:** Area-selective ALD
- **3D integration:** Through-silicon vias (TSV)
- **New precursors:** Lower temperature, higher purity

**10.4 Computational Advances**

- **GPU acceleration:** Faster CFD solvers
- **Cloud computing:** Large parameter studies
- **Multiscale coupling:** Seamless reactor-to-feature modeling
- **Real-time simulation:** For process control



**Physical Constants**

| Constant | Symbol | Value |
|----------|--------|-------|
| Boltzmann constant | $k_B$ | $1.381 \times 10^{-23} \, \text{J/K}$ |
| Universal gas constant | $R$ | $8.314 \, \text{J/mol} \cdot \text{K}$ |
| Avogadro's number | $N_A$ | $6.022 \times 10^{23} \, \text{mol}^{-1}$ |
| Stefan-Boltzmann constant | $\sigma$ | $5.67 \times 10^{-8} \, \text{W/m}^2 \cdot \text{K}^4$ |
| Elementary charge | $e$ | $1.602 \times 10^{-19} \, \text{C}$ |



**Typical Process Parameters**

**B.1 LPCVD Polysilicon**

- **Precursor:** SiH₄
- **Temperature:** $580 - 650 \, °\text{C}$
- **Pressure:** $0.2 - 1.0 \, \text{Torr}$
- **Deposition rate:** $5 - 20 \, \text{nm/min}$

**B.2 PECVD Silicon Nitride**

- **Precursors:** SiH₄ + NH₃ or SiH₄ + N₂
- **Temperature:** $250 - 400 \, °\text{C}$
- **Pressure:** $1 - 5 \, \text{Torr}$
- **RF Power:** $0.1 - 1 \, \text{W/cm}^2$

**B.3 ALD Hafnium Oxide**

- **Precursors:** HfCl₄ or TEMAH + H₂O or O₃
- **Temperature:** $200 - 350 \, °\text{C}$
- **GPC:** $\sim 1 \, \text{Å/cycle}$
- **Cycle time:** $2 - 10 \, \text{s}$

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/cvd-modeling) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
