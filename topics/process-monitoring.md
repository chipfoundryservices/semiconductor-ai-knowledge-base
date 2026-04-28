# Semiconductor Manufacturing Process Parameters Monitoring: Mathematical Modeling

**Keywords**: process monitoring, semiconductor process control, spc, statistical process control, sensor data, fault detection, run-to-run control, process optimization

---

**Semiconductor Manufacturing Process Parameters Monitoring: Mathematical Modeling**

**1. The Fundamental Challenge**

Modern semiconductor fabrication involves 500–1000+ sequential process steps, each with dozens of parameters requiring nanometer-scale precision.

**Key Process Types and Parameters**

- **Lithography**: exposure dose, focus, overlay alignment, resist thickness
- **Etching (dry/wet)**: etch rate, selectivity, uniformity, plasma parameters (power, pressure, gas flows)
- **Deposition (CVD, PVD, ALD)**: deposition rate, film thickness, uniformity, stress, composition
- **CMP (Chemical Mechanical Polishing)**: removal rate, within-wafer non-uniformity, dishing, erosion
- **Implantation**: dose, energy, angle, uniformity
- **Thermal processes**: temperature uniformity, ramp rates, time



**2. Statistical Process Control (SPC) — The Foundation**

**2.1 Univariate Control Charts**

For a process parameter $X$ with samples $x_1, x_2, \ldots, x_n$:

**Sample Mean:**

$$
\bar{x} = \frac{1}{n}\sum_{i=1}^{n} x_i
$$

**Sample Standard Deviation:**

$$
\sigma = \sqrt{\frac{1}{n-1}\sum_{i=1}^{n}(x_i - \bar{x})^2}
$$

**Control Limits (3-sigma):**

$$
\text{UCL} = \bar{x} + 3\sigma
$$

$$
\text{LCL} = \bar{x} - 3\sigma
$$

**2.2 Process Capability Indices**

These quantify how well a process meets specifications:

- **$C_p$ (Potential Capability):**

$$
C_p = \frac{USL - LSL}{6\sigma}
$$

- **$C_{pk}$ (Actual Capability)** — accounts for centering:

$$
C_{pk} = \min\left[\frac{USL - \mu}{3\sigma}, \frac{\mu - LSL}{3\sigma}\right]
$$

- **$C_{pm}$ (Taguchi Index)** — penalizes deviation from target $T$:

$$
C_{pm} = \frac{C_p}{\sqrt{1 + \left(\frac{\mu - T}{\sigma}\right)^2}}
$$

Semiconductor fabs typically require $C_{pk} \geq 1.67$, corresponding to defect rates below ~1 ppm.



**3. Multivariate Statistical Monitoring**

Since process parameters are highly correlated, univariate methods miss interaction effects.

**3.1 Principal Component Analysis (PCA)**

Given data matrix $\mathbf{X}$ ($n$ samples × $p$ variables), centered:

1. **Compute covariance matrix:**

$$
\mathbf{S} = \frac{1}{n-1}\mathbf{X}^T\mathbf{X}
$$

2. **Eigendecomposition:**

$$
\mathbf{S} = \mathbf{V}\mathbf{\Lambda}\mathbf{V}^T
$$

3. **Project to principal components:**

$$
\mathbf{T} = \mathbf{X}\mathbf{V}
$$

**3.2 Monitoring Statistics**

**Hotelling's $T^2$ Statistic**

Captures variation **within** the PCA model:

$$
T^2 = \sum_{i=1}^{k} \frac{t_i^2}{\lambda_i}
$$

where $k$ is the number of retained components. Under normal operation, $T^2$ follows a scaled F-distribution.

**Q-Statistic (Squared Prediction Error)**

Captures variation **outside** the model:

$$
Q = \sum_{j=1}^{p}(x_j - \hat{x}_j)^2 = \|\mathbf{x} - \mathbf{x}\mathbf{V}_k\mathbf{V}_k^T\|^2
$$

> Often more sensitive to novel faults than $T^2$.

