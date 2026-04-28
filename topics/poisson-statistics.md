# Semiconductor Manufacturing Process: Poisson Statistics & Mathematical Modeling

**Keywords**: Poisson statistics, defect distribution, yield modeling, critical area, clustering

---

**Semiconductor Manufacturing Process: Poisson Statistics & Mathematical Modeling**





**1. Introduction: Why Poisson Statistics?**

Semiconductor defects satisfy the classical **Poisson conditions**:

- **Rare events** — Defects are sparse relative to the total chip area
- **Independence** — Defect occurrences are approximately independent
- **Homogeneity** — Within local regions, defect rates are constant
- **No simultaneity** — At infinitesimal scales, simultaneous defects have zero probability

**1.1 The Poisson Probability Mass Function**

The probability of observing exactly $k$ defects:

$$
P(X = k) = \frac{\lambda^k e^{-\lambda}}{k!}
$$

where the expected number of defects is:

$$
\lambda = D_0 \cdot A
$$

**Parameter definitions:**

- $D_0$ — Defect density (defects per unit area, typically defects/cm²)
- $A$ — Chip area (cm²)
- $\lambda$ — Mean number of defects per chip

**1.2 Key Statistical Properties**

| Property | Formula |
|----------|---------|
| Mean | $E[X] = \lambda$ |
| Variance | $\text{Var}(X) = \lambda$ |
| Variance-to-Mean Ratio | $\frac{\text{Var}(X)}{E[X]} = 1$ |

> **Note:** The equality of mean and variance (equidispersion) is a signature property of the Poisson distribution. Real semiconductor data often shows **overdispersion** (variance > mean), motivating compound models.



**2. Fundamental Yield Equation**

**2.1 The Seeds Model (Simple Poisson)**

A chip is functional if and only if it has **zero killer defects**. Under Poisson assumptions:

$$
\boxed{Y = P(X = 0) = e^{-D_0 A}}
$$

**Derivation:**

$$
P(X = 0) = \frac{\lambda^0 e^{-\lambda}}{0!} = e^{-\lambda} = e^{-D_0 A}
$$

**2.2 Limitations of Simple Poisson**

- Assumes **uniform** defect density across the wafer (unrealistic)
- Does not account for **clustering** of defects
- Consistently **underestimates** yield for large chips
- Ignores wafer-to-wafer and lot-to-lot variation



**3. Compound Poisson Models**

**3.1 The Negative Binomial Approach**

Model the defect density $D_0$ as a **random variable** with Gamma distribution:

$$
D_0 \sim \text{Gamma}\left(\alpha, \frac{\alpha}{\bar{D}}\right)
$$

**Gamma probability density function:**

$$
f(D_0) = \frac{(\alpha/\bar{D})^\alpha}{\Gamma(\alpha)} D_0^{\alpha-1} e^{-\alpha D_0/\bar{D}}
$$

where:

- $\bar{D}$ — Mean defect density
- $\alpha$ — Clustering parameter (shape parameter)

**3.2 Resulting Yield Model**

When defect density is Gamma-distributed, the defect count follows a **Negative Binomial** distribution, yielding:

$$
\boxed{Y = \left(1 + \frac{D_0 A}{\alpha}\right)^{-\alpha}}
$$

**3.3 Physical Interpretation of Clustering Parameter $\alpha$**

| $\alpha$ Value | Physical Interpretation |
|----------------|------------------------|
| $\alpha \to \infty$ | Uniform defects — recovers simple Poisson model |
| $\alpha \approx 1-5$ | Typical semiconductor clustering |
| $\alpha \to 0$ | Extreme clustering — defects occur in tight groups |

**3.4 Overdispersion**

The variance-to-mean ratio for the Negative Binomial:

$$
\frac{\text{Var}(X)}{E[X]} = 1 + \frac{\bar{D}A}{\alpha} > 1
$$

This **overdispersion** (ratio > 1) matches empirical observations in semiconductor manufacturing.



**4. Classical Yield Models**

**4.1 Comparison Table**

| Model | Yield Formula | Assumed Density Distribution |
|-------|---------------|------------------------------|
| Seeds (Poisson) | $Y = e^{-D_0 A}$ | Delta function (uniform) |
| Murphy | $Y = \left(\frac{1 - e^{-D_0 A}}{D_0 A}\right)^2$ | Triangular |
| Negative Binomial | $Y = \left(1 + \frac{D_0 A}{\alpha}\right)^{-\alpha}$ | Gamma |
| Moore | $Y = e^{-\sqrt{D_0 A}}$ | Empirical |
| Bose-Einstein | $Y = \frac{1}{1 + D_0 A}$ | Exponential |

**4.2 Murphy's Yield Model**

Assumes triangular distribution of defect densities:

