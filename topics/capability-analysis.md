# Semiconductor Etch Process Capability Mathematics

**Keywords**: capability analysis, spc, statistical process control, etch capability, process metrics, defect analysis, yield optimization

---

**Semiconductor Etch Process Capability Mathematics**







**1. Fundamental Capability Indices**

**1.1 Basic Statistical Measures**

- **Sample Mean ($\bar{x}$):**

$$
\bar{x} = \frac{1}{n}\sum_{i=1}^{n} x_i
$$

- **Sample Standard Deviation ($s$):**

$$
s = \sqrt{\frac{1}{n-1}\sum_{i=1}^{n}(x_i - \bar{x})^2}
$$

**1.2 Process Capability (Cp)**

The **potential capability** measures the process spread relative to specification width:

$$
C_p = \frac{USL - LSL}{6\sigma}
$$

Where:
- $USL$ = Upper Specification Limit
- $LSL$ = Lower Specification Limit
- $\sigma$ = Process standard deviation

**Interpretation:**
- $C_p = 1.0$ means the process $\pm 3\sigma$ exactly fills the spec window
- Higher $C_p$ indicates greater potential capability

**1.3 Process Capability Index (Cpk)**

The **actual capability** accounts for process centering:

$$
C_{pk} = \min\left(\frac{USL - \mu}{3\sigma}, \frac{\mu - LSL}{3\sigma}\right)
$$

**Key relationship:**
- $C_{pk} \leq C_p$ (always)
- $C_{pk} = C_p$ only when process is perfectly centered

**1.4 Taguchi Capability Index (Cpm)**

Penalizes deviation from target $T$, not merely being within spec:

$$
C_{pm} = \frac{USL - LSL}{6\sqrt{\sigma^2 + (\mu - T)^2}}
$$

**1.5 Combined Index (Cpkm)**

$$
C_{pkm} = \frac{C_{pk}}{\sqrt{1 + \left(\frac{\mu - T}{\sigma}\right)^2}}
$$

**1.6 Industry Targets for Semiconductor Etch**

| Cpk Value | Sigma Level | Defect Rate | Typical Application |
|:---------:|:-----------:|:-----------:|:-------------------:|
| 1.00 | 3σ | 2,700 ppm | Minimum acceptable |
| 1.33 | 4σ | 63 ppm | Standard processes |
| 1.67 | 5σ | 0.57 ppm | Critical dimensions |
| 2.00 | 6σ | 0.002 ppm | Advanced nodes |



**2. Etch-Specific Uniformity Mathematics**

**2.1 Within-Wafer Uniformity (WIW)**

- **Range-based method:**

$$
\%U_{WIW} = \frac{X_{max} - X_{min}}{2 \cdot \bar{X}} \times 100\%
$$

- **Standard deviation-based method (preferred):**

$$
\%U_{1\sigma} = \frac{s}{\bar{X}} \times 100\%
$$

- **Typical target:** $<1\%$ $(1\sigma)$ uniformity for etch rate

**2.2 Wafer-to-Wafer Uniformity (WtW)**

$$
\%U_{WtW} = \frac{s_{\text{wafer means}}}{\bar{X}_{\text{overall}}} \times 100\%
$$

**2.3 Total Variance Decomposition**

Via nested ANOVA:

$$
\sigma^2_{\text{total}} = \sigma^2_{WIW} + \sigma^2_{WtW} + \sigma^2_{LtL} + \sigma^2_{TtT}
$$

Where:
- $\sigma^2_{WIW}$ = Within-Wafer variance
- $\sigma^2_{WtW}$ = Wafer-to-Wafer variance
- $\sigma^2_{LtL}$ = Lot-to-Lot variance
- $\sigma^2_{TtT}$ = Tool-to-Tool (chamber-to-chamber) variance



**3. Critical Dimension (CD) Control**

**3.1 CD Uniformity**

$$
CD_{\text{uniformity}} = \frac{CD_{max} - CD_{min}}{CD_{target}} \times 100\%
$$

**3.2 Etch Bias**

$$
\text{Etch Bias} = CD_{\text{after etch}} - CD_{\text{after litho}}
$$

