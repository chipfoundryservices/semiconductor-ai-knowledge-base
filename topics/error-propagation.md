# Semiconductor Manufacturing Error Propagation Mathematics

**Keywords**: error propagation,uncertainty propagation,variance decomposition,yield mathematics,overlay error,EPE,process capability,monte carlo

---

**Semiconductor Manufacturing Error Propagation Mathematics**



**1. Fundamental Error Propagation Theory**

For a function $f(x_1, x_2, \ldots, x_n)$ where each variable $x_i$ has uncertainty $\sigma_i$, the propagated uncertainty follows:

$$
\sigma_f^2 = \sum_{i=1}^{n} \left( \frac{\partial f}{\partial x_i} \right)^2 \sigma_i^2 + 2 \sum_{i < j} \frac{\partial f}{\partial x_i} \frac{\partial f}{\partial x_j} \, \text{cov}(x_i, x_j)
$$

For **uncorrelated errors**, this simplifies to the **Root-Sum-of-Squares (RSS)** formula:

$$
\sigma_f = \sqrt{\sum_{i=1}^{n} \left( \frac{\partial f}{\partial x_i} \right)^2 \sigma_i^2}
$$

**Applications in Semiconductor Manufacturing**

- **Critical Dimension (CD) variations**: Feature size deviations from target
- **Overlay errors**: Misalignment between lithography layers
- **Film thickness variations**: Deposition uniformity issues
- **Doping concentration variations**: Implant dose and energy fluctuations



**2. Process Chain Error Accumulation**

Semiconductor manufacturing involves hundreds of sequential process steps. Errors propagate through the chain in different modes:

**2.1 Additive Error Accumulation**

Used for overlay alignment between layers:

$$
E_{\text{total}} = \sum_{i=1}^{n} \varepsilon_i
$$

$$
\sigma_{\text{total}}^2 = \sum_{i=1}^{n} \sigma_i^2 \quad \text{(if uncorrelated)}
$$

**2.2 Multiplicative Error Accumulation**

Used for etch selectivity, deposition rates, and gain factors:

$$
G_{\text{total}} = \prod_{i=1}^{n} G_i
$$

$$
\frac{\sigma_G}{G} \approx \sqrt{\sum_{i=1}^{n} \left( \frac{\sigma_{G_i}}{G_i} \right)^2}
$$

**2.3 Error Accumulation Modes**

- **Additive**: Errors sum directly (overlay, thickness)
- **Multiplicative**: Errors compound through products (gain, selectivity)
- **Compensating**: Rare cases where errors cancel
- **Nonlinear interactions**: Complex dependencies requiring simulation



**3. Hierarchical Variance Decomposition**

Total variation decomposes across spatial and temporal hierarchies:

$$
\sigma_{\text{total}}^2 = \sigma_{\text{lot}}^2 + \sigma_{\text{wafer}}^2 + \sigma_{\text{die}}^2 + \sigma_{\text{within-die}}^2
$$

**Variance Sources by Level**

| Level | Sources |
|-------|---------|
| **Lot-to-lot** | Incoming material, chamber conditioning, recipe drift |
| **Wafer-to-wafer** | Slot position, thermal gradients, handling |
| **Die-to-die** | Across-wafer uniformity, lens field distortion |
| **Within-die** | Pattern density, microloading, proximity effects |

**Variance Component Analysis**

For $N$ measurements $y_{ijk}$ (lot $i$, wafer $j$, site $k$):

$$
y_{ijk} = \mu + L_i + W_{ij} + \varepsilon_{ijk}
$$

Where:

- $\mu$ = grand mean
- $L_i \sim N(0, \sigma_L^2)$ = lot effect
- $W_{ij} \sim N(0, \sigma_W^2)$ = wafer effect
- $\varepsilon_{ijk} \sim N(0, \sigma_\varepsilon^2)$ = residual



**4. Yield Mathematics**

**4.1 Poisson Defect Model (Random Defects)**

$$
Y = e^{-D_0 A}
$$

Where:

- $D_0$ = defect density (defects/cm²)
- $A$ = die area (cm²)

**4.2 Negative Binomial Model (Clustered Defects)**

More realistic for actual manufacturing:

$$
Y = \left( 1 + \frac{D_0 A}{\alpha} \right)^{-\alpha}
$$

Where:

- $\alpha$ = clustering parameter
- $\alpha \to \infty$ recovers Poisson model
- Smaller $\alpha$ = more clustering

**4.3 Total Yield**

$$
Y_{\text{total}} = Y_{\text{defect}} \times Y_{\text{parametric}}
$$

**4.4 Parametric Yield**

Integration over the multi-dimensional acceptable parameter space:

$$
Y_{\text{parametric}} = \int \int \cdots \int_{\text{spec}} f(p_1, p_2, \ldots, p_n) \, dp_1 \, dp_2 \cdots dp_n
$$

