# Weibull Distribution Mathematics in Semiconductor Manufacturing

**Keywords**: Weibull distribution, reliability, failure rate, lifetime prediction, MTTF

---

**Weibull Distribution Mathematics in Semiconductor Manufacturing**

A comprehensive guide to the mathematical foundations and applications of Weibull distribution in semiconductor reliability engineering.



**1. Fundamental Weibull Mathematics**

**1.1 The Core Equations**

**Two-parameter Weibull Probability Density Function (PDF):**

$$
f(t) = \frac{\beta}{\eta} \left(\frac{t}{\eta}\right)^{\beta-1} \exp\left[-\left(\frac{t}{\eta}\right)^\beta\right]
$$

**Cumulative Distribution Function (CDF) — probability of failure by time $t$:**

$$
F(t) = 1 - \exp\left[-\left(\frac{t}{\eta}\right)^\beta\right]
$$

**Reliability (Survival) Function:**

$$
R(t) = \exp\left[-\left(\frac{t}{\eta}\right)^\beta\right]
$$

**Parameter Definitions:**

- $t \geq 0$ — random variable (typically time or stress cycles)
- $\beta > 0$ — **shape parameter** (Weibull slope/modulus)
- $\eta > 0$ — **scale parameter** (characteristic life, where $F(\eta) = 0.632$)

**1.2 Three-Parameter Weibull**

Adding a location parameter $\gamma$ (threshold/minimum life):

$$
F(t) = 1 - \exp\left[-\left(\frac{t-\gamma}{\eta}\right)^\beta\right], \quad t \geq \gamma
$$

**1.3 The Hazard Function (Instantaneous Failure Rate)**

$$
h(t) = \frac{f(t)}{R(t)} = \frac{\beta}{\eta} \left(\frac{t}{\eta}\right)^{\beta-1}
$$

**Physical Interpretation of Shape Parameter $\beta$:**

| $\beta$ Value | Failure Rate | Physical Meaning |
|---------------|--------------|------------------|
| $\beta < 1$ | Decreasing | Infant mortality, early defects |
| $\beta = 1$ | Constant | Random failures (exponential distribution) |
| $\beta > 1$ | Increasing | Wear-out mechanisms |

This directly models the semiconductor **bathtub curve**.



**2. Semiconductor-Specific Applications**

**2.1 Time-Dependent Dielectric Breakdown (TDDB)**

Gate oxide breakdown follows Weibull statistics. The **area scaling law** derives from weakest-link theory:

$$
\eta_2 = \eta_1 \left(\frac{A_1}{A_2}\right)^{1/\beta}
$$

**Where:**

- $A_1$ — reference test area
- $A_2$ — target device area
- $\eta_1$ — characteristic life at area $A_1$
- $\eta_2$ — predicted characteristic life at area $A_2$

**Typical $\beta$ values for oxide breakdown:**

- Intrinsic breakdown: $\beta \approx 10$–$30$ (tight distribution)
- Extrinsic/defect-related: $\beta \approx 1$–$5$ (broader distribution)

**2.2 Electromigration**

Metal interconnect failure combines **Black's equation** with Weibull statistics:

$$
MTF = A \cdot j^{-n} \cdot \exp\left(\frac{E_a}{k_B T}\right)
$$

**Where:**

- $MTF$ — median time to failure
- $j$ — current density ($A/cm^2$)
- $n$ — current density exponent (typically 1–2)
- $E_a$ — activation energy (eV)
- $k_B$ — Boltzmann constant ($8.617 \times 10^{-5}$ eV/K)
- $T$ — absolute temperature (K)

Typical $\beta$ values: **2–4** (wear-out behavior)

**2.3 Hot Carrier Injection (HCI)**

Degradation follows power-law kinetics:

$$
\Delta V_{th} = A \cdot t^n
$$

**Where:**

- $\Delta V_{th}$ — threshold voltage shift
- $t$ — stress time
- $n$ — time exponent (typically 0.3–0.5)

**2.4 Negative Bias Temperature Instability (NBTI)**

For PMOS transistors:

$$
\Delta V_{th} = A \cdot t^n \cdot \exp\left(-\frac{E_a}{k_B T}\right)
$$



**3. Statistical Analysis Methods**