For anisotropic etch with undercut angle $\theta$:

$$
\Delta CD = 2 \cdot d \cdot \tan(\theta)
$$

Where:
- $d$ = etch depth
- $\theta$ = undercut angle
- For ideal anisotropic etch: $\theta = 0 \Rightarrow \Delta CD = 0$

**3.3 Iso-Dense Bias (IDB)**

$$
IDB = CD_{\text{isolated}} - CD_{\text{dense}}
$$

**Capability for IDB:**

$$
C_{pk,IDB} = \min\left(\frac{IDB_{USL} - \overline{IDB}}{3s_{IDB}}, \frac{\overline{IDB} - IDB_{LSL}}{3s_{IDB}}\right)
$$

**3.4 Line Edge Roughness (LER) / Line Width Roughness (LWR)**

- **LER Definition:**

$$
LER = 3\sigma_{\text{edge position}}
$$

- **LWR Definition:**

$$
LWR = 3\sigma_{\text{line width}}
$$

- **One-sided capability (upper limit only):**

$$
C_{pk,LER} = \frac{USL_{LER} - \overline{LER}}{3s_{LER}}
$$



**4. Selectivity Mathematics**

**4.1 Basic Selectivity Definition**

$$
\text{Selectivity} = \frac{ER_{\text{target material}}}{ER_{\text{mask or stop layer}}}
$$

**4.2 Selectivity Capability (One-Sided)**

$$
C_{pk,sel} = \frac{\overline{Sel} - LSL_{Sel}}{3s_{Sel}}
$$

**Note:** Higher selectivity is always better, so this is typically a one-sided specification.

**4.3 Common Selectivity Requirements**

| Etch Type | Material System | Typical Selectivity |
|:----------|:----------------|:-------------------:|
| SAC Etch | Oxide:Nitride | >30:1 |
| Gate Etch | Poly-Si:Oxide | >50:1 |
| Metal Etch | Al:Resist | >5:1 |
| Via Etch | Oxide:TiN | >20:1 |



**5. Variance Component Analysis**

**5.1 Mixed-Effects Model**

$$
X_{ijkl} = \mu + W_i + L_j + T_k + S_{l(ijk)} + \epsilon_{ijkl}
$$

Where:
- $\mu$ = Grand mean
- $W_i$ = Wafer random effect
- $L_j$ = Lot random effect
- $T_k$ = Tool/chamber random effect
- $S_{l(ijk)}$ = Site (within-wafer) effect
- $\epsilon_{ijkl}$ = Residual measurement error

**5.2 Variance Component Estimation**

Via REML (Restricted Maximum Likelihood):

$$
\hat{\sigma}^2_{\text{total}} = \hat{\sigma}^2_W + \hat{\sigma}^2_L + \hat{\sigma}^2_T + \hat{\sigma}^2_S + \hat{\sigma}^2_\epsilon
$$

**5.3 Percent Contribution**

$$
\%\text{Contribution}_i = \frac{\hat{\sigma}^2_i}{\hat{\sigma}^2_{\text{total}}} \times 100\%
$$



**6. Response Surface Modeling for Etch**

**6.1 Second-Order Polynomial Model**

$$
ER = \beta_0 + \sum_{i}\beta_i x_i + \sum_{i}\beta_{ii}x_i^2 + \sum_{i<j}\beta_{ij}x_i x_j + \epsilon
$$

Where $x_i$ represents process parameters:
- $P$ = RF Power
- $p$ = Chamber pressure
- $F$ = Gas flow rate
- $T$ = Temperature

**6.2 Process Window Definition**

$$
\mathcal{W} = \bigcap_{i=1}^{n} \{(P, p, F, T) : LSL_i \leq Y_i \leq USL_i\}
$$

**6.3 Desirability Function**

**Overall desirability:**

$$
D = \left(\prod_{i=1}^{n} d_i^{w_i}\right)^{1/\sum w_i}
$$

**Individual desirability functions:**

- **Target is best:**

$$
d = \exp\left(-\left|\frac{y-T}{s}\right|^r\right)
$$

