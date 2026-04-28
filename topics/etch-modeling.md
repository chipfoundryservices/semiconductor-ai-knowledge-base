# Semiconductor Manufacturing Process: Etch Modeling

**Keywords**: etch modeling, plasma etch, RIE, reactive ion etching, etch simulation, DRIE

---

**Semiconductor Manufacturing Process: Etch Modeling**





**1. Introduction**

Etch modeling is one of the most complex and critical areas in semiconductor fabrication simulation. As device geometries shrink below $10\ \text{nm}$ and structures become increasingly three-dimensional, accurate prediction of etch behavior becomes essential for:

- **Process Development**: Predict outcomes before costly fab experiments
- **Yield Optimization**: Understand how variations propagate to device performance
- **OPC/EPC Extension**: Compensate for etch-induced pattern distortions in mask design
- **Design-Technology Co-Optimization (DTCO)**: Feed process effects back into design rules
- **Virtual Metrology**: Predict wafer results from equipment sensor data in real time


**2. Fundamentals of Etching**

**2.1 What is Etching?**

Etching selectively removes material from a wafer to transfer lithographically defined patterns into underlying layers—silicon, oxides, nitrides, metals, or complex stacks.

**2.2 Types of Etching**

- **Wet Etching**
  - Uses liquid chemicals (acids, bases, solvents)
  - Typically isotropic (etches equally in all directions)
  - Etch rate follows Arrhenius relationship:
    $$
    R = A \exp\left(-\frac{E_a}{k_B T}\right)
    $$
    where:
    - $R$ = etch rate
    - $A$ = pre-exponential factor
    - $E_a$ = activation energy
    - $k_B$ = Boltzmann constant ($1.381 \times 10^{-23}\ \text{J/K}$)
    - $T$ = temperature (K)

- **Dry/Plasma Etching**
  - Uses ionized gases (plasma)
  - Anisotropic (directional)
  - Dominant for modern processes ($< 100\ \text{nm}$ nodes)

**2.3 Plasma Etching Mechanisms**

1. **Physical Sputtering**
   - Ion bombardment physically removes atoms
   - Sputter yield $Y$ depends on ion energy $E_i$:
     $$
     Y(E_i) = A \left( \sqrt{E_i} - \sqrt{E_{th}} \right)
     $$
     where $E_{th}$ is the threshold energy

2. **Chemical Etching**
   - Reactive species form volatile products
   - Example: Silicon etching with fluorine
     $$
     \text{Si} + 4\text{F} \rightarrow \text{SiF}_4 \uparrow
     $$

3. **Ion-Enhanced Etching**
   - Synergy between ion bombardment and chemical reactions
   - Etch yield enhancement factor:
     $$
     \eta = \frac{Y_{ion+chem}}{Y_{ion} + Y_{chem}}
     $$



**3. Hierarchy of Etch Models**

**3.1 Empirical Models**

Data-driven, fast, used in production:

- **Etch Bias Models**
  - Simple offset correction:
    $$
    CD_{final} = CD_{litho} + \Delta_{etch}
    $$
  - Pattern-dependent bias:
    $$
    \Delta_{etch} = f(\text{pitch}, \text{density}, \text{orientation})
    $$

- **Etch Proximity Correction (EPC)**
  - Kernel-based convolution:
    $$
    \Delta(x,y) = \iint K(x-x', y-y') \cdot I(x', y') \, dx' dy'
    $$
  - Where $K$ is the etch kernel and $I$ is the pattern intensity

- **Machine Learning Models**
  - Neural networks trained on metrology data
  - Gaussian process regression for uncertainty quantification

**3.2 Feature-Scale Models**

Semi-empirical, balance speed and physics:

- **String/Segment Models**
  - Represent edges as connected nodes
  - Each node moves according to local etch rate vector:
    $$
    \frac{d\vec{r}_i}{dt} = R(\theta_i, \Gamma_{ion}, \Gamma_{n}) \cdot \hat{n}_i
    $$
  - Where:
    - $\vec{r}_i$ = position of node $i$
    - $\theta_i$ = local surface angle
    - $\Gamma_{ion}$, $\Gamma_n$ = ion and neutral fluxes
    - $\hat{n}_i$ = surface normal

- **Level-Set Methods**
  - Track surface as zero-contour of signed distance function $\phi$:
    $$
    \frac{\partial \phi}{\partial t} + R(\vec{x}) |
abla \phi| = 0
    $$
  - Handles topology changes naturally (merging, splitting)

