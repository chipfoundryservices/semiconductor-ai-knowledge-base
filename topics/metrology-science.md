# Semiconductor Manufacturing Process Metrology: Science, Mathematics, and Modeling

**Keywords**: metrology science, metrology physics, ellipsometry, scatterometry, OCD metrology, CD-

---

**Semiconductor Manufacturing Process Metrology: Science, Mathematics, and Modeling**

A comprehensive exploration of the physics, mathematics, and computational methods underlying nanoscale measurement in semiconductor fabrication.



**1. The Fundamental Challenge**

Modern semiconductor manufacturing produces structures with critical dimensions of just a few nanometers. At leading-edge nodes (3nm, 2nm), we are measuring features only **10–20 atoms wide**.

**Key Requirements**

- **Sub-angstrom precision** in measurement
- **Complex 3D architectures**: FinFETs, Gate-All-Around (GAA) transistors, 3D NAND (200+ layers)
- **High throughput**: seconds per measurement in production
- **Multi-parameter extraction**: distinguish dozens of correlated parameters

**Metrology Techniques Overview**

| Technique | Principle | Resolution | Throughput |
|-----------|-----------|------------|------------|
| Spectroscopic Ellipsometry (SE) | Polarization change | ~0.1 Å | High |
| Optical CD (OCD/Scatterometry) | Diffraction analysis | ~0.1 nm | High |
| CD-SEM | Electron imaging | ~1 nm | Medium |
| CD-SAXS | X-ray scattering | ~0.1 nm | Low |
| AFM | Probe scanning | ~0.1 nm | Low |
| TEM | Electron transmission | Atomic | Very Low |



**2. Physics Foundation**

**2.1 Maxwell's Equations**

At the heart of optical metrology lies the solution to Maxwell's equations:

$$

abla \times \mathbf{E} = -\frac{\partial \mathbf{B}}{\partial t}
$$

$$

abla \times \mathbf{H} = \mathbf{J} + \frac{\partial \mathbf{D}}{\partial t}
$$

$$

abla \cdot \mathbf{D} = \rho
$$

$$

abla \cdot \mathbf{B} = 0
$$

Where:

- $\mathbf{E}$ = Electric field vector
- $\mathbf{H}$ = Magnetic field vector
- $\mathbf{D}$ = Electric displacement field
- $\mathbf{B}$ = Magnetic flux density
- $\mathbf{J}$ = Current density
- $\rho$ = Charge density

**2.2 Constitutive Relations**

For linear, isotropic media:

$$
\mathbf{D} = \varepsilon_0 \varepsilon_r \mathbf{E} = \varepsilon_0 (1 + \chi_e) \mathbf{E}
$$

$$
\mathbf{B} = \mu_0 \mu_r \mathbf{H}
$$

The complex dielectric function:

$$
\tilde{\varepsilon}(\omega) = \varepsilon_1(\omega) + i\varepsilon_2(\omega) = \tilde{n}^2 = (n + ik)^2
$$

Where:

- $n$ = Refractive index
- $k$ = Extinction coefficient

**2.3 Fresnel Equations**

At an interface between media with refractive indices $\tilde{n}_1$ and $\tilde{n}_2$:

**s-polarization (TE):**

$$
r_s = \frac{n_1 \cos\theta_i - n_2 \cos\theta_t}{n_1 \cos\theta_i + n_2 \cos\theta_t}
$$

$$
t_s = \frac{2 n_1 \cos\theta_i}{n_1 \cos\theta_i + n_2 \cos\theta_t}
$$

**p-polarization (TM):**

$$
r_p = \frac{n_2 \cos\theta_i - n_1 \cos\theta_t}{n_2 \cos\theta_i + n_1 \cos\theta_t}
$$

$$
t_p = \frac{2 n_1 \cos\theta_i}{n_2 \cos\theta_i + n_1 \cos\theta_t}
$$

With Snell's law:

$$
n_1 \sin\theta_i = n_2 \sin\theta_t
$$



**3. Mathematics of Inverse Problems**

**3.1 Problem Formulation**

Metrology is fundamentally an **inverse problem**:

| Problem Type | Description | Well-Posed? |
|--------------|-------------|-------------|
| **Forward** | Structure parameters → Measured signal | Yes |
| **Inverse** | Measured signal → Structure parameters | Often No |