- **Larger is better:**

$$
d = \left(\frac{y - L}{T - L}\right)^r \quad \text{for } L < y < T
$$

- **Smaller is better:**

$$
d = \left(\frac{U - y}{U - T}\right)^r \quad \text{for } T < y < U
$$



**7. Loading Effect Models**

**7.1 Macro-Loading**

As exposed area $A$ increases, etch rate decreases:

$$
ER(A) = ER_0 \cdot \frac{1}{1 + kA}
$$

**7.2 Micro-Loading (ARDE)**

**Aspect Ratio Dependent Etching:**

$$
\frac{ER_{\text{trench}}}{ER_{\text{open}}} = f(AR) = f\left(\frac{\text{depth}}{\text{width}}\right)
$$

**Knudsen diffusion model:**

$$
ER \propto \frac{1}{1 + \alpha \cdot AR}
$$

**7.3 RIE Lag Correction**

For high aspect ratio features $(AR > 20:1)$:

$$
ER_{\text{corrected}} = ER_{\text{open}} \cdot \exp\left(-\beta \cdot AR^{\gamma}\right)
$$



**8. Statistical Process Control Mathematics**

**8.1 X-bar Chart Control Limits**

$$
UCL_{\bar{x}} = \bar{\bar{x}} + A_2 \bar{R}
$$

$$
LCL_{\bar{x}} = \bar{\bar{x}} - A_2 \bar{R}
$$

**8.2 R Chart Control Limits**

$$
UCL_R = D_4 \bar{R}
$$

$$
LCL_R = D_3 \bar{R}
$$

**Control chart constants (selected values):**

| n | $A_2$ | $D_3$ | $D_4$ |
|:-:|:-----:|:-----:|:-----:|
| 2 | 1.880 | 0 | 3.267 |
| 3 | 1.023 | 0 | 2.575 |
| 4 | 0.729 | 0 | 2.282 |
| 5 | 0.577 | 0 | 2.115 |

**8.3 EWMA (Exponentially Weighted Moving Average)**

**Recursive formula:**

$$
EWMA_t = \lambda x_t + (1-\lambda)EWMA_{t-1}
$$

**Control limits:**

$$
UCL = \mu_0 + L\sigma\sqrt{\frac{\lambda}{2-\lambda}\left[1-(1-\lambda)^{2t}\right]}
$$

$$
LCL = \mu_0 - L\sigma\sqrt{\frac{\lambda}{2-\lambda}\left[1-(1-\lambda)^{2t}\right]}
$$

**Typical parameters:**
- $\lambda = 0.2$
- $L = 3$

**8.4 CUSUM (Cumulative Sum)**

**Upper CUSUM:**

$$
C^+_t = \max[0, x_t - (\mu_0 + K) + C^+_{t-1}]
$$

**Lower CUSUM:**

$$
C^-_t = \max[0, (\mu_0 - K) - x_t + C^-_{t-1}]
$$

Where:
- $K = \frac{\delta \sigma}{2}$ (reference value)
- $H = h\sigma$ (decision interval)



**9. Endpoint Detection Mathematics**

**9.1 Interferometric Endpoint**

$$
d = \frac{N \lambda}{2n \cos\theta}
$$

Where:
- $N$ = Number of interference fringes counted
- $\lambda$ = Wavelength of light
- $n$ = Refractive index of material
- $\theta$ = Angle of incidence

**9.2 Optical Emission Spectroscopy (OES)**

**Endpoint trigger condition:**

$$
\left|\frac{dI(\lambda, t)}{dt}\right| > \text{threshold}
$$

**Normalized derivative:**

$$
\frac{d}{dt}\left[\frac{I(\lambda, t)}{I_{ref}}\right] > \text{threshold}
$$

**9.3 Multi-Wavelength PCA Endpoint**

Principal component score:

$$
PC_1(t) = \sum_{i=1}^{p} w_i \cdot I_i(t)
$$

Where $w_i$ are PCA loadings for wavelength $i$.



**10. Measurement System Analysis (Gauge R&R)**

**10.1 Variance Decomposition**

**Total observed variance:**