$$
Y_{\text{Murphy}} = \left(\frac{1 - e^{-D_0 A}}{D_0 A}\right)^2
$$

**Taylor expansion for small $D_0 A$:**

$$
Y_{\text{Murphy}} \approx 1 - \frac{(D_0 A)^2}{12} + O((D_0 A)^4)
$$

**4.3 Limiting Behavior**

As $D_0 A \to 0$ (low defect density):

$$
\lim_{D_0 A \to 0} Y = 1 \quad \text{(all models)}
$$

As $D_0 A \to \infty$ (high defect density):

$$
\lim_{D_0 A \to \infty} Y = 0 \quad \text{(all models)}
$$



**5. Critical Area Analysis**

**5.1 Definition**

Not all chip area is equally vulnerable. **Critical area** $A_c$ is the region where a defect of size $d$ causes circuit failure.

$$
A_c(d) = \int_{\text{layout}} \mathbf{1}\left[\text{defect at } (x,y) \text{ with size } d \text{ causes failure}\right] \, dx \, dy
$$

**5.2 Critical Area for Shorts**

For two parallel conductors with:

- Length: $L$
- Spacing: $S$

$$
A_c^{\text{short}}(d) = 
\begin{cases}
2L(d - S) & \text{if } d > S \\
0 & \text{if } d \leq S
\end{cases}
$$

**5.3 Critical Area for Opens**

For a conductor with:

- Width: $W$
- Length: $L$

$$
A_c^{\text{open}}(d) = 
\begin{cases}
L(d - W) & \text{if } d > W \\
0 & \text{if } d \leq W
\end{cases}
$$

**5.4 Total Critical Area**

Integrate over the defect size distribution $f(d)$:

$$
A_c = \int_0^\infty A_c(d) \cdot f(d) \, dd
$$

**5.5 Defect Size Distribution**

Typically modeled as **power-law**:

$$
f(d) = C \cdot d^{-p} \quad \text{for } d \geq d_{\min}
$$

**Typical values:**

- Exponent: $p \approx 2-4$
- Normalization constant: $C = (p-1) \cdot d_{\min}^{p-1}$

**Alternative: Log-normal distribution** (common for particle contamination):

$$
f(d) = \frac{1}{d \sigma \sqrt{2\pi}} \exp\left(-\frac{(\ln d - \mu)^2}{2\sigma^2}\right)
$$



**6. Multi-Layer Yield Modeling**

**6.1 Modern IC Structure**

Modern integrated circuits have **10-15+ metal layers**. Each layer $i$ has:

- Defect density: $D_i$
- Critical area: $A_{c,i}$
- Clustering parameter: $\alpha_i$ (for Negative Binomial)

**6.2 Poisson Multi-Layer Yield**

$$
Y_{\text{total}} = \prod_{i=1}^{n} Y_i = \prod_{i=1}^{n} e^{-D_i A_{c,i}}
$$

Simplified form:

$$
\boxed{Y_{\text{total}} = \exp\left(-\sum_{i=1}^{n} D_i A_{c,i}\right)}
$$

**6.3 Negative Binomial Multi-Layer Yield**

$$
\boxed{Y_{\text{total}} = \prod_{i=1}^{n} \left(1 + \frac{D_i A_{c,i}}{\alpha_i}\right)^{-\alpha_i}}
$$

**6.4 Log-Yield Decomposition**

Taking logarithms for analysis:

$$
\ln Y_{\text{total}} = -\sum_{i=1}^{n} D_i A_{c,i} \quad \text{(Poisson)}
$$

$$
\ln Y_{\text{total}} = -\sum_{i=1}^{n} \alpha_i \ln\left(1 + \frac{D_i A_{c,i}}{\alpha_i}\right) \quad \text{(Negative Binomial)}
$$



**7. Spatial Point Process Formulation**

**7.1 Inhomogeneous Poisson Process**

Intensity function $\lambda(x, y)$ varies spatially across the wafer:

$$
P(k \text{ defects in region } R) = \frac{\Lambda(R)^k e^{-\Lambda(R)}}{k!}
$$

where the integrated intensity is:

$$
\Lambda(R) = \iint_R \lambda(x,y) \, dx \, dy
$$

**7.2 Cox Process (Doubly Stochastic)**

The intensity $\lambda(x,y)$ is itself a **random field**:

$$
\lambda(x,y) = \exp\left(\mu + Z(x,y)\right)
$$

where:

- $\mu$ — Baseline log-intensity
- $Z(x,y)$ — Gaussian random field with spatial correlation function $\rho(h)$

**Correlation structure:**

$$
\text{Cov}(Z(x_1, y_1), Z(x_2, y_2)) = \sigma^2 \rho(h)
$$

