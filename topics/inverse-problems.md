# Inverse Problems

**Keywords**: inverse problems,inverse problem,ill-posed problems,regularization,parameter estimation,OPC,scatterometry,virtual metrology

---

**Inverse Problems**





   1. Introduction to Inverse Problems

    1.1 Mathematical Definition

In mathematical terms, a  forward problem  is defined as:

$$
y = f(x)
$$

where:

- $x$ = input parameters (process conditions)
- $f$ = forward operator (physical model)
- $y$ = output observations (measurements, wafer state)

The  inverse problem  seeks to find $x$ given $y$:

$$
x = f^{-1}(y)
$$

    1.2 Hadamard Well-Posedness Criteria

A problem is  well-posed  if it satisfies:

1.  Existence : A solution exists for all admissible data
2.  Uniqueness : The solution is unique
3.  Stability : The solution depends continuously on the data

Most semiconductor inverse problems are  ill-posed , violating one or more criteria.

    1.3 Why Semiconductor Manufacturing Creates Ill-Posed Problems

-  Non-uniqueness : Multiple process conditions $\{x_1, x_2, \ldots\}$ can produce indistinguishable outputs within measurement precision
-  Sensitivity : Small perturbations in measurements cause large changes in estimated parameters:
  $$
  \|x_1 - x_2\| \gg \|y_1 - y_2\|
  $$
-  Incomplete information : Not all relevant physical quantities can be measured



   2. Lithography Inverse Problems

    2.1 Optical Proximity Correction (OPC)

     2.1.1 Forward Model

The aerial image intensity at the wafer plane:

$$
I(x, y) = \left| \int \int H(f_x, f_y) \cdot M(f_x, f_y) \cdot e^{i2\pi(f_x x + f_y y)} \, df_x \, df_y \right|^2
$$

where:

- $H(f_x, f_y)$ = optical transfer function (pupil function)
- $M(f_x, f_y)$ = Fourier transform of the mask pattern
- $(f_x, f_y)$ = spatial frequencies

     2.1.2 Inverse Problem Formulation

Find mask pattern $M$ that minimizes:

$$
\mathcal{L}(M) = \|T(M) - D\|^2 + \lambda R(M)
$$

where:

- $T(M)$ = printed pattern from mask $M$
- $D$ = desired (target) pattern
- $R(M)$ = regularization for mask manufacturability
- $\lambda$ = regularization weight

     2.1.3 Regularization Terms

Common regularization terms include:

-  Mask complexity penalty :
  $$
  R_{\text{complexity}}(M) = \int |
abla M|^2 \, dA
  $$

-  Minimum feature size constraint :
  $$
  R_{\text{MFS}}(M) = \sum_i \max(0, w_{\min} - w_i)^2
  $$

-  Sidelobe suppression :
  $$
  R_{\text{SRAF}}(M) = \int_{\Omega_{\text{dark}}} I(x,y)^2 \, dA
  $$

    2.2 Source-Mask Optimization (SMO)

Joint optimization over source shape $S$ and mask $M$:

$$
\min_{S, M} \|T(S, M) - D\|^2 + \lambda_1 R_S(S) + \lambda_2 R_M(M)
$$

This is a higher-dimensional inverse problem with:

- Source degrees of freedom: pupil discretization points
- Mask degrees of freedom: pixel-based mask representation
- Coupled nonlinear interactions

    2.3 Inverse Lithography Technology (ILT)

Full pixel-based mask optimization using gradient descent:

$$
M^{(k+1)} = M^{(k)} - \alpha 
abla_M \mathcal{L}(M^{(k)})
$$

Gradient computation via  adjoint method :

$$

abla_M \mathcal{L} = \text{Re}\left\{ \mathcal{F}^{-1}\left[ H^* \cdot \mathcal{F}\left[ \frac{\partial \mathcal{L}}{\partial I} \cdot \psi^* \right] \right] \right\}
$$

where $\psi$ is the complex field at the wafer plane.



   3. Thin Film Metrology Inverse Problems

    3.1 Ellipsometry

     3.1.1 Measured Quantities

Ellipsometry measures the complex reflectance ratio:

$$
\rho = \frac{r_p}{r_s} = \tan(\Psi) \cdot e^{i\Delta}
$$

where:

- $r_p$ = p-polarized reflection coefficient
- $r_s$ = s-polarized reflection coefficient
- $\Psi$ = amplitude ratio angle
- $\Delta$ = phase difference

     3.1.2 Forward Model (Fresnel Equations)

For a single film on substrate:

$$
r_{012} = \frac{r_{01} + r_{12} e^{-i2\beta}}{1 + r_{01} r_{12} e^{-i2\beta}}
$$

where:

- $r_{01}, r_{12}$ = interface Fresnel coefficients
- $\beta = \frac{2\pi d}{\lambda} \tilde{n}_1 \cos\theta_1$ = phase thickness
- $d$ = film thickness
- $\tilde{n}_1 = n_1 + ik_1$ = complex refractive index

     3.1.3 Inverse Problem

Given measured $\Psi(\lambda), \Delta(\lambda)$, find:

- Film thickness(es): $d_1, d_2, \ldots$
- Optical constants: $n(\lambda), k(\lambda)$ for each layer

 Objective function :

$$
\chi^2 = \sum_{\lambda} \left[ \left(\frac{\Psi_{\text{meas}} - \Psi_{\text{calc}}}{\sigma_\Psi}\right)^2 + \left(\frac{\Delta_{\text{meas}} - \Delta_{\text{calc}}}{\sigma_\Delta}\right)^2 \right]
$$

    3.2 Scatterometry (Optical Critical Dimension)

     3.2.1 Forward Model

Rigorous Coupled-Wave Analysis (RCWA) solves Maxwell's equations for periodic structures:

$$

abla \times \mathbf{E} = -\frac{\partial \mathbf{B}}{\partial t}, \quad 
abla \times \mathbf{H} = \frac{\partial \mathbf{D}}{\partial t}
$$

The grating is represented as Fourier series:

$$
\varepsilon(x, z) = \sum_m \varepsilon_m(z) e^{imGx}
$$

where $G = \frac{2\pi}{\Lambda}$ is the grating vector.

     3.2.2 Profile Parameterization

A trapezoidal line profile is characterized by:

-  CD (Critical Dimension) : $w$
-  Height : $h$
-  Sidewall Angle : $\theta_{\text{SWA}}$
-  Corner Rounding : $r$
-  Footing/Undercut : $\delta$

Parameter vector: $\mathbf{p} = [w, h, \theta_{\text{SWA}}, r, \delta, \ldots]^T$

     3.2.3 Inverse Problem

$$
\hat{\mathbf{p}} = \arg\min_{\mathbf{p}} \sum_{\lambda, \theta} \left( R_{\text{meas}}(\lambda, \theta) - R_{\text{RCWA}}(\lambda, \theta; \mathbf{p}) \right)^2
$$

 Challenges :

- Non-convex objective with multiple local minima
- Parameter correlations (e.g., height vs. refractive index)
- Sensitivity varies dramatically across parameters



   4. Plasma Etch Inverse Problems

    4.1 Etch Rate Modeling

     4.1.1 Ion-Enhanced Etching Model

$$
\text{ER} = k_0 \cdot \Gamma_{\text{ion}}^a \cdot \Gamma_{\text{neutral}}^b \cdot \exp\left(-\frac{E_a}{k_B T}\right)
$$

where:

- $\Gamma_{\text{ion}}$ = ion flux
- $\Gamma_{\text{neutral}}$ = neutral radical flux
- $E_a$ = activation energy
- $a, b$ = reaction orders

     4.1.2 Aspect Ratio Dependent Etching (ARDE)

Etch rate in high-aspect-ratio features:

$$
\text{ER}(AR) = \text{ER}_0 \cdot \frac{1}{1 + \alpha \cdot AR^\beta}
$$

where $AR = \frac{\text{depth}}{\text{width}}$ is the aspect ratio.

    4.2 Profile Reconstruction from OES

     4.2.1 Optical Emission Spectroscopy Model

Emission intensity for species $j$:

$$
I_j(\lambda) = A_j \cdot n_e \cdot n_j \cdot \langle \sigma v \rangle_{j}^{\text{exc}}
$$