**3.1 Weibull Probability Plotting**

**Linearization transformation** — take double logarithm of CDF:

$$
\ln\left[-\ln(1-F(t))\right] = \beta \ln(t) - \beta \ln(\eta)
$$

**Plotting $\ln[-\ln(1-F)]$ vs $\ln(t)$:**

- **Slope** = $\beta$
- **Intercept at $F = 0.632$** gives $t = \eta$

**Bernard's Median Rank Approximation** for ranking data:

$$
\hat{F}(t_{(r)}) \approx \frac{r - 0.3}{n + 0.4}
$$

**Where:**

- $r$ — rank of the $r$-th ordered failure
- $n$ — total sample size

**3.2 Maximum Likelihood Estimation (MLE)**

**Log-likelihood function** for $n$ samples with $r$ failures and $(n-r)$ censored units:

$$
\mathcal{L}(\beta, \eta) = \sum_{i=1}^{r} \left[\ln\beta - \beta\ln\eta + (\beta-1)\ln t_i - \left(\frac{t_i}{\eta}\right)^\beta\right] - \sum_{j=1}^{n-r}\left(\frac{t_j}{\eta}\right)^\beta
$$

**MLE Estimator for $\eta$:**

$$
\hat{\eta} = \left[\frac{1}{r}\sum_{i=1}^{n} t_i^{\hat{\beta}}\right]^{1/\hat{\beta}}
$$

**MLE Equation for $\beta$** (solve numerically):

$$
\frac{1}{\hat{\beta}} + \frac{\sum_{i=1}^{n} t_i^{\hat{\beta}} \ln t_i}{\sum_{i=1}^{n} t_i^{\hat{\beta}}} - \frac{1}{r}\sum_{i=1}^{r} \ln t_i = 0
$$



**4. Accelerated Life Testing Mathematics**

**4.1 Acceleration Factors**

**Arrhenius Model (Thermal Acceleration):**

$$
AF = \exp\left[\frac{E_a}{k_B}\left(\frac{1}{T_{use}} - \frac{1}{T_{stress}}\right)\right]
$$

**Exponential Voltage Acceleration:**

$$
AF = \exp\left[\gamma(V_{stress} - V_{use})\right]
$$

**Power-Law Voltage Acceleration:**

$$
AF = \left(\frac{V_{stress}}{V_{use}}\right)^n
$$

**Life Extrapolation:**

$$
\eta_{use} = AF \times \eta_{stress}
$$

**4.2 Combined Stress Models (Eyring)**

$$
AF = A \cdot \exp\left(\frac{E_a}{k_B T}\right) \cdot V^n \cdot (RH)^m
$$

**Where:**

- $RH$ — relative humidity
- $m$ — humidity exponent
- Additional stress factors can be included



**5. Competing Failure Modes**

**5.1 Series (Competing Risks) Model**

Device fails when the **first** mechanism fails:

$$
R(t) = \prod_{i=1}^{k} \exp\left[-\left(\frac{t}{\eta_i}\right)^{\beta_i}\right] = \exp\left[-\sum_{i=1}^{k}\left(\frac{t}{\eta_i}\right)^{\beta_i}\right]
$$

**Combined CDF:**

$$
F(t) = 1 - \exp\left[-\sum_{i=1}^{k}\left(\frac{t}{\eta_i}\right)^{\beta_i}\right]
$$

**5.2 Mixture Model**

Different subpopulations with different failure characteristics:

$$
F(t) = \sum_{i=1}^{k} p_i \cdot F_i(t)
$$

**Where:**

- $p_i$ — proportion in subpopulation $i$
- $\sum_{i=1}^{k} p_i = 1$
- $F_i(t)$ — CDF for subpopulation $i$

**PDF for mixture:**

$$
f(t) = \sum_{i=1}^{k} p_i \cdot f_i(t)
$$



**6. Key Derived Quantities**

**6.1 Moments of the Weibull Distribution**

**$k$-th Raw Moment:**

$$
E[T^k] = \eta^k \cdot \Gamma\left(1 + \frac{k}{\beta}\right)
$$

**Mean (MTTF — Mean Time To Failure):**

$$
\mu = \eta \cdot \Gamma\left(1 + \frac{1}{\beta}\right)
$$