where $h = \sqrt{(x_2-x_1)^2 + (y_2-y_1)^2}$

**7.3 Neyman Type A (Cluster Process)**

Models defects occurring in clusters:

1. **Cluster centers:** Poisson process with intensity $\lambda_c$
2. **Defects per cluster:** Poisson with mean $\mu$
3. **Defect positions:** Scattered around cluster center (e.g., isotropic Gaussian)

**Probability generating function:**

$$
G(s) = \exp\left[\lambda_c A \left(e^{\mu(s-1)} - 1\right)\right]
$$

**Mean and variance:**

$$
E[N] = \lambda_c A \mu
$$

$$
\text{Var}(N) = \lambda_c A \mu (1 + \mu)
$$



**8. Statistical Estimation Methods**

**8.1 Maximum Likelihood Estimation**

**8.1.1 Data Structure**

Given:

- $n$ chips with areas $A_1, A_2, \ldots, A_n$
- Binary outcomes $y_i \in \{0, 1\}$ (pass/fail)

**8.1.2 Likelihood Function**

$$
\mathcal{L}(D_0, \alpha) = \prod_{i=1}^n Y_i^{y_i} (1 - Y_i)^{1-y_i}
$$

where $Y_i = \left(1 + \frac{D_0 A_i}{\alpha}\right)^{-\alpha}$

**8.1.3 Log-Likelihood**

$$
\ell(D_0, \alpha) = \sum_{i=1}^n \left[y_i \ln Y_i + (1-y_i) \ln(1-Y_i)\right]
$$

**8.1.4 Score Equations**

$$
\frac{\partial \ell}{\partial D_0} = 0, \quad \frac{\partial \ell}{\partial \alpha} = 0
$$

> **Note:** Requires numerical optimization (Newton-Raphson, BFGS, or EM algorithm).

**8.2 Bayesian Estimation**

**8.2.1 Prior Distribution**

$$
D_0 \sim \text{Gamma}(a, b)
$$

$$
\pi(D_0) = \frac{b^a}{\Gamma(a)} D_0^{a-1} e^{-b D_0}
$$

**8.2.2 Posterior Distribution**

Given defect count $k$ on area $A$:

$$
D_0 \mid k \sim \text{Gamma}(a + k, b + A)
$$

**Posterior mean:**

$$
\hat{D}_0 = \frac{a + k}{b + A}
$$

**Posterior variance:**

$$
\text{Var}(D_0 \mid k) = \frac{a + k}{(b + A)^2}
$$

**8.2.3 Sequential Updating**

Bayesian framework enables sequential learning:

$$
\text{Prior}_n \xrightarrow{\text{data } k_n} \text{Posterior}_n = \text{Prior}_{n+1}
$$



**9. Statistical Process Control**

**9.1 c-Chart (Defect Counts)**

For **constant inspection area**:

- **Center line:** $\bar{c}$ (average defect count)
- **Upper Control Limit (UCL):** $\bar{c} + 3\sqrt{\bar{c}}$
- **Lower Control Limit (LCL):** $\max(0, \bar{c} - 3\sqrt{\bar{c}})$

**9.2 u-Chart (Defects per Unit Area)**

For **variable inspection area** $n_i$:

$$
u_i = \frac{c_i}{n_i}
$$

- **Center line:** $\bar{u}$
- **Control limits:** $\bar{u} \pm 3\sqrt{\frac{\bar{u}}{n_i}}$

**9.3 Overdispersion-Adjusted Charts**

For clustered defects (Negative Binomial), inflate the variance:

$$
\text{UCL} = \bar{c} + 3\sqrt{\bar{c}\left(1 + \frac{\bar{c}}{\alpha}\right)}
$$

$$
\text{LCL} = \max\left(0, \bar{c} - 3\sqrt{\bar{c}\left(1 + \frac{\bar{c}}{\alpha}\right)}\right)
$$

**9.4 CUSUM Chart**

Cumulative sum for detecting small persistent shifts:

$$
C_t^+ = \max(0, C_{t-1}^+ + (x_t - \mu_0 - K))
$$

$$
C_t^- = \max(0, C_{t-1}^- - (x_t - \mu_0 + K))
$$

where:

- $K$ — Slack value (typically $0.5\sigma$)
- Signal when $C_t^+$ or $C_t^-$ exceeds threshold $H$



**10. EUV Lithography Stochastic Effects**

**10.1 Photon Shot Noise**

At extreme ultraviolet wavelength (13.5 nm), **photon shot noise** becomes critical.

Number of photons absorbed in resist volume $V$:

$$
N \sim \text{Poisson}(\Phi \cdot \sigma \cdot V)
$$

where:

- $\Phi$ — Photon fluence (photons/area)
- $\sigma$ — Absorption cross-section
- $V$ — Resist volume