- **Cell-Based/Voxel Methods**
  - Discretize feature volume into cells
  - Apply probabilistic removal rules:
    $$
    P_{remove} = 1 - \exp\left( -\sum_j \sigma_j \Gamma_j \Delta t \right)
    $$
  - Where $\sigma_j$ is the reaction cross-section for species $j$

**3.3 Physics-Based Plasma Models**

Capture reactor-scale phenomena:

- **Plasma Bulk**
  - Electron energy distribution function (EEDF)
  - Boltzmann equation:
    $$
    \frac{\partial f}{\partial t} + \vec{v} \cdot 
abla f + \frac{q\vec{E}}{m} \cdot 
abla_v f = \left( \frac{\partial f}{\partial t} \right)_{coll}
    $$

- **Sheath Physics**
  - Child-Langmuir law for ion flux:
    $$
    J_{ion} = \frac{4\epsilon_0}{9} \sqrt{\frac{2e}{M}} \frac{V^{3/2}}{d^2}
    $$
  - Ion angular distribution at wafer surface

- **Transport**
  - Species continuity:
    $$
    \frac{\partial n_i}{\partial t} + 
abla \cdot (n_i \vec{v}_i) = S_i - L_i
    $$
  - Where $S_i$ and $L_i$ are source and loss terms

**3.4 Atomistic Models**

Fundamental understanding, computationally expensive:

- **Molecular Dynamics (MD)**
  - Newton's equations for all atoms:
    $$
    m_i \frac{d^2 \vec{r}_i}{dt^2} = -
abla_i U(\{\vec{r}\})
    $$
  - Interatomic potentials: Tersoff, Stillinger-Weber, ReaxFF

- **Monte Carlo (MC) Methods**
  - Statistical sampling of ion trajectories
  - Binary collision approximation (BCA) for high energies
  - Acceptance probability:
    $$
    P = \min\left(1, \exp\left(-\frac{\Delta E}{k_B T}\right)\right)
    $$

- **Kinetic Monte Carlo (KMC)**
  - Sample reactive events with rates $k_i$:
    $$
    k_i = 
u_0 \exp\left(-\frac{E_{a,i}}{k_B T}\right)
    $$
  - Event selection: $\sum_{j < i} k_j < r \cdot K_{tot} \leq \sum_{j \leq i} k_j$



**4. Key Physical Phenomena**

**4.1 Anisotropy**

Ratio of vertical to lateral etch rate:

$$
A = 1 - \frac{R_{lateral}}{R_{vertical}}
$$

- $A = 1$: Perfectly anisotropic (vertical sidewalls)
- $A = 0$: Perfectly isotropic

**Mechanisms for achieving anisotropy:**

- Directional ion bombardment
- Sidewall passivation (polymer deposition)
- Low pressure operation (fewer collisions → more directional ions)
- Ion angular distribution characterized by:
  $$
  f(\theta) \propto \cos^n(\theta)
  $$
  where higher $n$ indicates more directional flux

**4.2 Selectivity**

Ratio of etch rates between materials:

$$
S_{A/B} = \frac{R_A}{R_B}
$$

- **Mask selectivity**: Target material vs. photoresist/hard mask
- **Stop layer selectivity**: Target material vs. underlying layer

Example selectivities required:

| Process | Selectivity Required |
|---------|---------------------|
| Oxide/Nitride | $> 20:1$ |
| Poly-Si/Oxide | $> 50:1$ |
| Si/SiGe (channel release) | $> 100:1$ |

**4.3 Loading Effects**

**Microloading**

Local depletion of reactive species in dense pattern regions:

$$
R_{dense} = R_0 \cdot \frac{1}{1 + \beta \cdot \rho_{local}}
$$

where:
- $R_0$ = etch rate in isolated feature
- $\beta$ = loading coefficient
- $\rho_{local}$ = local pattern density

**Macroloading**

Wafer-scale depletion:

$$
R = R_0 \cdot \left(1 - \alpha \cdot A_{exposed}\right)
$$

where $A_{exposed}$ is total exposed area fraction

**4.4 Aspect Ratio Dependent Etching (ARDE)**

Deep, narrow features etch slower due to transport limitations:

$$
R(AR) = R_0 \cdot \exp\left(-\frac{AR}{AR_0}\right)
$$

where $AR = \text{depth}/\text{width}$

**Physical mechanisms:**

1. **Ion Shadowing**
   - Geometric shadowing angle:
     $$
     \theta_{shadow} = \arctan\left(\frac{1}{AR}\right)
     $$

2. **Neutral Transport**
   - Knudsen diffusion coefficient:
     $$
     D_K = \frac{d}{3} \sqrt{\frac{8 k_B T}{\pi m}}
     $$
   - where $d$ is feature diameter