We seek parameters $\mathbf{p}$ that minimize the difference between model $M(\mathbf{p})$ and data $\mathbf{D}$:

$$
\min_{\mathbf{p}} \left\| M(\mathbf{p}) - \mathbf{D} \right\|^2
$$

Or with weighted least squares:

$$
\chi^2 = \sum_{k=1}^{N} \frac{\left( M_k(\mathbf{p}) - D_k \right)^2}{\sigma_k^2}
$$

**3.2 Levenberg-Marquardt Algorithm**

The workhorse optimization algorithm interpolates between gradient descent and Gauss-Newton:

$$
\left( \mathbf{J}^T \mathbf{J} + \lambda \mathbf{I} \right) \delta\mathbf{p} = \mathbf{J}^T \left( \mathbf{D} - M(\mathbf{p}) \right)
$$

Where:

- $\mathbf{J}$ = Jacobian matrix (sensitivity matrix)
- $\lambda$ = Damping parameter
- $\delta\mathbf{p}$ = Parameter update step

The Jacobian elements:

$$
J_{ij} = \frac{\partial M_i}{\partial p_j}
$$

**Algorithm behavior:**

- Large $\lambda$ → Gradient descent (robust, slow)
- Small $\lambda$ → Gauss-Newton (fast near minimum)

**3.3 Regularization Techniques**

For ill-posed problems, regularization is essential:

**Tikhonov Regularization (L2):**

$$
\min_{\mathbf{p}} \left\| M(\mathbf{p}) - \mathbf{D} \right\|^2 + \alpha \left\| \mathbf{p} - \mathbf{p}_0 \right\|^2
$$

**LASSO Regularization (L1):**

$$
\min_{\mathbf{p}} \left\| M(\mathbf{p}) - \mathbf{D} \right\|^2 + \alpha \left\| \mathbf{p} \right\|_1
$$

**Bayesian Inference:**

$$
P(\mathbf{p} | \mathbf{D}) = \frac{P(\mathbf{D} | \mathbf{p}) \cdot P(\mathbf{p})}{P(\mathbf{D})}
$$

Where:

- $P(\mathbf{p} | \mathbf{D})$ = Posterior probability
- $P(\mathbf{D} | \mathbf{p})$ = Likelihood
- $P(\mathbf{p})$ = Prior probability



**4. Thin Film Optics**

**4.1 Ellipsometry Fundamentals**

Ellipsometry measures the change in polarization state upon reflection:

$$
\rho = \tan(\Psi) \cdot e^{i\Delta} = \frac{r_p}{r_s}
$$

Where:

- $\Psi$ = Amplitude ratio angle
- $\Delta$ = Phase difference
- $r_p, r_s$ = Complex reflection coefficients

**4.2 Transfer Matrix Method**

For multilayer stacks, the characteristic matrix for layer $j$:

$$
\mathbf{M}_j = \begin{pmatrix} \cos\delta_j & \frac{i \sin\delta_j}{\eta_j} \\ i\eta_j \sin\delta_j & \cos\delta_j \end{pmatrix}
$$

Where the phase thickness:

$$
\delta_j = \frac{2\pi}{\lambda} \tilde{n}_j d_j \cos\theta_j
$$

And the optical admittance:

$$
\eta_j = \begin{cases} \tilde{n}_j \cos\theta_j & \text{(s-pol)} \\ \frac{\tilde{n}_j}{\cos\theta_j} & \text{(p-pol)} \end{cases}
$$

**Total system matrix:**

$$
\mathbf{M}_{total} = \mathbf{M}_1 \cdot \mathbf{M}_2 \cdot \ldots \cdot \mathbf{M}_N = \begin{pmatrix} m_{11} & m_{12} \\ m_{21} & m_{22} \end{pmatrix}
$$

**Reflection coefficient:**

$$
r = \frac{\eta_0 m_{11} + \eta_0 \eta_s m_{12} - m_{21} - \eta_s m_{22}}{\eta_0 m_{11} + \eta_0 \eta_s m_{12} + m_{21} + \eta_s m_{22}}
$$

**4.3 Dispersion Models**

**Lorentz Oscillator Model:**

$$
\varepsilon(\omega) = \varepsilon_\infty + \sum_j \frac{A_j}{\omega_j^2 - \omega^2 - i\gamma_j \omega}
$$

**Tauc-Lorentz Model (for amorphous semiconductors):**

