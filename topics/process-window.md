# Process Window

**Keywords**: process window,exposure-defocus,bossung,depth of focus,dof,exposure latitude,cpk,lithography window,semiconductor process window

---

**Process Window**

 

  1. Fundamental

A  process window  is the region in parameter space where a manufacturing step yields acceptable results. Mathematically, for a response function $y(\mathbf{x})$ depending on parameter vector $\mathbf{x} = (x_1, x_2, \ldots, x_n)$:

$$
\text{Process Window} = \{\mathbf{x} : y_{\min} \leq y(\mathbf{x}) \leq y_{\max}\}
$$

 

  2. Single-Parameter Statistics

For a single parameter with lower and upper specification limits (LSL, USL):

  Process Capability Indices

-  $C_p$ (Process Capability):  Measures window width relative to process variation

$$
C_p = \frac{USL - LSL}{6\sigma}
$$

-  $C_{pk}$ (Process Capability Index):  Accounts for process centering

$$
C_{pk} = \min\left[\frac{USL - \mu}{3\sigma}, \frac{\mu - LSL}{3\sigma}\right]
$$

  Industry Standards

- $C_p \geq 1.0$: Process variation fits within specifications
- $C_{pk} \geq 1.33$: 4σ capability (standard requirement)
- $C_{pk} \geq 1.67$: 5σ capability (high-reliability applications)
- $C_{pk} \geq 2.0$: 6σ capability (Six Sigma standard)

 

  3. Lithography: Exposure-Defocus (E-D) Window

The most critical and mathematically developed process window in semiconductor manufacturing.

  3.1 Bossung Curve Model

Critical dimension (CD) as a function of exposure dose $E$ and defocus $F$:

$$
CD(E, F) = CD_0 + a_1 E + a_2 F + a_{11} E^2 + a_{22} F^2 + a_{12} EF + \ldots
$$

The process window boundary is defined by:

$$
|CD(E, F) - CD_{\text{target}}| = \Delta CD_{\text{tolerance}}
$$

  3.2 Key Metrics

-  Exposure Latitude (EL):  Percentage dose range for acceptable CD

$$
EL = \frac{E_{\max} - E_{\min}}{E_{\text{nominal}}} \times 100\%
$$

-  Depth of Focus (DOF):  Focus range for acceptable CD (at given EL)

$$
DOF = F_{\max} - F_{\min}
$$

-  Process Window Area:  Total acceptable region

$$
A_{PW} = \iint_{\text{acceptable}} dE \, dF
$$

  3.3 Rayleigh Equations

Resolution and DOF scale with wavelength $\lambda$ and numerical aperture $NA$:

-  Resolution (minimum feature size): 

$$
R = k_1 \frac{\lambda}{NA}
$$

-  Depth of Focus: 

$$
DOF = \pm k_2 \frac{\lambda}{NA^2}
$$

 Critical insight:  As $k_1$ decreases (smaller features), DOF shrinks as $(k_1)^2$ — process windows collapse rapidly at advanced nodes.

| Technology Node | $k_1$ Factor | Relative DOF |
|     --|    --|    --|
| 180nm           | 0.6          | 1.0          |
| 65nm            | 0.4          | 0.44         |
| 14nm            | 0.3          | 0.25         |
| 5nm (EUV)       | 0.25         | 0.17         |

 

  4. Image Quality Metrics

  4.1 Normalized Image Log-Slope (NILS)

$$
NILS = w \cdot \frac{1}{I} \left|\frac{dI}{dx}\right|_{\text{edge}}
$$

Where:

- $w$ = feature width
- $I$ = aerial image intensity
- $\frac{dI}{dx}$ = intensity gradient at feature edge

For a coherent imaging system with partial coherence $\sigma$:

$$
NILS \approx \pi \cdot \frac{w}{\lambda/NA} \cdot \text{(contrast factor)}
$$

 Interpretation: 

- Higher NILS → larger process window
- NILS > 2.0: Robust process
- NILS < 1.5: Marginal process window
- NILS < 1.0: Near resolution limit

  4.2 Mask Error Enhancement Factor (MEEF)

$$
MEEF = \frac{\partial CD_{\text{wafer}}}{\partial CD_{\text{mask}}}
$$

 Characteristics: 

- MEEF = 1: Ideal (1:1 transfer from mask to wafer)
- MEEF > 1: Mask errors are amplified on wafer
- Near resolution limit: MEEF typically 3–4 or higher
- Impacts effective process window: mask CD tolerance = wafer CD tolerance / MEEF

 

  5. Multi-Parameter Process Windows

  5.1 Ellipsoid Model

