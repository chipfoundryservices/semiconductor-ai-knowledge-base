# Pattern Placement

**Keywords**: pattern placement,overlay,registration,alignment,wafer alignment,die placement,pattern transfer,lithography alignment,overlay error,placement accuracy

---

**Pattern Placement**

  1. The Core Problem

In semiconductor manufacturing, we must transfer nanoscale patterns from a mask to a silicon wafer with sub-nanometer precision across billions of features. The mathematical challenge is threefold:

-  Forward modeling : Predicting what pattern will actually print given a mask design
-  Inverse problem : Determining what mask to use to achieve a desired pattern
-  Optimization under uncertainty : Ensuring robust manufacturing despite process variations



  2. Optical Lithography Mathematics

   2.1 Aerial Image Formation (Hopkins Formulation)

The intensity distribution at the wafer plane is governed by partially coherent imaging theory:

$$
I(x,y) = \iint\!\!\iint TCC(f_1,g_1,f_2,g_2) \cdot M(f_1,g_1) \cdot M^*(f_2,g_2) \cdot e^{2\pi i[(f_1-f_2)x + (g_1-g_2)y]} \, df_1\,dg_1\,df_2\,dg_2
$$

 Where: 

- $TCC$ (Transmission Cross-Coefficient) encodes the optical system
- $M(f,g)$ is the Fourier transform of the mask transmission function
- The double integral reflects the coherent superposition from different source points

   2.2 Resolution Limits

The Rayleigh criterion establishes fundamental constraints:

$$
R_{min} = k_1 \cdot \frac{\lambda}{NA}
$$

$$
DOF = k_2 \cdot \frac{\lambda}{NA^2}
$$

 Parameters: 

| Parameter | DUV (ArF) | EUV |
|-----------|-----------|-----|
| Wavelength $\lambda$ | 193 nm | 13.5 nm |
| Typical NA | 1.35 | 0.33 (High-NA: 0.55) |
| Min. pitch | ~36 nm | ~24 nm |

The $k_1$ factor (process-dependent, typically 0.25–0.4) is where most of the mathematical innovation occurs.

   2.3 Image Log-Slope (ILS)

The image log-slope is a critical metric for pattern fidelity:

$$
ILS = \frac{1}{I} \left| \frac{dI}{dx} \right|_{edge}
$$

Higher ILS values indicate better edge definition and process margin.

   2.4 Modulation Transfer Function (MTF)

The optical system's ability to transfer contrast is characterized by:

$$
MTF(f) = \frac{I_{max}(f) - I_{min}(f)}{I_{max}(f) + I_{min}(f)}
$$



  3. Photoresist Modeling

The resist transforms the aerial image into a physical pattern through coupled partial differential equations.

   3.1 Exposure Kinetics (Dill Model)

 Light absorption in resist: 

$$
\frac{\partial I}{\partial z} = -\alpha(M) \cdot I
$$

 Absorption coefficient: 

$$
\alpha = A \cdot M + B
$$

 Photoactive compound decomposition: 

$$
\frac{\partial M}{\partial t} = -C \cdot I \cdot M
$$

 Where: 

- $A$ = bleachable absorption coefficient (μm⁻¹)
- $B$ = non-bleachable absorption coefficient (μm⁻¹)
- $C$ = exposure rate constant (cm²/mJ)
- $M$ = relative PAC concentration (0 to 1)

   3.2 Chemically Amplified Resist (Diffusion-Reaction)

For modern resists, photoacid generation and diffusion govern pattern formation:

$$
\frac{\partial [H^+]}{\partial t} = D
abla^2[H^+] - k_{quench}[H^+][Q] - k_{react}[H^+][Polymer]
$$

 Components: 

- $D$ = diffusion coefficient of photoacid
- $k_{quench}$ = quencher reaction rate
- $k_{react}$ = deprotection reaction rate
- $[Q]$ = quencher concentration

   3.3 Development Rate Models

The Mack model relates local chemistry to dissolution:

$$
R(m) = R_{max} \cdot \frac{(a+1)(1-m)^n}{a + (1-m)^n} + R_{min}
$$

 Where: 

- $m$ = normalized inhibitor concentration
- $n$ = development selectivity parameter
- $a$ = threshold parameter
- $R_{max}$, $R_{min}$ = maximum and minimum development rates

   3.4 Resist Profile Evolution

The resist surface evolves according to:

$$
\frac{\partial z}{\partial t} = -R(m(x,y,z)) \cdot \hat{n}
$$

Where $\hat{n}$ is the surface normal vector.



  4. Pattern Placement and Overlay Mathematics

   4.1 Overlay Error Decomposition

Total placement error is modeled as a polynomial field:

$$
\delta x(X,Y) = a_0 + a_1 X + a_2 Y + a_3 XY + a_4 X^2 + a_5 Y^2 + \ldots
$$

$$
\delta y(X,Y) = b_0 + b_1 X + b_2 Y + b_3 XY + b_4 X^2 + b_5 Y^2 + \ldots
$$

 Physical interpretation of coefficients: 

| Term | Coefficient | Physical Meaning |
|------|-------------|------------------|
| Translation | $a_0, b_0$ | Rigid shift in x, y |
| Magnification | $a_1, b_2$ | Isotropic scaling |
| Rotation | $a_2, -b_1$ | In-plane rotation |
| Asymmetric Mag | $a_1 - b_2$ | Anisotropic scaling |
| Trapezoid | $a_3, b_3$ | Keystone distortion |
| Higher order | $a_4, a_5, \ldots$ | Lens aberrations, wafer distortion |

   4.2 Edge Placement Error (EPE) Budget

$$
EPE_{total}^2 = EPE_{overlay}^2 + EPE_{CD}^2 + EPE_{LER}^2 + EPE_{stochastic}^2
$$

 Error budget at 3nm node: 

- Total EPE budget: ~1-2 nm
- Each component must be controlled to sub-nanometer precision

   4.3 Overlay Correction Model

The correction applied to the scanner is:

$$
\begin{pmatrix} \Delta x \\ \Delta y \end{pmatrix} = 
\begin{pmatrix} 
1 + M_x & R + O_x \\
-R + O_y & 1 + M_y 
\end{pmatrix}
\begin{pmatrix} X \\ Y \end{pmatrix} +
\begin{pmatrix} T_x \\ T_y \end{pmatrix}
$$

 Where: 

- $T_x, T_y$ = translation corrections
- $M_x, M_y$ = magnification corrections
- $R$ = rotation correction
- $O_x, O_y$ = orthogonality corrections

   4.4 Wafer Distortion Modeling

Wafer-level distortion is often modeled using Zernike polynomials:

$$
W(r, \theta) = \sum_{n,m} Z_n^m \cdot R_n^m(r) \cdot \cos(m\theta)
$$



  5. Computational Lithography: The Inverse Problem

   5.1 Optical Proximity Correction (OPC)

Given target pattern $P_{target}$, find mask $M$ such that:

$$
\min_M \|Litho(M) - P_{target}\|^2 + \lambda \cdot \mathcal{R}(M)
$$

 Where: 

- $Litho(\cdot)$ is the forward lithography model
- $\mathcal{R}(M)$ enforces mask manufacturability constraints
- $\lambda$ is the regularization weight

   5.2 Gradient-Based Optimization

Using the chain rule through the forward model:

$$
\frac{\partial L}{\partial M} = \frac{\partial L}{\partial I} \cdot \frac{\partial I}{\partial M}
$$

The aerial image gradient $\frac{\partial I}{\partial M}$ can be computed efficiently via:

$$
\frac{\partial I}{\partial M}(x,y) = 2 \cdot \text{Re}\left[\iint TCC \cdot \frac{\partial M}{\partial M_{pixel}} \cdot M^* \cdot e^{i\phi} \, df\,dg\right]
$$

   5.3 Inverse Lithography Technology (ILT)

For curvilinear masks, the level-set method parametrizes the mask boundary:

$$
\frac{\partial \phi}{\partial t} + F|
abla\phi| = 0
$$

 Where: 

- $\phi$ is the signed distance function
- $F$ is the speed function derived from the cost gradient:

$$
F = -\frac{\partial L}{\partial \phi}
$$

   5.4 Source-Mask Optimization (SMO)

Joint optimization over source shape $S$ and mask $M$:

$$
\min_{S,M} \mathcal{L}(S,M) = \|I(S,M) - I_{target}\|^2 + \alpha \mathcal{R}_S(S) + \beta \mathcal{R}_M(M)
$$

 Optimization approach: 

1. Fix $S$, optimize $M$ (mask optimization)
2. Fix $M$, optimize $S$ (source optimization)
3. Iterate until convergence

   5.5 Process Window Optimization