where:

- $n_e$ = electron density
- $n_j$ = species density
- $\langle \sigma v \rangle$ = rate coefficient for excitation

     4.2.2 Inverse Problem

From observed $I_j(t)$ time traces, determine:

- Etch front position $z(t)$
- Layer interfaces
- Process endpoint

 State estimation formulation :

$$
\hat{z}(t) = \arg\min_{z} \|I_{\text{obs}}(t) - I_{\text{model}}(z, t)\|^2 + \lambda \left\|\frac{dz}{dt}\right\|^2
$$



   5. Ion Implantation Inverse Problems

    5.1 As-Implanted Profile

     5.1.1 LSS Theory (Lindhard-Scharff-Schiøtt)

The implanted concentration profile:

$$
C(x) = \frac{\Phi}{\sqrt{2\pi} \Delta R_p} \exp\left[-\frac{(x - R_p)^2}{2(\Delta R_p)^2}\right]
$$

where:

- $\Phi$ = implant dose (ions/cm²)
- $R_p$ = projected range
- $\Delta R_p$ = straggle (standard deviation)

     5.1.2 Dual-Pearson for Channeling

For crystalline substrates with channeling:

$$
C(x) = (1-f) \cdot P_1(x; R_{p1}, \Delta R_{p1}, \gamma_1, \beta_1) + f \cdot P_2(x; R_{p2}, \Delta R_{p2}, \gamma_2, \beta_2)
$$

where $P_i$ are Pearson IV distributions and $f$ is the channeled fraction.

    5.2 Diffusion Inversion

     5.2.1 Fick's Second Law with Concentration Dependence

$$
\frac{\partial C}{\partial t} = \frac{\partial}{\partial x}\left[D(C) \frac{\partial C}{\partial x}\right]
$$

For dopants like boron:

$$
D(C) = D_i^* \left[1 + \beta_1 \left(\frac{C}{n_i}\right) + \beta_2 \left(\frac{C}{n_i}\right)^2\right]
$$

     5.2.2 Inverse Problem

Given final SIMS profile $C_{\text{final}}(x)$, find:

- Initial implant conditions: $\Phi, E$ (energy)
- Anneal conditions: $T(t)$, time $t_a$
- Diffusion parameters: $D_i^*, \beta_1, \beta_2$

 Regularized formulation :

$$
\min_{\theta} \|C_{\text{SIMS}} - C_{\text{simulated}}(\theta)\|^2 + \lambda \|\theta - \theta_{\text{prior}}\|^2
$$



   6. Deposition Inverse Problems

    6.1 CVD Step Coverage

     6.1.1 Thiele Modulus

Conformality characterized by:

$$
\phi = L \sqrt{\frac{k_s}{D_{\text{Kn}}}}
$$

where:

- $L$ = feature depth
- $k_s$ = surface reaction rate
- $D_{\text{Kn}}$ = Knudsen diffusion coefficient

Step coverage:

$$
SC = \frac{1}{\cosh(\phi)}
$$

     6.1.2 Inverse Problem

Given target step coverage $SC_{\text{target}}$, find:

- Pressure $P$
- Temperature $T$
- Precursor partial pressures
- Carrier gas flow

    6.2 ALD Thickness Control

     6.2.1 Growth Per Cycle (GPC)

$$
\text{GPC} = \Theta_{\text{sat}} \cdot d_{\text{ML}}
$$

where:

- $\Theta_{\text{sat}}$ = saturation coverage (0 to 1)
- $d_{\text{ML}}$ = monolayer thickness

     6.2.2 Inverse Problem

For target thickness $d$:

$$
N_{\text{cycles}} = \left\lceil \frac{d}{\text{GPC}(T, t_{\text{pulse}}, t_{\text{purge}})} \right\rceil
$$

Optimize $(T, t_{\text{pulse}}, t_{\text{purge}})$ for throughput and uniformity.



   7. CMP Inverse Problems

    7.1 Preston Equation

Material removal rate:

$$
\text{MRR} = K_p \cdot P \cdot V
$$

where:

- $K_p$ = Preston coefficient
- $P$ = applied pressure
- $V$ = relative velocity

    7.2 Pattern Density Effects

     7.2.1 Effective Density Model