**10.2 Line Edge Roughness (LER)**

Stochastic photon absorption causes spatial variation in resist exposure:

$$
\sigma_{\text{LER}} \propto \frac{1}{\sqrt{\Phi \cdot V}}
$$

**Critical Design Rule:**

$$
\text{LER}_{3\sigma} < 0.1 \times \text{CD}
$$

where CD = Critical Dimension (feature size)

**10.3 Stochastic Printing Failures**

Probability of insufficient photons in a critical volume:

$$
P(\text{failure}) = P(N < N_{\text{threshold}}) = \sum_{k=0}^{N_{\text{threshold}}-1} \frac{\lambda^k e^{-\lambda}}{k!}
$$

where $\lambda = \Phi \sigma V$



**11. Reliability and Latent Defects**

**11.1 Defect Classification**

Not all defects cause immediate failure:

- **Killer defects:** Cause immediate functional failure
- **Latent defects:** May cause reliability failures over time

$$
\lambda_{\text{total}} = \lambda_{\text{killer}} + \lambda_{\text{latent}}
$$

**11.2 Yield vs. Reliability**

**Initial Yield:**

$$
Y = e^{-\lambda_{\text{killer}} \cdot A}
$$

**Reliability Function:**

$$
R(t) = e^{-\lambda_{\text{latent}} \cdot A \cdot H(t)}
$$

where $H(t)$ is the cumulative hazard function for latent defect activation.

**11.3 Weibull Activation Model**

$$
H(t) = \left(\frac{t}{\eta}\right)^\beta
$$

**Parameters:**

- $\eta$ — Scale parameter (characteristic life)
- $\beta$ — Shape parameter
  - $\beta < 1$: Decreasing failure rate (infant mortality)
  - $\beta = 1$: Constant failure rate (exponential)
  - $\beta > 1$: Increasing failure rate (wear-out)



**12. Complete Mathematical Framework**

**12.1 Hierarchical Model Structure**

```
-
┌─────────────────────────────────────────────────────────────┐
│              SEMICONDUCTOR YIELD MODEL HIERARCHY            │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Layer 1:  DEFECT PHYSICS                                   │
│            • Particle contamination                         │
│            • Process variation                              │
│            • Stochastic effects (EUV)                       │
│                           ↓                                 │
│  Layer 2:  SPATIAL POINT PROCESS                            │
│            • Inhomogeneous Poisson / Cox process            │
│            • Defect size distribution: f(d) ∝ d^(-p)        │
│                           ↓                                 │
│  Layer 3:  CRITICAL AREA CALCULATION                        │
│            • Layout-dependent geometry                      │
│            • Ac = ∫ Ac(d)$\cdot$f(d) dd                     │
│                           ↓                                 │
│  Layer 4:  YIELD MODEL                                      │
│            • Y = (1 + D₀Ac/α)^(-α)                          │
│            • Multi-layer: Y = ∏ Yᵢ                          │
│                           ↓                                 │
│  Layer 5:  STATISTICAL INFERENCE                            │
│            • MLE / Bayesian estimation                      │
│            • SPC monitoring                                 │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**12.2 Summary of Key Equations**

| Concept | Equation |
|---------|----------|
| Poisson PMF | $P(X=k) = \frac{\lambda^k e^{-\lambda}}{k!}$ |
| Simple Yield | $Y = e^{-D_0 A}$ |
| Negative Binomial Yield | $Y = \left(1 + \frac{D_0 A}{\alpha}\right)^{-\alpha}$ |
| Multi-Layer Yield | $Y = \prod_i \left(1 + \frac{D_i A_{c,i}}{\alpha_i}\right)^{-\alpha_i}$ |
| Critical Area (shorts) | $A_c^{\text{short}}(d) = 2L(d-S)$ for $d > S$ |
| Defect Size Distribution | $f(d) \propto d^{-p}$, $p \approx 2-4$ |
| Bayesian Posterior | $D_0 \mid k \sim \text{Gamma}(a+k, b+A)$ |
| Control Limits | $\bar{c} \pm 3\sqrt{\bar{c}(1 + \bar{c}/\alpha)}$ |
| LER Scaling | $\sigma_{\text{LER}} \propto (\Phi V)^{-1/2}$ |

**12.3 Typical Parameter Values**

| Parameter | Typical Range | Units |
|-----------|---------------|-------|
| Defect density $D_0$ | 0.01 - 1.0 | defects/cm² |
| Clustering parameter $\alpha$ | 0.5 - 5 | dimensionless |
| Defect size exponent $p$ | 2 - 4 | dimensionless |
| Chip area $A$ | 1 - 800 | mm² |

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/poisson-statistics) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
