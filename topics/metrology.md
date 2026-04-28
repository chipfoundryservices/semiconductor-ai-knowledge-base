# Semiconductor Manufacturing Process Metrology: Mathematical Modeling

**Keywords**: metrology, scatterometry, ellipsometry, x-ray reflectometry, inverse problems, optimization, statistical inference, mathematical modeling

---

**Semiconductor Manufacturing Process Metrology: Mathematical Modeling**



**1. The Core Problem Structure**

Semiconductor metrology faces a fundamental **inverse problem**: we make indirect measurements (optical spectra, scattered X-rays, electron signals) and must infer physical quantities (dimensions, compositions, defect states) that we cannot directly observe at the nanoscale.

**1.1 Mathematical Formulation**

The general measurement model:

$$
\mathbf{y} = \mathcal{F}(\mathbf{p}) + \boldsymbol{\epsilon}
$$

**Variable Definitions:**

- $\mathbf{y}$ — measured signal vector (spectrum, image intensity, scattered amplitude)
- $\mathbf{p}$ — physical parameters of interest (CD, thickness, sidewall angle, composition)
- $\mathcal{F}$ — forward model operator (physics of measurement process)
- $\boldsymbol{\epsilon}$ — noise/uncertainty term

**1.2 Key Mathematical Challenges**

- **Nonlinearity:** $\mathcal{F}$ is typically highly nonlinear
- **Computational cost:** Forward model evaluation is expensive
- **Ill-posedness:** Inverse may be non-unique or unstable
- **High dimensionality:** Many parameters from limited measurements



**2. Optical Critical Dimension (OCD) / Scatterometry**

This is the most mathematically intensive metrology technique in high-volume manufacturing.

**2.1 Forward Problem: Electromagnetic Scattering**

For periodic structures (gratings, arrays), solve Maxwell's equations with Floquet-Bloch boundary conditions.

**2.1.1 Maxwell's Equations**

$$

abla \times \mathbf{E} = -\frac{\partial \mathbf{B}}{\partial t}
$$

$$

abla \times \mathbf{H} = \mathbf{J} + \frac{\partial \mathbf{D}}{\partial t}
$$

**2.1.2 Rigorous Coupled Wave Analysis (RCWA)**

**Field Expansion in Fourier Series:**

The electric field in layer $j$ with grating vector $\mathbf{K}$:

$$
\mathbf{E}(\mathbf{r}) = \sum_{n=-N}^{N} \mathbf{E}_n^{(j)} \exp\left(i(\mathbf{k}_n \cdot \mathbf{r})\right)
$$

where the diffraction wave vectors are:

$$
\mathbf{k}_n = \mathbf{k}_0 + n\mathbf{K}
$$

**Key Properties:**

- Converts PDEs to eigenvalue problem
- Matches boundary conditions at layer interfaces
- Computational complexity: $O(N^3)$ where $N$ = number of Fourier orders

**2.2 Inverse Problem: Parameter Extraction**

Given measured spectra $R(\lambda, \theta)$, find best-fit parameters $\mathbf{p}$.

**2.2.1 Optimization Formulation**

$$
\hat{\mathbf{p}} = \arg\min_{\mathbf{p}} \left\| \mathbf{y}_{\text{meas}} - \mathcal{F}(\mathbf{p}) \right\|^2 + \lambda R(\mathbf{p})
$$

**Regularization Options:**

- **Tikhonov regularization:**
  $$
  R(\mathbf{p}) = \left\| \mathbf{p} - \mathbf{p}_0 \right\|^2
  $$

- **Sparsity-promoting (L1):**
  $$
  R(\mathbf{p}) = \left\| \mathbf{p} \right\|_1
  $$

- **Total variation:**
  $$
  R(\mathbf{p}) = \int |
abla \mathbf{p}| \, d\mathbf{x}
  $$

**2.2.2 Library-Based Approach**

1. **Precomputation:** Generate forward model on dense parameter grid
2. **Storage:** Build library with millions of entries
3. **Search:** Find best match using regression methods

**Regression Methods:**

- Polynomial regression — fast but limited accuracy
- Neural networks — handle nonlinearity well
- Gaussian process regression — provides uncertainty estimates

**2.3 Parameter Correlations and Uncertainty**

**2.3.1 Fisher Information Matrix**

$$
[\mathbf{I}(\mathbf{p})]_{ij} = \mathbb{E}\left[\frac{\partial \ln L}{\partial p_i}\frac{\partial \ln L}{\partial p_j}\right]
$$

**2.3.2 Cramér-Rao Lower Bound**

