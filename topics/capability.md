# Semiconductor Manufacturing Process SPC: Statistical Process Control Mathematics

**Keywords**: capability, cpk, cp, capability index, six sigma, dpmo, statistical process control, SPC mathematics

---

**Semiconductor Manufacturing Process SPC: Statistical Process Control Mathematics**



**1. Introduction**

**Why SPC Mathematics Matters in Semiconductor Fabs**

Semiconductor manufacturing operates at nanometer scales across hundreds of process steps, presenting unique challenges:

- **High Value**: A single wafer can be worth $10,000 to $100,000+
- **Tight Tolerances**: Process variations of a few nanometers cause yield collapse
- **Long Feedback Loops**: Days to weeks between process and measurement
- **Compounding Variation**: Multiple variance sources multiply through the process flow

The mathematics of SPC provides the framework to:

- Detect process shifts before they cause defects
- Quantify and decompose sources of variation
- Maintain processes within nanometer-scale tolerances
- Optimize yield through statistical understanding



**2. Fundamental Statistical Measures**

**2.1 Descriptive Statistics**

For a sample of $n$ measurements $x_1, x_2, \ldots, x_n$:

| Measure | Formula | Description |
|---------|---------|-------------|
| **Sample Mean** | $\bar{x} = \frac{1}{n} \sum_{i=1}^{n} x_i$ | Central tendency |
| **Sample Variance** | $s^2 = \frac{\sum_{i=1}^{n}(x_i - \bar{x})^2}{n-1}$ | Spread (unbiased) |
| **Sample Std Dev** | $s = \sqrt{s^2}$ | Spread in original units |
| **Range** | $R = x_{max} - x_{min}$ | Total spread |

**2.2 The Normal (Gaussian) Distribution**

The mathematical backbone of classical SPC:

$$
f(x) = \frac{1}{\sigma\sqrt{2\pi}} \exp\left(-\frac{(x-\mu)^2}{2\sigma^2}\right)
$$

Where:

- $\mu$ = population mean
- $\sigma$ = population standard deviation
- $\sigma^2$ = population variance

**2.3 Critical Probability Intervals**

| Interval | Probability Contained | Application |
|----------|----------------------|-------------|
| $\pm 1\sigma$ | 68.27% | Typical variation |
| $\pm 2\sigma$ | 95.45% | Warning limits |
| $\pm 3\sigma$ | 99.73% | Control limits |
| $\pm 4\sigma$ | 99.9937% | Cpk = 1.33 |
| $\pm 5\sigma$ | 99.99994% | Cpk = 1.67 |
| $\pm 6\sigma$ | 99.9999998% | Six Sigma (3.4 DPMO) |

**2.4 Standard Normal Transformation**

Any normal variable can be standardized:

$$
Z = \frac{X - \mu}{\sigma}
$$

Where $Z \sim N(0, 1)$ (standard normal distribution).



**3. Control Chart Mathematics**

**3.1 Shewhart X̄-R Charts**

The workhorse of semiconductor SPC for monitoring subgroup data.

**X̄ Chart (Monitoring Process Mean)**

$$
\begin{aligned}
CL &= \bar{\bar{X}} \quad \text{(grand mean of subgroup means)} \\
UCL &= \bar{\bar{X}} + A_2 \bar{R} \\
LCL &= \bar{\bar{X}} - A_2 \bar{R}
\end{aligned}
$$

**Theoretical basis:**

$$
UCL / LCL = \mu \pm \frac{3\sigma}{\sqrt{n}}
$$

**R Chart (Monitoring Process Spread)**

$$
\begin{aligned}
CL &= \bar{R} \\
UCL &= D_4 \bar{R} \\
LCL &= D_3 \bar{R}
\end{aligned}
$$

**Control Chart Constants**

| $n$ | $A_2$ | $D_3$ | $D_4$ | $d_2$ |
|-----|-------|-------|-------|-------|
| 2 | 1.880 | 0 | 3.267 | 1.128 |
| 3 | 1.023 | 0 | 2.574 | 1.693 |
| 4 | 0.729 | 0 | 2.282 | 2.059 |
| 5 | 0.577 | 0 | 2.114 | 2.326 |
| 6 | 0.483 | 0 | 2.004 | 2.534 |
| 7 | 0.419 | 0.076 | 1.924 | 2.704 |
| 8 | 0.373 | 0.136 | 1.864 | 2.847 |
| 9 | 0.337 | 0.184 | 1.816 | 2.970 |
| 10 | 0.308 | 0.223 | 1.777 | 3.078 |

