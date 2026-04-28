# Regression Analysis

**Keywords**: regression analysis,regression,ols,least squares,pls,partial least squares,ridge,lasso,semiconductor regression,process regression

---

**Regression Analysis**


Semiconductor fabrication involves hundreds of sequential process steps, each governed by dozens of parameters. Regression analysis serves critical functions:

- Process Modeling: Understanding relationships between inputs and quality outputs
- Virtual Metrology: Predicting measurements from real-time sensor data
- Run-to-Run Control: Adaptive process adjustment
- Yield Optimization: Maximizing device performance and throughput
- Fault Detection: Identifying and diagnosing process excursions



Core Mathematical Framework

Ordinary Least Squares (OLS)

The foundational linear regression model:

$$
\mathbf{y} = \mathbf{X}\boldsymbol{\beta} + \boldsymbol{\varepsilon}
$$

Variable Definitions:

- $\mathbf{y}$ — $n \times 1$ response vector (e.g., film thickness, etch rate, yield)
- $\mathbf{X}$ — $n \times (k+1)$ design matrix of process parameters
- $\boldsymbol{\beta}$ — $(k+1) \times 1$ coefficient vector
- $\boldsymbol{\varepsilon} \sim N(\mathbf{0}, \sigma^2\mathbf{I})$ — error term

OLS Estimator:

$$
\hat{\boldsymbol{\beta}} = (\mathbf{X}^\top\mathbf{X})^{-1}\mathbf{X}^\top\mathbf{y}
$$

Variance-Covariance Matrix of Estimator:

$$
\text{Var}(\hat{\boldsymbol{\beta}}) = \sigma^2(\mathbf{X}^\top\mathbf{X})^{-1}
$$

Unbiased Variance Estimate:

$$
\hat{\sigma}^2 = \frac{\mathbf{e}^\top\mathbf{e}}{n - k - 1} = \frac{\sum_{i=1}^{n}(y_i - \hat{y}_i)^2}{n - k - 1}
$$



Response Surface Methodology (RSM)

Critical for semiconductor process optimization, RSM uses second-order polynomial models.

Second-Order Model

$$
y = \beta_0 + \sum_{i=1}^{k}\beta_i x_i + \sum_{i=1}^{k}\beta_{ii}x_i^2 + \sum_{i<j}\beta_{ij}x_i x_j + \varepsilon
$$

Matrix Form:

$$
y = \beta_0 + \mathbf{x}^\top\mathbf{b} + \mathbf{x}^\top\mathbf{B}\mathbf{x} + \varepsilon
$$

Where:

- $\mathbf{b}$ — vector of first-order coefficients
- $\mathbf{B}$ — symmetric matrix of second-order coefficients

Stationary Point Analysis

Stationary Point (Optimum):

$$
\mathbf{x}_s = -\frac{1}{2}\mathbf{B}^{-1}\mathbf{b}
$$

Nature Determination:

- All eigenvalues of $\mathbf{B}$ negative → Maximum
- All eigenvalues of $\mathbf{B}$ positive → Minimum
- Mixed signs → Saddle point

Canonical Analysis

$$
\hat{y} = \hat{y}_s + \sum_{i=1}^{k}\lambda_i w_i^2
$$

Where $\lambda_i$ are eigenvalues and $w_i$ are canonical variables.



Regularized Regression Methods

Semiconductor data often exhibits multicollinearity and high dimensionality.

Ridge Regression (L2 Penalty)

Objective Function:

$$
\min_{\boldsymbol{\beta}} \left\{ \|\mathbf{y} - \mathbf{X}\boldsymbol{\beta}\|_2^2 + \lambda\|\boldsymbol{\beta}\|_2^2 \right\}
$$

Closed-Form Solution:

$$
\hat{\boldsymbol{\beta}}_{\text{ridge}} = (\mathbf{X}^\top\mathbf{X} + \lambda\mathbf{I})^{-1}\mathbf{X}^\top\mathbf{y}
$$

Properties:

- Shrinks coefficients toward zero
- Does not perform variable selection
- Handles multicollinearity effectively

LASSO (L1 Penalty)

Objective Function:

$$
\min_{\boldsymbol{\beta}} \left\{ \|\mathbf{y} - \mathbf{X}\boldsymbol{\beta}\|_2^2 + \lambda\|\boldsymbol{\beta}\|_1 \right\}
$$

Properties:

- Performs automatic variable selection
- Sets some coefficients exactly to zero
- Crucial for identifying which process parameters matter

Elastic Net

Objective Function:

$$
\min_{\boldsymbol{\beta}} \left\{ \|\mathbf{y} - \mathbf{X}\boldsymbol{\beta}\|_2^2 + \lambda_1\|\boldsymbol{\beta}\|_1 + \lambda_2\|\boldsymbol{\beta}\|_2^2 \right\}
$$

Alternative Parameterization:

$$
\min_{\boldsymbol{\beta}} \left\{ \|\mathbf{y} - \mathbf{X}\boldsymbol{\beta}\|_2^2 + \lambda\left[\alpha\|\boldsymbol{\beta}\|_1 + (1-\alpha)\|\boldsymbol{\beta}\|_2^2\right] \right\}
$$

Where $\alpha \in [0,1]$ controls the mix between L1 and L2 penalties.



Partial Least Squares (PLS) Regression

The most important technique for semiconductor process modeling.

Why PLS?

- Handles high dimensionality ($p > n$)
- Addresses multicollinearity
- Captures latent variable structures
- Simultaneously models X and Y relationships

NIPALS Algorithm

1. Initialize: $\mathbf{u} = \mathbf{y}$

2. X-weight: 
   $$\mathbf{w} = \frac{\mathbf{X}^\top\mathbf{u}}{\|\mathbf{X}^\top\mathbf{u}\|}$$

3. X-score: 
   $$\mathbf{t} = \mathbf{X}\mathbf{w}$$

4. Y-loading: 
   $$q = \frac{\mathbf{y}^\top\mathbf{t}}{\mathbf{t}^\top\mathbf{t}}$$

5. Y-score update: 
   $$\mathbf{u} = \frac{\mathbf{y}q}{q^2}$$

6. Iterate until convergence

7. Deflate X and Y, extract next component

Model Structure

$$
\mathbf{X} = \mathbf{T}\mathbf{P}^\top + \mathbf{E}
$$

$$
\mathbf{Y} = \mathbf{T}\mathbf{Q}^\top + \mathbf{F}
$$

Where:

- $\mathbf{T}$ — score matrix (latent variables)
- $\mathbf{P}$ — X-loadings
- $\mathbf{Q}$ — Y-loadings
- $\mathbf{E}, \mathbf{F}$ — residuals



Spatial Regression for Wafer Maps

Wafer-level variation exhibits spatial patterns requiring specialized models.

Zernike Polynomial Decomposition

General Form:

$$
Z(r,\theta) = \sum_{n,m} a_{nm} Z_n^m(r,\theta)
$$

Standard Zernike Polynomials (first few terms):

| Index | Name | Formula |
|-------|------|---------|
| $Z_0^0$ | Piston | $1$ |
| $Z_1^{-1}$ | Tilt Y | $r\sin\theta$ |
| $Z_1^{1}$ | Tilt X | $r\cos\theta$ |
| $Z_2^{-2}$ | Astigmatism 45° | $r^2\sin 2\theta$ |
| $Z_2^{0}$ | Defocus | $2r^2 - 1$ |
| $Z_2^{2}$ | Astigmatism 0° | $r^2\cos 2\theta$ |
| $Z_3^{-1}$ | Coma Y | $(3r^3 - 2r)\sin\theta$ |
| $Z_3^{1}$ | Coma X | $(3r^3 - 2r)\cos\theta$ |
| $Z_4^{0}$ | Spherical | $6r^4 - 6r^2 + 1$ |

Orthogonality Property:

$$
\int_0^1 \int_0^{2\pi} Z_n^m(r,\theta) Z_{n'}^{m'}(r,\theta) \, r \, dr \, d\theta = \frac{\pi}{n+1}\delta_{nn'}\delta_{mm'}
$$

Gaussian Process Regression (Kriging)

Prior Distribution:

$$
f(\mathbf{x}) \sim \mathcal{GP}(m(\mathbf{x}), k(\mathbf{x}, \mathbf{x}'))
$$

Common Kernel Functions:

*Squared Exponential (RBF)*:

$$
k(\mathbf{x}, \mathbf{x}') = \sigma^2 \exp\left(-\frac{\|\mathbf{x} - \mathbf{x}'\|^2}{2\ell^2}\right)
$$

*Matérn Kernel*:

$$
k(r) = \sigma^2 \frac{2^{1-
u}}{\Gamma(
u)}\left(\frac{\sqrt{2
u}r}{\ell}\right)^
u K_
u\left(\frac{\sqrt{2
u}r}{\ell}\right)
$$

Where $K_
u$ is the modified Bessel function of the second kind.

Posterior Predictive Mean:

$$
\bar{f}_* = \mathbf{k}_*^\top(\mathbf{K} + \sigma_n^2\mathbf{I})^{-1}\mathbf{y}
$$

Posterior Predictive Variance:

$$
\text{Var}(f_*) = k(\mathbf{x}_*, \mathbf{x}_*) - \mathbf{k}_*^\top(\mathbf{K} + \sigma_n^2\mathbf{I})^{-1}\mathbf{k}_*
$$



Mixed Effects Models

Semiconductor data has hierarchical structure (wafers within lots, lots within tools).

General Model

$$
y_{ijk} = \mathbf{x}_{ijk}^\top\boldsymbol{\beta} + b_i^{(\text{tool})} + b_{ij}^{(\text{lot})} + \varepsilon_{ijk}
$$

Random Effects Distribution:

- $b_i^{(\text{tool})} \sim N(0, \sigma_{\text{tool}}^2)$
- $b_{ij}^{(\text{lot})} \sim N(0, \sigma_{\text{lot}}^2)$
- $\varepsilon_{ijk} \sim N(0, \sigma^2)$

Matrix Notation

$$
\mathbf{y} = \mathbf{X}\boldsymbol{\beta} + \mathbf{Z}\mathbf{b} + \boldsymbol{\varepsilon}
$$

Where:

- $\mathbf{b} \sim N(\mathbf{0}, \mathbf{G})$
- $\boldsymbol{\varepsilon} \sim N(\mathbf{0}, \mathbf{R})$
- $\text{Var}(\mathbf{y}) = \mathbf{V} = \mathbf{Z}\mathbf{G}\mathbf{Z}^\top + \mathbf{R}$

REML Estimation

Restricted Log-Likelihood:

$$
\ell_{\text{REML}}(\boldsymbol{\theta}) = -\frac{1}{2}\left[\log|\mathbf{V}| + \log|\mathbf{X}^\top\mathbf{V}^{-1}\mathbf{X}| + \mathbf{r}^\top\mathbf{V}^{-1}\mathbf{r}\right]
$$

Where $\mathbf{r} = \mathbf{y} - \mathbf{X}\hat{\boldsymbol{\beta}}$.



Physics-Informed Regression Models

Arrhenius-Based Models (Thermal Processes)

Rate Equation:

$$
k = A \exp\left(-\frac{E_a}{RT}\right)
$$

Linearized Form (for regression):

$$
\ln(k) = \ln(A) - \frac{E_a}{R} \cdot \frac{1}{T}
$$

Parameters:

- $k$ — rate constant
- $A$ — pre-exponential factor
- $E_a$ — activation energy (J/mol)
- $R$ — gas constant (8.314 J/mol·K)
- $T$ — absolute temperature (K)

Preston's Equation (CMP)

Basic Form:

$$
\text{MRR} = K_p \cdot P \cdot V
$$

Extended Model:

$$
\text{MRR} = K_p \cdot P^a \cdot V^b \cdot f(\text{slurry}, \text{pad})
$$

Where:

- MRR — material removal rate
- $K_p$ — Preston coefficient
- $P$ — applied pressure
- $V$ — relative velocity

Lithography Focus-Exposure Model

$$
\text{CD} = \beta_0 + \beta_1 E + \beta_2 F + \beta_3 E^2 + \beta_4 F^2 + \beta_5 EF + \varepsilon
$$

Variables:

- CD — critical dimension
- $E$ — exposure dose
- $F$ — focus offset

Bossung Curve: Plot of CD vs. focus at various exposure levels.



Virtual Metrology Mathematics

Predicting quality measurements from equipment sensor data in real-time.

Model Structure

$$
\hat{y} = f(\mathbf{x}_{\text{FDC}}; \boldsymbol{\theta})
$$

Where $\mathbf{x}_{\text{FDC}}$ is Fault Detection and Classification sensor data.

EWMA Run-to-Run Control

Exponentially Weighted Moving Average:

$$
\hat{T}_{n+1} = \lambda y_n + (1-\lambda)\hat{T}_n
$$

Properties:

- $\lambda \in (0,1]$ — smoothing parameter
- Smaller $\lambda$ → more smoothing
- Larger $\lambda$ → faster response to changes

Kalman Filter Approach

State Equation:

$$
\mathbf{x}_{k} = \mathbf{A}\mathbf{x}_{k-1} + \mathbf{w}_k, \quad \mathbf{w}_k \sim N(\mathbf{0}, \mathbf{Q})
$$

Measurement Equation:

$$
y_k = \mathbf{H}\mathbf{x}_k + v_k, \quad v_k \sim N(0, R)
$$

Update Equations:

*Predict*:
$$
\hat{\mathbf{x}}_{k|k-1} = \mathbf{A}\hat{\mathbf{x}}_{k-1|k-1}
$$

$$
\mathbf{P}_{k|k-1} = \mathbf{A}\mathbf{P}_{k-1|k-1}\mathbf{A}^\top + \mathbf{Q}
$$

*Update*:
$$
\mathbf{K}_k = \mathbf{P}_{k|k-1}\mathbf{H}^\top(\mathbf{H}\mathbf{P}_{k|k-1}\mathbf{H}^\top + R)^{-1}
$$

$$
\hat{\mathbf{x}}_{k|k} = \hat{\mathbf{x}}_{k|k-1} + \mathbf{K}_k(y_k - \mathbf{H}\hat{\mathbf{x}}_{k|k-1})
$$



Classification and Count Models

Logistic Regression (Binary Outcomes)

For pass/fail or defect/no-defect classification:

Model:

$$
P(Y=1|\mathbf{x}) = \frac{1}{1 + \exp(-\mathbf{x}^\top\boldsymbol{\beta})} = \sigma(\mathbf{x}^\top\boldsymbol{\beta})
$$

Logit Link:

$$
\text{logit}(p) = \ln\left(\frac{p}{1-p}\right) = \mathbf{x}^\top\boldsymbol{\beta}
$$

Log-Likelihood:

$$
\ell(\boldsymbol{\beta}) = \sum_{i=1}^{n}\left[y_i \log(\pi_i) + (1-y_i)\log(1-\pi_i)\right]
$$

Newton-Raphson Update:

$$
\boldsymbol{\beta}^{(t+1)} = \boldsymbol{\beta}^{(t)} + (\mathbf{X}^\top\mathbf{W}\mathbf{X})^{-1}\mathbf{X}^\top(\mathbf{y} - \boldsymbol{\pi})
$$

Where $\mathbf{W} = \text{diag}(\pi_i(1-\pi_i))$.

Poisson Regression (Defect Counts)

Model:

$$
\log(\mu) = \mathbf{x}^\top\boldsymbol{\beta}, \quad Y \sim \text{Poisson}(\mu)
$$

Probability Mass Function:

$$
P(Y = y) = \frac{\mu^y e^{-\mu}}{y!}
$$



Model Validation and Diagnostics

Goodness of Fit Metrics

Coefficient of Determination:

$$
R^2 = 1 - \frac{\text{SSE}}{\text{SST}} = 1 - \frac{\sum_{i=1}^{n}(y_i - \hat{y}_i)^2}{\sum_{i=1}^{n}(y_i - \bar{y})^2}
$$

Adjusted R-Squared:

$$
R^2_{\text{adj}} = 1 - (1-R^2)\frac{n-1}{n-k-1}
$$

Root Mean Square Error:

$$
\text{RMSE} = \sqrt{\frac{1}{n}\sum_{i=1}^{n}(y_i - \hat{y}_i)^2}
$$

Mean Absolute Error:

$$
\text{MAE} = \frac{1}{n}\sum_{i=1}^{n}|y_i - \hat{y}_i|
$$

Cross-Validation

K-Fold CV Error:

$$
\text{CV}_{(K)} = \frac{1}{K}\sum_{k=1}^{K}\text{MSE}_k
$$

Leave-One-Out CV:

$$
\text{LOOCV} = \frac{1}{n}\sum_{i=1}^{n}(y_i - \hat{y}_{(-i)})^2
$$

Information Criteria

Akaike Information Criterion:

$$
\text{AIC} = 2k - 2\ln(\hat{L})
$$

Bayesian Information Criterion:

$$
\text{BIC} = k\ln(n) - 2\ln(\hat{L})
$$

Diagnostic Statistics

Variance Inflation Factor:

$$
\text{VIF}_j = \frac{1}{1-R_j^2}
$$

Where $R_j^2$ is the $R^2$ from regressing $x_j$ on all other predictors.

Rule of thumb: VIF > 10 indicates problematic multicollinearity.

Cook's Distance:

$$
D_i = \frac{(\hat{\mathbf{y}} - \hat{\mathbf{y}}_{(-i)})^\top(\hat{\mathbf{y}} - \hat{\mathbf{y}}_{(-i)})}{k \cdot \text{MSE}}
$$

Leverage:

$$
h_{ii} = [\mathbf{H}]_{ii}
$$

Where $\mathbf{H} = \mathbf{X}(\mathbf{X}^\top\mathbf{X})^{-1}\mathbf{X}^\top$ is the hat matrix.

Studentized Residuals:

$$
r_i = \frac{e_i}{\hat{\sigma}\sqrt{1 - h_{ii}}}
$$



Bayesian Regression

Provides full uncertainty quantification for risk-sensitive manufacturing decisions.

Bayesian Linear Regression

Prior:

$$
\boldsymbol{\beta} | \sigma^2 \sim N(\boldsymbol{\beta}_0, \sigma^2\mathbf{V}_0)
$$

$$
\sigma^2 \sim \text{Inverse-Gamma}(a_0, b_0)
$$

Posterior:

$$
\boldsymbol{\beta} | \mathbf{y}, \sigma^2 \sim N(\boldsymbol{\beta}_n, \sigma^2\mathbf{V}_n)
$$

Posterior Parameters:

$$
\mathbf{V}_n = (\mathbf{V}_0^{-1} + \mathbf{X}^\top\mathbf{X})^{-1}
$$

$$
\boldsymbol{\beta}_n = \mathbf{V}_n(\mathbf{V}_0^{-1}\boldsymbol{\beta}_0 + \mathbf{X}^\top\mathbf{y})
$$

Predictive Distribution

$$
p(y_*|\mathbf{x}_*, \mathbf{y}) = \int p(y_*|\mathbf{x}_*, \boldsymbol{\beta}, \sigma^2) \, p(\boldsymbol{\beta}, \sigma^2|\mathbf{y}) \, d\boldsymbol{\beta} \, d\sigma^2
$$

For conjugate priors, this is a Student-t distribution.

Credible Intervals

95% Credible Interval for $\beta_j$:

$$
\beta_j \in \left[\hat{\beta}_j - t_{0.025,
u}\cdot \text{SE}(\hat{\beta}_j), \quad \hat{\beta}_j + t_{0.025,
u}\cdot \text{SE}(\hat{\beta}_j)\right]
$$



Design of Experiments (DOE)

Full Factorial Design

For $k$ factors at 2 levels:

$$
N = 2^k \text{ runs}
$$

Fractional Factorial Design

$$
N = 2^{k-p} \text{ runs}
$$

Resolution:

- Resolution III: Main effects aliased with 2-factor interactions
- Resolution IV: Main effects clear; 2FIs aliased with each other
- Resolution V: Main effects and 2FIs clear

Central Composite Design (CCD)

Components:

- $2^k$ factorial points
- $2k$ axial (star) points at distance $\alpha$
- $n_0$ center points

Rotatability Condition:

$$
\alpha = (2^k)^{1/4}
$$

D-Optimal Design

Maximizes the determinant of the information matrix:

$$
\max_{\mathbf{X}} |\mathbf{X}^\top\mathbf{X}|
$$

Equivalently, minimizes the generalized variance of $\hat{\boldsymbol{\beta}}$.

I-Optimal Design

Minimizes average prediction variance:

$$
\min_{\mathbf{X}} \int_{\mathcal{R}} \text{Var}(\hat{y}(\mathbf{x})) \, d\mathbf{x}
$$



Reliability Analysis

Cox Proportional Hazards Model

Hazard Function:

$$
h(t|\mathbf{x}) = h_0(t) \cdot \exp(\mathbf{x}^\top\boldsymbol{\beta})
$$

Where:

- $h(t|\mathbf{x})$ — hazard at time $t$ given covariates $\mathbf{x}$
- $h_0(t)$ — baseline hazard
- $\boldsymbol{\beta}$ — regression coefficients

Partial Likelihood

$$
L(\boldsymbol{\beta}) = \prod_{i: \delta_i = 1} \frac{\exp(\mathbf{x}_i^\top\boldsymbol{\beta})}{\sum_{j \in \mathcal{R}(t_i)} \exp(\mathbf{x}_j^\top\boldsymbol{\beta})}
$$

Where $\mathcal{R}(t_i)$ is the risk set at time $t_i$.



Challenge-Method Mapping

| Manufacturing Challenge | Mathematical Approach |
|------------------------|----------------------|
| High dimensionality | PLS, LASSO, Elastic Net |
| Multicollinearity | Ridge regression, PCR, VIF analysis |
| Spatial wafer patterns | Zernike polynomials, GP regression |
| Hierarchical data | Mixed effects models, REML |
| Nonlinear processes | RSM, polynomial models, transformations |
| Physics constraints | Arrhenius, Preston equation integration |
| Uncertainty quantification | Bayesian methods, bootstrap, prediction intervals |
| Binary outcomes | Logistic regression |
| Count data | Poisson regression |
| Real-time control | Kalman filter, EWMA |
| Time-to-failure | Cox proportional hazards |



Equations Quick Reference

Estimation

$$
\hat{\boldsymbol{\beta}}_{\text{OLS}} = (\mathbf{X}^\top\mathbf{X})^{-1}\mathbf{X}^\top\mathbf{y}
$$

$$
\hat{\boldsymbol{\beta}}_{\text{Ridge}} = (\mathbf{X}^\top\mathbf{X} + \lambda\mathbf{I})^{-1}\mathbf{X}^\top\mathbf{y}
$$

Prediction Interval

$$
\hat{y}_0 \pm t_{\alpha/2, n-k-1} \cdot \sqrt{\text{MSE}\left(1 + \mathbf{x}_0^\top(\mathbf{X}^\top\mathbf{X})^{-1}\mathbf{x}_0\right)}
$$

Confidence Interval for $\beta_j$

$$
\hat{\beta}_j \pm t_{\alpha/2, n-k-1} \cdot \text{SE}(\hat{\beta}_j)
$$

Process Capability

$$
C_p = \frac{\text{USL} - \text{LSL}}{6\sigma}
$$

$$
C_{pk} = \min\left(\frac{\text{USL} - \mu}{3\sigma}, \frac{\mu - \text{LSL}}{3\sigma}\right)
$$



Reference

| Symbol | Description |
|--------|-------------|
| $\mathbf{y}$ | Response vector |
| $\mathbf{X}$ | Design matrix |
| $\boldsymbol{\beta}$ | Coefficient vector |
| $\hat{\boldsymbol{\beta}}$ | Estimated coefficients |
| $\boldsymbol{\varepsilon}$ | Error vector |
| $\sigma^2$ | Error variance |
| $\lambda$ | Regularization parameter |
| $\mathbf{I}$ | Identity matrix |
| $\|\cdot\|_1$ | L1 norm (sum of absolute values) |
| $\|\cdot\|_2$ | L2 norm (Euclidean) |
| $\mathbf{A}^\top$ | Matrix transpose |
| $\mathbf{A}^{-1}$ | Matrix inverse |
| $|\mathbf{A}|$ | Matrix determinant |
| $N(\mu, \sigma^2)$ | Normal distribution |
| $\mathcal{GP}$ | Gaussian Process |

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/regression-analysis) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