3. **Byproduct Redeposition**
   - Sticking probability affects escape

**4.5 Profile Anomalies**

| Phenomenon | Description | Cause |
|------------|-------------|-------|
| **Bowing** | Lateral bulge in sidewall | Ion scattering off sidewalls |
| **Notching** | Lateral etching at interface | Charge buildup on insulators |
| **Microtrenching** | Deep spots at corners | Ion reflection at feature bottom |
| **Footing** | Undercut at bottom | Isotropic chemical component |
| **Tapering** | Non-vertical sidewalls | Insufficient passivation |



**5. Mathematical Foundations**

**5.1 Surface Evolution Equation**

General form for surface height $h(x,y,t)$:

$$
\frac{\partial h}{\partial t} = -R_0 \cdot V(\theta) \cdot \sqrt{1 + |
abla h|^2}
$$

where:
- $R_0$ = baseline etch rate
- $V(\theta)$ = visibility/flux function
- $\theta = \arctan(|
abla h|)$

**5.2 Ion Angular Distribution**

At wafer surface, ion flux angular distribution:

$$
\Gamma(\theta, \phi) = \Gamma_0 \cdot f(\theta) \cdot g(E)
$$

Common models:

- **Gaussian distribution:**
  $$
  f(\theta) = \frac{1}{\sqrt{2\pi}\sigma_\theta} \exp\left(-\frac{\theta^2}{2\sigma_\theta^2}\right)
  $$

- **Thompson distribution** (for sputtered neutrals):
  $$
  f(E) \propto \frac{E}{(E + E_b)^3}
  $$

**5.3 Visibility Calculation**

For a point on the surface, visibility to incoming flux:

$$
V(\vec{r}) = \frac{1}{2\pi} \int_0^{2\pi} \int_0^{\theta_{max}(\phi)} f(\theta) \sin\theta \cos\theta \, d\theta \, d\phi
$$

where $\theta_{max}(\phi)$ is determined by local geometry (shadowing)

**5.4 Surface Reaction Kinetics**

Langmuir-Hinshelwood mechanism:

$$
R = k \cdot \theta_A \cdot \theta_B
$$

where surface coverages follow:

$$
\frac{d\theta_i}{dt} = s_i \Gamma_i (1 - \theta_{total}) - k_d \theta_i - k_r \theta_i
$$

- $s_i$ = sticking coefficient
- $k_d$ = desorption rate
- $k_r$ = reaction rate

**5.5 Plasma-Surface Interaction Yield**

Ion-enhanced etch yield:

$$
Y_{etch} = Y_0 + Y_1 \cdot \sqrt{E_{ion} - E_{th}} + Y_{chem} \cdot \frac{\Gamma_n}{\Gamma_{ion}}
$$

where:
- $Y_0$ = chemical baseline yield
- $Y_1$ = ion enhancement coefficient
- $E_{th}$ = threshold energy (~15-50 eV typically)
- $Y_{chem}$ = chemical enhancement factor



**6. Modern Modeling Approaches**

**6.1 Hybrid Multi-Scale Frameworks**

Coupling different scales:

```
-
┌─────────────────────────────────────────────────────────────┐
│                    REACTOR SCALE                            │
│    Plasma simulation (fluid or PIC)                         │
│    Output: Ion/neutral fluxes, energies, angular dist.      │
└────────────────────────┬────────────────────────────────────┘
                         │ Boundary conditions
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                    FEATURE SCALE                            │
│    Level-set or Monte Carlo                                 │
│    Output: Profile evolution, etch rates                    │
└────────────────────────┬────────────────────────────────────┘
                         │ Parameter extraction
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                    ATOMISTIC SCALE                          │
│    MD/KMC simulations                                       │
│    Output: Sticking coefficients, sputter yields            │
└─────────────────────────────────────────────────────────────┘
```

**6.2 Machine Learning Integration**

- **Surrogate Models**
  - Train neural network on physics simulation outputs:
    $$
    \hat{y} = f_{NN}(\vec{x}; \vec{w})
    $$
  - Loss function:
    $$
    \mathcal{L} = \frac{1}{N} \sum_{i=1}^{N} \|y_i - \hat{y}_i\|^2 + \lambda \|\vec{w}\|^2
    $$

- **Physics-Informed Neural Networks (PINNs)**
  - Embed physics constraints in loss:
    $$
    \mathcal{L}_{total} = \mathcal{L}_{data} + \alpha \mathcal{L}_{physics}
    $$
  - Where $\mathcal{L}_{physics}$ enforces governing equations

- **Virtual Metrology**
  - Predict CD, profile from chamber sensors:
    $$
    CD_{predicted} = g(P, T, V_{bias}, \text{OES}, ...)
    $$

