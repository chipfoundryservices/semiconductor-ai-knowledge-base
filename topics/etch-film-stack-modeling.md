# Etch Film Stack Mathematical Modeling

**Keywords**: etch film stack modeling, etch film stack, etch modeling, etch film stack math, film stack etch modeling

---

**Etch Film Stack Mathematical Modeling**

 1. Introduction and Problem Setup

A film stack in semiconductor manufacturing consists of multiple thin-film layers that must be precisely etched. Typical structures include:

- Photoresist (masking layer)
- Hard mask (SiN, SiO₂, or metal)
- Target film (material to be etched)
- Etch stop layer
- Substrate (Si wafer)

Objectives

- Remove target material at a controlled rate
- Stop precisely at interfaces (selectivity)
- Maintain profile fidelity (anisotropy, sidewall angle)
- Achieve uniformity across the wafer



 2. Fundamental Etch Rate Models

2.1 Surface Reaction Kinetics

The Langmuir-Hinshelwood model captures competitive adsorption of reactive species:

$$
R = \frac{k \cdot \theta_A \cdot \theta_B}{\left(1 + K_A[A] + K_B[B]\right)^2}
$$

Where:

- $R$ = etch rate
- $k$ = reaction rate constant
- $\theta_A, \theta_B$ = fractional surface coverage of species A and B
- $K_A, K_B$ = adsorption equilibrium constants
- $[A], [B]$ = gas-phase concentrations

2.2 Temperature Dependence (Arrhenius)

$$
R = R_0 \exp\left(-\frac{E_a}{k_B T}\right)
$$

Where:

- $R_0$ = pre-exponential factor
- $E_a$ = activation energy
- $k_B$ = Boltzmann constant ($1.38 \times 10^{-23}$ J/K)
- $T$ = absolute temperature (K)

2.3 Ion-Enhanced Etching Model

Most plasma etching exhibits synergistic behavior—ions enhance chemical reactions:

$$
R_{total} = R_{chem} + R_{phys} + R_{synergy}
$$

The ion-enhanced component dominates in RIE/ICP:

$$
R_{ie} = Y(E, \theta) \cdot \Gamma_{ion} \cdot \Theta_{react}
$$

Where:

- $Y(E, \theta)$ = ion yield function (depends on energy $E$ and angle $\theta$)
- $\Gamma_{ion}$ = ion flux to surface (ions/cm²·s)
- $\Theta_{react}$ = fractional coverage of reactive species



 3. Profile Evolution Mathematics

3.1 Level Set Method

The evolving surface is represented as the zero-contour of a level set function $\phi(\mathbf{x}, t)$:

$$
\frac{\partial \phi}{\partial t} + V(\mathbf{x}, t) \cdot |
abla \phi| = 0
$$

Where:

- $\phi(\mathbf{x}, t)$ = level set function
- $V(\mathbf{x}, t)$ = local etch velocity (material and flux dependent)
- $
abla \phi$ = gradient of the level set function
- $|
abla \phi|$ = magnitude of the gradient

The surface normal is computed as:

$$
\hat{n} = \frac{
abla \phi}{|
abla \phi|}
$$

3.2 Visibility and Shadowing Integrals

For a point $\mathbf{p}$ inside a feature, the effective flux is:

$$
\Gamma(\mathbf{p}) = \int_{\Omega_{visible}} f(\hat{\Omega}) \cdot (\hat{\Omega} \cdot \hat{n}) \, d\Omega
$$

Where:

- $\Omega_{visible}$ = solid angle visible from point $\mathbf{p}$
- $f(\hat{\Omega})$ = ion angular distribution function (IADF)
- $\hat{n}$ = local surface normal

3.3 Ion Angular Distribution Function (IADF)

Typically modeled as a Gaussian:

$$
f(\theta) = \frac{1}{\sqrt{2\pi}\sigma} \exp\left(-\frac{\theta^2}{2\sigma^2}\right)
$$

Where:

- $\theta$ = angle from surface normal
- $\sigma$ = angular spread (related to $T_i / T_e$ ratio)



 4. Multi-Layer Stack Modeling

4.1 Interface Tracking

For a stack with $n$ layers at depths $z_1, z_2, \ldots, z_n$:

$$
\frac{dz_{etch}}{dt} = -R_i(t)
$$

Where $i$ indicates the current material being etched. Material transitions occur when $z_{etch}$ crosses an interface boundary.

4.2 Selectivity Definition

$$
S_{A:B} = \frac{R_A}{R_B}
$$

Design requirements:

- Mask selectivity: $S_{target:mask} < 1$ (mask erodes slowly)
- Stop layer selectivity: $S_{target:stop} \gg 1$ (typically > 10:1)

4.3 Time-to-Clear Calculation

For layer thickness $d_i$ with etch rate $R_i$:

$$
t_{clear,i} = \frac{d_i}{R_i}
$$

Total etch time through multiple layers:

$$
t_{total} = \sum_{i=1}^{n} \frac{d_i}{R_i} + t_{overetch}
$$



 5. Aspect Ratio Dependent Etching (ARDE)

5.1 General ARDE Model

Etch rate decreases with aspect ratio (AR = depth/width):

$$
R(AR) = R_0 \cdot f(AR)
$$

5.2 Neutral Transport Limited (Knudsen Regime)

$$
R(AR) = \frac{R_0}{1 + \alpha \cdot AR}
$$

The Knudsen diffusivity in a cylindrical feature:

$$
D_K = \frac{d}{3}\sqrt{\frac{8 k_B T}{\pi m}}
$$

Where:

- $d$ = feature diameter
- $m$ = molecular mass of neutral species
- $T$ = gas temperature

5.3 Clausing Factor for Molecular Flow

For a tube of length $L$ and radius $r$:

$$
W = \frac{1}{1 + \frac{3L}{8r}}
$$

5.4 Ion Angular Distribution Limited

$$
R(AR) = R_0 \cdot \int_0^{\theta_{max}(AR)} f(\theta) \cos\theta \, d\theta
$$

Where $\theta_{max}$ is the maximum acceptance angle:

$$
\theta_{max} = \arctan\left(\frac{w}{2h}\right)
$$



 6. Plasma and Transport Modeling

6.1 Sheath Physics

Child-Langmuir Law (Collisionless Sheath)

$$
J = \frac{4\varepsilon_0}{9}\sqrt{\frac{2e}{M}}\frac{V_0^{3/2}}{d^2}
$$

Where:

- $J$ = ion current density
- $\varepsilon_0$ = permittivity of free space
- $e$ = electron charge
- $M$ = ion mass
- $V_0$ = sheath voltage
- $d$ = sheath thickness

Sheath Thickness (Matrix Sheath)

$$
s = \lambda_D \sqrt{\frac{2eV_0}{k_B T_e}}
$$

Where $\lambda_D$ is the Debye length:

$$
\lambda_D = \sqrt{\frac{\varepsilon_0 k_B T_e}{n_e e^2}}
$$

6.2 Ion Flux to Surface

At the sheath edge, ions reach the Bohm velocity:

$$
u_B = \sqrt{\frac{k_B T_e}{M_i}}
$$

Ion flux:

$$
\Gamma_i = n_s \cdot u_B = n_s \sqrt{\frac{k_B T_e}{M_i}}
$$

Where $n_s \approx 0.61 \cdot n_0$ (sheath edge density).

6.3 Neutral Species Balance

Continuity equation for neutral species:

$$

abla \cdot (D 
abla n) + \sum_j k_j n_j n_e - k_{loss} n = 0
$$

Where:

- $D$ = diffusion coefficient
- $k_j$ = generation rate constants
- $k_{loss}$ = surface loss rate



 7. Feature-Scale Monte Carlo Methods

7.1 Algorithm Overview

1. Sample particles from flux distributions at feature entrance
2. Track trajectories (ballistic for ions, random walk for neutrals)
3. Surface interactions: React, reflect, or stick with probabilities
4. Accumulate statistics for local etch rates
5. Advance surface using accumulated rates

7.2 Reflection Probability Models

Specular Reflection

$$
\theta_{out} = \theta_{in}
$$

Diffuse (Cosine) Reflection

$$
P(\theta_{out}) \propto \cos(\theta_{out})
$$

Mixed Model

$$
P_{reflect} = (1 - s) \cdot P_{specular} + s \cdot P_{diffuse}
$$

Where $s$ is the scattering coefficient.

7.3 Sticking Coefficient Model

$$
\gamma = \gamma_0 \cdot (1 - \Theta)^n
$$

Where:

- $\gamma_0$ = bare surface sticking coefficient
- $\Theta$ = surface coverage
- $n$ = reaction order



 8. Loading Effects

8.1 Macroloading (Wafer Scale)

$$
R = \frac{R_0}{1 + \beta \cdot A_{exposed}}
$$

Where:

- $A_{exposed}$ = total exposed etchable area
- $\beta$ = loading coefficient

8.2 Microloading (Pattern Scale)

Local etch rate depends on pattern density $\rho$:

$$
R_{local} = R_0 \cdot \left(1 - \gamma \cdot \rho\right)
$$

Dense patterns etch slower due to local reactant depletion.

8.3 Reactive Species Depletion Model

For a feature with area $A$ in a cell of area $A_{cell}$:

$$
R = R_0 \cdot \frac{1}{1 + \frac{k_{etch} \cdot A}{k_{supply} \cdot A_{cell}}}
$$



 9. Atomic Layer Etching (ALE) Models

9.1 Two-Step Process

Step 1 - Surface Modification:

$$
A_{(g)} + S_{(s)} \rightarrow A\text{-}S_{(s)}
$$

Step 2 - Removal:

$$
A\text{-}S_{(s)} + B_{(g/ion)} \rightarrow \text{volatile products}
$$