For $n$ interacting parameters, the window is often an $n$-dimensional ellipsoid:

$$
(\mathbf{x} - \mathbf{x}_0)^T \mathbf{A} (\mathbf{x} - \mathbf{x}_0) \leq 1
$$

Where:

- $\mathbf{x}$ = parameter vector $(x_1, x_2, \ldots, x_n)$
- $\mathbf{x}_0$ = optimal operating point (center of ellipsoid)
- $\mathbf{A}$ = positive definite matrix encoding parameter correlations

 Geometric interpretation: 

- Eigenvalues of $\mathbf{A}$: $\lambda_1, \lambda_2, \ldots, \lambda_n$
- Principal axes lengths: $a_i = 1/\sqrt{\lambda_i}$
- Eigenvectors: orientation of principal axes

  5.2 Overlapping Windows

Real processes require multiple steps to simultaneously work:

$$
PW_{\text{total}} = \bigcap_{i=1}^{N} PW_i
$$

 Example:  Combined lithography + etch window

$$
PW_{\text{combined}} = PW_{\text{litho}}(E, F) \cap PW_{\text{etch}}(P, W, T)
$$

If individual windows are ellipsoids, their intersection is a more complex polytope — often computed numerically via:

- Linear programming
- Convex hull algorithms
- Monte Carlo sampling

 

  6. Response Surface Methodology (RSM)

  6.1 Quadratic Model

$$
y = \beta_0 + \sum_{i=1}^{n} \beta_i x_i + \sum_{i=1}^{n} \beta_{ii} x_i^2 + \sum_{i<j} \beta_{ij} x_i x_j + \epsilon
$$

In matrix form:

$$
y = \beta_0 + \mathbf{b}^T\mathbf{x} + \mathbf{x}^T\mathbf{B}\mathbf{x} + \epsilon
$$

Where:

- $\mathbf{b}$ = vector of first-order coefficients $(\beta_1, \beta_2, \ldots, \beta_n)$
- $\mathbf{B}$ = symmetric matrix of second-order coefficients

  6.2 Stationary Point (Optimum)

$$

abla y = \mathbf{b} + 2\mathbf{B}\mathbf{x} = 0
$$

$$
\mathbf{x}^* = -\frac{1}{2}\mathbf{B}^{-1}\mathbf{b}
$$

 Classification of stationary point: 

- All eigenvalues of $\mathbf{B}$ negative: Maximum
- All eigenvalues of $\mathbf{B}$ positive: Minimum
- Mixed signs: Saddle point

  6.3 Experimental Designs

-  Central Composite Design (CCD):  $2^n$ factorial + $2n$ axial points + center points
-  Box-Behnken Design:  Midpoints of edges of hypercube
-  Full Factorial:  $k^n$ runs for $k$ levels and $n$ factors

 

  7. Probabilistic Process Windows

  7.1 Success Probability Function

Instead of hard boundaries, define:

$$
P(\text{success}|\mathbf{x}) = \prod_{i=1}^{n} P_i(x_i)
$$

  7.2 Common Models

-  Probit Model: 

$$
P = \Phi\left(\frac{x - \mu}{\sigma}\right)
$$

Where $\Phi$ is the standard normal CDF.

-  Logistic Model: 

$$
P = \frac{1}{1 + e^{-\beta(x - x_0)}}
$$

-  Weibull Model (for reliability): 

$$
P = 1 - e^{-(x/\eta)^\beta}
$$

  7.3 Confidence-Level Process Window

The process window at confidence level $p$ is:

$$
PW_p = \{\mathbf{x} : P(\text{success}|\mathbf{x}) \geq p\}
$$

Typical values:

- $p = 0.95$: Standard production
- $p = 0.99$: High-yield requirement
- $p = 0.999$: Critical applications

 

  8. Stochastic Effects (Critical for EUV)

  8.1 Photon Statistics

At EUV wavelengths (13.5 nm), photon shot noise dominates:

$$
N_{\text{photons}} = \frac{\text{Dose} \times \text{Area}}{E_{\text{photon}}}
$$

Where:

$$
E_{\text{photon}} = \frac{hc}{\lambda} = \frac{(6.626 \times 10^{-34})(3 \times 10^8)}{13.5 \times 10^{-9}} \approx 92 \text{ eV}
$$

 Relative fluctuation: 

$$
\sigma_{\text{relative}} = \frac{1}{\sqrt{N}}
$$

  8.2 Line Edge Roughness (LER)

$$
LER \propto \frac{1}{\sqrt{\text{Dose}}}
$$

 Power Spectral Density (PSD) of edge roughness: 