**6.3 Computational Lithography Integration**

Major EDA tools couple lithography + etch:

1. Litho simulation → Resist profile $h_R(x,y)$
2. Etch simulation → Final pattern $h_F(x,y)$
3. Combined model:
   $$
   CD_{final} = CD_{design} + \Delta_{OPC} + \Delta_{litho} + \Delta_{etch}
   $$



**7. Challenges at Advanced Nodes**

**7.1 FinFET / Gate-All-Around (GAA)**

- **Fin Etch**
  - Sidewall angle uniformity: $90° \pm 1°$
  - Width control: $\pm 1\ \text{nm}$ at $W_{fin} < 10\ \text{nm}$

- **Channel Release**
  - Selective SiGe vs. Si etching
  - Required selectivity: $> 100:1$
  - Etch rate:
    $$
    R_{SiGe} \gg R_{Si}
    $$

- **Inner Spacer Formation**
  - Isotropic lateral etch in confined geometry
  - Depth control: $\pm 0.5\ \text{nm}$

**7.2 3D NAND**

Extreme aspect ratio challenges:

| Generation | Layers | Aspect Ratio |
|------------|--------|--------------|
| 96L | 96 | ~60:1 |
| 128L | 128 | ~80:1 |
| 176L | 176 | ~100:1 |
| 232L+ | 232+ | ~150:1 |

Critical issues:
- ARDE variation across depth
- Bowing control
- Twisting in elliptical holes

**7.3 EUV Patterning**

- Very thin resists: $< 40\ \text{nm}$
- Hard mask stacks with multiple layers
- LER/LWR amplification:
  $$
  LER_{final} = \sqrt{LER_{litho}^2 + LER_{etch}^2}
  $$
- Target: $LER < 1.2\ \text{nm}$ ($3\sigma$)

**7.4 Stochastic Effects**

At small dimensions, statistical fluctuations dominate:

$$
\sigma_{CD} \propto \frac{1}{\sqrt{N_{events}}}
$$

where $N_{events}$ = number of etching events per feature



**8. Industry Tools**

**8.1 Commercial Software**

| Category | Tools |
|----------|-------|
| **TCAD/Process** | Synopsys Sentaurus Process, Silvaco Victory Process |
| **Virtual Fab** | Coventor SEMulator3D |
| **Equipment Vendor** | Lam Research, Applied Materials (proprietary) |
| **Computational Litho** | Synopsys S-Litho, Siemens Calibre |

**8.2 Research Tools**

- **MCFPM** (Monte Carlo Feature Profile Model) - University of Illinois
- **LAMMPS** - Molecular dynamics
- **SPARTA** - Direct Simulation Monte Carlo
- **OpenFOAM** - Plasma fluid modeling



**9. Future Directions**

**9.1 Digital Twins**

Real-time chamber models for closed-loop process control:

$$
\vec{u}_{control}(t) = \mathcal{K} \left[ y_{target} - y_{model}(t) \right]
$$

**9.2 Atomistic-Continuum Coupling**

Seamless multi-scale simulation using:
- Adaptive mesh refinement
- Concurrent coupling methods
- Machine-learned interscale bridging

**9.3 New Materials**

Modeling requirements for:
- 2D materials (graphene, MoS$_2$, WS$_2$)
- High-$\kappa$ dielectrics
- Ferroelectrics (HfZrO)
- High-mobility channels (InGaAs, Ge)

**9.4 Uncertainty Quantification**

Predicting distributions, not just means:

$$
P(CD) = \int P(CD | \vec{\theta}) P(\vec{\theta}) d\vec{\theta}
$$

Key metrics:
- Process capability: $C_{pk} = \frac{\min(USL - \mu, \mu - LSL)}{3\sigma}$
- Target: $C_{pk} > 1.67$ for production



**Summary**

Etch modeling spans from atomic-scale surface reactions to reactor-scale plasma physics to fab-level empirical correlations. The art lies in choosing the right abstraction level:

| Application | Model Type | Speed | Accuracy |
|-------------|------------|-------|----------|
| Production OPC/EPC | Empirical/ML | ★★★★★ | ★★☆☆☆ |
| Process Development | Feature-scale | ★★★☆☆ | ★★★★☆ |
| Mechanism Research | Atomistic MD/MC | ★☆☆☆☆ | ★★★★★ |
| Equipment Design | Plasma + Feature | ★★☆☆☆ | ★★★★☆ |

As geometries shrink and structures become more 3D, accurate etch modeling becomes essential for first-time-right process development and continued yield improvement.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/etch-modeling) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
