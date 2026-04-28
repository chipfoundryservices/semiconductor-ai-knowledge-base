# Optimization: Mathematical Modeling

**Keywords**: process optimization,recipe optimization,response surface methodology,rsm,gaussian process,bayesian optimization,run to run control,r2r,robust optimization,multi-objective optimization

---

**Optimization: Mathematical Modeling**



   1. Context

A recipe  is a vector of controllable parameters:

$$
\mathbf{x} = \begin{bmatrix} T \\ P \\ Q_1 \\ Q_2 \\ \vdots \\ t \\ P_{\text{RF}} \end{bmatrix} \in \mathbb{R}^n
$$

Where:

- $T$ = Temperature (°C or K)
- $P$ = Pressure (mTorr or Pa)
- $Q_i$ = Gas flow rates (sccm)
- $t$ = Process time (seconds)
- $P_{\text{RF}}$ = RF power (Watts)

 Goal : Find optimal $\mathbf{x}$ such that output properties $\mathbf{y}$ meet specifications while accounting for variability.



   2. Mathematical Modeling Approaches

    2.1 Physics-Based (First-Principles) Models

     Chemical Vapor Deposition (CVD) Example

 Mass transport and reaction equation: 

$$
\frac{\partial C}{\partial t} + 
abla \cdot (\mathbf{u}C) = D
abla^2 C + R(C, T)
$$

Where:

- $C$ = Species concentration
- $\mathbf{u}$ = Velocity field
- $D$ = Diffusion coefficient
- $R(C, T)$ = Reaction rate

 Surface reaction kinetics (Arrhenius form): 

$$
k_s = A \exp\left(-\frac{E_a}{RT}\right)
$$

Where:

- $A$ = Pre-exponential factor
- $E_a$ = Activation energy
- $R$ = Gas constant
- $T$ = Temperature

 Deposition rate (transport-limited regime): 

$$
r = \frac{k_s C_s}{1 + \frac{k_s}{h_g}}
$$

Where:

- $C_s$ = Surface concentration
- $h_g$ = Gas-phase mass transfer coefficient

 Characteristics: 

-  Advantages : Extrapolates outside training data, physically interpretable
-  Disadvantages : Computationally expensive, requires detailed mechanism knowledge



    2.2 Empirical/Statistical Models (Response Surface Methodology)

 Second-order polynomial model: 

$$
y = \beta_0 + \sum_{i=1}^{n}\beta_i x_i + \sum_{i=1}^{n}\beta_{ii}x_i^2 + \sum_{i<j}\beta_{ij}x_i x_j + \varepsilon
$$

Where:

- $\beta_0$ = Intercept
- $\beta_i$ = Linear coefficients
- $\beta_{ii}$ = Quadratic coefficients
- $\beta_{ij}$ = Interaction coefficients
- $\varepsilon$ = Random error, $\varepsilon \sim \mathcal{N}(0, \sigma^2)$

 Matrix form: 

$$
\mathbf{y} = \mathbf{X}\boldsymbol{\beta} + \boldsymbol{\varepsilon}
$$

 Least squares estimator: 

$$
\hat{\boldsymbol{\beta}} = (\mathbf{X}^T\mathbf{X})^{-1}\mathbf{X}^T\mathbf{y}
$$

 Design of Experiments (DOE) Options: 

- Central Composite Design (CCD)
- Box-Behnken Design
- D-optimal designs
- Fractional factorial designs



    2.3 Gaussian Process (Kriging) Models

 GP prior: 

$$
y(\mathbf{x}) \sim \mathcal{GP}\big(m(\mathbf{x}), k(\mathbf{x}, \mathbf{x}')\big)
$$

 Squared exponential (RBF) kernel: 

$$
k(\mathbf{x}, \mathbf{x}') = \sigma_f^2 \exp\left(-\frac{1}{2}\sum_{d=1}^{n}\frac{(x_d - x'_d)^2}{\ell_d^2}\right)
$$

Where:

- $\sigma_f^2$ = Signal variance
- $\ell_d$ = Length scale for dimension $d$

 Matérn 5/2 kernel (alternative): 