Maximize the overlapping process window:

$$
\max_{M} \left[ \min_{(dose, focus) \in PW} \left( CD_{target} - |CD(dose, focus) - CD_{target}| \right) \right]
$$



  6. Multi-Patterning Mathematics

Below ~40nm pitch with 193nm lithography, single exposure cannot resolve features.

   6.1 Graph Coloring Formulation

 Problem:  Assign features to masks such that no two features on the same mask violate minimum spacing.

 Graph representation: 

-  Nodes  = pattern features
-  Edges  = spacing conflicts (features too close for single exposure)
-  Colors  = mask assignments

For double patterning (LELE), this becomes  graph 2-coloring .

   6.2 Integer Linear Programming Formulation

 Objective:  Minimize stitches (pattern splits)

$$
\min \sum_i c_i \cdot s_i
$$

 Subject to: 

$$
x_i + x_j \geq 1 \quad \forall (i,j) \in \text{Conflicts}
$$

$$
x_i \in \{0,1\}
$$

   6.3 Conflict Graph Analysis

The chromatic number $\chi(G)$ determines minimum masks needed:

- $\chi(G) = 2$ → Double patterning feasible
- $\chi(G) = 3$ → Triple patterning required
- $\chi(G) > 3$ → Layout modification needed

 Odd cycle detection: 

$$
\text{Conflict if } \exists \text{ cycle of odd length in conflict graph}
$$

   6.4 Self-Aligned Patterning (SADP/SAQP)

Spacer-based approaches achieve pitch multiplication:

$$
Pitch_{final} = \frac{Pitch_{mandrel}}{2^n}
$$

Where $n$ is the number of spacer iterations.

 SADP constraints: 

- All lines have same width (spacer width)
- Only certain topologies are achievable
- Tip-to-tip spacing constraints



  7. Stochastic Effects (Critical for EUV)

At EUV wavelengths, photon shot noise becomes significant.

   7.1 Photon Statistics

Photon count follows Poisson statistics:

$$
P(n) = \frac{\lambda^n e^{-\lambda}}{n!}
$$

 Where: 

- $n$ = number of photons
- $\lambda$ = expected photon count

The resulting dose variation:

$$
\frac{\sigma_{dose}}{dose} = \frac{1}{\sqrt{N_{photons}}}
$$

   7.2 Photon Count Estimation

Number of photons per pixel:

$$
N_{photons} = \frac{Dose \cdot A_{pixel}}{E_{photon}} = \frac{Dose \cdot A_{pixel} \cdot \lambda}{hc}
$$

 For EUV (λ = 13.5 nm): 

$$
E_{photon} = \frac{hc}{\lambda} \approx 92 \text{ eV}
$$

   7.3 Stochastic Edge Placement Error

$$
\sigma_{SEPE} \propto \frac{1}{\sqrt{Dose \cdot ILS}}
$$

 The stochastic EPE relationship: 

$$
\sigma_{EPE,stoch} = \frac{\sigma_{dose,local}}{ILS_{resist}} \approx \sqrt{\frac{2}{\pi}} \cdot \frac{1}{ILS \cdot \sqrt{n_{eff}}}
$$

Where $n_{eff}$ is the effective number of photons contributing to the edge.

   7.4 Line Edge Roughness (LER)

Power spectral density of edge roughness:

$$
PSD(f) = \frac{2\sigma^2 \xi}{1 + (2\pi f \xi)^{2\alpha}}
$$

 Where: 

- $\sigma$ = RMS roughness amplitude
- $\xi$ = correlation length
- $\alpha$ = roughness exponent (Hurst parameter)

   7.5 Defect Probability

The probability of a stochastic failure:

$$
P_{fail} = 1 - \text{erf}\left(\frac{CD/2 - \mu_{edge}}{\sqrt{2}\sigma_{edge}}\right)
$$



  8. Physical Design Placement Optimization

At the design level, cell placement is a large-scale optimization problem.

   8.1 Quadratic Placement

Minimize half-perimeter wirelength approximation:

$$
W = \sum_{(i,j) \in E} w_{ij} \left[(x_i - x_j)^2 + (y_i - y_j)^2\right]
$$

This yields a sparse linear system:

$$
Qx = b_x, \quad Qy = b_y
$$

Where $Q$ is the weighted graph Laplacian:

$$
Q_{ii} = \sum_{j 
eq i} w_{ij}, \quad Q_{ij} = -w_{ij}
$$

   8.2 Half-Perimeter Wirelength (HPWL)

For a net with pins at positions $\{(x_i, y_i)\}$:

$$
HPWL = \left(\max_i x_i - \min_i x_i\right) + \left(\max_i y_i - \min_i y_i\right)
$$

   8.3 Density-Aware Placement

To prevent overlap, add density constraints:

$$
\sum_{c \in bin(k)} A_c \leq D_{max} \cdot A_{bin} \quad \forall k
$$

 Solved via augmented Lagrangian: 

$$
\mathcal{L}(x, \lambda) = W(x) + \sum_k \lambda_k \left(\sum_{c \in bin(k)} A_c - D_{max} \cdot A_{bin}\right)
$$

   8.4 Timing-Driven Placement

With timing criticality weights $w_i$:

$$
\min \sum_i w_i \cdot d_i(placement)
$$

 Delay model (Elmore delay): 

$$
\tau_{Elmore} = \sum_{i} R_i \cdot C_{downstream,i}
$$

   8.5 Electromigration-Aware Placement

Current density constraint:

$$
J = \frac{I}{A_{wire}} \leq J_{max}
$$

$$
MTTF = A \cdot J^{-n} \cdot e^{\frac{E_a}{kT}}
$$



  9. Process Control Mathematics

   9.1 Run-to-Run Control

 EWMA (Exponentially Weighted Moving Average): 

$$
Target_{n+1} = \lambda \cdot Measurement_n + (1-\lambda) \cdot Target_n
$$

 Where: 

- $\lambda$ = smoothing factor (0 < λ ≤ 1)
- Smaller $\lambda$ → more smoothing, slower response
- Larger $\lambda$ → less smoothing, faster response

   9.2 State-Space Model

 Process dynamics: 

$$
x_{k+1} = Ax_k + Bu_k + w_k
$$

$$
y_k = Cx_k + v_k
$$

 Where: 

- $x_k$ = state vector (e.g., tool drift)
- $u_k$ = control input (recipe adjustments)
- $y_k$ = measurement output
- $w_k, v_k$ = process and measurement noise

   9.3 Kalman Filter

 Prediction step: 

$$
\hat{x}_{k|k-1} = A\hat{x}_{k-1|k-1} + Bu_k
$$

$$
P_{k|k-1} = AP_{k-1|k-1}A^T + Q
$$

 Update step: 

$$
K_k = P_{k|k-1}C^T(CP_{k|k-1}C^T + R)^{-1}
$$

$$
\hat{x}_{k|k} = \hat{x}_{k|k-1} + K_k(y_k - C\hat{x}_{k|k-1})
$$

   9.4 Model Predictive Control (MPC)

Optimize over prediction horizon $N$:

$$
\min_{u_0, \ldots, u_{N-1}} \sum_{k=0}^{N-1} \left[ (y_k - y_{ref})^T Q (y_k - y_{ref}) + u_k^T R u_k \right]
$$

 Subject to: 

- State dynamics
- Input constraints: $u_{min} \leq u_k \leq u_{max}$
- Output constraints: $y_{min} \leq y_k \leq y_{max}$

   9.5 Virtual Metrology

Predict wafer quality from equipment sensor data:

$$
\hat{y} = f(\mathbf{s}; \theta) = \mathbf{s}^T \mathbf{w} + b
$$

 For PLS (Partial Least Squares): 

$$
\mathbf{X} = \mathbf{T}\mathbf{P}^T + \mathbf{E}
$$

$$
\mathbf{y} = \mathbf{T}\mathbf{q} + \mathbf{f}
$$



  10. Machine Learning Integration

Modern fabs increasingly use ML alongside physics-based models.

   10.1 Hotspot Detection

 Classification problem: 

$$
P(hotspot | pattern) = \sigma\left(\mathbf{W}^T \cdot CNN(pattern) + b\right)
$$

 Where: 

- $\sigma$ = sigmoid function
- $CNN$ = convolutional neural network feature extractor

 Input representations: 

- Rasterized pattern images
- Graph neural networks on layout topology

   10.2 Accelerated OPC

Neural networks predict corrections:

$$
\Delta_{OPC} = NN(P_{local}, context)
$$

 Benefits: 