Local removal rate depends on pattern density $\rho$:

$$
\text{MRR}_{\text{local}} = \frac{\text{MRR}_{\text{blanket}}}{\rho + (1-\rho) \cdot \eta}
$$

where $\eta$ is the selectivity ratio.

     7.2.2 Dishing and Erosion

-  Dishing  (over-polish of metal in trench):
  $$
  D = K_d \cdot w \cdot t_{\text{over}}
  $$

-  Erosion  (over-polish of dielectric):
  $$
  E = K_e \cdot \rho \cdot t_{\text{over}}
  $$

    7.3 Inverse Problem

Given target post-CMP topography, find:

- Polish time
- Pressure profile (zone control)
- Slurry chemistry
- Potentially: design rule modifications for pattern density



   8. TCAD Parameter Extraction

    8.1 Device Model

MOSFET drain current:

$$
I_D = \mu_{\text{eff}} C_{\text{ox}} \frac{W}{L} \left[(V_{GS} - V_{th})V_{DS} - \frac{V_{DS}^2}{2}\right] (1 + \lambda V_{DS})
$$

    8.2 Inverse Problem Formulation

Given measured $I_D(V_{GS}, V_{DS})$ characteristics, extract:

- $V_{th}$ = threshold voltage
- $\mu_{\text{eff}}$ = effective mobility
- $L_{\text{eff}}$ = effective channel length
- $\lambda$ = channel length modulation

 Optimization :

$$
\min_{\theta} \sum_{i,j} \left( I_{D,\text{meas}}(V_{GS,i}, V_{DS,j}) - I_{D,\text{model}}(V_{GS,i}, V_{DS,j}; \theta) \right)^2
$$

    8.3 Interface Trap Density from C-V

From measured capacitance $C(V_G)$:

$$
D_{it}(E) = \frac{1}{qA}\left(\frac{1}{C_{\text{meas}}} - \frac{1}{C_{\text{ox}}}\right)^{-1} - \frac{C_s}{qA}
$$

where $C_s$ is the semiconductor capacitance.



   9. Mathematical Solution Approaches

    9.1 Regularization Methods

     9.1.1 Tikhonov Regularization

$$
\hat{x} = \arg\min_x \|Ax - y\|^2 + \lambda\|Lx\|^2
$$

Closed-form solution:

$$
\hat{x} = (A^T A + \lambda L^T L)^{-1} A^T y
$$

     9.1.2 Total Variation Regularization

$$
\min_x \|Ax - y\|^2 + \lambda \int |
abla x| \, dA
$$

Preserves edges while smoothing noise.

     9.1.3 L1 Regularization (LASSO)

$$
\min_x \|Ax - y\|^2 + \lambda\|x\|_1
$$

Promotes sparse solutions.

    9.2 Bayesian Inference

     9.2.1 Posterior Distribution

By Bayes' theorem:

$$
p(x|y) = \frac{p(y|x) \cdot p(x)}{p(y)} \propto p(y|x) \cdot p(x)
$$

where:

- $p(y|x)$ = likelihood
- $p(x)$ = prior
- $p(x|y)$ = posterior

     9.2.2 Maximum A Posteriori (MAP) Estimate

$$
\hat{x}_{\text{MAP}} = \arg\max_x p(x|y) = \arg\max_x [\log p(y|x) + \log p(x)]
$$

For Gaussian likelihood and prior:

$$
\hat{x}_{\text{MAP}} = \arg\min_x \left[\frac{\|y - Ax\|^2}{2\sigma_n^2} + \frac{\|x - x_0\|^2}{2\sigma_x^2}\right]
$$

This recovers Tikhonov regularization with $\lambda = \frac{\sigma_n^2}{\sigma_x^2}$.

    9.3 Adjoint Methods for Gradient Computation

For objective $\mathcal{L}(x) = \|F(x) - y\|^2$ with expensive forward model $F$:

 Forward solve :

$$
F(x) = y_{\text{sim}}
$$

 Adjoint solve :

$$
\left(\frac{\partial F}{\partial u}\right)^T \lambda = \frac{\partial \mathcal{L}}{\partial u}
$$

 Gradient :