**3.3 Partial Least Squares (PLS)**

When relating process inputs $\mathbf{X}$ to quality outputs $\mathbf{Y}$:

$$
\mathbf{Y} = \mathbf{X}\mathbf{B} + \mathbf{E}
$$

PLS finds latent variables that maximize covariance between $\mathbf{X}$ and $\mathbf{Y}$, providing both monitoring capability and a predictive model.



**4. Virtual Metrology (VM) Models**

Virtual metrology predicts physical measurement outcomes from process sensor data, enabling 100% wafer coverage without costly measurements.

**4.1 Linear Models**

For process parameters $\mathbf{x} \in \mathbb{R}^p$ and metrology target $y$:

- **Ordinary Least Squares (OLS):**

$$
\hat{\boldsymbol{\beta}} = (\mathbf{X}^T\mathbf{X})^{-1}\mathbf{X}^T\mathbf{y}
$$

- **Ridge Regression** ($L_2$ regularization for collinearity):

$$
\hat{\boldsymbol{\beta}} = (\mathbf{X}^T\mathbf{X} + \lambda\mathbf{I})^{-1}\mathbf{X}^T\mathbf{y}
$$

- **LASSO** ($L_1$ regularization for sparsity/feature selection):

$$
\min_{\boldsymbol{\beta}} \|\mathbf{y} - \mathbf{X}\boldsymbol{\beta}\|^2 + \lambda\|\boldsymbol{\beta}\|_1
$$

**4.2 Nonlinear Models**

**Gaussian Process Regression (GPR)**

