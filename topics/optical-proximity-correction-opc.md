# Optical Proximity Correction (OPC): Mathematical Modeling

**Keywords**: optical proximity correction opc, opc correction, proximity correction, mask opc, lithography proximity correction, opc algorithms

---

**Optical Proximity Correction (OPC): Mathematical Modeling**



**1. The Physical Problem**

When projecting mask patterns onto a silicon wafer using light (typically 193nm DUV or 13.5nm EUV), several phenomena distort the image:

- **Diffraction**: Light bending around features near or below the wavelength
- **Interference**: Constructive/destructive wave interactions
- **Optical aberrations**: Lens imperfections
- **Resist effects**: Photochemical behavior during exposure and development
- **Etch loading**: Pattern-density-dependent etch rates

**OPC pre-distorts the mask** so that after all these effects, the printed pattern matches the design intent.

**Key Parameters**

| Parameter | Typical Value | Description |
|-----------|---------------|-------------|
| $\lambda$ | 193 nm (DUV), 13.5 nm (EUV) | Exposure wavelength |
| $NA$ | 0.33 - 1.35 | Numerical aperture |
| $k_1$ | 0.25 - 0.40 | Process factor |
| Resolution | $\frac{k_1 \lambda}{NA}$ | Minimum feature size |



**2. Hopkins Imaging Model**

The foundational mathematical framework for **partially coherent lithographic imaging** comes from Hopkins' theory (1953).

**Aerial Image Intensity**

The aerial image intensity at position $\mathbf{r} = (x, y)$ is given by:

$$
I(\mathbf{r}) = \iiint\!\!\!\iint TCC(\mathbf{f}_1, \mathbf{f}_2) \cdot M(\mathbf{f}_1) \cdot M^*(\mathbf{f}_2) \cdot e^{2\pi i (\mathbf{f}_1 - \mathbf{f}_2) \cdot \mathbf{r}} \, d\mathbf{f}_1 \, d\mathbf{f}_2
$$

Where:

- $M(\mathbf{f})$ — Fourier transform of the mask transmission function
- $M^*(\mathbf{f})$ — Complex conjugate of $M(\mathbf{f})$
- $TCC$ — Transmission Cross Coefficient
- $\mathbf{f} = (f_x, f_y)$ — Spatial frequency coordinates

**Transmission Cross Coefficient (TCC)**

The TCC encodes the optical system characteristics:

$$
TCC(\mathbf{f}_1, \mathbf{f}_2) = \iint J(\mathbf{f}) \cdot H(\mathbf{f} + \mathbf{f}_1) \cdot H^*(\mathbf{f} + \mathbf{f}_2) \, d\mathbf{f}
$$

Where:

- $J(\mathbf{f})$ — Source (illumination) intensity distribution (mutual intensity at mask)
- $H(\mathbf{f})$ — Pupil function of the projection lens
- $H^*(\mathbf{f})$ — Complex conjugate of pupil function

**Pupil Function**

For an ideal circular aperture:

$$
H(\mathbf{f}) = \begin{cases} 
1 & \text{if } |\mathbf{f}| \leq \frac{NA}{\lambda} \\
0 & \text{otherwise}
\end{cases}
$$

With aberrations included:

$$
H(\mathbf{f}) = P(\mathbf{f}) \cdot e^{i \cdot W(\mathbf{f})}
$$

Where $W(\mathbf{f})$ is the wavefront aberration function (Zernike polynomial expansion).



**3. SOCS Decomposition**

**Sum of Coherent Systems**

To make computation tractable, the TCC (a Hermitian matrix when discretized) is decomposed via **eigenvalue decomposition**:

$$
TCC(\mathbf{f}_1, \mathbf{f}_2) = \sum_{n=1}^{N} \lambda_n \cdot \phi_n(\mathbf{f}_1) \cdot \phi_n^*(\mathbf{f}_2)
$$

Where:

- $\lambda_n$ — Eigenvalues (sorted in descending order)
- $\phi_n(\mathbf{f})$ — Eigenvectors (orthonormal kernels)

**Image Computation**

This allows the image to be computed as a **sum of coherent images**:

$$
I(\mathbf{r}) = \sum_{n=1}^{N} \lambda_n \left| \mathcal{F}^{-1}\{\phi_n \cdot M\} \right|^2
$$