**3.2 Individuals-Moving Range (I-MR) Charts**

Common in semiconductor when rational subgrouping isn't practical (e.g., one measurement per wafer lot).

**Individuals Chart**

$$
\begin{aligned}
CL &= \bar{X} \\
UCL &= \bar{X} + 3 \cdot \frac{\overline{MR}}{d_2} \\
LCL &= \bar{X} - 3 \cdot \frac{\overline{MR}}{d_2}
\end{aligned}
$$

Where $d_2 = 1.128$ for moving range of span 2.

**Moving Range Chart**

$$
\begin{aligned}
CL &= \overline{MR} \\
UCL &= D_4 \cdot \overline{MR} = 3.267 \cdot \overline{MR} \\
LCL &= D_3 \cdot \overline{MR} = 0
\end{aligned}
$$

**3.3 EWMA Charts (Exponentially Weighted Moving Average)**

More sensitive to small, persistent shifts than Shewhart charts.

**EWMA Statistic**

$$
EWMA_t = \lambda x_t + (1-\lambda) EWMA_{t-1}
$$

Where:

- $\lambda$ = smoothing parameter ($0 < \lambda \leq 1$, typically 0.05–0.25)
- $EWMA_0 = \mu_0$ (target mean)

**Control Limits (Time-Varying)**

$$
UCL/LCL = \mu_0 \pm L\sigma\sqrt{\frac{\lambda}{2-\lambda}\left[1-(1-\lambda)^{2t}\right]}
$$

**Asymptotic Control Limits**

As $t \to \infty$:

$$
UCL/LCL = \mu_0 \pm L\sigma\sqrt{\frac{\lambda}{2-\lambda}}
$$

**Typical parameters:**

- $\lambda = 0.10$ to $0.20$
- $L = 2.5$ to $3.0$

**EWMA Variance**

$$
Var(EWMA_t) = \sigma^2 \cdot \frac{\lambda}{2-\lambda} \left[1 - (1-\lambda)^{2t}\right]
$$

**3.4 CUSUM Charts (Cumulative Sum)**

Accumulates deviations from target—excellent for detecting sustained shifts.

**Tabular CUSUM**

**Upper CUSUM (detecting upward shifts):**

$$
C_t^+ = \max\left[0, x_t - (\mu_0 + K) + C_{t-1}^+\right]
$$

**Lower CUSUM (detecting downward shifts):**

$$
C_t^- = \max\left[0, (\mu_0 - K) - x_t + C_{t-1}^-\right]
$$

**Signal condition:**

$$
C_t^+ > H \quad \text{or} \quad C_t^- > H
$$

Where:

- $K$ = allowable slack (reference value), typically $0.5\sigma$
- $H$ = decision interval, typically $4\sigma$ to $5\sigma$
- $C_0^+ = C_0^- = 0$

**Standardized Form**

For standardized observations $z_t = (x_t - \mu_0)/\sigma$:

$$
\begin{aligned}
S_t^+ &= \max(0, z_t - k + S_{t-1}^+) \\
S_t^- &= \max(0, -z_t - k + S_{t-1}^-)
\end{aligned}
$$

With $k = 0.5$ (half the shift to detect) and $h = 4$ or $5$.



**4. Process Capability Indices**

**4.1 Cp (Potential Capability)**

Measures the ratio of specification width to process spread:

$$
C_p = \frac{USL - LSL}{6\sigma}
$$

Where:

- $USL$ = Upper Specification Limit
- $LSL$ = Lower Specification Limit
- $\sigma$ = process standard deviation

**Interpretation:**

- $C_p$ does **not** account for centering
- Represents potential capability if process were perfectly centered

**4.2 Cpk (Actual Capability)**

Accounts for off-center processes:

$$
C_{pk} = \min\left[\frac{USL - \mu}{3\sigma}, \frac{\mu - LSL}{3\sigma}\right]
$$

**Alternative formulation:**

$$
C_{pk} = C_p(1 - k)
$$

Where $k = \frac{|T - \mu|}{(USL - LSL)/2}$ and $T$ is the target (specification midpoint).

**Key property:** $C_{pk} \leq C_p$ always.

