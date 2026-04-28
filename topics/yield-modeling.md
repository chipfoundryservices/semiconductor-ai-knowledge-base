# Semiconductor Manufacturing Process Yield Modeling: Mathematical Foundations

**Keywords**: yield modeling, production yield, defect density, die yield, wafer yield, yield management

---

**Semiconductor Manufacturing Process Yield Modeling: Mathematical Foundations**




**1. Overview**

Yield modeling in semiconductor manufacturing is the mathematical framework for predicting the fraction of functional dies on a wafer. Since fabrication involves hundreds of process steps where defects can occur, accurate yield prediction is critical for:

- Cost estimation and financial planning
- Process optimization and control
- Manufacturing capacity decisions
- Design-for-manufacturability feedback



**2. Fundamental Definitions**

**Yield ($Y$)** is defined as:

$$
Y = \frac{\text{Number of good dies}}{\text{Total dies on wafer}}
$$

The mathematical challenge involves relating yield to:

- Defect density ($D$)
- Die area ($A$)
- Defect clustering behavior ($\alpha$)
- Process variations ($\sigma$)



**3. The Poisson Model (Baseline)**

The simplest model assumes defects are randomly and uniformly distributed across the wafer.

**3.1 Basic Equation**

$$
Y = e^{-AD}
$$

Where:

- $A$ = die area (cm²)
- $D$ = average defect density (defects/cm²)

**3.2 Mathematical Derivation**

If defects follow a Poisson distribution with mean $\lambda = AD$, the probability of zero defects (functional die) is:

$$
P(X = 0) = \frac{e^{-\lambda} \lambda^0}{0!} = e^{-AD}
$$

**3.3 Limitations**

- **Problem**: This model consistently *underestimates* real yields
- **Reason**: Actual defects cluster—they don't distribute uniformly
- **Result**: Some wafer regions have high defect density while others are nearly defect-free



**4. Defect Clustering Models**

Real defects cluster due to:

- Particle contamination patterns
- Equipment-related issues
- Process variations across the wafer
- Lithography and etch non-uniformities

**4.1 Murphy's Model (1964)**

Assumes defect density is uniformly distributed between $0$ and $2D_0$:

$$
Y = \frac{1 - e^{-2AD_0}}{2AD_0}
$$

For large $AD_0$, this approximates to:

$$
Y \approx \frac{1}{2AD_0}
$$

**4.2 Seeds' Model**

Assumes exponential distribution of defect density:

$$
Y = e^{-\sqrt{AD}}
$$

**4.3 Negative Binomial Model (Industry Standard)**

This is the most widely used model in semiconductor manufacturing.

**4.3.1 Main Equation**

$$
Y = \left(1 + \frac{AD}{\alpha}\right)^{-\alpha}
$$

Where $\alpha$ is the **clustering parameter**:

- $\alpha \to \infty$: Reduces to Poisson (no clustering)
- $\alpha \to 0$: Extreme clustering (highly non-uniform)
- Typical values: $\alpha \approx 0.5$ to $5$

**4.3.2 Mathematical Origin**

The negative binomial arises from a **compound Poisson process**:

1. Let $X \sim \text{Poisson}(\lambda)$ be the defect count
2. Let $\lambda \sim \text{Gamma}(\alpha, \beta)$ be the varying rate
3. Marginalizing over $\lambda$ gives $X \sim \text{Negative Binomial}$

The probability mass function is:

$$
P(X = k) = \binom{k + \alpha - 1}{k} \left(\frac{\beta}{\beta + 1}\right)^\alpha \left(\frac{1}{\beta + 1}\right)^k
$$

The yield (probability of zero defects) becomes:

$$
Y = P(X = 0) = \left(\frac{\beta}{\beta + 1}\right)^\alpha = \left(1 + \frac{AD}{\alpha}\right)^{-\alpha}
$$

**4.4 Model Comparison**

At $AD = 1$:

| Model | Yield |
|:------|------:|
| Poisson | 36.8% |
| Murphy | 43.2% |
| Negative Binomial ($\alpha = 2$) | 57.7% |
| Negative Binomial ($\alpha = 1$) | 50.0% |
| Seeds | 36.8% |



**5. Critical Area Analysis**

Not all die area is equally sensitive to defects. **Critical area** ($A_c$) is the region where a defect of given size causes failure.

**5.1 Definition**

For a defect of radius $r$:

- **Short critical area**: Region where defect center causes a short circuit
- **Open critical area**: Region where defect causes an open circuit

**5.2 Stapper's Critical Area Model**

For parallel lines of width $w$, spacing $s$, and length $l$:

$$
A_c(r) = \begin{cases} 
0 & \text{if } r < \frac{s}{2} \\[8pt]
2l\left(r - \frac{s}{2}\right) & \text{if } \frac{s}{2} \leq r < \frac{w+s}{2} \\[8pt]
lw & \text{if } r \geq \frac{w+s}{2}
\end{cases}
$$