9.2 Self-Limiting Kinetics

Surface coverage during modification:

$$
\theta_{mod}(t) = 1 - \exp\left(-\Gamma_A \cdot s_A \cdot t\right)
$$

Where:

- $\Gamma_A$ = flux of modifying species
- $s_A$ = sticking probability
- $t$ = exposure time

9.3 Etch Per Cycle (EPC)

$$
EPC = \theta_{sat} \cdot \delta_{ML}
$$

Where:

- $\theta_{sat}$ = saturation coverage (ideally 1.0)
- $\delta_{ML}$ = monolayer thickness (typically 0.1–0.5 nm)

9.4 Synergy Factor

$$
S_f = \frac{EPC_{ALE}}{EPC_{step1} + EPC_{step2}}
$$

Values $S_f > 1$ indicate synergistic enhancement.



 10. Process Window Modeling

10.1 Response Surface Methodology

$$
CD = \beta_0 + \sum_{i=1}^{k} \beta_i x_i + \sum_{i=1}^{k} \beta_{ii} x_i^2 + \sum_{i<j} \beta_{ij} x_i x_j + \varepsilon
$$

Where:

- $CD$ = critical dimension (response variable)
- $x_i$ = process parameters (pressure, power, gas flows, time, etc.)
- $\beta$ = regression coefficients
- $\varepsilon$ = random error

10.2 Sensitivity Analysis

$$
\frac{\partial CD}{\partial x_i} = \beta_i + 2\beta_{ii}x_i + \sum_{j 
eq i} \beta_{ij} x_j
$$

10.3 Process Capability

$$
C_p = \frac{USL - LSL}{6\sigma}
$$

$$
C_{pk} = \min\left(\frac{USL - \mu}{3\sigma}, \frac{\mu - LSL}{3\sigma}\right)
$$

Where:

- $USL, LSL$ = upper and lower specification limits
- $\mu$ = process mean
- $\sigma$ = process standard deviation



 11. Computational Implementation

11.1 Multi-Scale Hierarchy

| Scale | Method | Outputs |
|:------|:-------|:--------|
| Atomic (Å) | MD, DFT | Yield functions, surface chemistry |
| Feature (nm–μm) | Monte Carlo, Level Set | Profile evolution |
| Reactor (cm) | Fluid/hybrid plasma models | Plasma uniformity |
| Wafer (mm–cm) | Empirical/FEM | Loading, CD uniformity |

11.2 Level Set Discretization

Using upwind finite differences:

$$
\phi_i^{n+1} = \phi_i^n - \Delta t \cdot V_i \cdot |
abla \phi|_i
$$

With the gradient approximated by:

$$
|
abla \phi| \approx \sqrt{\max(D^{-x}, 0)^2 + \min(D^{+x}, 0)^2 + \max(D^{-y}, 0)^2 + \min(D^{+y}, 0)^2}
$$

Where $D^{\pm x}$ are forward/backward differences.

11.3 CFL Condition for Stability

$$
\Delta t < \frac{\Delta x}{V_{max}}
$$



 12. Advanced Considerations

12.1 High Aspect Ratio (HAR) Challenges

For 3D NAND (AR > 50:1):

$$
R_{HAR} = R_0 \cdot \exp\left(-\frac{AR}{AR_c}\right)
$$

Where $AR_c$ is a characteristic decay constant.

12.2 Stochastic Effects at Atomic Scale

Line edge roughness (LER) from statistical fluctuations:

$$
\sigma_{LER} \propto \sqrt{\frac{1}{N_{atoms}}} \propto \frac{1}{\sqrt{CD}}
$$

12.3 Pattern-Dependent Charging

Electron shading leads to differential charging:

$$
V_{bottom} = V_{plasma} - \frac{J_e - J_i}{C_{feature}}
$$

This causes notching and profile distortion in HAR features.

12.4 Etch-Induced Damage

Ion damage depth follows:

$$
R_p = \frac{E}{S_n + S_e}
$$

Where:

- $E$ = ion energy
- $S_n$ = nuclear stopping power
- $S_e$ = electronic stopping power



 13. Equations

| Physics | Equation |
|:--------|:---------|
| Etch rate | $R = Y(E) \cdot \Gamma_{ion} \cdot \Theta$ |
| Level set evolution | $\frac{\partial \phi}{\partial t} + V|
abla\phi| = 0$ |
| Selectivity | $S_{A:B} = R_A / R_B$ |
| ARDE | $R(AR) = R_0 / (1 + \alpha \cdot AR)$ |
| Bohm flux | $\Gamma_i = n_s \sqrt{k_B T_e / M_i}$ |
| ALE EPC | $EPC = \theta_{sat} \cdot \delta_{ML}$ |
| Knudsen diffusion | $D_K = \frac{d}{3}\sqrt{8k_BT/\pi m}$ |

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/etch-film-stack-modeling) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