**Variance:**

$$
\sigma^2 = \eta^2 \left[\Gamma\left(1 + \frac{2}{\beta}\right) - \Gamma^2\left(1 + \frac{1}{\beta}\right)\right]
$$

**Standard Deviation:**

$$
\sigma = \eta \sqrt{\Gamma\left(1 + \frac{2}{\beta}\right) - \Gamma^2\left(1 + \frac{1}{\beta}\right)}
$$

**6.2 Percentile Lives (B$X$ Life)**

Time by which $X\%$ have failed:

$$
t_X = \eta \cdot \left[\ln\left(\frac{1}{1-X/100}\right)\right]^{1/\beta}
$$

**Common Percentile Lives:**

| Percentile | Formula | Application |
|------------|---------|-------------|
| B1 Life | $t_1 = \eta \cdot (0.01005)^{1/\beta}$ | High-reliability |
| B10 Life | $t_{10} = \eta \cdot (0.1054)^{1/\beta}$ | Automotive/Aerospace |
| B50 Life (Median) | $t_{50} = \eta \cdot (0.6931)^{1/\beta}$ | General reference |
| B0.1 Life | $t_{0.1} = \eta \cdot (0.001001)^{1/\beta}$ | Critical systems |

**6.3 Characteristic Life Significance**

At $t = \eta$:

$$
F(\eta) = 1 - \exp(-1) = 1 - 0.368 = 0.632
$$

This means **63.2% of units have failed** by the characteristic life, regardless of $\beta$.



**7. Confidence Bounds**

**7.1 Fisher Information Matrix Approach**

**Information Matrix:**

$$
I(\beta, \eta) = -E\left[\frac{\partial^2 \mathcal{L}}{\partial \theta_i \partial \theta_j}\right]
$$

**Asymptotic Variance-Covariance Matrix:**

$$
\text{Var}(\hat{\theta}) \approx I^{-1}(\hat{\theta})
$$

**Fisher Matrix Elements:**

$$
I_{\beta\beta} = \frac{r}{\beta^2}\left[1 + \frac{\pi^2}{6}\right]
$$

$$
I_{\eta\eta} = \frac{r\beta^2}{\eta^2}
$$

$$
I_{\beta\eta} = \frac{r}{\eta}(1 - \gamma_E)
$$

Where $\gamma_E \approx 0.5772$ is the Euler-Mascheroni constant.

**7.2 Likelihood Ratio Bounds (Preferred for Small Samples)**

$$
-2\left[\mathcal{L}(\theta_0) - \mathcal{L}(\hat{\theta})\right] \leq \chi^2_{\alpha, df}
$$

**Approximate $(1-\alpha)$ Confidence Interval:**

$$
\left\{\theta : -2\left[\mathcal{L}(\theta) - \mathcal{L}(\hat{\theta})\right] \leq \chi^2_{\alpha, p}\right\}
$$



**8. Order Statistics**

**8.1 Expected Value of Order Statistics**

For $n$ samples, the expected value of the $r$-th order statistic:

$$
E[t_{(r)}] = \eta \cdot \Gamma\left(1 + \frac{1}{\beta}\right) \cdot \sum_{j=0}^{r-1} \frac{(-1)^j \binom{r-1}{j}}{(n-r+1+j)^{1+1/\beta}}
$$

**8.2 Plotting Positions**

**Bernard's Approximation (recommended):**

$$
\hat{F}_i = \frac{i - 0.3}{n + 0.4}
$$

**Hazen's Approximation:**

$$
\hat{F}_i = \frac{i - 0.5}{n}
$$

**Mean Rank:**

$$
\hat{F}_i = \frac{i}{n + 1}
$$



**9. Practical Example: Gate Oxide Qualification**

**9.1 Test Setup**

- **Sample size:** 50 oxide capacitors
- **Stress conditions:** 125°C, 1.2× nominal voltage
- **Test duration:** 1000 hours
- **Failures:** 8 units at times: 156, 289, 412, 523, 678, 734, 891, 967 hours
- **Censored:** 42 units still running at 1000h

**9.2 Analysis Steps**

**Step 1: Calculate Median Ranks**