$$
\text{Var}(\hat{p}_i) \geq \left[\mathbf{I}^{-1}\right]_{ii}
$$

**Physical Interpretation:** Strong correlations (e.g., height vs. sidewall angle) manifest as near-singular information matrices—a fundamental limit on independent resolution.



**3. Thin Film Metrology: Ellipsometry**

**3.1 Physical Model**

Ellipsometry measures polarization state change upon reflection:

$$
\rho = \frac{r_p}{r_s} = \tan(\Psi)\exp(i\Delta)
$$

**Variables:**

- $r_p$ — p-polarized reflection coefficient
- $r_s$ — s-polarized reflection coefficient
- $\Psi$ — amplitude ratio angle
- $\Delta$ — phase difference

**3.2 Transfer Matrix Formalism**

For multilayer stacks:

$$
\mathbf{M} = \prod_{j=1}^{N} \mathbf{M}_j = \prod_{j=1}^{N} \begin{pmatrix} \cos\delta_j & \dfrac{i\sin\delta_j}{\eta_j} \\[10pt] i\eta_j\sin\delta_j & \cos\delta_j \end{pmatrix}
$$

where the phase thickness is:

$$
\delta_j = \frac{2\pi}{\lambda} n_j d_j \cos(\theta_j)
$$

**Parameters:**

- $n_j$ — refractive index of layer $j$
- $d_j$ — thickness of layer $j$
- $\theta_j$ — angle of propagation in layer $j$
- $\eta_j$ — optical admittance

**3.3 Dispersion Models**

**3.3.1 Cauchy Model (Transparent Materials)**

$$
n(\lambda) = A + \frac{B}{\lambda^2} + \frac{C}{\lambda^4}
$$

**3.3.2 Sellmeier Equation**

$$
n^2(\lambda) = 1 + \sum_{i} \frac{B_i \lambda^2}{\lambda^2 - C_i}
$$

**3.3.3 Tauc-Lorentz Model (Amorphous Semiconductors)**

$$
\varepsilon_2(E) = \begin{cases} 
\dfrac{A E_0 C (E - E_g)^2}{(E^2 - E_0^2)^2 + C^2 E^2} \cdot \dfrac{1}{E} & E > E_g \\[10pt]
0 & E \leq E_g
\end{cases}
$$

with $\varepsilon_1$ derived via Kramers-Kronig relations:

$$
\varepsilon_1(E) = \varepsilon_{1\infty} + \frac{2}{\pi} \mathcal{P} \int_0^\infty \frac{\xi \varepsilon_2(\xi)}{\xi^2 - E^2} d\xi
$$

**3.3.4 Drude Model (Metals/Conductors)**

$$
\varepsilon(\omega) = \varepsilon_\infty - \frac{\omega_p^2}{\omega^2 + i\gamma\omega}
$$

**Parameters:**

- $\omega_p$ — plasma frequency
- $\gamma$ — damping coefficient
- $\varepsilon_\infty$ — high-frequency dielectric constant



**4. X-ray Metrology Mathematics**

**4.1 X-ray Reflectivity (XRR)**

**4.1.1 Parratt Recursion Formula**

For specular reflection at grazing incidence:

$$
R_j = \frac{r_{j,j+1} + R_{j+1}\exp(2ik_{z,j+1}d_{j+1})}{1 + r_{j,j+1}R_{j+1}\exp(2ik_{z,j+1}d_{j+1})}
$$

where $r_{j,j+1}$ is the Fresnel coefficient at interface $j$.

**4.1.2 Roughness Correction (Névot-Croce Factor)**

$$
r'_{j,j+1} = r_{j,j+1} \exp\left(-2k_{z,j}k_{z,j+1}\sigma_j^2\right)
$$

**Parameters:**

- $k_{z,j}$ — perpendicular wave vector component in layer $j$
- $\sigma_j$ — RMS roughness at interface $j$

**4.2 CD-SAXS (Critical Dimension Small Angle X-ray Scattering)**

**4.2.1 Scattering Intensity**

For transmission scattering from 3D nanostructures:

$$
I(\mathbf{q}) = \left|\tilde{\rho}(\mathbf{q})\right|^2 = \left|\int \Delta\rho(\mathbf{r})\exp(-i\mathbf{q}\cdot\mathbf{r})d^3\mathbf{r}\right|^2
$$

**4.2.2 Form Factor for Simple Shapes**

**Rectangular parallelepiped:**

$$
F(\mathbf{q}) = V \cdot \text{sinc}\left(\frac{q_x a}{2}\right) \cdot \text{sinc}\left(\frac{q_y b}{2}\right) \cdot \text{sinc}\left(\frac{q_z c}{2}\right)
$$