$$
\varepsilon_2(E) = \begin{cases} \frac{A E_0 C (E - E_g)^2}{(E^2 - E_0^2)^2 + C^2 E^2} \cdot \frac{1}{E} & E > E_g \\ 0 & E \leq E_g \end{cases}
$$

With $\varepsilon_1$ obtained via Kramers-Kronig relations:

$$
\varepsilon_1(E) = \varepsilon_{1,\infty} + \frac{2}{\pi} \mathcal{P} \int_{E_g}^{\infty} \frac{\xi \varepsilon_2(\xi)}{\xi^2 - E^2} d\xi
$$



**5. Scatterometry and RCWA**

**5.1 Rigorous Coupled-Wave Analysis**

For a grating with period $\Lambda$, electromagnetic fields are expanded in Fourier orders:

$$
E(x,z) = \sum_{m=-M}^{M} E_m(z) \exp(i k_{xm} x)
$$

Where the diffracted wave vectors:

$$
k_{xm} = k_{x0} + \frac{2\pi m}{\Lambda} = k_0 \left( n_1 \sin\theta_i + \frac{m\lambda}{\Lambda} \right)
$$

**5.2 Eigenvalue Problem**

In each layer, the field satisfies:

$$
\frac{d^2 \mathbf{E}}{dz^2} = \mathbf{\Omega}^2 \mathbf{E}
$$

Where $\mathbf{\Omega}^2$ is a matrix determined by the Fourier components of the permittivity:

$$
\varepsilon(x) = \sum_n \varepsilon_n \exp\left( i \frac{2\pi n}{\Lambda} x \right)
$$

The eigenvalue decomposition:

$$
\mathbf{\Omega}^2 = \mathbf{W} \mathbf{\Lambda} \mathbf{W}^{-1}
$$

Provides propagation constants (eigenvalues $\lambda_m$) and field profiles (eigenvectors in $\mathbf{W}$).

**5.3 S-Matrix Formulation**

For numerical stability, use the scattering matrix formulation:

$$
\begin{pmatrix} \mathbf{a}_1^- \\ \mathbf{a}_N^+ \end{pmatrix} = \mathbf{S} \begin{pmatrix} \mathbf{a}_1^+ \\ \mathbf{a}_N^- \end{pmatrix}
$$

Where $\mathbf{a}^+$ and $\mathbf{a}^-$ represent forward and backward propagating waves.

The S-matrix is built recursively:

$$
\mathbf{S}_{1 \to j+1} = \mathbf{S}_{1 \to j} \star \mathbf{S}_{j,j+1}
$$

Using the Redheffer star product $\star$.



**6. Statistical Process Control**

**6.1 Control Charts**

**$\bar{X}$ Chart (Mean):**

$$
UCL = \bar{\bar{X}} + A_2 \bar{R}
$$

$$
LCL = \bar{\bar{X}} - A_2 \bar{R}
$$

**R Chart (Range):**

$$
UCL_R = D_4 \bar{R}
$$

$$
LCL_R = D_3 \bar{R}
$$

**EWMA (Exponentially Weighted Moving Average):**

$$
Z_t = \lambda X_t + (1 - \lambda) Z_{t-1}
$$

With control limits:

$$
UCL = \mu_0 + L \sigma \sqrt{\frac{\lambda}{2 - \lambda} \left[ 1 - (1-\lambda)^{2t} \right]}
$$

**6.2 Process Capability Indices**

**$C_p$ (Process Capability):**

$$
C_p = \frac{USL - LSL}{6\sigma}
$$

**$C_{pk}$ (Centered Process Capability):**

$$
C_{pk} = \min \left( \frac{USL - \mu}{3\sigma}, \frac{\mu - LSL}{3\sigma} \right)
$$

**$C_{pm}$ (Taguchi Capability):**

$$
C_{pm} = \frac{USL - LSL}{6\sqrt{\sigma^2 + (\mu - T)^2}}
$$

Where:

- $USL$ = Upper Specification Limit
- $LSL$ = Lower Specification Limit
- $T$ = Target value
- $\mu$ = Process mean
- $\sigma$ = Process standard deviation

**6.3 Gauge R&R Analysis**

Total measurement variance decomposition:

$$
\sigma^2_{total} = \sigma^2_{part} + \sigma^2_{gauge}
$$