| Rank ($i$) | Failure Time (h) | Median Rank $\hat{F}_i$ |
|------------|------------------|-------------------------|
| 1 | 156 | 0.0139 |
| 2 | 289 | 0.0337 |
| 3 | 412 | 0.0535 |
| 4 | 523 | 0.0733 |
| 5 | 678 | 0.0931 |
| 6 | 734 | 0.1129 |
| 7 | 891 | 0.1327 |
| 8 | 967 | 0.1525 |

**Step 2: MLE Results**

$$
\hat{\beta} \approx 2.1, \quad \hat{\eta} \approx 1850 \text{ hours (at stress)}
$$

**Step 3: Calculate Acceleration Factor**

Given: $E_a = 0.7$ eV, voltage exponent $n = 40$

$$
AF_{thermal} = \exp\left[\frac{0.7}{8.617 \times 10^{-5}}\left(\frac{1}{298} - \frac{1}{398}\right)\right] \approx 85
$$

$$
AF_{voltage} = (1.2)^{40} \approx 1.8
$$

$$
AF_{total} \approx 85 \times 1.8 \approx 150
$$

**Step 4: Extrapolate to Use Conditions**

$$
\eta_{use} = 1850 \times 150 = 277{,}500 \text{ hours}
$$

**Step 5: Calculate B0.1 Life**

$$
t_{0.1} = 277{,}500 \times (0.001001)^{1/2.1} \approx 3{,}200 \text{ hours}
$$



**10. Key Equations**

**10.1 Quick Reference Table**

| Quantity | Formula |
|----------|---------|
| PDF | $f(t) = \frac{\beta}{\eta}\left(\frac{t}{\eta}\right)^{\beta-1}\exp\left[-\left(\frac{t}{\eta}\right)^\beta\right]$ |
| CDF | $F(t) = 1 - \exp\left[-\left(\frac{t}{\eta}\right)^\beta\right]$ |
| Reliability | $R(t) = \exp\left[-\left(\frac{t}{\eta}\right)^\beta\right]$ |
| Hazard Rate | $h(t) = \frac{\beta}{\eta}\left(\frac{t}{\eta}\right)^{\beta-1}$ |
| Mean Life | $\mu = \eta \cdot \Gamma(1 + 1/\beta)$ |
| B10 Life | $t_{10} = \eta \cdot (0.1054)^{1/\beta}$ |
| Area Scaling | $\eta_2 = \eta_1 (A_1/A_2)^{1/\beta}$ |
| Linearization | $\ln[-\ln(1-F)] = \beta\ln t - \beta\ln\eta$ |

**10.2 Why Weibull Works for Semiconductors**

1. **Physical meaning of $\beta$** — directly indicates failure mechanism type
2. **Area/volume scaling** — derives from extreme value theory (weakest-link)
3. **Censored data handling** — essential since most test units don't fail
4. **Acceleration compatibility** — seamlessly integrates with physics-based models
5. **Competing risks framework** — models complex multi-mechanism devices



**Gamma Function Values**

Common values of $\Gamma(1 + 1/\beta)$ for mean life calculations:

| $\beta$ | $\Gamma(1 + 1/\beta)$ | $\mu/\eta$ |
|---------|------------------------|------------|
| 0.5 | 2.000 | 2.000 |
| 1.0 | 1.000 | 1.000 |
| 1.5 | 0.903 | 0.903 |
| 2.0 | 0.886 | 0.886 |
| 2.5 | 0.887 | 0.887 |
| 3.0 | 0.893 | 0.893 |
| 3.5 | 0.900 | 0.900 |
| 4.0 | 0.906 | 0.906 |
| 5.0 | 0.918 | 0.918 |
| 10.0 | 0.951 | 0.951 |



**Common Activation Energies**

| Failure Mechanism | Typical $E_a$ (eV) | Typical $\beta$ |
|-------------------|---------------------|-----------------|
| TDDB (oxide breakdown) | 0.6–0.8 | 1–3 |
| Electromigration | 0.5–0.9 | 2–4 |
| Hot Carrier Injection | 0.1–0.3 | 2–5 |
| NBTI | 0.1–0.2 | 2–4 |
| Corrosion | 0.3–0.5 | 1–3 |
| Solder Fatigue | — | 2–6 |

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/weibull-distribution) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