**Cylinder:**

$$
F(\mathbf{q}) = 2\pi R^2 L \cdot \frac{J_1(q_\perp R)}{q_\perp R} \cdot \text{sinc}\left(\frac{q_z L}{2}\right)
$$

where $J_1$ is the first-order Bessel function.



**5. Statistical Process Control Mathematics**

**5.1 Virtual Metrology**

Predict wafer properties from tool sensor data without direct measurement:

$$
y = f(\mathbf{x}) + \varepsilon
$$

**5.1.1 Partial Least Squares (PLS)**

Handles high-dimensional, correlated inputs:

1. Find latent variables: $\mathbf{T} = \mathbf{X}\mathbf{W}$
2. Maximize covariance with $y$
3. Model: $y = \mathbf{T}\mathbf{Q} + e$

**Optimization objective:**

$$
\max_{\mathbf{w}} \text{Cov}(\mathbf{X}\mathbf{w}, y)^2 \quad \text{subject to} \quad \|\mathbf{w}\| = 1
$$

**5.1.2 Gaussian Process Regression**

$$
y(\mathbf{x}) \sim \mathcal{GP}\left(m(\mathbf{x}), k(\mathbf{x}, \mathbf{x}')\right)
$$

**Common Kernel Functions:**

- **Squared Exponential (RBF):**
  $$
  k(\mathbf{x}, \mathbf{x}') = \sigma_f^2 \exp\left(-\frac{\|\mathbf{x} - \mathbf{x}'\|^2}{2\ell^2}\right)
  $$

- **Matérn 5/2:**
  $$
  k(r) = \sigma_f^2 \left(1 + \frac{\sqrt{5}r}{\ell} + \frac{5r^2}{3\ell^2}\right) \exp\left(-\frac{\sqrt{5}r}{\ell}\right)
  $$

**5.2 Run-to-Run Control**

**5.2.1 EWMA Controller**

$$
\hat{d}_t = \lambda y_{t-1} + (1-\lambda)\hat{d}_{t-1}
$$

$$
x_t = x_{\text{nom}} - \frac{\hat{d}_t}{\hat{\beta}}
$$

**Parameters:**

- $\lambda$ — smoothing factor (typically 0.2–0.4)
- $\hat{\beta}$ — estimated process gain
- $x_{\text{nom}}$ — nominal recipe setting

**5.2.2 Model Predictive Control (MPC)**

$$
\min_{\mathbf{u}} \sum_{k=0}^{N} \left\| y_{t+k} - y_{\text{target}} \right\|_Q^2 + \left\| \Delta u_{t+k} \right\|_R^2
$$

subject to:

- Process dynamics: $\mathbf{x}_{t+1} = \mathbf{A}\mathbf{x}_t + \mathbf{B}\mathbf{u}_t$
- Output equation: $y_t = \mathbf{C}\mathbf{x}_t$
- Constraints: $\mathbf{u}_{\min} \leq \mathbf{u}_t \leq \mathbf{u}_{\max}$

**5.3 Wafer-Level Spatial Modeling**

**5.3.1 Zernike Polynomial Decomposition**

$$
W(r,\theta) = \sum_{n=0}^{N} \sum_{m=-n}^{n} a_{nm} Z_n^m(r,\theta)
$$

**First few Zernike polynomials:**

| Index | Name | Formula |
|-------|------|---------|
| $Z_0^0$ | Piston | $1$ |
| $Z_1^{-1}$ | Tilt Y | $2r\sin\theta$ |
| $Z_1^1$ | Tilt X | $2r\cos\theta$ |
| $Z_2^0$ | Defocus | $\sqrt{3}(2r^2-1)$ |
| $Z_2^{-2}$ | Astigmatism | $\sqrt{6}r^2\sin2\theta$ |
| $Z_2^2$ | Astigmatism | $\sqrt{6}r^2\cos2\theta$ |

**5.3.2 Gaussian Random Fields**

For spatially correlated residuals:

$$
\text{Cov}\left(W(\mathbf{s}_1), W(\mathbf{s}_2)\right) = \sigma^2 \rho\left(\|\mathbf{s}_1 - \mathbf{s}_2\|; \phi\right)
$$

**Common correlation functions:**

- **Exponential:**
  $$
  \rho(h) = \exp\left(-\frac{h}{\phi}\right)
  $$

- **Gaussian:**
  $$
  \rho(h) = \exp\left(-\frac{h^2}{\phi^2}\right)
  $$



**6. Overlay Metrology Mathematics**

**6.1 Higher-Order Correction Models**

Overlay error as polynomial expansion:

$$
\delta x = T_x + M_x \cdot x + R_x \cdot y + \sum_{i+j \leq n} c_{ij}^x x^i y^j
$$

$$
\delta y = T_y + M_y \cdot y + R_y \cdot x + \sum_{i+j \leq n} c_{ij}^y x^i y^j
$$

**Physical interpretation of linear terms:**

- $T_x, T_y$ — Translation
- $M_x, M_y$ — Magnification
- $R_x, R_y$ — Rotation

**6.2 Sampling Strategy Optimization**

**6.2.1 D-Optimal Design**

$$
\mathbf{s}^* = \arg\max_{\mathbf{s}} \det\left(\mathbf{X}_s^T \mathbf{X}_s\right)
$$

Minimizes the volume of the confidence ellipsoid for parameter estimates.

**6.2.2 Information-Theoretic Approach**

Maximize expected information gain:

$$
I(\mathbf{s}) = H(\mathbf{p}) - \mathbb{E}_{\mathbf{y}}\left[H(\mathbf{p}|\mathbf{y})\right]
$$



**7. Machine Learning Integration**

**7.1 Physics-Informed Neural Networks (PINNs)**

Combine data fitting with physical constraints:

$$
\mathcal{L} = \mathcal{L}_{\text{data}} + \lambda \mathcal{L}_{\text{physics}}
$$

**Components:**

- **Data loss:**
  $$
  \mathcal{L}_{\text{data}} = \frac{1}{N} \sum_{i=1}^{N} \left\| y_i - f_\theta(\mathbf{x}_i) \right\|^2
  $$

- **Physics loss (example: Maxwell residual):**
  $$
  \mathcal{L}_{\text{physics}} = \frac{1}{M} \sum_{j=1}^{M} \left\| 
abla \times \mathbf{E}_\theta - i\omega\mu\mathbf{H}_\theta \right\|^2
  $$

**7.2 Neural Network Surrogates**

**Architecture for forward model approximation:**

- **Input:** Geometric parameters $\mathbf{p} \in \mathbb{R}^d$
- **Hidden layers:** Multiple fully-connected layers with ReLU/GELU activation
- **Output:** Simulated spectrum $\mathbf{y} \in \mathbb{R}^m$

**Speedup:** $10^4$ – $10^6\times$ over rigorous simulation

**7.3 Deep Learning for Defect Detection**

**Methods:**

- **CNNs** — Classification and localization
- **Autoencoders** — Anomaly detection via reconstruction error:
  $$
  \text{Score}(\mathbf{x}) = \left\| \mathbf{x} - D(E(\mathbf{x})) \right\|^2
  $$
- **Instance segmentation** — Precise defect boundary delineation



**8. Uncertainty Quantification**

**8.1 GUM Framework (Guide to Uncertainty in Measurement)**

Combined standard uncertainty:

$$
u_c^2(y) = \sum_{i} \left(\frac{\partial f}{\partial x_i}\right)^2 u^2(x_i) + 2\sum_{i<j} \frac{\partial f}{\partial x_i}\frac{\partial f}{\partial x_j} u(x_i, x_j)
$$

**8.2 Total Measurement Uncertainty (TMU)**

$$
TMU^2 = TMU_{\text{precision}}^2 + TMU_{\text{accuracy}}^2 + TMU_{\text{sample}}^2
$$

**Components:**

- **Precision:** Repeatability and reproducibility
- **Accuracy:** Systematic offset from reference
- **Sample:** Variation in test structures

**8.3 Bayesian Approaches**

**8.3.1 Posterior Inference**

$$
P(\mathbf{p}|\mathbf{y}) \propto P(\mathbf{y}|\mathbf{p}) \cdot P(\mathbf{p})
$$

**8.3.2 Sampling Methods**

- **Markov Chain Monte Carlo (MCMC):**
  - Metropolis-Hastings
  - Hamiltonian Monte Carlo
  - No-U-Turn Sampler (NUTS)

- **Variational Inference:**
  $$
  q^*(\mathbf{p}) = \arg\min_{q \in \mathcal{Q}} D_{KL}\left(q(\mathbf{p}) \| P(\mathbf{p}|\mathbf{y})\right)
  $$



**9. Emerging Mathematical Challenges**

| Challenge | Mathematical Response |
|-----------|----------------------|
| **3D architectures** (GAA, CFET) | 3D electromagnetic solvers, efficient parameterization |
| **Sub-nm precision** | Enhanced uncertainty quantification, systematic error modeling |
| **High-throughput requirements** | Surrogate models, compressed sensing |
| **Hybrid metrology** | Bayesian data fusion, multi-fidelity modeling |
| **New materials** (2D, high-κ) | First-principles optical property models |

**9.1 Compressed Sensing for Spectroscopic Metrology**

$$
\min_{\mathbf{x}} \|\mathbf{x}\|_1 \quad \text{subject to} \quad \mathbf{A}\mathbf{x} = \mathbf{y}
$$

**Restricted Isometry Property (RIP):**

$$
(1-\delta_s)\|\mathbf{x}\|_2^2 \leq \|\mathbf{A}\mathbf{x}\|_2^2 \leq (1+\delta_s)\|\mathbf{x}\|_2^2
$$

**9.2 Hybrid Metrology Data Fusion**

Combine multiple measurement techniques (OCD + SEM + AFM):

$$
P(\mathbf{p}|\mathbf{y}_1, \mathbf{y}_2, \ldots) \propto P(\mathbf{p}) \prod_i P(\mathbf{y}_i|\mathbf{p})
$$

**Weighted combination (Gaussian case):**

$$
\hat{\mathbf{p}}_{\text{fused}} = \left(\sum_i \mathbf{\Sigma}_i^{-1}\right)^{-1} \sum_i \mathbf{\Sigma}_i^{-1} \hat{\mathbf{p}}_i
$$



**10. Summary: The Mathematical Ecosystem**

```
-
                    ┌─────────────────────────────────────┐
                    │     PHYSICAL FORWARD MODELS         │
                    │  Maxwell, Schrödinger, Monte Carlo  │
                    └──────────────────┬──────────────────┘
                                       │
            ┌──────────────────────────┼──────────────────────────┐
            │                          │                          │
            ▼                          ▼                          ▼
    ┌───────────────┐        ┌─────────────────┐        ┌─────────────────┐
    │   INVERSE     │        │   STATISTICAL   │        │    MACHINE      │
    │   PROBLEMS    │        │   INFERENCE     │        │    LEARNING     │
    │               │        │                 │        │                 │
    │ Optimization  │        │ Bayesian UQ     │        │ Neural networks │
    │ Regulartic    │        │ MCMC            │        │ Surrogates      │
    │ Library search│        │ Information     │        │ PINNs           │
    └───────────────┘        └─────────────────┘        └─────────────────┘
            │                          │                          │
            └──────────────────────────┼──────────────────────────┘
                                       │
                    ┌──────────────────┴──────────────────┐
                    │        PROCESS CONTROL              │
                    │   Run-to-run, APC, SPC, VM          │
                    └─────────────────────────────────────┘
```



**Key Equations**

**A.1 Inverse Problem**

$$\hat{\mathbf{p}} = \arg\min_{\mathbf{p}} \left\| \mathbf{y} - \mathcal{F}(\mathbf{p}) \right\|^2 + \lambda R(\mathbf{p})$$

**A.2 Ellipsometry**

$$\rho = \tan(\Psi)e^{i\Delta}$$

**A.3 XRR Parratt**

$$R_j = \frac{r_{j,j+1} + R_{j+1}e^{2ik_{z,j+1}d_{j+1}}}{1 + r_{j,j+1}R_{j+1}e^{2ik_{z,j+1}d_{j+1}}}$$

**A.4 Fisher Information**

$$[\mathbf{I}]_{ij} = \mathbb{E}\left[\frac{\partial \ln L}{\partial p_i}\frac{\partial \ln L}{\partial p_j}\right]$$

**A.5 Gaussian Process**

$$y(\mathbf{x}) \sim \mathcal{GP}(m(\mathbf{x}), k(\mathbf{x}, \mathbf{x}'))$$

**A.6 EWMA Control**

$$\hat{d}_t = \lambda y_{t-1} + (1-\lambda)\hat{d}_{t-1}$$

**A.7 Bayesian Posterior**

$$P(\mathbf{p}|\mathbf{y}) \propto P(\mathbf{y}|\mathbf{p}) \cdot P(\mathbf{p})$$



**Notation Reference**

| Symbol | Description |
|--------|-------------|
| $\mathbf{y}$ | Measurement vector |
| $\mathbf{p}$ | Parameter vector |
| $\mathcal{F}$ | Forward model operator |
| $\lambda$ | Regularization parameter / wavelength (context-dependent) |
| $n, k$ | Refractive index, extinction coefficient |
| $\Psi, \Delta$ | Ellipsometric angles |
| $\mathbf{I}$ | Fisher information matrix |
| $\sigma$ | Standard deviation / roughness |
| $\mathcal{GP}$ | Gaussian process |

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/metrology) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