For Gaussian parameters with specs at $\pm k\sigma$:

$$
Y_{\text{parametric}} \approx \left[ \text{erf}\left( \frac{k}{\sqrt{2}} \right) \right]^n
$$



**5. Edge Placement Error (EPE)**

Critical metric at advanced nodes combining multiple error sources:

$$
EPE^2 = \left( \frac{\Delta CD}{2} \right)^2 + OVL^2 + \left( \frac{LER}{2} \right)^2
$$

**EPE Components**

- $\Delta CD$ = Critical dimension error
- $OVL$ = Overlay error
- $LER$ = Line edge roughness

**Extended EPE Model**

Including additional terms:

$$
EPE^2 = \left( \frac{\Delta CD}{2} \right)^2 + OVL^2 + \left( \frac{LER}{2} \right)^2 + \sigma_{\text{mask}}^2 + \sigma_{\text{etch}}^2
$$



**6. Overlay Error Modeling**

Overlay at any point $(x, y)$ is modeled as:

$$
OVL(x, y) = \vec{T} + R\theta + M \cdot \vec{r} + \text{HOT}
$$

**Overlay Components**

- $\vec{T} = (T_x, T_y)$ = Translation
- $R\theta$ = Rotation
- $M$ = Magnification
- $\text{HOT}$ = Higher-Order Terms (lens distortions, wafer non-flatness)

**Overlay Budget (RSS)**

$$
OVL_{\text{budget}}^2 = OVL_{\text{tool}}^2 + OVL_{\text{process}}^2 + OVL_{\text{wafer}}^2 + OVL_{\text{mask}}^2
$$

**10-Parameter Overlay Model**

$$
\begin{aligned}
dx &= T_x + R_x \cdot y + M_x \cdot x + N_x \cdot x \cdot y + \ldots \\
dy &= T_y + R_y \cdot x + M_y \cdot y + N_y \cdot x \cdot y + \ldots
\end{aligned}
$$



**7. Stochastic Effects in EUV Lithography**

At EUV wavelengths (13.5 nm), photon shot noise becomes fundamental.

**Photon Statistics**

Photons per pixel follow Poisson distribution:

$$
N \sim \text{Poisson}(\bar{N})
$$

$$
\sigma_N = \sqrt{\bar{N}}
$$

**Relative Dose Fluctuation**

$$
\frac{\sigma_N}{\bar{N}} = \frac{1}{\sqrt{\bar{N}}}
$$

**Stochastic Failure Probability**

$$
P_{\text{fail}} \propto \exp\left( -\frac{E}{E_{\text{threshold}}} \right)
$$

**RLS Triangle Trade-off**

- **R**esolution
- **L**ine edge roughness (LER)
- **S**ensitivity (dose)

$$
LER \propto \frac{1}{\sqrt{\text{Dose}}} \propto \frac{1}{\sqrt{N_{\text{photons}}}}
$$



**8. Spatial Correlation Modeling**

Errors are spatially correlated. Modeled using variograms or correlation functions.

**Variogram**

$$
\gamma(h) = \frac{1}{2} E\left[ (Z(x+h) - Z(x))^2 \right]
$$

**Correlation Function**

$$
\rho(h) = \frac{\text{cov}(Z(x+h), Z(x))}{\text{var}(Z(x))}
$$

**Common Correlation Models**

| Model | Formula |
|-------|---------|
| **Exponential** | $\rho(h) = \exp\left( -\frac{h}{\lambda} \right)$ |
| **Gaussian** | $\rho(h) = \exp\left( -\left( \frac{h}{\lambda} \right)^2 \right)$ |
| **Spherical** | $\rho(h) = 1 - \frac{3h}{2\lambda} + \frac{h^3}{2\lambda^3}$ for $h \leq \lambda$ |

**Implications**

- Nearby devices are more correlated → better matching for analog
- Correlation length $\lambda$ determines effective samples per die
- Extreme values are less severe than independent variation suggests



**9. Process Capability and Tail Statistics**

**Process Capability Index**

$$
C_{pk} = \min \left[ \frac{USL - \mu}{3\sigma}, \frac{\mu - LSL}{3\sigma} \right]
$$

**Defect Rates vs. Cpk (Gaussian)**

| $C_{pk}$ | PPM Outside Spec | Sigma Level |
|----------|------------------|-------------|
| 1.00 | ~2,700 | 3σ |
| 1.33 | ~63 | 4σ |
| 1.67 | ~0.6 | 5σ |
| 2.00 | ~0.002 | 6σ |

**Extreme Value Statistics**

For $n$ independent samples from distribution $F(x)$, the maximum follows:

$$
P(M_n \leq x) = [F(x)]^n
$$

For large $n$, converges to Generalized Extreme Value (GEV):