Or equivalently:

$$
I(\mathbf{r}) = \sum_{n=1}^{N} \lambda_n \left| I_n(\mathbf{r}) \right|^2
$$

Where each coherent image is:

$$
I_n(\mathbf{r}) = \mathcal{F}^{-1}\{\phi_n(\mathbf{f}) \cdot M(\mathbf{f})\}
$$

**Practical Considerations**

- **Eigenvalue decay**: $\lambda_n$ decay rapidly; typically only 10–50 terms needed
- **Speedup**: Converts one $O(N^4)$ partially coherent calculation into $\sim$20 $O(N^2 \log N)$ FFT operations
- **Accuracy**: Trade-off between number of terms and simulation accuracy



**4. OPC Problem Formulation**

**Forward Problem**

Given mask $M(\mathbf{r})$, predict wafer pattern $W(\mathbf{r})$:

$$
M \xrightarrow{\text{optics}} I(\mathbf{r}) \xrightarrow{\text{resist}} R(\mathbf{r}) \xrightarrow{\text{etch}} W(\mathbf{r})
$$

**Mathematical chain:**

1. **Optical Model**: $I = \mathcal{O}(M)$ — Hopkins/SOCS imaging
2. **Resist Model**: $R = \mathcal{R}(I)$ — Threshold or convolution model
3. **Etch Model**: $W = \mathcal{E}(R)$ — Etch bias and loading

**Inverse Problem (OPC)**

Given target pattern $T(\mathbf{r})$, find mask $M(\mathbf{r})$ such that:

$$
W(M) \approx T
$$

**This is fundamentally ill-posed:**

- Non-unique: Many masks could produce similar results
- Nonlinear: The imaging equation is quadratic in mask transmission
- Constrained: Mask must be manufacturable



**5. Edge Placement Error Minimization**

**Objective Function**

The standard OPC objective minimizes **Edge Placement Error (EPE)**:

$$
\min_M \mathcal{L}(M) = \sum_{i=1}^{N_{\text{edges}}} w_i \cdot \text{EPE}_i^2
$$

Where:

$$
\text{EPE}_i = x_i^{\text{printed}} - x_i^{\text{target}}
$$

- $x_i^{\text{printed}}$ — Actual edge position after lithography
- $x_i^{\text{target}}$ — Desired edge position from design
- $w_i$ — Weight for edge $i$ (can prioritize critical features)

**Constraints**

Subject to mask manufacturability:

- **Minimum feature size**: $\text{CD}_{\text{mask}} \geq \text{CD}_{\min}$
- **Minimum spacing**: $\text{Space}_{\text{mask}} \geq \text{Space}_{\min}$
- **Maximum jog**: Limit on edge fragmentation complexity
- **MEEF constraint**: Mask Error Enhancement Factor within spec

**Iterative Edge-Based OPC Algorithm**

The classic algorithm moves mask edges iteratively:

$$
\Delta x^{(n+1)} = \Delta x^{(n)} - \alpha \cdot \text{EPE}^{(n)}
$$

Where:

- $\Delta x$ — Edge movement from original position
- $\alpha$ — Damping factor (typically 0.3–0.8)
- $n$ — Iteration number

**Convergence criterion:**

$$
\max_i |\text{EPE}_i| < \epsilon \quad \text{or} \quad n > n_{\max}
$$

**Gradient Computation**

Using the chain rule:

$$
\frac{\partial \text{EPE}}{\partial m} = \frac{\partial \text{EPE}}{\partial I} \cdot \frac{\partial I}{\partial m}
$$

Where $m$ represents mask parameters (edge positions, segment lengths).

At a contour position where $I = I_{th}$:

$$
\frac{\partial x_{\text{edge}}}{\partial m} = -\frac{1}{|
abla I|} \cdot \frac{\partial I}{\partial m}
$$

The **image log-slope (ILS)** is a key metric:

$$
\text{ILS} = \frac{1}{I} \left| \frac{\partial I}{\partial x} \right|_{I = I_{th}}
$$

Higher ILS → better process latitude, lower EPE sensitivity.



**6. Resist Modeling**

**Threshold Model (Simplest)**

The resist develops where intensity exceeds threshold:

$$
R(\mathbf{r}) = \begin{cases} 
1 & \text{if } I(\mathbf{r}) > I_{th} \\
0 & \text{otherwise}
\end{cases}
$$

The printed contour is the $I_{th}$ isoline.

**Variable Threshold Resist (VTR)**

The threshold varies with local context:

$$
I_{th}(\mathbf{r}) = I_{th,0} + \beta_1 \cdot \bar{I}_{\text{local}} + \beta_2 \cdot 
abla^2 I + \beta_3 \cdot (
abla I)^2 + \ldots
$$

Where:

- $I_{th,0}$ — Base threshold
- $\bar{I}_{\text{local}}$ — Local average intensity (density effect)
- $
abla^2 I$ — Laplacian (curvature effect)
- $\beta_i$ — Fitted coefficients

**Compact Phenomenological Models**

For OPC speed, empirical models are used instead of physics-based resist simulation:

$$
R(\mathbf{r}) = \sum_{j=1}^{N_k} w_j \cdot \left( K_j \otimes g_j(I) \right)
$$

Where:

- $K_j$ — Convolution kernels (typically Gaussians):
  
  $$K_j(\mathbf{r}) = \frac{1}{2\pi\sigma_j^2} \exp\left( -\frac{|\mathbf{r}|^2}{2\sigma_j^2} \right)$$

- $g_j(I)$ — Nonlinear functions: $I$, $I^2$, $\log(I)$, $\sqrt{I}$, etc.
- $w_j$ — Fitted weights
- $\otimes$ — Convolution operator

**Physical Interpretation**

| Kernel Width | Physical Effect |
|--------------|-----------------|
| Small $\sigma$ | Optical proximity effects |
| Medium $\sigma$ | Acid/base diffusion in resist |
| Large $\sigma$ | Long-range loading effects |

**Model Calibration**

Parameters are fitted to wafer measurements:

$$
\min_{\theta} \sum_{k=1}^{N_{\text{test}}} \left( \text{CD}_k^{\text{measured}} - \text{CD}_k^{\text{model}}(\theta) \right)^2 + \lambda \|\theta\|^2
$$

Where:

- $\theta = \{w_j, \sigma_j, \beta_i, \ldots\}$ — Model parameters
- $\lambda \|\theta\|^2$ — Regularization term
- Test structures: Lines, spaces, contacts, line-ends at various pitches/densities



**7. Inverse Lithography Technology**

**Full Optimization Formulation**

ILT treats the mask as a continuous optimization variable (pixelated):

$$
\min_{M} \mathcal{L}(M) = \| W(M) - T \|^2 + \lambda \cdot \mathcal{R}(M)
$$

Where:

- $W(M)$ — Predicted wafer pattern
- $T$ — Target pattern
- $\mathcal{R}(M)$ — Regularization for manufacturability
- $\lambda$ — Regularization weight

**Cost Function Components**

**Pattern Fidelity Term:**

$$
\mathcal{L}_{\text{fidelity}} = \int \left( W(\mathbf{r}) - T(\mathbf{r}) \right)^2 d\mathbf{r}
$$

Or in discrete form:

$$
\mathcal{L}_{\text{fidelity}} = \sum_{\mathbf{r} \in \text{grid}} \left( W(\mathbf{r}) - T(\mathbf{r}) \right)^2
$$

**Regularization Terms**

**Total Variation** (promotes piecewise constant, sharp edges):

$$
\mathcal{R}_{TV}(M) = \int |
abla M| \, d\mathbf{r} = \int \sqrt{\left(\frac{\partial M}{\partial x}\right)^2 + \left(\frac{\partial M}{\partial y}\right)^2} \, d\mathbf{r}
$$

**Curvature Penalty** (promotes smooth contours):

$$
\mathcal{R}_{\kappa}(M) = \oint_{\partial M} \kappa^2 \, ds
$$

Where $\kappa$ is the local curvature of the mask boundary.

**Minimum Feature Size** (MRC - Mask Rule Check):

$$
\mathcal{R}_{MRC}(M) = \sum_{\text{violations}} \text{penalty}(\text{violation severity})
$$

**Sigmoid Regularization** (push mask toward binary):