$$
\sigma^2_{\text{observed}} = \sigma^2_{\text{part}} + \sigma^2_{\text{measurement}}
$$

**Measurement variance:**

$$
\sigma^2_{\text{measurement}} = \sigma^2_{\text{repeatability}} + \sigma^2_{\text{reproducibility}}
$$

**10.2 Percent GRR Calculations**

**To total variation:**

$$
\%GRR_{\text{TV}} = \frac{\sigma_{\text{GRR}}}{\sigma_{\text{total}}} \times 100\%
$$

**To tolerance:**

$$
\%GRR_{\text{Tol}} = \frac{6\sigma_{\text{GRR}}}{USL - LSL} \times 100\%
$$

**10.3 GRR Assessment Criteria**

| %GRR | Assessment | Action |
|:----:|:----------:|:------:|
| <10% | Excellent | Acceptable |
| 10-30% | Marginal | May be acceptable |
| >30% | Unacceptable | Improve measurement system |

**10.4 Number of Distinct Categories (ndc)**

$$
ndc = 1.41 \cdot \frac{\sigma_{\text{part}}}{\sigma_{\text{GRR}}}
$$

**Requirement:** $ndc \geq 5$



**11. Confidence Intervals for Capability**

**11.1 Confidence Interval for Cp**

**Chi-square based:**

$$
P\left(\hat{C}_p \sqrt{\frac{\chi^2_{n-1, 1-\alpha/2}}{n-1}} \leq C_p \leq \hat{C}_p \sqrt{\frac{\chi^2_{n-1, \alpha/2}}{n-1}}\right) = 1-\alpha
$$

**Approximate form:**

$$
\hat{C}_p \pm z_{\alpha/2}\sqrt{\frac{C_p^2}{2(n-1)}}
$$

**11.2 Lower Confidence Bound for Cpk**

$$
LCL_{C_{pk}} = \hat{C}_{pk} - z_{\alpha}\sqrt{\frac{1}{9n\hat{C}_{pk}^2} + \frac{1}{2(n-1)}}
$$

**11.3 Sample Size Guidelines**

**Rule of thumb for Cpk studies:**
- Minimum: $n \geq 50$ data points
- Recommended: $n \geq 100$ data points
- For high confidence: $n \geq 200$ data points



**12. Non-Normal Data Handling**

**12.1 Box-Cox Transformation**

$$
y^{(\lambda)} = \begin{cases} 
\dfrac{y^\lambda - 1}{\lambda} & \text{if } \lambda 
eq 0 \\[10pt]
\ln(y) & \text{if } \lambda = 0 
\end{cases}
$$

**Common transformations:**
- $\lambda = 0.5$: Square root
- $\lambda = 0$: Natural log
- $\lambda = -1$: Inverse

**12.2 Percentile-Based Capability**

$$
C_p = \frac{USL - LSL}{X_{99.865\%} - X_{0.135\%}}
$$

$$
C_{pk} = \min\left(\frac{USL - X_{50\%}}{X_{99.865\%} - X_{50\%}}, \frac{X_{50\%} - LSL}{X_{50\%} - X_{0.135\%}}\right)
$$

**12.3 Johnson Transformation System**

**Three distribution families:**

- **$S_B$ (bounded):**

$$
z = \gamma + \delta \ln\left(\frac{x - \xi}{\lambda + \xi - x}\right)
$$

- **$S_L$ (lognormal):**

$$
z = \gamma + \delta \ln(x - \xi)
$$

- **$S_U$ (unbounded):**

$$
z = \gamma + \delta \sinh^{-1}\left(\frac{x - \xi}{\lambda}\right)
$$


**13. Multivariate Capability**

**13.1 Multivariate Capability Index (MCp)**

$$
MC_p = \frac{\text{Vol}(\text{specification region})}{\text{Vol}(\text{process region})}
$$

**13.2 Principal Component Approach**

For correlated outputs, transform to uncorrelated PCs:

$$
\mathbf{z} = \mathbf{P}^T(\mathbf{x} - \boldsymbol{\mu})
$$

Where $\mathbf{P}$ is the matrix of eigenvectors.