$$
\sigma^2_{gauge} = \sigma^2_{repeatability} + \sigma^2_{reproducibility}
$$

**Precision-to-Tolerance Ratio:**

$$
P/T = \frac{6 \sigma_{gauge}}{USL - LSL} \times 100\%
$$

| P/T Ratio | Assessment |
|-----------|------------|
| < 10% | Excellent |
| 10-30% | Acceptable |
| > 30% | Unacceptable |



**7. Uncertainty Quantification**

**7.1 Fisher Information Matrix**

The Fisher Information Matrix for parameter estimation:

$$
F_{ij} = \sum_{k=1}^{N} \frac{1}{\sigma_k^2} \frac{\partial M_k}{\partial p_i} \frac{\partial M_k}{\partial p_j}
$$

Or equivalently:

$$
F_{ij} = -E \left[ \frac{\partial^2 \ln L}{\partial p_i \partial p_j} \right]
$$

Where $L$ is the likelihood function.

**7.2 Cramér-Rao Lower Bound**

The covariance matrix of any unbiased estimator is bounded:

$$
\text{Cov}(\hat{\mathbf{p}}) \geq \mathbf{F}^{-1}
$$

For a single parameter:

$$
\text{Var}(\hat{\theta}) \geq \frac{1}{I(\theta)}
$$

**Interpretation:**

- Diagonal elements of $\mathbf{F}^{-1}$ give minimum variance for each parameter
- Off-diagonal elements indicate parameter correlations
- Large condition number of $\mathbf{F}$ indicates ill-conditioning

**7.3 Correlation Coefficient**

$$
\rho_{ij} = \frac{F^{-1}_{ij}}{\sqrt{F^{-1}_{ii} F^{-1}_{jj}}}
$$

| |$\rho$| | Interpretation |
|--------|----------------|
| < 0.3 | Weak correlation |
| 0.3 – 0.7 | Moderate correlation |
| > 0.7 | Strong correlation |
| > 0.95 | Severe: consider fixing one parameter |

**7.4 GUM Framework**

According to the Guide to the Expression of Uncertainty in Measurement:

**Combined standard uncertainty:**

$$
u_c^2(y) = \sum_{i=1}^{N} \left( \frac{\partial f}{\partial x_i} \right)^2 u^2(x_i) + 2 \sum_{i=1}^{N-1} \sum_{j=i+1}^{N} \frac{\partial f}{\partial x_i} \frac{\partial f}{\partial x_j} u(x_i, x_j)
$$

**Expanded uncertainty:**

$$
U = k \cdot u_c(y)
$$

Where $k$ is the coverage factor (typically $k=2$ for 95% confidence).



**8. Machine Learning in Metrology**

**8.1 Neural Network Surrogate Models**

Replace expensive physics simulations with trained neural networks:

$$
M_{NN}(\mathbf{p}; \mathbf{W}) \approx M_{physics}(\mathbf{p})
$$

**Training objective:**

$$
\mathcal{L} = \frac{1}{N} \sum_{i=1}^{N} \left\| M_{NN}(\mathbf{p}_i) - M_{physics}(\mathbf{p}_i) \right\|^2 + \lambda \left\| \mathbf{W} \right\|^2
$$

**Speedup:** Typically $10^4$ – $10^6 \times$ faster than RCWA/FEM.

**8.2 Physics-Informed Neural Networks (PINNs)**

Incorporate physical laws into the loss function:

$$
\mathcal{L}_{total} = \mathcal{L}_{data} + \lambda_{physics} \mathcal{L}_{physics}
$$

Where:

$$
\mathcal{L}_{physics} = \left\| 
abla \times \mathbf{E} + \frac{\partial \mathbf{B}}{\partial t} \right\|^2 + \ldots
$$

**8.3 Gaussian Process Regression**

A non-parametric Bayesian approach:

$$
f(\mathbf{x}) \sim \mathcal{GP}\left( m(\mathbf{x}), k(\mathbf{x}, \mathbf{x}') \right)
$$

**Common kernel (RBF/Squared Exponential):**

$$
k(\mathbf{x}, \mathbf{x}') = \sigma_f^2 \exp\left( -\frac{\left\| \mathbf{x} - \mathbf{x}' \right\|^2}{2\ell^2} \right)
$$

**Posterior prediction:**

$$
\mu_* = \mathbf{k}_*^T (\mathbf{K} + \sigma_n^2 \mathbf{I})^{-1} \mathbf{y}
$$

$$
\sigma_*^2 = k_{**} - \mathbf{k}_*^T (\mathbf{K} + \sigma_n^2 \mathbf{I})^{-1} \mathbf{k}_*
$$

**Advantages:**

- Provides uncertainty estimates naturally
- Works well with limited training data
- Interpretable hyperparameters

**8.4 Virtual Metrology**

Predict wafer properties from equipment sensor data:

$$
\hat{y} = f(FDC_1, FDC_2, \ldots, FDC_n)
$$

Where $FDC_i$ are Fault Detection and Classification sensor readings.

**Common approaches:**

- Partial Least Squares (PLS) regression
- Random Forests
- Gradient Boosting (XGBoost, LightGBM)
- Deep neural networks



**9. Advanced Topics and Frontiers**

**9.1 3D Metrology Challenges**

Modern structures require 3D measurement:

| Structure | Complexity | Key Challenge |
|-----------|------------|---------------|
| FinFET | Moderate | Fin height, sidewall angle |
| GAA/Nanosheet | High | Sheet thickness, spacing |
| 3D NAND | Very High | 200+ layers, bowing, tilt |
| DRAM HAR | Extreme | 100:1 aspect ratio structures |

**9.2 Hybrid Metrology**

Combining multiple techniques to break parameter correlations:

$$
\chi^2_{total} = \sum_{techniques} w_t \chi^2_t
$$

**Example combination:**

- OCD for periodic structure parameters
- Ellipsometry for film optical constants
- XRR for density and interface roughness

**Mathematical framework:**

$$
\mathbf{F}_{hybrid} = \sum_t \mathbf{F}_t
$$

Reduces off-diagonal elements, improving condition number.

**9.3 Atomic-Scale Considerations**

At the 2nm node and beyond:

**Line Edge Roughness (LER):**

$$
\sigma_{LER} = \sqrt{\frac{1}{L} \int_0^L \left[ x(z) - \bar{x} \right]^2 dz}
$$

**Power Spectral Density:**

$$
PSD(f) = \frac{\sigma^2 \xi}{1 + (2\pi f \xi)^{2(1+H)}}
$$

Where:

- $\xi$ = Correlation length
- $H$ = Hurst exponent (roughness character)

**Quantum Effects:**

- Tunneling through thin barriers
- Discrete dopant effects
- Wave function penetration

**9.4 Model-Measurement Circularity**

A fundamental epistemological challenge:

```
-
┌──────────────┐      ┌──────────────┐
│   Physical   │ ───► │   Measured   │
│   Structure  │      │    Signal    │
└──────────────┘      └──────────────┘
       ▲                     │
       │                     ▼
       │              ┌──────────────┐
       │              │    Model     │
       └────────────◄─┤   Inversion  │
                      └──────────────┘
```

**Key questions:**

- How do we validate models when "truth" requires modeling?
- Reference metrology (TEM) also requires interpretation
- What does it mean to "know" a dimension at atomic scale?



**Key Symbols and Notation**

| Symbol | Description | Units |
|--------|-------------|-------|
| $\lambda$ | Wavelength | nm |
| $\theta$ | Angle of incidence | degrees |
| $n$ | Refractive index | dimensionless |
| $k$ | Extinction coefficient | dimensionless |
| $d$ | Film thickness | nm |
| $\Lambda$ | Grating period | nm |
| $\Psi, \Delta$ | Ellipsometric angles | degrees |
| $\sigma$ | Standard deviation | varies |
| $\mathbf{J}$ | Jacobian matrix | varies |
| $\mathbf{F}$ | Fisher Information Matrix | varies |



**Computational Complexity**

| Method | Complexity | Typical Time |
|--------|------------|--------------|
| Transfer Matrix | $O(N)$ | $\mu$s |
| RCWA | $O(M^3 \cdot L)$ | ms – s |
| FEM | $O(N^{1.5})$ | s – min |
| FDTD | $O(N \cdot T)$ | s – min |
| Monte Carlo (SEM) | $O(N_{electrons})$ | min – hr |
| Neural Network (inference) | $O(1)$ | $\mu$s |

Where:

- $N$ = Number of layers / mesh elements
- $M$ = Number of Fourier orders
- $L$ = Number of layers
- $T$ = Number of time steps

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/metrology-science) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