$$
\mathcal{R}_{\text{binary}}(M) = \int M(1-M) \, d\mathbf{r}
$$

**Level Set Formulation**

Represent the mask boundary implicitly via level set function $\phi(\mathbf{r})$:

- Inside chrome: $\phi(\mathbf{r}) < 0$
- Outside chrome: $\phi(\mathbf{r}) > 0$
- Boundary: $\phi(\mathbf{r}) = 0$

**Evolution equation:**

$$
\frac{\partial \phi}{\partial t} = -v \cdot |
abla \phi|
$$

Where velocity $v$ is derived from the cost function gradient:

$$
v = -\frac{\delta \mathcal{L}}{\delta \phi}
$$

**Advantages:**

- Naturally handles topological changes (features splitting/merging)
- Implicit curvature regularization available
- Well-studied numerical methods

**Optimization Algorithms**

Since the problem is **non-convex**, various methods are used:

1. **Gradient Descent with Momentum:**
   
   $$
   M^{(n+1)} = M^{(n)} - \eta 
abla_M \mathcal{L} + \mu \left( M^{(n)} - M^{(n-1)} \right)
   $$

2. **Conjugate Gradient:**
   
   $$
   d^{(n+1)} = -
abla \mathcal{L}^{(n+1)} + \beta^{(n)} d^{(n)}
   $$

3. **Adam Optimizer:**
   
   $$
   m_t = \beta_1 m_{t-1} + (1-\beta_1) g_t
   $$
   $$
   v_t = \beta_2 v_{t-1} + (1-\beta_2) g_t^2
   $$
   $$
   M_{t+1} = M_t - \eta \frac{\hat{m}_t}{\sqrt{\hat{v}_t} + \epsilon}
   $$

4. **Genetic Algorithms** (for discrete/combinatorial aspects)

5. **Simulated Annealing** (for escaping local minima)



**8. Source-Mask Optimization**

**Joint Optimization**

SMO optimizes both illumination source $S$ and mask $M$ simultaneously:

$$
\min_{S, M} \sum_{j \in \text{PW}} w_j \cdot \| W(S, M, \text{condition}_j) - T \|^2
$$

**Source Parameterization**

**Pixelated Source:**

$$
S = \{s_{ij}\} \quad \text{where } s_{ij} \in [0, 1]
$$

Each pixel in the pupil plane is a free variable.

**Parametric Source:**

- Annular: $(R_{\text{inner}}, R_{\text{outer}})$
- Quadrupole: $(R, \theta, \sigma)$
- Freeform: Spline or Zernike coefficients

**Alternating Optimization**

**Algorithm:**

```
Initialize: S⁰, M⁰
for k = 1 to max_iter:
    # Step 1: Fix S, optimize M (standard OPC)
    M^k = argmin_M L(S^(k-1), M)
    
    # Step 2: Fix M, optimize S
    S^k = argmin_S L(S, M^k)
    
    # Check convergence
    if |L^k - L^(k-1)| < tolerance:
        break
```

**Note:** Step 2 is often convex in $S$ when $M$ is fixed (linear in source pixels for intensity-based metrics).

**Mathematical Form for Source Optimization**

When mask is fixed, the image is linear in source:

$$
I(\mathbf{r}; S) = \sum_{ij} s_{ij} \cdot I_{ij}(\mathbf{r})
$$

Where $I_{ij}$ is the image contribution from source pixel $(i,j)$.

This makes source optimization a **quadratic program** (convex if cost is convex in $I$).


**9. Process Window Optimization**

**Multi-Condition Optimization**

Real manufacturing has variations. Robust OPC optimizes across a **process window (PW)**:

$$
\min_M \sum_{j \in \text{PW}} w_j \cdot \mathcal{L}(M, \text{condition}_j)
$$

**Process Window Dimensions**

| Dimension | Typical Range | Effect |
|-----------|---------------|--------|
| Focus | $\pm 50$ nm | Defocus blur |
| Dose | $\pm 3\%$ | Threshold shift |
| Mask CD | $\pm 2$ nm | Feature size bias |
| Aberrations | Per-lens | Pattern distortion |

**Worst-Case (Minimax) Formulation**

$$
\min_M \max_{j \in \text{PW}} \text{EPE}_j(M)
$$

This is more conservative but ensures robustness.