**4.3 Cpm (Taguchi Capability Index)**

Penalizes deviation from target $T$:

$$
C_{pm} = \frac{USL - LSL}{6\sqrt{\sigma^2 + (\mu - T)^2}}
$$

Or equivalently:

$$
C_{pm} = \frac{C_p}{\sqrt{1 + \left(\frac{\mu - T}{\sigma}\right)^2}}
$$

**4.4 Pp and Ppk (Performance Indices)**

Same formulas but use **overall** standard deviation (including between-subgroup variation):

$$
P_p = \frac{USL - LSL}{6s_{overall}}
$$

$$
P_{pk} = \min\left[\frac{USL - \bar{x}}{3s_{overall}}, \frac{\bar{x} - LSL}{3s_{overall}}\right]
$$

**Relationship:**

- $C_p, C_{pk}$: Use within-subgroup $\sigma$ (short-term capability)
- $P_p, P_{pk}$: Use overall $s$ (long-term performance)

**4.5 Relating Cpk to Defect Rates**

| $C_{pk}$ | $\sigma$-level | DPMO | Yield |
|----------|----------------|------|-------|
| 0.33 | 1σ | 317,311 | 68.27% |
| 0.67 | 2σ | 45,500 | 95.45% |
| 1.00 | 3σ | 2,700 | 99.73% |
| 1.33 | 4σ | 63 | 99.9937% |
| 1.67 | 5σ | 0.57 | 99.99994% |
| 2.00 | 6σ | 0.002 | 99.9999998% |

> **Note:** With 1.5σ shift allowance (industry standard), 6σ = 3.4 DPMO.

**4.6 Confidence Intervals for Cpk**

$$
\hat{C}_{pk} \pm z_{\alpha/2} \sqrt{\frac{1}{9n} + \frac{C_{pk}^2}{2(n-1)}}
$$

For reliable capability estimates, need $n \geq 30$, preferably $n \geq 50$.



**5. Variance Components Analysis**

**5.1 Typical Variance Hierarchy in Semiconductor**

$$
\sigma^2_{total} = \sigma^2_{lot} + \sigma^2_{wafer(lot)} + \sigma^2_{site(wafer)} + \sigma^2_{measurement}
$$

Each component represents:

- **Lot-to-lot ($\sigma^2_{lot}$)**: Variation between production lots
- **Wafer-to-wafer ($\sigma^2_{wafer}$)**: Variation between wafers within a lot
- **Within-wafer ($\sigma^2_{site}$)**: Variation across measurement sites on a wafer
- **Measurement ($\sigma^2_{meas}$)**: Gauge/metrology variation

**5.2 One-Way ANOVA**

**Sum of Squares Decomposition**

$$
SS_T = SS_B + SS_W
$$

**Total Sum of Squares:**

$$
SS_T = \sum_{i=1}^{k}\sum_{j=1}^{n}(x_{ij} - \bar{x}_{..})^2
$$

**Between-Groups Sum of Squares:**

$$
SS_B = n\sum_{i=1}^{k}(\bar{x}_{i.} - \bar{x}_{..})^2
$$

**Within-Groups Sum of Squares:**

$$
SS_W = \sum_{i=1}^{k}\sum_{j=1}^{n}(x_{ij} - \bar{x}_{i.})^2
$$

**Mean Squares**

$$
\begin{aligned}
MS_B &= \frac{SS_B}{k-1} \\
MS_W &= \frac{SS_W}{N-k}
\end{aligned}
$$

**F-Statistic**

$$
F = \frac{MS_B}{MS_W} \sim F_{k-1, N-k}
$$

**5.3 Variance Component Estimation**

From mean squares:

$$
\begin{aligned}
\hat{\sigma}^2_{within} &= MS_W \\
\hat{\sigma}^2_{between} &= \frac{MS_B - MS_W}{n}
\end{aligned}
$$

If $MS_B < MS_W$, set $\hat{\sigma}^2_{between} = 0$.

**5.4 Nested (Hierarchical) ANOVA**

For semiconductor's nested structure (sites within wafers within lots):

$$
x_{ijk} = \mu + \alpha_i + \beta_{j(i)} + \varepsilon_{k(ij)}
$$

Where:

- $\alpha_i$ = lot effect (random)
- $\beta_{j(i)}$ = wafer effect nested within lot (random)
- $\varepsilon_{k(ij)}$ = site/measurement error