$$
PSD(f) = \frac{LER^2 \cdot \xi}{1 + (2\pi f \xi)^{2H+1}}
$$

Where:

- $\xi$ = correlation length
- $H$ = Hurst exponent (typically 0.5–0.8)
- $f$ = spatial frequency

  8.3 Stochastic Failure Probability

For a feature of length $L$, the probability of at least one stochastic defect:

$$
P_{\text{failure}} = 1 - e^{-\lambda L}
$$

Where $\lambda$ = defect density per unit length.

 Impact on process window:  Stochastic failures create "probabilistic cliffs" — the process window shrinks because even within classical optical limits, random defects occur.

 

  9. Yield Integration

  9.1 General Yield Formula

Total yield is the integral over the process window weighted by parameter distributions:

$$
Y = \int_{PW} f(\mathbf{x}) \, d\mathbf{x}
$$

Where $f(\mathbf{x})$ is the joint probability density of parameter variations.

  9.2 Independent Gaussian Variations

For independent parameters with Gaussian distributions:

$$
Y = \prod_{i=1}^{n} \left[\Phi\left(\frac{USL_i - \mu_i}{\sigma_i}\right) - \Phi\left(\frac{LSL_i - \mu_i}{\sigma_i}\right)\right]
$$

  9.3 Defect-Limited Yield (Poisson Model)

$$
Y = e^{-D \cdot A}
$$

Where:

- $D$ = defect density (defects/cm²)
- $A$ = chip area (cm²)

  9.4 Combined Yield

$$
Y_{\text{total}} = Y_{\text{parametric}} \times Y_{\text{defect}} \times Y_{\text{systematic}}
$$

 

  10. Robust Optimization

  10.1 Maximize Inscribed Hypersphere

Find the operating point maximizing distance to all window boundaries:

$$
\max_{\mathbf{x}_0} \min_{\mathbf{x} \in \partial PW} \|\mathbf{x} - \mathbf{x}_0\|
$$

  10.2 Taguchi Loss Function

$$
L(y) = k(y - T)^2
$$

Where:

- $L$ = quality loss
- $y$ = actual value
- $T$ = target value
- $k$ = loss coefficient

 Expected loss: 

$$
E[L] = k\left[\sigma^2 + (\mu - T)^2\right] = k \cdot MSE
$$

  10.3 Weighted Area Maximization

For lithography OPC optimization:

$$
\max_{\text{OPC}} \iint_{PW} w(E, F) \, dE \, dF
$$

Where $w(E, F)$ weights central regions more heavily:

$$
w(E, F) = e^{-\alpha[(E-E_0)^2 + (F-F_0)^2]}
$$

 

  11. Overlay Budget

  11.1 Error Combination Rules

 For independent random errors (RSS - Root Sum Square): 

$$
\sigma_{\text{total}}^2 = \sum_{i=1}^{n} \sigma_i^2
$$

$$
\sigma_{\text{total}} = \sqrt{\sigma_1^2 + \sigma_2^2 + \cdots + \sigma_n^2}
$$

 For systematic errors (linear addition): 

$$
\text{Error}_{\text{total}} = \sum_{i=1}^{n} |\text{Error}_i|
$$

  11.2 Overlay Budget Allocation

Typical overlay contributors:

| Error Source | Type | Typical Contribution |
|    --|  |       |
| Stage positioning | Random | 1–2 nm |
| Lens distortion | Systematic | 0.5–1 nm |
| Wafer clamping | Random | 0.5–1 nm |
| Reticle alignment | Systematic | 0.5–1 nm |
| Thermal effects | Systematic | 0.5–2 nm |
| Measurement | Random | 0.5–1 nm |

 Design rule:  Overlay tolerance ≤ 1/4 to 1/3 of minimum feature size.

 

  12. Etch Process Windows

  12.1 Langmuir-Hinshelwood Kinetics

$$
\text{Rate} = \frac{k \cdot \theta_A \cdot \theta_B}{1 + K_A P_A + K_B P_B}
$$

Where:

- $k$ = rate constant
- $\theta_A, \theta_B$ = surface coverages of reactants A and B
- $K_A, K_B$ = adsorption equilibrium constants
- $P_A, P_B$ = partial pressures

  12.2 Ion Angular Distribution

Profile angle $\phi$ depends on ion angular distribution:

$$
\phi = \arctan\left(\frac{V_{\text{lateral}}}{V_{\text{vertical}}}\right)
$$

$$
V_{\text{lateral}} = \int_0^{\pi/2} f(\theta) \sin\theta \cos\theta \, d\theta
$$

Where $f(\theta)$ = ion angular distribution function.

  12.3 Selectivity