- Reduce iterations from ~20 to ~3-5
- Enable curvilinear OPC at practical runtime

   10.3 Etch Modeling with ML

Hybrid physics-ML approach:

$$
CD_{final} = CD_{resist} + \Delta_{etch}(params)
$$

$$
\Delta_{etch} = f_{physics}(params) + NN_{correction}(params, pattern)
$$

   10.4 Physics-Informed Neural Networks (PINNs)

Combine data with physics constraints:

$$
\mathcal{L} = \mathcal{L}_{data} + \lambda \cdot \mathcal{L}_{physics}
$$

 Physics loss example (diffusion equation): 

$$
\mathcal{L}_{physics} = \left\| \frac{\partial u}{\partial t} - D
abla^2 u \right\|^2
$$

   10.5 Yield Prediction

 Random Forest / Gradient Boosting: 

$$
\hat{Y} = \sum_{m=1}^{M} \gamma_m h_m(\mathbf{x})
$$

 Where: 

- $h_m$ = weak learners (decision trees)
- $\gamma_m$ = weights



  11. Design-Technology Co-Optimization (DTCO)

At advanced nodes, design and process must be optimized jointly.

   11.1 Multi-Objective Formulation

$$
\min \left[ f_{performance}(x), f_{power}(x), f_{area}(x), f_{yield}(x) \right]
$$

 Subject to: 

- Design rule constraints: $g_{DR}(x) \leq 0$
- Process capability constraints: $g_{process}(x) \leq 0$
- Reliability constraints: $g_{reliability}(x) \leq 0$

   11.2 Pareto Optimality

A solution $x^*$ is Pareto optimal if:

$$

exists x : f_i(x) \leq f_i(x^*) \; \forall i \text{ and } f_j(x) < f_j(x^*) \text{ for some } j
$$

   11.3 Design Rule Optimization

Minimize total cost:

$$
\min_{DR} \left[ C_{area}(DR) + C_{yield}(DR) + C_{performance}(DR) \right]
$$

 Trade-off relationships: 

- Tighter metal pitch → smaller area, lower yield
- Larger via size → better reliability, larger area
- More routing layers → better routability, higher cost

   11.4 Standard Cell Optimization

Cell height optimization:

$$
H_{cell} = n \cdot CPP \cdot k
$$

 Where: 

- $CPP$ = contacted poly pitch
- $n$ = number of tracks
- $k$ = scaling factor

   11.5 Interconnect RC Optimization

 Resistance: 

$$
R = \rho \cdot \frac{L}{W \cdot H}
$$

 Capacitance (parallel plate approximation): 

$$
C = \epsilon \cdot \frac{A}{d}
$$

 RC delay: 

$$
\tau_{RC} = R \cdot C \propto \frac{\rho \epsilon L^2}{W H d}
$$



  12. Mathematical Stack

| Level | Mathematics | Key Challenge |
|-------|-------------|---------------|
| Optics | Fourier optics, Maxwell equations | Partially coherent imaging |
| Resist | Diffusion-reaction PDEs | Nonlinear kinetics |
| Pattern Transfer | Etch modeling, surface evolution | Multiphysics coupling |
| Placement | Graph theory, ILP, quadratic programming | NP-hard decomposition |
| Overlay | Polynomial field fitting | Sub-nm registration |
| OPC/ILT | Nonlinear inverse problems | Non-convex optimization |
| Stochastics | Poisson processes, Monte Carlo | Low-photon regimes |
| Control | State-space, Kalman filtering | Real-time adaptation |
| ML | CNNs, GNNs, PINNs | Generalization, interpretability |



  Equations

   Fundamental Lithography

$$
R_{min} = k_1 \cdot \frac{\lambda}{NA} \quad \text{(Resolution)}
$$

$$
DOF = k_2 \cdot \frac{\lambda}{NA^2} \quad \text{(Depth of Focus)}
$$

   Edge Placement

$$
EPE_{total} = \sqrt{EPE_{overlay}^2 + EPE_{CD}^2 + EPE_{LER}^2 + EPE_{stoch}^2}
$$

   Stochastic Limits (EUV)

$$
\sigma_{EPE,stoch} \propto \frac{1}{\sqrt{Dose \cdot ILS}}
$$

   OPC Optimization

$$
\min_M \|Litho(M) - P_{target}\|^2 + \lambda \mathcal{R}(M)
$$

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/pattern-placement) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