**6. Measurement System Analysis (Gauge R&R)**

**6.1 Variance Decomposition**

$$
\sigma^2_{total} = \sigma^2_{part} + \sigma^2_{gauge}
$$

$$
\sigma^2_{gauge} = \sigma^2_{repeatability} + \sigma^2_{reproducibility}
$$

Where:

- **Repeatability (Equipment Variation):** Same operator, same equipment, multiple measurements
- **Reproducibility (Appraiser Variation):** Different operators or equipment

**6.2 ANOVA Method for Gauge R&R**

**Two-Factor Crossed Design**

| Source | SS | df | MS | EMS |
|--------|----|----|----|----|
| Part (P) | $SS_P$ | $p-1$ | $MS_P$ | $\sigma^2_E + r\sigma^2_{OP} + or\sigma^2_P$ |
| Operator (O) | $SS_O$ | $o-1$ | $MS_O$ | $\sigma^2_E + r\sigma^2_{OP} + pr\sigma^2_O$ |
| P×O | $SS_{PO}$ | $(p-1)(o-1)$ | $MS_{PO}$ | $\sigma^2_E + r\sigma^2_{OP}$ |
| Error (E) | $SS_E$ | $po(r-1)$ | $MS_E$ | $\sigma^2_E$ |

**Variance Component Estimates**

$$
\begin{aligned}
\hat{\sigma}^2_{repeatability} &= MS_E \\
\hat{\sigma}^2_{operator} &= \frac{MS_O - MS_{PO}}{pr} \\
\hat{\sigma}^2_{interaction} &= \frac{MS_{PO} - MS_E}{r} \\
\hat{\sigma}^2_{reproducibility} &= \hat{\sigma}^2_{operator} + \hat{\sigma}^2_{interaction} \\
\hat{\sigma}^2_{part} &= \frac{MS_P - MS_{PO}}{or}
\end{aligned}
$$

**6.3 Key Metrics**

**Percentage of Total Variation**

$$
\%GRR = 100 \times \frac{\sigma_{gauge}}{\sigma_{total}}
$$

Or using study variation (5.15σ for 99%):

$$
\%GRR = 100 \times \frac{5.15 \cdot \sigma_{gauge}}{5.15 \cdot \sigma_{total}}
$$

**Precision-to-Tolerance Ratio (P/T)**

$$
P/T = \frac{k \cdot \sigma_{gauge}}{USL - LSL}
$$

Where $k = 5.15$ (99%) or $k = 6$ (99.73%).

**Number of Distinct Categories (ndc)**

$$
ndc = 1.41 \cdot \frac{\sigma_{part}}{\sigma_{gauge}}
$$

**6.4 Acceptance Criteria**

| %GRR | Assessment | Action |
|------|------------|--------|
| < 10% | Excellent | Acceptable for all applications |
| 10–30% | Acceptable | May be acceptable depending on application |
| > 30% | Unacceptable | Measurement system needs improvement |

| ndc | Assessment |
|-----|------------|
| ≥ 5 | Acceptable |
| < 5 | Measurement system cannot distinguish parts |


**7. Run Rules (Western Electric / Nelson Rules)**

**7.1 Standard Run Rules**

Pattern detection beyond simple control limits:

| Rule | Pattern | Interpretation |
|------|---------|----------------|
| **1** | 1 point beyond ±3σ | Large shift or outlier |
| **2** | 9 consecutive points on same side of CL | Small sustained shift |
| **3** | 6 consecutive points steadily increasing or decreasing | Trend/drift |
| **4** | 14 consecutive points alternating up and down | Systematic oscillation (over-adjustment) |
| **5** | 2 of 3 consecutive points beyond ±2σ (same side) | Shift warning |
| **6** | 4 of 5 consecutive points beyond ±1σ (same side) | Shift warning |
| **7** | 15 consecutive points within ±1σ | Stratification (mixture) |
| **8** | 8 consecutive points beyond ±1σ (either side) | Mixture of populations |

**7.2 Zone Definitions**

Control charts are divided into zones:

- **Zone A:** Between 2σ and 3σ from center line
- **Zone B:** Between 1σ and 2σ from center line
- **Zone C:** Within 1σ of center line

**7.3 False Alarm Probabilities**