$$
k(\mathbf{x}, \mathbf{x}') = \sigma_f^2 \left(1 + \sqrt{5}r + \frac{5r^2}{3}\right) \exp\left(-\sqrt{5}r\right)
$$

Where $r = \sqrt{\sum_d \frac{(x_d - x'_d)^2}{\ell_d^2}}$

 Predictive mean: 

$$
\mu(\mathbf{x}_*) = \mathbf{k}_*^T(\mathbf{K} + \sigma_n^2\mathbf{I})^{-1}\mathbf{y}
$$

 Predictive variance: 

$$
\sigma^2(\mathbf{x}_*) = k(\mathbf{x}_*, \mathbf{x}_*) - \mathbf{k}_*^T(\mathbf{K} + \sigma_n^2\mathbf{I})^{-1}\mathbf{k}_*
$$

Where:

- $\mathbf{k}_* = [k(\mathbf{x}_*, \mathbf{x}_1), \ldots, k(\mathbf{x}_*, \mathbf{x}_n)]^T$
- $\mathbf{K}$ = Gram matrix with $K_{ij} = k(\mathbf{x}_i, \mathbf{x}_j)$
- $\sigma_n^2$ = Noise variance

 Key Advantage : Native uncertainty quantification—critical for expensive semiconductor experiments.



    2.4 Neural Network Models

 Feedforward neural network: 

$$
\mathbf{y} = f_{\theta}(\mathbf{x}) = \sigma_L\left(\mathbf{W}_L \cdots \sigma_1(\mathbf{W}_1\mathbf{x} + \mathbf{b}_1) \cdots + \mathbf{b}_L\right)
$$

Where:

- $\mathbf{W}_l$ = Weight matrix for layer $l$
- $\mathbf{b}_l$ = Bias vector for layer $l$
- $\sigma_l$ = Activation function (ReLU, tanh, etc.)

 Physics-Informed Neural Networks (PINNs): 

$$
\mathcal{L} = \mathcal{L}_{\text{data}} + \lambda \mathcal{L}_{\text{physics}}
$$

$$
\mathcal{L}_{\text{data}} = \frac{1}{N}\sum_{i=1}^{N}\|y_i - f_\theta(\mathbf{x}_i)\|^2
$$

$$
\mathcal{L}_{\text{physics}} = \frac{1}{M}\sum_{j=1}^{M}\left\|\mathcal{F}[f_\theta](\mathbf{x}_j)\right\|^2
$$

Where $\mathcal{F}$ is the differential operator from governing equations.



   3. Optimization Formulations

    3.1 Standard Constrained Optimization

 General form: 

$$
\min_{\mathbf{x}} \quad f(\mathbf{x}) = \|\mathbf{y}(\mathbf{x}) - \mathbf{y}_{\text{target}}\|_2^2
$$

Subject to:

$$
\begin{aligned}
\mathbf{y}_L &\leq \mathbf{y}(\mathbf{x}) \leq \mathbf{y}_U \quad &\text{(specification limits)} \\
\mathbf{x}_L &\leq \mathbf{x} \leq \mathbf{x}_U \quad &\text{(equipment limits)} \\
\mathbf{g}(\mathbf{x}) &\leq 0 \quad &\text{(process constraints)}
\end{aligned}
$$

 Weighted multi-output objective: 

$$
f(\mathbf{x}) = \sum_{j=1}^{m} w_j \left(\frac{y_j(\mathbf{x}) - y_{j,\text{target}}}{\Delta_j}\right)^2
$$

Where $\Delta_j$ is the tolerance for output $j$.



    3.2 Multi-Objective Optimization

 Vector optimization problem: 

$$
\min_{\mathbf{x}} \quad \mathbf{F}(\mathbf{x}) = \begin{bmatrix} f_1(\mathbf{x}) \\ f_2(\mathbf{x}) \\ \vdots \\ f_k(\mathbf{x}) \end{bmatrix}
$$

 Example objectives: 

- $f_1(\mathbf{x})$ = Deviation from target thickness: $|t - t_{\text{target}}|$
- $f_2(\mathbf{x})$ = Within-wafer non-uniformity (WIWNU): $\frac{\sigma}{\mu} \times 100\%$
- $f_3(\mathbf{x})$ = Cycle time: $T_{\text{process}}$
- $f_4(\mathbf{x})$ = Defect density: $D_0$

 Pareto dominance: 
$\mathbf{x}^{(1)}$ dominates $\mathbf{x}^{(2)}$ if:

$$
f_i(\mathbf{x}^{(1)}) \leq f_i(\mathbf{x}^{(2)}) \quad \forall i \quad \text{and} \quad \exists j: f_j(\mathbf{x}^{(1)}) < f_j(\mathbf{x}^{(2)})
$$

 Solution methods: 

-  Weighted sum : $\min \sum_{i=1}^{k} w_i f_i(\mathbf{x})$
-  $\varepsilon$-constraint : $\min f_1(\mathbf{x})$ s.t. $f_i(\mathbf{x}) \leq \varepsilon_i$ for $i \geq 2$
-  Evolutionary algorithms : NSGA-II, MOEA/D



    3.3 Robust Optimization

 Sources of uncertainty $\boldsymbol{\xi}$: 

- Control noise (recipe execution variability)
- Equipment drift
- Incoming material variation
- Measurement noise

 Mean-variance formulation: 

$$
\min_{\mathbf{x}} \quad \mathbb{E}[f(\mathbf{x}, \boldsymbol{\xi})] + \kappa \cdot \sqrt{\text{Var}[f(\mathbf{x}, \boldsymbol{\xi})]}
$$

Where $\kappa$ is the risk aversion parameter.

 Uncertainty propagation (first-order Taylor): 

$$
\mathbb{E}[y] \approx y(\bar{\mathbf{x}})
$$

$$
\text{Var}[y] \approx \sum_{i=1}^{n}\left(\frac{\partial y}{\partial x_i}\right)^2 \sigma_{x_i}^2 + 2\sum_{i<j}\frac{\partial y}{\partial x_i}\frac{\partial y}{\partial x_j}\text{Cov}(x_i, x_j)
$$

 Sensitivity coefficient: 

$$
S_i = \left|\frac{\partial y}{\partial x_i}\right| \cdot \frac{\sigma_{x_i}}{\sigma_y}
$$

 Chance-constrained optimization: 

$$
\Pr\big(y(\mathbf{x}, \boldsymbol{\xi}) \in [\text{LSL}, \text{USL}]\big) \geq 1 - \alpha
$$

For Gaussian outputs:

$$
\mu - z_{\alpha/2}\sigma \geq \text{LSL} \quad \text{and} \quad \mu + z_{\alpha/2}\sigma \leq \text{USL}
$$

 Process capability index: 

$$
C_{pk} = \min\left(\frac{\text{USL} - \mu}{3\sigma}, \frac{\mu - \text{LSL}}{3\sigma}\right) \geq 1.33
$$



    3.4 Bayesian Optimization

 Algorithm for expensive-to-evaluate functions: 

1. Fit GP surrogate: $y(\mathbf{x}) \sim \mathcal{GP}(m, k)$
2. Optimize acquisition function: $\mathbf{x}_{\text{next}} = \arg\max_{\mathbf{x}} \alpha(\mathbf{x})$
3. Evaluate true function: $y_{\text{next}} = f(\mathbf{x}_{\text{next}}) + \varepsilon$
4. Update dataset: $\mathcal{D} \leftarrow \mathcal{D} \cup \{(\mathbf{x}_{\text{next}}, y_{\text{next}})\}$
5. Repeat until budget exhausted

 Acquisition functions: 

 Expected Improvement (EI): 

$$
\text{EI}(\mathbf{x}) = \mathbb{E}\big[\max(y^* - y(\mathbf{x}), 0)\big]
$$

 Closed form (for GP): 

$$
\text{EI}(\mathbf{x}) = (y^* - \mu(\mathbf{x}))\Phi(z) + \sigma(\mathbf{x})\phi(z)
$$

Where:

$$
z = \frac{y^* - \mu(\mathbf{x})}{\sigma(\mathbf{x})}
$$

- $\Phi(\cdot)$ = Standard normal CDF
- $\phi(\cdot)$ = Standard normal PDF
- $y^*$ = Best observed value

 Upper Confidence Bound (UCB): 

$$
\text{UCB}(\mathbf{x}) = \mu(\mathbf{x}) + \kappa \sigma(\mathbf{x})
$$

Where $\kappa$ balances exploration vs. exploitation.

 Probability of Improvement (PI): 

$$
\text{PI}(\mathbf{x}) = \Phi\left(\frac{y^* - \mu(\mathbf{x})}{\sigma(\mathbf{x})}\right)
$$



   4. Run-to-Run (R2R) Control

    4.1 EWMA Controller

 Exponentially Weighted Moving Average prediction: 

$$
\hat{y}_{k+1} = \lambda y_k + (1-\lambda)\hat{y}_k
$$

Where $\lambda \in (0,1]$ is the smoothing factor.

 Recipe update: 

$$
x_{k+1} = x_k + G^{-1}(y_{\text{target}} - \hat{y}_{k+1})
$$

Where $G = \frac{\partial y}{\partial x}$ is the process gain matrix.

    4.2 Model Predictive Control (MPC)

 Optimization problem: 

$$
\min_{\{u_k, \ldots, u_{k+N-1}\}} \sum_{j=0}^{N-1} \left[\|y_{k+j} - y_{\text{target}}\|_{\mathbf{Q}}^2 + \|\Delta u_{k+j}\|_{\mathbf{R}}^2\right]
$$

Subject to:

$$
\begin{aligned}
\mathbf{x}_{k+j+1} &= \mathbf{A}\mathbf{x}_{k+j} + \mathbf{B}u_{k+j} \quad &\text{(state dynamics)} \\
y_{k+j} &= \mathbf{C}\mathbf{x}_{k+j} \quad &\text{(output equation)} \\
u_{\min} &\leq u_{k+j} \leq u_{\max} \quad &\text{(input constraints)}
\end{aligned}
$$

Where:

- $\mathbf{Q}$ = Output weighting matrix
- $\mathbf{R}$ = Control effort weighting matrix
- $N$ = Prediction horizon



   5. Mathematical Challenges

| Challenge | Mathematical Approach |
|:----------|:---------------------|
| High dimensionality ($n > 50$ parameters) | PCA, PLS, sparse regression (LASSO), feature selection |
| Small datasets (limited wafer runs) | Bayesian methods, transfer learning, multi-fidelity modeling |
| Nonlinearity | GPs, neural networks, tree ensembles (RF, XGBoost) |
| Equipment-to-equipment variation | Mixed-effects models, hierarchical Bayesian models |
| Drift over time | Adaptive/recursive estimation, change-point detection, Kalman filtering |
| Multiple correlated responses | Multi-task learning, co-kriging, multivariate GP |
| Missing data | EM algorithm, multiple imputation, probabilistic PCA |



   6. Dimensionality Reduction

    6.1 Principal Component Analysis (PCA)

 Objective: 

$$
\max_{\mathbf{w}} \quad \mathbf{w}^T\mathbf{S}\mathbf{w} \quad \text{s.t.} \quad \|\mathbf{w}\|_2 = 1
$$

Where $\mathbf{S}$ is the sample covariance matrix.

 Solution:  Eigenvectors of $\mathbf{S}$

$$
\mathbf{S} = \mathbf{W}\boldsymbol{\Lambda}\mathbf{W}^T
$$

 Reduced representation: 

$$
\mathbf{z} = \mathbf{W}_k^T(\mathbf{x} - \bar{\mathbf{x}})
$$

Where $\mathbf{W}_k$ contains the top $k$ eigenvectors.

    6.2 Partial Least Squares (PLS)

 Objective:  Maximize covariance between $\mathbf{X}$ and $\mathbf{Y}$

$$
\max_{\mathbf{w}, \mathbf{c}} \quad \text{Cov}(\mathbf{Xw}, \mathbf{Yc}) \quad \text{s.t.} \quad \|\mathbf{w}\|=\|\mathbf{c}\|=1
$$



   7. Multi-Fidelity Optimization

 Combine cheap simulations with expensive experiments: 

 Auto-regressive model (Kennedy-O'Hagan): 

$$
y_{\text{HF}}(\mathbf{x}) = \rho \cdot y_{\text{LF}}(\mathbf{x}) + \delta(\mathbf{x})
$$

Where:

- $y_{\text{HF}}$ = High-fidelity (experimental) response
- $y_{\text{LF}}$ = Low-fidelity (simulation) response
- $\rho$ = Scaling factor
- $\delta(\mathbf{x}) \sim \mathcal{GP}$ = Discrepancy function

 Multi-fidelity GP: 

$$
\begin{bmatrix} \mathbf{y}_{\text{LF}} \\ \mathbf{y}_{\text{HF}} \end{bmatrix} \sim \mathcal{N}\left(\mathbf{0}, \begin{bmatrix} \mathbf{K}_{\text{LL}} & \rho\mathbf{K}_{\text{LH}} \\ \rho\mathbf{K}_{\text{HL}} & \rho^2\mathbf{K}_{\text{LL}} + \mathbf{K}_{\delta} \end{bmatrix}\right)
$$



   8. Transfer Learning

 Domain adaptation for tool-to-tool transfer: 

$$
y_{\text{target}}(\mathbf{x}) = y_{\text{source}}(\mathbf{x}) + \Delta(\mathbf{x})
$$

 Offset model (simple): 

$$
\Delta(\mathbf{x}) = c_0 \quad \text{(constant offset)}
$$

 Linear adaptation: 

$$
\Delta(\mathbf{x}) = \mathbf{c}^T\mathbf{x} + c_0
$$

 GP adaptation: 

$$
\Delta(\mathbf{x}) \sim \mathcal{GP}(0, k_\Delta)
$$



   9. Complete Optimization Framework

┌───────────────────────────────────────────────────────┐
│         RECIPE OPTIMIZATION FRAMEWORK                 │
├───────────────────────────────────────────────────────┤
│                                                       │
│  INPUTS              MODEL             OUTPUTS        │
│  ──────              ─────             ───────        │
│                   ┌─────────┐                         │
│  x₁: Temp    ───► │         │ ───►  y₁: Thickness     │
│  x₂: Press   ───► │ y=f(x;θ)│ ───►  y₂: Uniformity    │
│  x₃: Flow1   ───► │         │ ───►  y₃: CD            │
│  x₄: Flow2   ───► │   + ε   │ ───►  y₄: Defects       │
│  x₅: Power   ───► │         │                         │
│  x₆: Time    ───► └─────────┘                         │
│                        ▲                              │
│                   Uncertainty ξ                       │
│                                                       │
├───────────────────────────────────────────────────────┤
│  OPTIMIZATION PROBLEM:                                │
│                                                       │
│  min  Σⱼ wⱼ(E[yⱼ] - yⱼ,target)² + λ·Var[y]            │
│   x                                                   │
│                                                       │
│  subject to:                                          │
│    y_L ≤ E[y] ≤ y_U      (spec limits)                │
│    Pr(y ∈ spec) ≥ 0.9973 (Cpk ≥ 1.0)                  │
│    x_L ≤ x ≤ x_U         (equipment limits)           │
│    g(x) ≤ 0              (process constraints)        │
│                                                       │
└───────────────────────────────────────────────────────┘




   10. Equations:

    Process Modeling

| Model Type | Equation |
|:-----------|:---------|
| Linear regression | $y = \mathbf{X}\boldsymbol{\beta} + \varepsilon$ |
| Quadratic RSM | $y = \beta_0 + \sum_i \beta_i x_i + \sum_i \beta_{ii}x_i^2 + \sum_{i<j}\beta_{ij}x_ix_j$ |
| Gaussian Process | $y(\mathbf{x}) \sim \mathcal{GP}(m(\mathbf{x}), k(\mathbf{x},\mathbf{x}'))$ |
| Neural Network | $y = \sigma_L(\mathbf{W}_L\cdots\sigma_1(\mathbf{W}_1\mathbf{x}+\mathbf{b}_1)+\mathbf{b}_L)$ |

    Optimization

| Formulation | Equation |
|:------------|:---------|
| Least squares | $\min_\mathbf{x} \|\mathbf{y}(\mathbf{x}) - \mathbf{y}_{\text{target}}\|_2^2$ |
| Robust | $\min_\mathbf{x} \mathbb{E}[f] + \kappa\sqrt{\text{Var}[f]}$ |
| Chance-constrained | $\Pr(y \in \text{spec}) \geq 1-\alpha$ |
| Expected Improvement | $\text{EI}(\mathbf{x}) = (y^*-\mu)\Phi(z) + \sigma\phi(z)$ |

    Run-to-Run Control

| Controller | Equation |
|:-----------|:---------|
| EWMA | $\hat{y}_{k+1} = \lambda y_k + (1-\lambda)\hat{y}_k$ |
| Recipe update | $x_{k+1} = x_k + G^{-1}(y_{\text{target}} - \hat{y}_{k+1})$ |




   Notation:

| Symbol | Description |
|:-------|:------------|
| $\mathbf{x}$ | Recipe parameter vector |
| $\mathbf{y}$ | Process output vector |
| $n$ | Number of input parameters |
| $m$ | Number of outputs |
| $\boldsymbol{\beta}$ | Regression coefficients |
| $\mathcal{GP}$ | Gaussian Process |
| $k(\cdot,\cdot)$ | Kernel/covariance function |
| $\mu(\mathbf{x})$ | Predictive mean |
| $\sigma^2(\mathbf{x})$ | Predictive variance |
| $\mathbb{E}[\cdot]$ | Expectation operator |
| $\text{Var}[\cdot]$ | Variance operator |
| $\Phi(\cdot)$ | Standard normal CDF |
| $\phi(\cdot)$ | Standard normal PDF |
| $\boldsymbol{\xi}$ | Random disturbance/uncertainty |
| $G$ | Process gain matrix |
| LSL, USL | Lower/Upper Specification Limits |
| $C_{pk}$ | Process capability index |

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/process-optimization) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