**5.3 Integration Over Defect Size Distribution**

The total critical area integrates over the defect size distribution $f(r)$:

$$
A_c = \int_0^\infty A_c(r) \cdot f(r) \, dr
$$

Common distributions for $f(r)$:

- **Log-normal**: $f(r) = \frac{1}{r\sigma\sqrt{2\pi}} \exp\left(-\frac{(\ln r - \mu)^2}{2\sigma^2}\right)$
- **Power-law**: $f(r) \propto r^{-p}$ for $r_{\min} \leq r \leq r_{\max}$

**5.4 Yield with Critical Area**

$$
Y = \exp\left(-\int_0^\infty A_c(r) \cdot D(r) \, dr\right)
$$



**6. Yield Decomposition**

Total yield is typically factored into independent components:

$$
Y_{\text{total}} = Y_{\text{gross}} \times Y_{\text{random}} \times Y_{\text{parametric}}
$$

**6.1 Component Definitions**

| Component | Description | Typical Range |
|:----------|:------------|:-------------:|
| $Y_{\text{gross}}$ | Catastrophic defects, edge loss, handling damage | 95–99% |
| $Y_{\text{random}}$ | Random particle defects (main focus of yield modeling) | 70–95% |
| $Y_{\text{parametric}}$ | Process variation causing spec failures | 90–99% |

**6.2 Extended Decomposition**

For more detailed analysis:

$$
Y_{\text{total}} = Y_{\text{gross}} \times \prod_{i=1}^{N_{\text{layers}}} Y_{\text{random},i} \times \prod_{j=1}^{M_{\text{params}}} Y_{\text{param},j}
$$



**7. Parametric Yield Modeling**

Dies may function but fail to meet performance specifications due to process variation.

**7.1 Single Parameter Model**

If parameter $X \sim \mathcal{N}(\mu, \sigma^2)$ with specification limits $[L, U]$:

$$
Y_p = \Phi\left(\frac{U - \mu}{\sigma}\right) - \Phi\left(\frac{L - \mu}{\sigma}\right)
$$

Where $\Phi(\cdot)$ is the standard normal cumulative distribution function:

$$
\Phi(z) = \frac{1}{\sqrt{2\pi}} \int_{-\infty}^{z} e^{-t^2/2} \, dt
$$

**7.2 Process Capability Indices**

**7.2.1 Cp (Process Capability)**

$$
C_p = \frac{USL - LSL}{6\sigma}
$$

**7.2.2 Cpk (Process Capability Index)**

$$
C_{pk} = \min\left(\frac{USL - \mu}{3\sigma}, \frac{\mu - LSL}{3\sigma}\right)
$$

**7.3 Cpk to Yield Conversion**

| $C_{pk}$ | Sigma Level | Yield | DPMO |
|:--------:|:-----------:|:-----:|-----:|
| 0.33 | 1σ | 68.27% | 317,300 |
| 0.67 | 2σ | 95.45% | 45,500 |
| 1.00 | 3σ | 99.73% | 2,700 |
| 1.33 | 4σ | 99.9937% | 63 |
| 1.67 | 5σ | 99.999943% | 0.57 |
| 2.00 | 6σ | 99.9999998% | 0.002 |

**7.4 Multiple Correlated Parameters**

For $n$ parameters with mean vector $\boldsymbol{\mu}$ and covariance matrix $\boldsymbol{\Sigma}$:

$$
Y_p = \int \int \cdots \int_{\mathcal{R}} \frac{1}{(2\pi)^{n/2}|\boldsymbol{\Sigma}|^{1/2}} \exp\left(-\frac{1}{2}(\mathbf{x}-\boldsymbol{\mu})^T \boldsymbol{\Sigma}^{-1}(\mathbf{x}-\boldsymbol{\mu})\right) d\mathbf{x}
$$

Where $\mathcal{R}$ is the specification region.

**Computational Methods**:

- Monte Carlo integration
- Gaussian quadrature
- Importance sampling



**8. Spatial Yield Models**

Modern fabs analyze spatial patterns using wafer maps to identify systematic issues.

**8.1 Radial Defect Density Model**

Accounts for edge effects:

$$
D(r) = D_0 + D_1 r^2
$$

Where:

- $r$ = distance from wafer center
- $D_0$ = baseline defect density
- $D_1$ = radial coefficient

**8.2 General Spatial Model**

$$
D(x, y) = D_0 + \sum_{i} \beta_i \phi_i(x, y)
$$

Where $\phi_i(x, y)$ are spatial basis functions (e.g., Zernike polynomials).