| Rule | Probability (per test) |
|------|----------------------|
| Rule 1 (±3σ) | 0.0027 |
| Rule 2 (9 same side) | 0.0039 |
| Rule 3 (6 trending) | 0.0028 |
| Rule 5 (2 of 3 in Zone A) | 0.0044 |

**Combined false alarm rate** increases when multiple rules are applied.



**8. Average Run Length (ARL)**

**8.1 Definitions**

- **ARL₀ (In-Control ARL):** Average number of samples until false alarm when process is in control (want high)
- **ARL₁ (Out-of-Control ARL):** Average number of samples to detect a shift (want low)

**8.2 Shewhart Chart ARL**

For 3σ limits:

$$
ARL_0 = \frac{1}{\alpha} = \frac{1}{0.0027} \approx 370
$$

For detecting a shift of $\delta$ standard deviations:

$$
ARL_1 = \frac{1}{P(\text{signal} | \text{shift})}
$$

$$
P(\text{signal}) = 1 - \Phi(3-\delta) + \Phi(-3-\delta)
$$

**8.3 Comparison of Chart Performance**

| Shift ($\delta\sigma$) | Shewhart ARL₁ | EWMA ARL₁ ($\lambda$=0.1) | CUSUM ARL₁ |
|------------------------|---------------|---------------------------|------------|
| 0.25 | 281 | 66 | 38 |
| 0.50 | 155 | 26 | 17 |
| 0.75 | 81 | 15 | 10 |
| 1.00 | 44 | 10 | 8 |
| 1.50 | 15 | 5 | 5 |
| 2.00 | 6 | 4 | 4 |
| 3.00 | 2 | 2 | 2 |

**Key insight:** EWMA and CUSUM are far superior for detecting small shifts ($\delta < 1.5\sigma$).

**8.4 ARL Formulas for CUSUM**

Approximate ARL for CUSUM detecting shift of size $\delta$:

$$
ARL_1 \approx \frac{e^{-2\Delta b} + 2\Delta b - 1}{2\Delta^2}
$$

Where:

- $\Delta = \delta - k$ (excess over reference value)
- $b = h + 1.166$ (adjusted decision interval)



**9. Multivariate SPC**

**9.1 Why Multivariate?**

Semiconductor processes involve many correlated parameters. Univariate charts on correlated variables:

- Increase false alarm rates
- Miss shifts in correlated directions
- Fail to detect process changes that affect multiple parameters simultaneously

**9.2 Hotelling's T² Statistic**

For $p$ variables measured on a sample of size $n$:

$$
T^2 = n(\bar{\mathbf{x}} - \boldsymbol{\mu}_0)' \mathbf{S}^{-1} (\bar{\mathbf{x}} - \boldsymbol{\mu}_0)
$$

Where:

- $\bar{\mathbf{x}}$ = sample mean vector ($p \times 1$)
- $\boldsymbol{\mu}_0$ = target mean vector ($p \times 1$)
- $\mathbf{S}$ = sample covariance matrix ($p \times p$)

**9.3 Control Limit for T²**

**Phase I (establishing control):**

$$
UCL = \frac{(m-1)(m+1)p}{m(m-p)} F_{\alpha, p, m-p}
$$

**Phase II (monitoring):**

$$
UCL = \frac{p(m+1)(m-1)}{m(m-p)} F_{\alpha, p, m-p}
$$

Where $m$ = number of historical samples.

For large $m$:

$$
UCL \approx \chi^2_{\alpha, p}
$$

**9.4 Multivariate EWMA (MEWMA)**

$$
\mathbf{Z}_t = \Lambda\mathbf{X}_t + (\mathbf{I} - \Lambda)\mathbf{Z}_{t-1}
$$

Where $\Lambda = \text{diag}(\lambda_1, \lambda_2, \ldots, \lambda_p)$.

**Statistic:**

$$
T^2_t = \mathbf{Z}_t' \boldsymbol{\Sigma}_{\mathbf{Z}_t}^{-1} \mathbf{Z}_t
$$

**Covariance of MEWMA:**

$$
\boldsymbol{\Sigma}_{\mathbf{Z}_t} = \frac{\lambda}{2-\lambda}\left[1 - (1-\lambda)^{2t}\right]\boldsymbol{\Sigma}
$$

**9.5 Principal Component Analysis (PCA) for SPC**

Decompose correlated variables into uncorrelated principal components:

$$
\mathbf{X} = \mathbf{T}\mathbf{P}' + \mathbf{E}
$$