**Soft Constraints via Barrier Functions**

$$
\mathcal{L}_{PW}(M) = \sum_j w_j \cdot \text{EPE}_j^2 + \mu \sum_j \sum_i \max(0, |\text{EPE}_{ij}| - \text{spec})^2
$$

**Process Window Metrics**

**Common Process Window (CPW):**

$$
\text{CPW} = \text{Focus Range} \times \text{Dose Range}
$$

Where all specs are simultaneously met.

**Exposure Latitude (EL):**

$$
\text{EL} = \frac{\Delta \text{Dose}}{\text{Dose}_{\text{nom}}} \times 100\%
$$

**Depth of Focus (DOF):**

$$
\text{DOF} = \text{Focus range where } |\text{EPE}| < \text{spec}
$$



**10. Stochastic Effects (EUV)**

At EUV wavelengths (13.5 nm), **photon counts are low** and shot noise becomes significant.

**Photon Statistics**

Number of photons per pixel follows **Poisson distribution**:

$$
P(n | \bar{n}) = \frac{\bar{n}^n e^{-\bar{n}}}{n!}
$$

Where:

$$
\bar{n} = \frac{E \cdot A \cdot \eta}{\frac{hc}{\lambda}}
$$

- $E$ — Exposure dose (mJ/cm²)
- $A$ — Pixel area
- $\eta$ — Quantum efficiency
- $\frac{hc}{\lambda}$ — Photon energy

**Signal-to-Noise Ratio**

$$
\text{SNR} = \frac{\bar{n}}{\sqrt{\bar{n}}} = \sqrt{\bar{n}}
$$

For reliable imaging, need $\text{SNR} > 5$, requiring $\bar{n} > 25$ photons/pixel.

**Line Edge Roughness (LER)**

Random edge fluctuations characterized by:

- **3σ LER**: $3 \times \text{standard deviation of edge position}$
- **Correlation length** $\xi$: Spatial extent of roughness

**Power Spectral Density:**

$$
\text{PSD}(f) = \frac{2\sigma^2 \xi}{1 + (2\pi f \xi)^{2\alpha}}
$$

Where $\alpha$ is the roughness exponent (typically 0.5–1.0).

**Stochastic Defect Probability**

Probability of a stochastic failure (missing contact, bridging):

$$
P_{\text{fail}} = 1 - \prod_{\text{features}} (1 - p_i)
$$

For rare events, approximately:

$$
P_{\text{fail}} \approx \sum_i p_i
$$

**Stochastic-Aware OPC Objective**

$$
\min_M \mathbb{E}[\text{EPE}^2] + \lambda_1 \cdot \text{Var}(\text{EPE}) + \lambda_2 \cdot P_{\text{fail}}
$$

**Monte Carlo Simulation**

For stochastic modeling:

1. Sample photon arrival: $n_{ij} \sim \text{Poisson}(\bar{n}_{ij})$
2. Simulate acid generation: Proportional to absorbed photons
3. Simulate diffusion: Random walk or stochastic PDE
4. Simulate development: Threshold with noise
5. Repeat $N$ times, compute statistics



**11. Machine Learning Approaches**

**Neural Network Forward Models**

Train networks to approximate expensive simulations:

$$
\hat{I} = f_\theta(M) \approx I_{\text{optical}}(M)
$$

**Architectures:**

- **CNN**: Convolutional neural networks for local pattern effects
- **U-Net**: Encoder-decoder for image-to-image translation
- **GAN**: Generative adversarial networks for realistic image generation

**Training:**

$$
\min_\theta \sum_{k} \| f_\theta(M_k) - I_k^{\text{simulation}} \|^2
$$

**End-to-End ILT with Deep Learning**

Directly predict corrected masks:

$$
\hat{M}_{\text{OPC}} = G_\theta(T)
$$

**Training data:** Pairs $(T, M_{\text{optimal}})$ from conventional ILT.

**Loss function:**

$$
\mathcal{L} = \| W(G_\theta(T)) - T \|^2 + \lambda \| G_\theta(T) - M_{\text{ref}} \|^2
$$

**Hybrid Approaches**

Combine ML speed with physics accuracy:

1. **ML Initialization**: $M^{(0)} = G_\theta(T)$
2. **Physics Refinement**: Run conventional OPC starting from $M^{(0)}$

**Benefits:**

- Faster convergence (good starting point)
- Physics ensures accuracy
- ML handles global pattern context

**Neural Network Architectures for OPC**

| Architecture | Use Case | Advantages |
|--------------|----------|------------|
| CNN | Local correction prediction | Fast inference |
| U-Net | Full mask prediction | Multi-scale features |
| GAN | Realistic mask generation | Sharp boundaries |
| Transformer | Global context | Long-range dependencies |
| Physics-Informed NN | Constrained prediction | Respects physics |



**12. Computational Complexity**

**Scale of Full-Chip OPC**

- **Features per chip**: $10^9 - 10^{10}$
- **Evaluation points**: $\sim 10^{12}$ (multiple points per feature)
- **Iterations**: 10–50 per feature
- **Optical simulations**: $O(N \log N)$ per FFT

**Complexity Analysis**

**Single feature OPC:**

$$
T_{\text{feature}} = O(N_{\text{iter}} \times N_{\text{SOCS}} \times N_{\text{grid}} \log N_{\text{grid}})
$$

**Full chip:**

$$
T_{\text{chip}} = O(N_{\text{features}} \times T_{\text{feature}})
$$

**Result:** Hours to days on large compute clusters.

**Acceleration Strategies**

**Hierarchical Processing:**

- Identify repeated cells (memory arrays, standard cells)
- Compute OPC once, reuse for identical instances
- Speedup: $10\times - 100\times$ for regular designs

**GPU Parallelization:**

- FFTs parallelize well on GPUs
- Convolutions map to tensor operations
- Multiple features processed simultaneously
- Speedup: $10\times - 50\times$

**Approximate Models:**

- **Kernel-based**: Pre-compute influence functions
- **Variable resolution**: Fine grid only near edges
- **Neural surrogates**: Replace simulation with inference

**Domain Decomposition:**

- Divide chip into tiles
- Process tiles in parallel
- Handle tile boundaries with overlap or iteration



**13. Mathematical Toolkit Summary**

| Domain | Techniques |
|--------|-----------|
| **Optics** | Fourier transforms, Hopkins theory, SOCS decomposition, Abbe imaging |
| **Optimization** | Gradient descent, conjugate gradient, level sets, genetic algorithms, simulated annealing |
| **Linear Algebra** | Eigendecomposition (TCC), sparse matrices, SVD, matrix factorization |
| **PDEs** | Diffusion equations (resist), level set evolution, Hamilton-Jacobi |
| **Statistics** | Poisson processes, Monte Carlo, stochastic simulation, Bayesian inference |
| **Machine Learning** | CNNs, GANs, U-Net, transformers, physics-informed neural networks |
| **Computational Geometry** | Polygon operations, fragmentation, contour extraction, Boolean operations |
| **Numerical Methods** | FFT, finite differences, quadrature, interpolation |



**Equations Quick Reference**

**Hopkins Imaging**

$$
I(\mathbf{r}) = \iiint\!\!\!\iint TCC(\mathbf{f}_1, \mathbf{f}_2) \cdot M(\mathbf{f}_1) \cdot M^*(\mathbf{f}_2) \cdot e^{2\pi i (\mathbf{f}_1 - \mathbf{f}_2) \cdot \mathbf{r}} \, d\mathbf{f}_1 \, d\mathbf{f}_2
$$

**SOCS Image**

$$
I(\mathbf{r}) = \sum_{n=1}^{N} \lambda_n \left| \mathcal{F}^{-1}\{\phi_n \cdot M\} \right|^2
$$

**EPE Minimization**

$$
\min_M \sum_{i} w_i \left( x_i^{\text{printed}} - x_i^{\text{target}} \right)^2
$$

**ILT Cost Function**

$$
\min_{M} \| W(M) - T \|^2 + \lambda \cdot \mathcal{R}(M)
$$

**Level Set Evolution**

$$
\frac{\partial \phi}{\partial t} = -v \cdot |
abla \phi|
$$

**Poisson Photon Statistics**

$$
P(n | \bar{n}) = \frac{\bar{n}^n e^{-\bar{n}}}{n!}
$$

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/optical-proximity-correction-opc) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