$$
y \sim \mathcal{GP}(m(\mathbf{x}), k(\mathbf{x}, \mathbf{x}'))
$$

**Posterior predictive distribution:**

- **Mean:**

$$
\mu_* = \mathbf{K}_*^T(\mathbf{K} + \sigma_n^2\mathbf{I})^{-1}\mathbf{y}
$$

- **Variance:**

$$
\sigma_*^2 = K_{**} - \mathbf{K}_*^T(\mathbf{K} + \sigma_n^2\mathbf{I})^{-1}\mathbf{K}_*
$$

GPs provide uncertainty quantification — critical for knowing when to trigger actual metrology.

**Support Vector Regression (SVR)**

$$
\min \frac{1}{2}\|\mathbf{w}\|^2 + C\sum_i(\xi_i + \xi_i^*)
$$

Subject to $\epsilon$-insensitive tube constraints. Kernel trick enables nonlinear modeling.

**Neural Networks**

- **MLPs**: Multi-layer perceptrons for general function approximation
- **CNNs**: Convolutional neural networks for wafer map pattern recognition
- **LSTMs**: Long Short-Term Memory networks for time-series FDC traces



**5. Run-to-Run (R2R) Control**

R2R control adjusts recipe setpoints between wafers/lots to compensate for drift and disturbances.

**5.1 EWMA Controller**

For a process with model $y = a_0 + a_1 u + \epsilon$:

**Prediction update:**

$$
\hat{y}_{k+1} = \lambda y_k + (1-\lambda)\hat{y}_k
$$

**Control action:**

$$
u_{k+1} = \frac{T - \hat{y}_{k+1} + a_0}{a_1}
$$

where:
- $T$ is the target
- $\lambda \in (0,1)$ is the smoothing weight

**5.2 Double EWMA (for Linear Drift)**

When process drifts linearly:

$$
\hat{y}_{k+1} = a_k + b_k
$$

$$
a_k = \lambda y_k + (1-\lambda)(a_{k-1} + b_{k-1})
$$

$$
b_k = \gamma(a_k - a_{k-1}) + (1-\gamma)b_{k-1}
$$

**5.3 State-Space Formulation**

More general framework:

**State equation:**

$$
\mathbf{x}_{k+1} = \mathbf{A}\mathbf{x}_k + \mathbf{B}\mathbf{u}_k + \mathbf{w}_k
$$

**Observation equation:**

$$
\mathbf{y}_k = \mathbf{C}\mathbf{x}_k + \mathbf{D}\mathbf{u}_k + \mathbf{v}_k
$$

Use **Kalman filtering** for state estimation and **LQR/MPC** for optimal control.

**5.4 Model Predictive Control (MPC)**

**Objective function:**

$$
\min \sum_{i=1}^{N} \|\mathbf{y}_{k+i} - \mathbf{r}_{k+i}\|_\mathbf{Q}^2 + \sum_{j=0}^{N-1}\|\Delta\mathbf{u}_{k+j}\|_\mathbf{R}^2
$$

subject to process model and operational constraints.

> MPC handles multivariable systems with constraints naturally.



**6. Fault Detection and Classification (FDC)**

**6.1 Detection Methods**

**Mahalanobis Distance**

$$
D^2 = (\mathbf{x} - \boldsymbol{\mu})^T\mathbf{S}^{-1}(\mathbf{x} - \boldsymbol{\mu})
$$

Follows $\chi^2$ distribution under multivariate normality.

**Other Detection Methods**

- **One-Class SVM**: Learn boundary of normal operation
- **Autoencoders**: Detect anomalies via reconstruction error

**6.2 Classification Features**

For trace data (time-series from sensors), extract features:

- **Statistical moments**: mean, variance, skewness, kurtosis
- **Frequency domain**: FFT coefficients, spectral power
- **Wavelet coefficients**: Multi-resolution analysis
- **DTW distances**: Dynamic Time Warping to reference signatures

**6.3 Classification Algorithms**

- Support Vector Machines (SVM)
- Random Forest
- CNNs for pattern recognition on wafer maps
- Gradient Boosting (XGBoost, LightGBM)



**7. Spatial Modeling (Within-Wafer Variation)**

Systematic spatial patterns require explicit modeling.

**7.1 Polynomial Basis Expansion**

**Zernike Polynomials (common in lithography)**

$$
z(\rho, \theta) = \sum_{n,m} Z_n^m(\rho, \theta)
$$

These form an orthogonal basis on the unit disk, capturing radial and azimuthal variation.

**7.2 Gaussian Process Spatial Models**

$$
y(\mathbf{s}) \sim \mathcal{GP}(\mu(\mathbf{s}), k(\mathbf{s}, \mathbf{s}'))
$$

**Common Covariance Kernels**

- **Squared Exponential (RBF):**

$$
k(\mathbf{s}, \mathbf{s}') = \sigma^2 \exp\left(-\frac{\|\mathbf{s} - \mathbf{s}'\|^2}{2\ell^2}\right)
$$

- **Matérn** (more flexible smoothness):

$$
k(r) = \sigma^2 \frac{2^{1-
u}}{\Gamma(
u)}\left(\frac{\sqrt{2
u}r}{\ell}\right)^
u K_
u\left(\frac{\sqrt{2
u}r}{\ell}\right)
$$

where $K_
u$ is the modified Bessel function of the second kind.



**8. Dynamic/Time-Series Modeling**

For plasma processes, endpoint detection, and transient behavior.

**8.1 Autoregressive Models**

**AR(p) model:**

$$
x_t = \sum_{i=1}^{p} \phi_i x_{t-i} + \epsilon_t
$$

ARIMA extends this to non-stationary series.

**8.2 Dynamic PCA**

Augment data with time-lagged values:

$$
\tilde{\mathbf{X}} = [\mathbf{X}(t), \mathbf{X}(t-1), \ldots, \mathbf{X}(t-l)]
$$

Then apply standard PCA to capture temporal dynamics.

**8.3 Deep Sequence Models**

**LSTM Networks**

Gating mechanisms:

- **Forget gate:** $f_t = \sigma(W_f \cdot [h_{t-1}, x_t] + b_f)$
- **Input gate:** $i_t = \sigma(W_i \cdot [h_{t-1}, x_t] + b_i)$
- **Output gate:** $o_t = \sigma(W_o \cdot [h_{t-1}, x_t] + b_o)$

**Cell state update:**

$$
c_t = f_t \odot c_{t-1} + i_t \odot \tilde{c}_t
$$

**Hidden state:**

$$
h_t = o_t \odot \tanh(c_t)
$$



**9. Model Maintenance and Adaptation**

Semiconductor processes drift — models must adapt.

**9.1 Drift Detection Methods**

**CUSUM (Cumulative Sum)**

$$
S_k = \max(0, S_{k-1} + (x_k - \mu_0) - k)
$$

Signal when $S_k$ exceeds threshold.

**Page-Hinkley Test**

$$
m_k = \sum_{i=1}^{k}(x_i - \bar{x}_k - \delta)
$$

$$
M_k = \max_{i \leq k} m_i
$$

Alarm when $M_k - m_k > \lambda$.

**ADWIN (Adaptive Windowing)**

Automatically detects distribution changes and adjusts window size.

**9.2 Online Model Updating**

**Recursive Least Squares (RLS)**

$$
\hat{\boldsymbol{\beta}}_k = \hat{\boldsymbol{\beta}}_{k-1} + \mathbf{K}_k(y_k - \mathbf{x}_k^T\hat{\boldsymbol{\beta}}_{k-1})
$$

where $\mathbf{K}_k$ is the gain matrix updated via the Riccati equation:

$$
\mathbf{K}_k = \frac{\mathbf{P}_{k-1}\mathbf{x}_k}{\lambda + \mathbf{x}_k^T\mathbf{P}_{k-1}\mathbf{x}_k}
$$

$$
\mathbf{P}_k = \frac{1}{\lambda}(\mathbf{P}_{k-1} - \mathbf{K}_k\mathbf{x}_k^T\mathbf{P}_{k-1})
$$

**Just-in-Time (JIT) Learning**

Build local models around each new prediction point using nearest historical samples.



**10. Integrated Framework**

A complete monitoring system layers these methods:

| Layer | Methods | Purpose |
|-------|---------|---------|
| **Preprocessing** | Cleaning, synchronization, normalization | Data quality |
| **Feature Engineering** | Domain features, wavelets, PCA | Dimensionality management |
| **Monitoring** | $T^2$, Q-statistic, control charts | Detect out-of-control states |
| **Virtual Metrology** | PLS, GPR, neural networks | Predict quality without measurement |
| **FDC** | Classification models | Diagnose fault root causes |
| **Control** | R2R, MPC | Compensate for drift/disturbances |
| **Adaptation** | Online learning, drift detection | Maintain model validity |



**11. Key Mathematical Challenges**

1. **High dimensionality** — hundreds of sensors, requiring regularization and dimension reduction
2. **Collinearity** — process variables are physically coupled
3. **Non-stationarity** — drift, maintenance events, recipe changes
4. **Small sample sizes** — new recipes have limited historical data (transfer learning, Bayesian methods help)
5. **Real-time constraints** — decisions needed in seconds
6. **Rare events** — faults are infrequent, creating class imbalance



**12. Key Equations**

**Process Capability**

$$
C_{pk} = \min\left[\frac{USL - \mu}{3\sigma}, \frac{\mu - LSL}{3\sigma}\right]
$$

**Multivariate Monitoring**

$$
T^2 = \sum_{i=1}^{k} \frac{t_i^2}{\lambda_i}, \quad Q = \|\mathbf{x} - \hat{\mathbf{x}}\|^2
$$

**Virtual Metrology (Ridge Regression)**

$$
\hat{\boldsymbol{\beta}} = (\mathbf{X}^T\mathbf{X} + \lambda\mathbf{I})^{-1}\mathbf{X}^T\mathbf{y}
$$

**EWMA Control**

$$
\hat{y}_{k+1} = \lambda y_k + (1-\lambda)\hat{y}_k
$$

**Mahalanobis Distance**

$$
D^2 = (\mathbf{x} - \boldsymbol{\mu})^T\mathbf{S}^{-1}(\mathbf{x} - \boldsymbol{\mu})
$$

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/process-monitoring) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