Where:

- $\mathbf{T}$ = scores matrix
- $\mathbf{P}$ = loadings matrix
- $\mathbf{E}$ = residuals

**Hotelling's T² in PC space:**

$$
T^2 = \sum_{i=1}^{k} \frac{t_i^2}{\lambda_i}
$$

**Squared Prediction Error (SPE):**

$$
SPE = \mathbf{e}'\mathbf{e} = \sum_{i=k+1}^{p} t_i^2
$$



**10. Autocorrelation Handling**

**10.1 The Problem**

Semiconductor tool data often exhibits serial correlation, violating the independence assumption of standard SPC.

**Consequences of ignoring autocorrelation:**

- Actual ARL₀ << 370 (excessive false alarms)
- Control limits are too tight
- Patterns in data are misinterpreted

**10.2 Autocorrelation Function (ACF)**

**Population autocorrelation at lag $k$:**

$$
\rho_k = \frac{Cov(X_t, X_{t+k})}{Var(X_t)} = \frac{\gamma_k}{\gamma_0}
$$

**Sample autocorrelation:**

$$
r_k = \frac{\sum_{t=1}^{n-k}(x_t - \bar{x})(x_{t+k} - \bar{x})}{\sum_{t=1}^{n}(x_t - \bar{x})^2}
$$

**10.3 AR(1) Process**

The simplest autocorrelated model:

$$
X_t = \phi X_{t-1} + \varepsilon_t
$$

Where:

- $\phi$ = autoregressive parameter ($|\phi| < 1$ for stationarity)
- $\varepsilon_t \sim N(0, \sigma^2_\varepsilon)$ (white noise)

**Properties:**

$$
\begin{aligned}
Var(X_t) &= \frac{\sigma^2_\varepsilon}{1 - \phi^2} \\
\rho_k &= \phi^k
\end{aligned}
$$

**10.4 Solutions for Autocorrelated Data**

1. **Residual Charts:**
   - Fit time series model (AR, ARMA, etc.)
   - Apply SPC to residuals $\hat{\varepsilon}_t = X_t - \hat{X}_t$

2. **Modified Control Limits:**
   $$
   UCL/LCL = \mu \pm 3\sigma_X \sqrt{\frac{1 + \phi}{1 - \phi}}
   $$

3. **EWMA with Adjusted Parameters:**
   - Use $\lambda = 1 - \phi$ for optimal smoothing

4. **Special Cause Charts:**
   - Designed specifically for autocorrelated processes



**11. Run-to-Run (R2R) Process Control**

**11.1 Basic Concept**

Active feedback control layered on SPC—adjust recipe parameters based on measured outputs.

**11.2 EWMA Controller**

**Prediction:**

$$
\hat{y}_{t+1} = \lambda y_t + (1-\lambda)\hat{y}_t
$$

**Recipe Adjustment:**

$$
u_{t+1} = u_t - G(\hat{y}_t - y_{target})
$$

Where:

- $G$ = controller gain
- $u$ = recipe parameter (e.g., etch time, dose)
- $y$ = output measurement (e.g., CD, thickness)

**11.3 Double EWMA (for Drifting Processes)**

Track both level and slope:

**Level estimate:**

$$
L_t = \lambda y_t + (1-\lambda)(L_{t-1} + T_{t-1})
$$

**Trend estimate:**

$$
T_t = \gamma(L_t - L_{t-1}) + (1-\gamma)T_{t-1}
$$

**Forecast:**

$$
\hat{y}_{t+1} = L_t + T_t
$$

**11.4 Process Model Integration**

For process with known gain $\beta$:

$$
y_t = \alpha + \beta u_t + \varepsilon_t
$$

**Optimal control:**

$$
u_{t+1} = \frac{y_{target} - \hat{\alpha}_{t+1}}{\beta}
$$



**12. Yield Modeling Mathematics**

**12.1 Defect Density**

$$
D_0 = \frac{\text{Number of defects}}{\text{Area (cm}^2\text{)}}
$$

**12.2 Poisson Model (Random Defects)**

Assumes defects are randomly distributed:

$$
Y = e^{-D_0 A}
$$

Where:

- $D_0$ = defect density (defects/cm²)
- $A$ = die area (cm²)

**Probability of $k$ defects on a die:**