$$
\text{Selectivity} = \frac{\text{Etch rate of target material}}{\text{Etch rate of mask/underlayer}}
$$

Process window requires:

- Selectivity > 3–5 (typical)
- Selectivity > 10 (high aspect ratio features)
- Selectivity > 50 (critical etch stop layers)

 

  13. CMP Process Windows

  13.1 Preston Equation

$$
RR = K_p \cdot P \cdot V
$$

Where:

- $RR$ = removal rate (nm/min or Å/min)
- $K_p$ = Preston coefficient (material/consumable dependent)
- $P$ = applied pressure (psi or kPa)
- $V$ = relative velocity (m/s)

  13.2 Within-Wafer Non-Uniformity (WIWNU)

$$
WIWNU = \frac{\sigma_{RR}}{\mu_{RR}} \times 100\%
$$

Target: WIWNU < 3–5%

  13.3 Dishing and Erosion

-  Dishing:  Excess removal at center of wide features

$$
\text{Dishing} = t_{\text{initial}} - t_{\text{center}}
$$

-  Erosion:  Thinning of dielectric between metal lines

$$
\text{Erosion} = t_{\text{field}} - t_{\text{local}}
$$

 

  14. Key Equations Summary Table

| Metric | Formula | Significance |
|  --|   |    --|
| Resolution | $R = k_1 \frac{\lambda}{NA}$ | Minimum feature size |
| Depth of Focus | $DOF = \pm k_2 \frac{\lambda}{NA^2}$ | Focus tolerance |
| NILS | $NILS = \frac{w}{I} \left\|\frac{dI}{dx}\right\|$ | Image contrast at edge |
| MEEF | $MEEF = \frac{\partial CD_w}{\partial CD_m}$ | Mask error amplification |
| Process Capability | $C_{pk} = \frac{\min(USL-\mu, \mu-LSL)}{3\sigma}$ | Process capability |
| Exposure Latitude | $EL = \frac{E_{max} - E_{min}}{E_{nom}} \times 100\%$ | Dose tolerance |
| Stochastic LER | $LER \propto \frac{1}{\sqrt{Dose}}$ | Shot noise floor |
| Yield (Poisson) | $Y = e^{-DA}$ | Defect-limited yield |
| Preston Equation | $RR = K_p P V$ | CMP removal rate |

 

  15. Modern Computational Approaches

  15.1 Monte Carlo Simulation


Algorithm: Monte Carlo Yield Estimation
1. Define parameter distributions: x_i ~ N(μ_i, σ_i²)
2. For trial = 1 to N_trials:
   a. Sample x from joint distribution
   b. Evaluate y(x) for all responses
   c. Check if y ∈ [y_min, y_max] for all responses
   d. Record pass/fail
3. Yield = N_pass / N_trials
4. Confidence interval: Y ± z_α √(Y(1-Y)/N)


  15.2 Machine Learning Classification

-  Support Vector Machine (SVM):  Decision boundary defines process window
-  Neural Networks:  Complex, non-convex window shapes
-  Random Forest:  Ensemble method for robustness
-  Gaussian Process:  Probabilistic boundaries with uncertainty

  15.3 Digital Twin Approach

$$
\hat{y}_{t+1} = f(y_t, \mathbf{x}_t, \boldsymbol{\theta})
$$

Where:

- $\hat{y}_{t+1}$ = predicted next-step output
- $y_t$ = current measured output
- $\mathbf{x}_t$ = current process parameters
- $\boldsymbol{\theta}$ = model parameters (updated via Bayesian inference)

 

  16. Advanced Node Challenges

  16.1 Process Window Shrinkage

At advanced nodes (sub-7nm), multiple factors compound:

$$
PW_{\text{effective}} = PW_{\text{optical}} \cap PW_{\text{stochastic}} \cap PW_{\text{overlay}} \cap PW_{\text{etch}}
$$

  16.2 Multi-Patterning Complexity

For N-patterning (e.g., SAQP with N=4):

$$
\sigma_{\text{total}}^2 = \sum_{i=1}^{N} \sigma_{\text{step}_i}^2
$$

Error budget per step:

$$
\sigma_{\text{step}} = \frac{\sigma_{\text{target}}}{\sqrt{N}}
$$

  16.3 Design-Technology Co-Optimization (DTCO)

$$
\text{Objective: } \max_{\text{design}, \text{process}} \left[ \text{Performance} \times Y(\text{design}, \text{process}) \right]
$$

Subject to:

- Design rules: $DR_i(\text{layout}) \geq 0$
- Process windows: $\mathbf{x} \in PW$
- Reliability: $MTTF \geq \text{target}$

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/process-window) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