**Capability on each PC:**

$$
C_{pk,i} = \frac{\min(|USL_{z_i}|, |LSL_{z_i}|)}{3\sqrt{\lambda_i}}
$$

Where $\lambda_i$ is the eigenvalue (variance) of PC $i$.

**13.3 Hotelling's T² Statistic**

$$
T^2 = n(\bar{\mathbf{x}} - \boldsymbol{\mu}_0)^T \mathbf{S}^{-1} (\bar{\mathbf{x}} - \boldsymbol{\mu}_0)
$$

**Control limit:**

$$
UCL = \frac{p(n-1)(n+1)}{n(n-p)} F_{\alpha, p, n-p}
$$



**14. Practical Example: Gate Etch Capability Study**

**14.1 Process Specifications**

| Parameter | Target | LSL | USL | Unit |
|:----------|:------:|:---:|:---:|:----:|
| CD | 45 | 42 | 48 | nm |
| Etch Depth | 200 | 190 | 210 | nm |
| Selectivity | >20:1 | 20 | - | ratio |
| LWR | <4 | - | 4 | nm |

**14.2 Data Collection**

- **Wafers:** 25 wafers
- **Sites per wafer:** 49 sites
- **Total measurements:** $25 \times 49 = 1,225$

**14.3 Results Summary**

| Parameter | Mean | σ | Cpk | Status |
|:----------|:----:|:-:|:---:|:------:|
| CD | 44.8 nm | 0.9 nm | 1.03 | ❌ Below target |
| Depth | 199 nm | 2.5 nm | 1.33 | ✓ Acceptable |
| LWR | 3.2 nm | 0.4 nm | 0.67 | ❌ Major issue |

**14.4 Cpk Calculations**

**CD Cpk:**

$$
C_{pk,CD} = \min\left(\frac{48-44.8}{3 \times 0.9}, \frac{44.8-42}{3 \times 0.9}\right) = \min(1.19, 1.04) = 1.04
$$

**Depth Cpk:**

$$
C_{pk,Depth} = \min\left(\frac{210-199}{3 \times 2.5}, \frac{199-190}{3 \times 2.5}\right) = \min(1.47, 1.20) = 1.20
$$

**LWR Cpk (one-sided):**

$$
C_{pk,LWR} = \frac{4 - 3.2}{3 \times 0.4} = \frac{0.8}{1.2} = 0.67
$$

**14.5 Variance Decomposition for CD**

| Source | Variance (nm²) | % Contribution |
|:-------|:--------------:|:--------------:|
| Within-Wafer | 0.53 | 65% |
| Wafer-to-Wafer | 0.16 | 20% |
| Measurement | 0.12 | 15% |
| **Total** | **0.81** | **100%** |

**Conclusions:**
- Chamber uniformity issue (WIW dominant)
- Consider improving CD-SEM recipe to reduce measurement variance



**Key Mathematical Tools**

| Application | Key Mathematics |
|:------------|:----------------|
| Basic capability | $C_p$, $C_{pk}$, $C_{pm}$ |
| Uniformity | $1\sigma\%$, range-based $\%$ |
| Variance sourcing | Nested ANOVA, variance components |
| Process optimization | RSM, desirability functions |
| Drift detection | EWMA, CUSUM charts |
| Measurement quality | Gauge R&R, $\%GRR$, $ndc$ |
| Non-normal data | Box-Cox, percentile methods |
| Loading effects | ARDE models, Knudsen transport |
| Multi-response | Multivariate $C_p$, Hotelling's $T^2$ |



**Quick Reference: Essential Formulas**

```
-
┌─────────────────────────────────────────────────────────────┐
│  Cp  = (USL - LSL) / 6σ                                     │
│  Cpk = min[(USL - μ)/3σ, (μ - LSL)/3σ]                      │
│  %U  = (s / x̄) × 100%                                       │
│  GRR = √(σ²_repeatability + σ²_reproducibility)             │
│  EWMA_t = λx_t + (1-λ)EWMA_{t-1}                            │
└─────────────────────────────────────────────────────────────┘
```

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/capability-analysis) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