**8.3 Spatial Autocorrelation (Moran's I)**

$$
I = \frac{n \sum_i \sum_j w_{ij}(Z_i - \bar{Z})(Z_j - \bar{Z})}{W \sum_i (Z_i - \bar{Z})^2}
$$

Where:

- $Z_i$ = pass/fail indicator for die $i$ (1 = fail, 0 = pass)
- $w_{ij}$ = spatial weight between dies $i$ and $j$
- $W = \sum_i \sum_j w_{ij}$
- $\bar{Z}$ = mean failure rate

**Interpretation**:

- $I > 0$: Clustered failures (systematic issue)
- $I \approx 0$: Random failures
- $I < 0$: Dispersed failures (rare)

**8.4 Variogram Analysis**

The semi-variogram $\gamma(h)$ measures spatial dependence:

$$
\gamma(h) = \frac{1}{2|N(h)|} \sum_{(i,j) \in N(h)} (Z_i - Z_j)^2
$$

Where $N(h)$ is the set of die pairs separated by distance $h$.



**9. Multi-Layer Yield**

Modern ICs have many process layers, each contributing to yield loss.

**9.1 Independent Layers**

$$
Y_{\text{total}} = \prod_{i=1}^{N} Y_i = \prod_{i=1}^{N} \left(1 + \frac{A_i D_i}{\alpha_i}\right)^{-\alpha_i}
$$

**9.2 Simplified Model**

If defects are independent across layers with similar clustering:

$$
Y = \left(1 + \frac{A \cdot D_{\text{total}}}{\alpha}\right)^{-\alpha}
$$

Where:

$$
D_{\text{total}} = \sum_{i=1}^{N} D_i
$$

**9.3 Layer-Specific Critical Areas**

$$
Y = \prod_{i=1}^{N} \exp\left(-A_{c,i} \cdot D_i\right)
$$

For Poisson model, or:

$$
Y = \prod_{i=1}^{N} \left(1 + \frac{A_{c,i} D_i}{\alpha_i}\right)^{-\alpha_i}
$$

For negative binomial.



**10. Yield Learning Curves**

Yield improves over time as processes mature and defect sources are eliminated.

**10.1 Exponential Learning Model**

$$
D(t) = D_\infty + (D_0 - D_\infty)e^{-t/\tau}
$$

Where:

- $D_0$ = initial defect density
- $D_\infty$ = asymptotic (mature) defect density
- $\tau$ = learning time constant

**10.2 Power Law (Wright's Learning Curve)**

$$
D(n) = D_1 \cdot n^{-b}
$$

Where:

- $n$ = cumulative production volume (wafers or lots)
- $D_1$ = defect density after first unit
- $b$ = learning rate exponent (typically $0.2 \leq b \leq 0.4$)

**10.3 Yield vs. Time**

Combining with yield model:

$$
Y(t) = \left(1 + \frac{A \cdot D(t)}{\alpha}\right)^{-\alpha}
$$



**11. Yield-Redundancy Models (Memory)**

Memory arrays use redundant rows/columns for defect tolerance through laser repair or electrical fusing.

**11.1 Poisson Model with Redundancy**

If a memory has $R$ spare elements and defects follow Poisson:

$$
Y_{\text{repaired}} = \sum_{k=0}^{R} \frac{(AD)^k e^{-AD}}{k!}
$$

This is the CDF of the Poisson distribution:

$$
Y_{\text{repaired}} = \frac{\Gamma(R+1, AD)}{\Gamma(R+1)} = \frac{\gamma(R+1, AD)}{R!}
$$

Where $\gamma(\cdot, \cdot)$ is the lower incomplete gamma function.

**11.2 Negative Binomial Model with Redundancy**

$$
Y_{\text{repaired}} = \sum_{k=0}^{R} \binom{k+\alpha-1}{k} \left(\frac{\alpha}{\alpha + AD}\right)^\alpha \left(\frac{AD}{\alpha + AD}\right)^k
$$

**11.3 Repair Coverage Factor**

$$
Y_{\text{repaired}} = Y_{\text{base}} + (1 - Y_{\text{base}}) \cdot RC
$$

Where $RC$ is the repair coverage (fraction of defective dies that can be repaired).



**12. Statistical Estimation**

**12.1 Maximum Likelihood Estimation for Negative Binomial**

Given wafer data with $n_i$ dies and $k_i$ failures per wafer $i$:

**Likelihood function**:

$$
\mathcal{L}(D, \alpha) = \prod_{i=1}^{W} \binom{n_i}{k_i} (1-Y)^{k_i} Y^{n_i - k_i}
$$

**Log-likelihood**:

$$
\ell(D, \alpha) = \sum_{i=1}^{W} \left[ \ln\binom{n_i}{k_i} + k_i \ln(1-Y) + (n_i - k_i) \ln Y \right]
$$

**Estimation**: Requires iterative numerical methods:

- Newton-Raphson
- EM algorithm
- Gradient descent

**12.2 Bayesian Estimation**

With prior distributions $P(D)$ and $P(\alpha)$:

$$
P(D, \alpha \mid \text{data}) \propto P(\text{data} \mid D, \alpha) \cdot P(D) \cdot P(\alpha)
$$

Common priors:

- $D \sim \text{Gamma}(a_D, b_D)$
- $\alpha \sim \text{Gamma}(a_\alpha, b_\alpha)$

**12.3 Model Selection**

Use information criteria to compare models:

**Akaike Information Criterion (AIC)**:

$$
AIC = -2\ln(\mathcal{L}) + 2k
$$

**Bayesian Information Criterion (BIC)**:

$$
BIC = -2\ln(\mathcal{L}) + k\ln(n)
$$

Where $k$ = number of parameters, $n$ = sample size.



**13. Economic Model**

**13.1 Die Cost**

$$
\text{Cost}_{\text{die}} = \frac{\text{Cost}_{\text{wafer}}}{N_{\text{dies}} \times Y}
$$

**13.2 Dies Per Wafer**

Accounting for edge exclusion (dies must fit entirely within usable area):

$$
N \approx \frac{\pi D_w^2}{4A} - \frac{\pi D_w}{\sqrt{2A}}
$$

Where:

- $D_w$ = wafer diameter
- $A$ = die area

**More accurate formula**:

$$
N = \frac{\pi (D_w/2 - E)^2}{A} \cdot \eta
$$

Where:

- $E$ = edge exclusion distance
- $\eta$ = packing efficiency factor ($\approx 0.9$)

**13.3 Cost Sensitivity Analysis**

Marginal cost impact of yield change:

$$
\frac{\partial \text{Cost}_{\text{die}}}{\partial Y} = -\frac{\text{Cost}_{\text{wafer}}}{N \cdot Y^2}
$$

**13.4 Break-Even Analysis**

Minimum yield for profitability:

$$
Y_{\text{min}} = \frac{\text{Cost}_{\text{wafer}}}{N \cdot \text{Price}_{\text{die}}}
$$



**14. Key Models**

**14.1 Yield Models Comparison**

| Model | Formula | Best Application |
|:------|:--------|:-----------------|
| Poisson | $Y = e^{-AD}$ | Lower bound estimate, theoretical baseline |
| Murphy | $Y = \frac{1-e^{-2AD}}{2AD}$ | Moderate clustering |
| Seeds | $Y = e^{-\sqrt{AD}}$ | Exponential clustering |
| **Negative Binomial** | $Y = \left(1 + \frac{AD}{\alpha}\right)^{-\alpha}$ | **Industry standard**, tunable clustering |
| Critical Area | $Y = e^{-\int A_c(r)D(r)dr}$ | Layout-aware prediction |

**14.2 Key Parameters**

| Parameter | Symbol | Typical Range | Description |
|:----------|:------:|:-------------:|:------------|
| Defect Density | $D$ | 0.01–1 /cm² | Defects per unit area |
| Die Area | $A$ | 10–800 mm² | Size of single chip |
| Clustering Parameter | $\alpha$ | 0.5–5 | Degree of defect clustering |
| Learning Rate | $b$ | 0.2–0.4 | Yield improvement rate |

**14.3 Quick Reference Equations**

**Basic yield**:
$$Y = e^{-AD}$$

**Industry standard**:
$$Y = \left(1 + \frac{AD}{\alpha}\right)^{-\alpha}$$

**Total yield**:
$$Y_{\text{total}} = Y_{\text{gross}} \times Y_{\text{random}} \times Y_{\text{parametric}}$$

**Die cost**:
$$\text{Cost}_{\text{die}} = \frac{\text{Cost}_{\text{wafer}}}{N \times Y}$$



**Practical Implementation Workflow**

1. **Data Collection**
   - Gather wafer test data (pass/fail maps)
   - Record lot/wafer identifiers and timestamps

2. **Parameter Estimation**
   - Estimate $D$ and $\alpha$ via MLE or Bayesian methods
   - Validate with holdout data

3. **Spatial Analysis**
   - Generate wafer maps
   - Calculate Moran's I to detect clustering
   - Identify systematic defect patterns

4. **Parametric Analysis**
   - Model electrical parameter distributions
   - Calculate $C_{pk}$ for key parameters
   - Estimate parametric yield losses

5. **Model Integration**
   - Combine: $Y_{\text{total}} = Y_{\text{gross}} \times Y_{\text{random}} \times Y_{\text{parametric}}$
   - Validate against actual production data

6. **Trend Monitoring**
   - Track $D$ and $\alpha$ over time
   - Fit learning curve models
   - Project future yields

7. **Cost Optimization**
   - Calculate die cost at current yield
   - Identify highest-impact improvement opportunities
   - Optimize die size vs. yield trade-off

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/yield-modeling) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