$$
P(k) = \frac{(D_0 A)^k e^{-D_0 A}}{k!}
$$

**12.3 Murphy's Model (Distributed Defects)**

Accounts for defect density variation across wafer:

$$
Y = \left[\frac{1 - e^{-D_0 A}}{D_0 A}\right]^2
$$

**12.4 Negative Binomial Model (Clustered Defects)**

More realistic for semiconductor:

$$
Y = \left(1 + \frac{D_0 A}{\alpha}\right)^{-\alpha}
$$

Where $\alpha$ = clustering parameter:

- $\alpha \to \infty$: Approaches Poisson (random)
- $\alpha$ small: Highly clustered

**12.5 Seeds Model**

$$
Y = e^{-D_0 A_s}
$$

Where $A_s$ = sensitive area (fraction of die area susceptible to defects).

**12.6 Yield Loss Calculations**

**Defect-Limited Yield:**

$$
Y_D = e^{-D_0 A}
$$

**Parametric Yield:**

$$
Y_P = \prod_{i} P(\text{parameter}_i \text{ in spec})
$$

**Total Yield:**

$$
Y_{total} = Y_D \times Y_P
$$



**13. Spatial Statistics for Wafer Maps**

**13.1 Radial Uniformity**

$$
\sigma_{radial} = \sqrt{\frac{1}{n}\sum_{i=1}^{n}(x_i - f(r_i))^2}
$$

Where $f(r_i)$ is the fitted radial profile at radius $r_i$.

**13.2 Wafer-Level Variation Components**

$$
\sigma^2_{total} = \sigma^2_{W2W} + \sigma^2_{WIW}
$$

Within-wafer variation often decomposed:

$$
\sigma^2_{WIW} = \sigma^2_{systematic} + \sigma^2_{random}
$$

Where:

- **Systematic WIW:** Modeled and corrected (radial, azimuthal patterns)
- **Random WIW:** Inherent noise

**13.3 Spatial Correlation Function**

For locations $\mathbf{s}_i$ and $\mathbf{s}_j$:

$$
C(h) = Cov(X(\mathbf{s}_i), X(\mathbf{s}_j))
$$

Where $h = \|\mathbf{s}_i - \mathbf{s}_j\|$ (distance between points).

**Variogram:**

$$
\gamma(h) = \frac{1}{2}Var[X(\mathbf{s}_i) - X(\mathbf{s}_j)]
$$

**13.4 Common Wafer Signatures**

Mathematical models for common spatial patterns:

**Radial (bowl/dome):**

$$
f(r) = a_0 + a_1 r + a_2 r^2
$$

**Azimuthal:**

$$
f(\theta) = b_0 + b_1 \cos(\theta) + b_2 \sin(\theta)
$$

**Combined:**

$$
f(r, \theta) = \sum_{n,m} a_{nm} Z_n^m(r, \theta)
$$

Where $Z_n^m$ are Zernike polynomials.



**14. Practical Implementation Considerations**

**14.1 Sample Size Effects**

Uncertainty in estimated standard deviation:

$$
SE(\hat{\sigma}) \approx \frac{\sigma}{\sqrt{2(n-1)}}
$$

**For reliable capability estimates:**

- Minimum: $n \geq 30$
- Preferred: $n \geq 50$
- For critical processes: $n \geq 100$

**14.2 Confidence Interval for σ**

$$
\sqrt{\frac{(n-1)s^2}{\chi^2_{\alpha/2, n-1}}} \leq \sigma \leq \sqrt{\frac{(n-1)s^2}{\chi^2_{1-\alpha/2, n-1}}}
$$

**14.3 Rational Subgrouping**

**Principles:**

- Subgroups should capture short-term (within) variation
- Between-subgroup variation captures long-term drift
- Subgroup size $n$ typically 3–5 for continuous data

**In semiconductor:**

- Subgroup = wafers from same lot, run, or time window
- Site-to-site variation often treated as within-subgroup

**14.4 Control Limit Estimation**

**Using Range Method:**

$$
\hat{\sigma} = \frac{\bar{R}}{d_2}
$$

**Using Sample Standard Deviation:**

$$
\hat{\sigma} = \frac{\bar{s}}{c_4}
$$

Where $c_4$ is the unbiasing constant for standard deviation.

**14.5 Short-Run SPC**

For limited data (new process, low volume):

**Z-MR charts using target:**