$$

abla_x \mathcal{L} = \left(\frac{\partial F}{\partial x}\right)^T \lambda
$$

Computational cost: $O(1)$ forward + adjoint solves regardless of parameter dimension.

    9.4 Machine Learning Approaches

     9.4.1 Neural Network Surrogate Models

Train $\hat{F}_\theta(x) \approx F(x)$:

$$
\theta^* = \arg\min_\theta \sum_i \|F(x_i) - \hat{F}_\theta(x_i)\|^2
$$

Then use $\hat{F}_\theta$ for fast inverse optimization.

     9.4.2 Physics-Informed Neural Networks (PINNs)

Loss function includes physics residual:

$$
\mathcal{L} = \mathcal{L}_{\text{data}} + \lambda_{\text{PDE}} \mathcal{L}_{\text{PDE}} + \lambda_{\text{BC}} \mathcal{L}_{\text{BC}}
$$

where:

$$
\mathcal{L}_{\text{PDE}} = \left\|\mathcal{N}[u_\theta(x,t)]\right\|^2
$$

for PDE operator $\mathcal{N}$.



   10. Key Challenges and Considerations

    10.1 Non-Uniqueness

-  Definition : Multiple solutions $\{x_1, x_2, \ldots\}$ satisfy $\|F(x_i) - y\| < \epsilon$
-  Mitigation : Additional measurements, physical constraints, regularization
-  Quantification : Null space analysis, condition number $\kappa(A) = \frac{\sigma_{\max}}{\sigma_{\min}}$

    10.2 High Dimensionality

-  Parameter space : $\dim(x) \sim 10^2$ to $10^6$ (e.g., ILT masks)
-  Curse of dimensionality : Sampling density scales as $N^d$
-  Approaches : Dimensionality reduction, sparse representations, hierarchical models

    10.3 Computational Cost

-  Forward model cost : RCWA: $O(N^3)$ per wavelength; TCAD: hours for full 3D
-  Inverse iterations : Typically $10^2$ to $10^4$ forward evaluations
-  Mitigation : Surrogate models, multi-fidelity methods, parallel computing

    10.4 Model Uncertainty

-  Sources : Unmodeled physics, parameter drift, measurement bias
-  Impact : Inverse solution may fit model but not reality
-  Approaches : Model calibration, uncertainty propagation, robust optimization



   11. Emerging Directions

    11.1 Digital Twins

- Real-time state estimation combining physics models with sensor data
- Kalman filtering for dynamic process tracking:
  $$
  \hat{x}_{k|k} = \hat{x}_{k|k-1} + K_k(y_k - H\hat{x}_{k|k-1})
  $$

    11.2 Multi-Fidelity Methods

- Hierarchy of models: analytical → reduced-order → full numerical
- Efficient exploration with cheap models, refinement with expensive ones
- Multi-fidelity Gaussian processes for Bayesian optimization

    11.3 Uncertainty Quantification

- Full posterior distributions, not just point estimates
- Sensitivity analysis: which measurements reduce uncertainty most?
- Propagation to downstream process steps and device performance

    11.4 End-to-End Differentiable Simulation

- Automatic differentiation through entire process flow
- Enables gradient-based optimization across traditionally separate steps
- Requires differentiable forward models



   12. Summary

|  Process Step  |  Forward Problem  |  Inverse Problem  |
|------------------|---------------------|---------------------|
| Lithography | Mask → Printed pattern | Target pattern → Optimal mask |
| Ellipsometry | Stack parameters → $\Psi, \Delta$ | $\Psi, \Delta$ → Thickness, n, k |
| Scatterometry | Profile → Diffraction spectrum | Spectrum → Profile dimensions |
| Plasma Etch | Recipe → Etch profile | Target profile → Recipe |
| Ion Implant | Dose, energy → Dopant profile | Target profile → Implant conditions |
| CVD/ALD | Recipe → Film properties | Target properties → Recipe |
| CMP | Recipe, pattern → Final topography | Target topography → Recipe |
| TCAD | Process/device params → I-V curves | I-V curves → Extracted parameters |

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/inverse-problems) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
