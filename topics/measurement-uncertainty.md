# Semiconductor Manufacturing Process Measurement Uncertainty: Mathematical Modeling

**Keywords**: measurement uncertainty, metrology, GUM, type A uncertainty, type B uncertainty, uncertainty propagation

---

**Semiconductor Manufacturing Process Measurement Uncertainty: Mathematical Modeling**


**1. The Fundamental Challenge**

At modern nodes (3nm, 2nm), we face a profound problem: **measurement uncertainty can consume 30–50% of the tolerance budget**.

Consider typical values:

- Feature dimension: ~15nm
- Tolerance: ±1nm (≈7% variation allowed)
- Measurement repeatability: ~0.3–0.5nm
- Reproducibility (tool-to-tool): additional 0.3–0.5nm

This means we cannot naively interpret measured variation as process variation—a significant portion is measurement noise.



**2. Variance Decomposition Framework**

The foundational mathematical structure is the decomposition of total observed variance:

$$
\sigma^2_{\text{observed}} = \sigma^2_{\text{process}} + \sigma^2_{\text{measurement}}
$$

**2.1 Hierarchical Decomposition**

For a full fab model:

$$
Y_{ijklm} = \mu + L_i + W_{j(i)} + D_{k(ij)} + T_l + (LT)_{il} + \eta_{lm} + \epsilon_{ijklm}
$$

Where:

| Term | Meaning | Type |
|------|---------|------|
| $L_i$ | Lot effect | Random |
| $W_{j(i)}$ | Wafer nested in lot | Random |
| $D_{k(ij)}$ | Die/site within wafer | Random or systematic |
| $T_l$ | Measurement tool | Random or fixed |
| $(LT)_{il}$ | Lot × tool interaction | Random |
| $\eta_{lm}$ | Tool drift/bias | Systematic |
| $\epsilon_{ijklm}$ | Pure repeatability | Random |

The variance components:

$$
\text{Var}(Y) = \sigma^2_L + \sigma^2_W + \sigma^2_D + \sigma^2_T + \sigma^2_{LT} + \sigma^2_\eta + \sigma^2_\epsilon
$$

**Measurement system variance:**

$$
\sigma^2_{\text{meas}} = \sigma^2_T + \sigma^2_\eta + \sigma^2_\epsilon
$$



**3. Gauge R&R Mathematics**

The standard Gauge Repeatability and Reproducibility analysis partitions measurement variance:

$$
\sigma^2_{\text{meas}} = \sigma^2_{\text{repeatability}} + \sigma^2_{\text{reproducibility}}
$$

**3.1 Key Metrics**

**Precision-to-Tolerance Ratio:**

$$
\text{P/T} = \frac{k \cdot \sigma_{\text{meas}}}{\text{USL} - \text{LSL}}
$$

where $k = 5.15$ (99% coverage) or $k = 6$ (99.73% coverage)

**Discrimination Ratio:**

$$
\text{ndc} = 1.41 \times \frac{\sigma_{\text{process}}}{\sigma_{\text{meas}}}
$$

This gives the number of distinct categories the measurement system can reliably distinguish.

- Industry standard requires: $\text{ndc} \geq 5$

**Signal-to-Noise Ratio:**

$$
\text{SNR} = \frac{\sigma_{\text{process}}}{\sigma_{\text{meas}}}
$$



**4. GUM-Based Uncertainty Propagation**

Following the Guide to the Expression of Uncertainty in Measurement (GUM):

**4.1 Combined Standard Uncertainty**

For a measurand $y = f(x_1, x_2, \ldots, x_n)$:

$$
u_c(y) = \sqrt{\sum_{i=1}^{n} \left(\frac{\partial f}{\partial x_i}\right)^2 u^2(x_i) + 2\sum_{i=1}^{n-1}\sum_{j=i+1}^{n} \frac{\partial f}{\partial x_i}\frac{\partial f}{\partial x_j} u(x_i, x_j)}
$$

**4.2 Type A vs. Type B Uncertainties**

**Type A** (statistical):

$$
u_A(\bar{x}) = \frac{s}{\sqrt{n}} = \sqrt{\frac{1}{n(n-1)}\sum_{i=1}^{n}(x_i - \bar{x})^2}
$$

**Type B** (other sources):