$$
Z_i = \frac{x_i - T}{\sigma_0}
$$

**Q-charts (self-starting):**

$$
Q_i = \Phi^{-1}\left(F_{i-1}\left(\frac{x_i - \bar{x}_{i-1}}{s_{i-1}\sqrt{1 + 1/(i-1)}}\right)\right)
$$



**15. Key Mathematical Relationships**

**Quick Reference Table**

| Concept | Core Mathematics |
|---------|------------------|
| **Control Limits** | $\mu \pm \frac{3\sigma}{\sqrt{n}}$ |
| **Cp** | $\frac{USL - LSL}{6\sigma}$ |
| **Cpk** | $\min\left[\frac{USL-\mu}{3\sigma}, \frac{\mu-LSL}{3\sigma}\right]$ |
| **EWMA** | $\lambda x_t + (1-\lambda)EWMA_{t-1}$ |
| **CUSUM** | $\max[0, x_t - (\mu_0 + K) + C_{t-1}]$ |
| **Hotelling's T²** | $n(\bar{\mathbf{x}}-\boldsymbol{\mu})'S^{-1}(\bar{\mathbf{x}}-\boldsymbol{\mu})$ |
| **Gauge R&R** | $\sigma^2_{total} = \sigma^2_{part} + \sigma^2_{gauge}$ |
| **Yield (Poisson)** | $Y = e^{-D_0 A}$ |
| **ARL₀ (3σ)** | $\frac{1}{0.0027} \approx 370$ |
| **AR(1) Variance** | $\frac{\sigma^2_\varepsilon}{1-\phi^2}$ |

**Decision Guide: Which Chart to Use?**

| Situation | Recommended Chart |
|-----------|------------------|
| Standard monitoring, subgroups | X̄-R or X̄-S |
| Individual measurements | I-MR |
| Detect small shifts ($< 1.5\sigma$) | EWMA or CUSUM |
| Multiple correlated parameters | Hotelling's T² or MEWMA |
| Autocorrelated data | Residual charts or modified EWMA |
| Short production runs | Q-charts or Z-MR |

**Critical Success Factors**

1. **Validate measurement system first** (Gauge R&R < 10%)
2. **Ensure rational subgrouping** captures meaningful variation
3. **Check for autocorrelation** before applying standard charts
4. **Use appropriate capability indices** (Cpk vs Ppk)
5. **Decompose variance** to target improvement efforts
6. **Match chart sensitivity** to required detection speed



**Control Chart Constant Tables**

**Constants for X̄ and R Charts**

| $n$ | $A_2$ | $A_3$ | $d_2$ | $d_3$ | $D_3$ | $D_4$ | $c_4$ | $B_3$ | $B_4$ |
|-----|-------|-------|-------|-------|-------|-------|-------|-------|-------|
| 2 | 1.880 | 2.659 | 1.128 | 0.853 | 0 | 3.267 | 0.7979 | 0 | 3.267 |
| 3 | 1.023 | 1.954 | 1.693 | 0.888 | 0 | 2.574 | 0.8862 | 0 | 2.568 |
| 4 | 0.729 | 1.628 | 2.059 | 0.880 | 0 | 2.282 | 0.9213 | 0 | 2.266 |
| 5 | 0.577 | 1.427 | 2.326 | 0.864 | 0 | 2.114 | 0.9400 | 0 | 2.089 |
| 6 | 0.483 | 1.287 | 2.534 | 0.848 | 0 | 2.004 | 0.9515 | 0.030 | 1.970 |
| 7 | 0.419 | 1.182 | 2.704 | 0.833 | 0.076 | 1.924 | 0.9594 | 0.118 | 1.882 |
| 8 | 0.373 | 1.099 | 2.847 | 0.820 | 0.136 | 1.864 | 0.9650 | 0.185 | 1.815 |
| 9 | 0.337 | 1.032 | 2.970 | 0.808 | 0.184 | 1.816 | 0.9693 | 0.239 | 1.761 |
| 10 | 0.308 | 0.975 | 3.078 | 0.797 | 0.223 | 1.777 | 0.9727 | 0.284 | 1.716 |

**Standard Normal Distribution Critical Values**

| Confidence | $z_{\alpha/2}$ |
|------------|----------------|
| 90% | 1.645 |
| 95% | 1.960 |
| 99% | 2.576 |
| 99.73% | 3.000 |

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/capability) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