$$
G(x) = \exp\left\{ -\left[ 1 + \xi \left( \frac{x - \mu}{\sigma} \right) \right]^{-1/\xi} \right\}
$$

**Critical Insight**

For a chip with $10^{10}$ transistors:

$$
P_{\text{chip fail}} = 1 - (1 - P_{\text{transistor fail}})^{10^{10}} \approx 10^{10} \cdot P_{\text{transistor fail}}
$$

Even $P_{\text{transistor fail}} = 10^{-11}$ matters!



**10. Sensitivity Analysis and Error Attribution**

**Sensitivity Coefficient**

$$
S_i = \frac{\partial Y}{\partial \sigma_i} \times \frac{\sigma_i}{Y}
$$

**Variance Contribution**

$$
\text{Contribution}_i = \frac{\left( \frac{\partial f}{\partial x_i} \right)^2 \sigma_i^2}{\sigma_f^2} \times 100\%
$$

**Bayesian Root Cause Attribution**

$$
P(\text{cause} \mid \text{observation}) = \frac{P(\text{observation} \mid \text{cause}) \cdot P(\text{cause})}{P(\text{observation})}
$$

**Pareto Analysis Steps**

1. Compute variance contribution from each source
2. Rank sources by contribution
3. Focus improvement on top contributors
4. Verify improvement with updated measurements



**11. Monte Carlo Simulation Methods**

Due to complexity and nonlinearity, Monte Carlo methods are essential.

**Algorithm**

```
FOR i = 1 to N_samples:
    1. Sample process parameters: p_i ~ distributions
    2. Simulate device/circuit: y_i = f(p_i)
    3. Store result: Y[i] = y_i
END FOR
Compute statistics from Y[]
```

**Key Advantages**

- Captures non-Gaussian behavior
- Handles nonlinear transfer functions
- Reveals correlations between outputs
- Provides full distribution, not just moments

**Sample Size Requirements**

For estimating probability $p$ of rare events:

$$
N \geq \frac{1 - p}{p \cdot \varepsilon^2}
$$

Where $\varepsilon$ is the desired relative error.

For $p = 10^{-6}$ with 10% error: $N \approx 10^8$ samples



**12. Design-Technology Co-Optimization (DTCO)**

Error propagation feeds back into design rules:

$$
\text{Design Margin} = k \times \sigma_{\text{total}}
$$

Where $k$ depends on required yield and number of instances.

**Margin Calculation**

For yield $Y$ over $N$ instances:

$$
k = \Phi^{-1}\left( Y^{1/N} \right)
$$

Where $\Phi^{-1}$ is the inverse normal CDF.

**Example**

- Target yield: 99%
- Number of gates: $10^9$
- Required: $k \approx 7\sigma$ per gate



**13. Key Mathematical Insights**

**Insight 1: RSS Dominates Budgets**

Uncorrelated errors add in quadrature:

$$
\sigma_{\text{total}} = \sqrt{\sigma_1^2 + \sigma_2^2 + \cdots + \sigma_n^2}
$$

**Implication**: Reducing the largest contributor gives the most improvement.

**Insight 2: Tails Matter More Than Means**

High-volume manufacturing lives in the $6\sigma$ tails where:

- Gaussian assumptions break down
- Extreme value statistics become essential
- Rare events dominate yield loss

**Insight 3: Nonlinearity Creates Surprises**

Even Gaussian inputs produce non-Gaussian outputs:

$$
Y = f(X) \quad \text{where } X \sim N(\mu, \sigma^2)
$$

If $f$ is nonlinear, $Y$ is not Gaussian.

**Insight 4: Correlations Can Help or Hurt**

- **Positive correlations**: Worsen tail probabilities
- **Negative correlations**: Can provide compensation
- **Designed-in correlations**: Can dramatically improve yield

**Insight 5: Scaling Amplifies Relative Error**

$$
\text{Relative Error} = \frac{\sigma}{\text{Feature Size}}
$$

A 1 nm variation:

- 5% of 20 nm feature
- 10% of 10 nm feature
- 20% of 5 nm feature



**14. Summary Equations**

**Core Error Propagation**

$$
\sigma_f^2 = \sum_i \left( \frac{\partial f}{\partial x_i} \right)^2 \sigma_i^2
$$

**Yield (Negative Binomial)**

$$
Y = \left( 1 + \frac{D_0 A}{\alpha} \right)^{-\alpha}
$$

**Edge Placement Error**

$$
EPE = \sqrt{\left( \frac{\Delta CD}{2} \right)^2 + OVL^2 + \left( \frac{LER}{2} \right)^2}
$$

**Process Capability**

$$
C_{pk} = \min \left[ \frac{USL - \mu}{3\sigma}, \frac{\mu - LSL}{3\sigma} \right]
$$

**Stochastic LER**

$$
LER \propto \frac{1}{\sqrt{N_{\text{photons}}}}
$$

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/error-propagation) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