- Calibration certificates: $u_B = \frac{U}{k}$ where $U$ is expanded uncertainty
- Rectangular distribution (tolerance): $u_B = \frac{a}{\sqrt{3}}$
- Triangular distribution: $u_B = \frac{a}{\sqrt{6}}$



**5. Spatial Modeling of Within-Wafer Variation**

Within-wafer variation often has systematic spatial structure that must be separated from random measurement error.

**5.1 Polynomial Surface Model (Zernike Polynomials)**

$$
z(r, \theta) = \sum_{n=0}^{N}\sum_{m=-n}^{n} a_{nm} Z_n^m(r, \theta)
$$

Using Zernike polynomials—natural for circular wafer geometry:

- $Z_0^0$: piston (mean)
- $Z_1^1$: tilt
- $Z_2^0$: defocus (bowl shape)
- Higher orders: astigmatism, coma, spherical aberration analogs

**5.2 Gaussian Process Model**

For flexible, non-parametric spatial modeling:

$$
z(\mathbf{s}) \sim \mathcal{GP}(m(\mathbf{s}), k(\mathbf{s}, \mathbf{s}'))
$$

With squared exponential covariance:

$$
k(\mathbf{s}_i, \mathbf{s}_j) = \sigma^2_f \exp\left(-\frac{\|\mathbf{s}_i - \mathbf{s}_j\|^2}{2\ell^2}\right) + \sigma^2_n \delta_{ij}
$$

Where:

- $\sigma^2_f$: process variance (spatial signal)
- $\ell$: length scale (spatial correlation distance)
- $\sigma^2_n$: measurement noise (nugget effect)

**This naturally separates spatial process variation from measurement noise.**



**6. Bayesian Hierarchical Modeling**

Bayesian approaches provide natural uncertainty quantification and handle small samples common in expensive semiconductor metrology.

**6.1 Basic Hierarchical Model**

**Level 1** (within-wafer measurements):

$$
y_{ij} \mid \theta_i, \sigma^2_{\text{meas}} \sim \mathcal{N}(\theta_i, \sigma^2_{\text{meas}})
$$

**Level 2** (wafer-to-wafer variation):

$$
\theta_i \mid \mu, \sigma^2_{\text{proc}} \sim \mathcal{N}(\mu, \sigma^2_{\text{proc}})
$$

**Level 3** (hyperpriors):

$$
\begin{aligned}
\mu &\sim \mathcal{N}(\mu_0, \tau^2_0) \\
\sigma^2_{\text{meas}} &\sim \text{Inv-Gamma}(\alpha_m, \beta_m) \\
\sigma^2_{\text{proc}} &\sim \text{Inv-Gamma}(\alpha_p, \beta_p)
\end{aligned}
$$

**6.2 Posterior Inference**

The posterior distribution:

$$
p(\mu, \sigma^2_{\text{proc}}, \sigma^2_{\text{meas}} \mid \mathbf{y}) \propto p(\mathbf{y} \mid \boldsymbol{\theta}, \sigma^2_{\text{meas}}) \cdot p(\boldsymbol{\theta} \mid \mu, \sigma^2_{\text{proc}}) \cdot p(\mu, \sigma^2_{\text{proc}}, \sigma^2_{\text{meas}})
$$

Solved via MCMC methods:

- Gibbs sampling
- Hamiltonian Monte Carlo (HMC)
- No-U-Turn Sampler (NUTS)



**7. Monte Carlo Uncertainty Propagation**

For complex, non-linear measurement models where analytical propagation fails:

**7.1 Algorithm (GUM Supplement 1)**

1. **Define** probability distributions for all input quantities $X_i$
2. **Sample** $M$ realizations: $\{x_1^{(k)}, x_2^{(k)}, \ldots, x_n^{(k)}\}$ for $k = 1, \ldots, M$
3. **Propagate** each sample: $y^{(k)} = f(x_1^{(k)}, \ldots, x_n^{(k)})$
4. **Analyze** output distribution to obtain uncertainty

Typically $M \geq 10^6$ for reliable coverage interval estimation.

**7.2 Application: OCD (Optical CD) Metrology**

Scatterometry fits measured spectra to electromagnetic models with parameters:

- CD (critical dimension)
- Sidewall angle
- Height
- Layer thicknesses
- Optical constants

The measurement equation is highly non-linear:

$$
\mathbf{R}_{\text{meas}} = \mathbf{R}_{\text{model}}(\text{CD}, \theta_{\text{swa}}, h, \mathbf{t}, \mathbf{n}, \mathbf{k}) + \boldsymbol{\epsilon}
$$

Monte Carlo propagation captures correlations and non-linearities that linearized GUM misses.


**8. The Deconvolution Problem**

Given observed data that is a convolution of true process variation and measurement noise:

$$
f_{\text{obs}}(x) = (f_{\text{true}} * f_{\text{meas}})(x) = \int f_{\text{true}}(t) \cdot f_{\text{meas}}(x-t) \, dt
$$

**Goal:** Recover $f_{\text{true}}$ given $f_{\text{obs}}$ and knowledge of $f_{\text{meas}}$.

**8.1 Fourier Approach**

In frequency domain:

$$
\hat{f}_{\text{obs}}(\omega) = \hat{f}_{\text{true}}(\omega) \cdot \hat{f}_{\text{meas}}(\omega)
$$

Naively:

$$
\hat{f}_{\text{true}}(\omega) = \frac{\hat{f}_{\text{obs}}(\omega)}{\hat{f}_{\text{meas}}(\omega)}
$$

**Problem:** Ill-posed—small errors in $\hat{f}_{\text{obs}}$ amplified where $\hat{f}_{\text{meas}}$ is small.

**8.2 Regularization Techniques**

**Tikhonov regularization:**

$$
\hat{f}_{\text{true}} = \arg\min_f \left\{ \|f_{\text{obs}} - f * f_{\text{meas}}\|^2 + \lambda \|Lf\|^2 \right\}
$$

**Bayesian approach:**

$$
p(f_{\text{true}} \mid f_{\text{obs}}) \propto p(f_{\text{obs}} \mid f_{\text{true}}) \cdot p(f_{\text{true}})
$$

With appropriate priors (smoothness, non-negativity) to regularize the solution.



**9. Virtual Metrology with Uncertainty Quantification**

Virtual metrology predicts measurements from process tool data, reducing physical sampling requirements.

**9.1 Model Structure**

$$
\hat{y} = f(\mathbf{x}_{\text{FDC}}) + \epsilon
$$

Where $\mathbf{x}_{\text{FDC}}$ = fault detection and classification data (temperatures, pressures, flows, RF power, etc.)

**9.2 Uncertainty-Aware ML Approaches**

**Gaussian Process Regression:**

Provides natural predictive uncertainty:

$$
p(y^* \mid \mathbf{x}^*, \mathcal{D}) = \mathcal{N}(\mu^*, \sigma^{*2})
$$

$$
\mu^* = \mathbf{k}^{*T}(\mathbf{K} + \sigma^2_n\mathbf{I})^{-1}\mathbf{y}
$$

$$
\sigma^{*2} = k(\mathbf{x}^*, \mathbf{x}^*) - \mathbf{k}^{*T}(\mathbf{K} + \sigma^2_n\mathbf{I})^{-1}\mathbf{k}^*
$$

**Conformal Prediction:**

Distribution-free prediction intervals:

$$
\hat{C}(x) = \left[\hat{y}(x) - \hat{q}, \hat{y}(x) + \hat{q}\right]
$$

Where $\hat{q}$ is calibrated on held-out data to guarantee coverage probability.



**10. Control Chart Implications**

Measurement uncertainty affects statistical process control profoundly.

**10.1 Inflated Control Limits**

Standard control chart limits:

$$
\text{UCL} = \bar{\bar{x}} + 3\sigma_{\bar{x}}
$$

But $\sigma_{\bar{x}}$ includes measurement variance:

$$
\sigma^2_{\bar{x}} = \frac{\sigma^2_{\text{proc}} + \sigma^2_{\text{meas}}/n_{\text{rep}}}{n_{\text{sample}}}
$$

**10.2 Adjusted Process Capability**

True process capability:

$$
\hat{C}_p = \frac{\text{USL} - \text{LSL}}{6\hat{\sigma}_{\text{proc}}}
$$

Must correct observed variance:

$$
\hat{\sigma}^2_{\text{proc}} = \hat{\sigma}^2_{\text{obs}} - \hat{\sigma}^2_{\text{meas}}
$$

> **Warning:** This can yield negative estimates if measurement variance dominates—indicating the measurement system is inadequate.



**11. Multi-Tool Matching and Reference Frame**

**11.1 Tool-to-Tool Bias Model**

$$
y_{\text{tool}_k} = y_{\text{true}} + \beta_k + \epsilon_k
$$

Where $\beta_k$ is systematic bias for tool $k$.

**11.2 Mixed-Effects Formulation**

$$
Y_{ij} = \mu + \tau_i + t_j + \epsilon_{ij}
$$

- $\tau_i$: true sample value (random)
- $t_j$: tool effect (random or fixed)
- $\epsilon_{ij}$: residual

**REML (Restricted Maximum Likelihood)** estimation separates these components.

**11.3 Traceability Chain**

$$
\text{SI unit} \xrightarrow{u_1} \text{NMI reference} \xrightarrow{u_2} \text{Fab golden tool} \xrightarrow{u_3} \text{Production tools}
$$

Total reference uncertainty:

$$
u_{\text{ref}} = \sqrt{u_1^2 + u_2^2 + u_3^2}
$$



**12. Practical Uncertainty Budget Example**

For CD-SEM measurement of a 20nm line:

| Source | Type | $u_i$ (nm) | Sensitivity | Contribution (nm²) |
|--------|------|-----------|-------------|-------------------|
| Repeatability | A | 0.25 | 1 | 0.0625 |
| Tool matching | B | 0.30 | 1 | 0.0900 |
| SEM calibration | B | 0.15 | 1 | 0.0225 |
| Algorithm uncertainty | B | 0.20 | 1 | 0.0400 |
| Edge definition model | B | 0.35 | 1 | 0.1225 |
| Charging effects | B | 0.10 | 1 | 0.0100 |

**Combined standard uncertainty:**

$$
u_c = \sqrt{\sum u_i^2} = \sqrt{0.3475} \approx 0.59 \text{ nm}
$$

**Expanded uncertainty** ($k=2$, 95% confidence):

$$
U = k \cdot u_c = 2 \times 0.59 = 1.18 \text{ nm}
$$

For a ±1nm tolerance, this means **P/T ≈ 60%**—marginally acceptable.



**13. Key Takeaways**

The mathematical modeling of measurement uncertainty in semiconductor manufacturing requires:

1. **Hierarchical variance decomposition** (ANOVA, mixed models) to separate process from measurement variation

2. **Spatial statistics** (Gaussian processes, Zernike decomposition) for within-wafer systematic patterns

3. **Bayesian inference** for rigorous uncertainty quantification with limited samples

4. **Monte Carlo methods** for non-linear measurement models (OCD, model-based metrology)

5. **Deconvolution techniques** to recover true process distributions

6. **Machine learning with uncertainty** for virtual metrology

**The Fundamental Insight**

At nanometer scales, measurement uncertainty is not a nuisance to be ignored—it is a **primary object of study** that directly determines our ability to control and optimize semiconductor processes.



**Key Equations Quick Reference**

**Variance Decomposition**

$$
\sigma^2_{\text{total}} = \sigma^2_{\text{process}} + \sigma^2_{\text{measurement}}
$$

**GUM Combined Uncertainty**

$$
u_c(y) = \sqrt{\sum_{i=1}^{n} c_i^2 u^2(x_i)}
$$

where $c_i = \frac{\partial f}{\partial x_i}$ are sensitivity coefficients.

**Precision-to-Tolerance Ratio**

$$
\text{P/T} = \frac{6\sigma_{\text{meas}}}{\text{USL} - \text{LSL}} \times 100\%
$$

**Process Capability (Corrected)**

$$
C_{p,\text{true}} = \frac{\text{USL} - \text{LSL}}{6\sqrt{\sigma^2_{\text{obs}} - \sigma^2_{\text{meas}}}}
$$



**Notation Reference**

| Symbol | Description |
|--------|-------------|
| $\sigma^2$ | Variance |
| $u$ | Standard uncertainty |
| $U$ | Expanded uncertainty |
| $k$ | Coverage factor |
| $\mu$ | Population mean |
| $\bar{x}$ | Sample mean |
| $s$ | Sample standard deviation |
| $n$ | Sample size |
| $\mathcal{N}(\mu, \sigma^2)$ | Normal distribution |
| $\mathcal{GP}$ | Gaussian Process |
| $\text{USL}$, $\text{LSL}$ | Upper/Lower Specification Limits |
| $C_p$, $C_{pk}$ | Process capability indices |

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/measurement-uncertainty) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
