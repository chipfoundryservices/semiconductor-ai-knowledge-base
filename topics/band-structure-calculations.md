# Band Structure Calculations in Semiconductor Manufacturing

**Keywords**: band structure calculations, band structure, electronic band, DFT, density functional theory, Kohn-Sham, Bloch theorem, Brillouin zone, effective mass, kp theory, GW approximation, tight binding, pseudopotential

---

**Band Structure Calculations in Semiconductor Manufacturing**

**Mathematical Framework**



**1. The Fundamental Problem**

We need to solve the many-body Schrödinger equation for electrons in a crystal:

$$
\hat{H}\Psi = E\Psi
$$

The full Hamiltonian includes kinetic energy, ion-electron interaction, and electron-electron repulsion:

$$
\hat{H} = -\sum_i \frac{\hbar^2}{2m}
abla_i^2 + \sum_i V_{\text{ion}}(\mathbf{r}_i) + \frac{1}{2}\sum_{i 
eq j} \frac{e^2}{|\mathbf{r}_i - \mathbf{r}_j|}
$$

**Key challenges:**

- The system contains ~$10^{23}$ electrons
- Electron-electron interactions couple all particles
- Analytical solution is impossible for real materials
- Requires a hierarchy of approximations



**2. Density Functional Theory (DFT)**

The workhorse of modern band structure calculations rests on the **Hohenberg-Kohn theorems**:

1. Ground-state properties are uniquely determined by electron density $n(\mathbf{r})$
2. The true ground-state density minimizes the energy functional

**2.1 Kohn-Sham Equations**

The many-body problem is mapped to non-interacting electrons in an effective potential:

$$
\left[-\frac{\hbar^2}{2m}
abla^2 + V_{\text{eff}}(\mathbf{r})\right]\psi_i(\mathbf{r}) = \epsilon_i\psi_i(\mathbf{r})
$$

where the effective potential is:

$$
V_{\text{eff}}(\mathbf{r}) = V_{\text{ion}}(\mathbf{r}) + V_H(\mathbf{r}) + V_{xc}[n]
$$

**Components of $V_{\text{eff}}$:**

- **Ionic potential**: $V_{\text{ion}}(\mathbf{r})$ — interaction with nuclei
- **Hartree potential**: $V_H(\mathbf{r}) = \int \frac{n(\mathbf{r}')}{|\mathbf{r}-\mathbf{r}'|}d\mathbf{r}'$ — classical electrostatic repulsion
- **Exchange-correlation**: $V_{xc}[n] = \frac{\delta E_{xc}[n]}{\delta n(\mathbf{r})}$ — quantum many-body effects

The density is reconstructed self-consistently:

$$
n(\mathbf{r}) = \sum_i^{\text{occupied}} |\psi_i(\mathbf{r})|^2
$$

**2.2 Exchange-Correlation Functionals**

The unknown piece requiring approximation:

- **Local Density Approximation (LDA)**:
  $$
  E_{xc}^{\text{LDA}}[n] = \int n(\mathbf{r})\,\epsilon_{xc}^{\text{homog}}(n(\mathbf{r}))\,d\mathbf{r}
  $$

- **Generalized Gradient Approximation (GGA)**:
  $$
  E_{xc}^{\text{GGA}}[n] = \int f\left(n(\mathbf{r}), 
abla n(\mathbf{r})\right)\,d\mathbf{r}
  $$

- **Hybrid Functionals (HSE06)**:
  $$
  E_{xc}^{\text{HSE}} = \frac{1}{4}E_x^{\text{HF,SR}}(\mu) + \frac{3}{4}E_x^{\text{PBE,SR}}(\mu) + E_x^{\text{PBE,LR}}(\mu) + E_c^{\text{PBE}}
  $$
  - Mixing parameter: $\alpha = 0.25$
  - Screening parameter: $\mu \approx 0.2\,\text{Å}^{-1}$



**3. Bloch's Theorem and Reciprocal Space**

For a periodic crystal with lattice vectors $\mathbf{R}$, the fundamental symmetry relation:

$$
\psi_{n\mathbf{k}}(\mathbf{r}) = e^{i\mathbf{k}\cdot\mathbf{r}}\,u_{n\mathbf{k}}(\mathbf{r})
$$

where:

- $u_{n\mathbf{k}}(\mathbf{r})$ has lattice periodicity: $u_{n\mathbf{k}}(\mathbf{r} + \mathbf{R}) = u_{n\mathbf{k}}(\mathbf{r})$
- $\mathbf{k}$ is the crystal momentum (wavevector)
- $n$ is the band index

**3.1 Reciprocal Lattice**

Reciprocal lattice vectors $\mathbf{G}$ satisfy:

$$
\mathbf{G} \cdot \mathbf{R} = 2\pi m \quad (m \in \mathbb{Z})
$$

For a cubic lattice with parameter $a$:

$$
\mathbf{G} = \frac{2\pi}{a}(h\hat{\mathbf{x}} + k\hat{\mathbf{y}} + l\hat{\mathbf{z}})
$$

The **band structure** $E_n(\mathbf{k})$ emerges as eigenvalues indexed by:

- Band number $n$
- Wavevector $\mathbf{k}$ within the first Brillouin zone



**4. Basis Set Expansions**

**4.1 Plane Wave Basis**

Expand the periodic part in Fourier series:

$$
u_{n\mathbf{k}}(\mathbf{r}) = \sum_{\mathbf{G}} c_{n,\mathbf{k}+\mathbf{G}}\,e^{i\mathbf{G}\cdot\mathbf{r}}
$$

The Schrödinger equation becomes a matrix eigenvalue problem:

$$
\sum_{\mathbf{G}'} H_{\mathbf{G},\mathbf{G}'}(\mathbf{k})\,c_{\mathbf{G}'} = E_{n\mathbf{k}}\,c_{\mathbf{G}}
$$

**Matrix elements:**

$$
H_{\mathbf{G},\mathbf{G}'} = \frac{\hbar^2|\mathbf{k}+\mathbf{G}|^2}{2m}\delta_{\mathbf{G},\mathbf{G}'} + V(\mathbf{G}-\mathbf{G}')
$$

**Basis truncation** via kinetic energy cutoff:

$$
\frac{\hbar^2|\mathbf{k}+\mathbf{G}|^2}{2m} < E_{\text{cut}}
$$

Typical values: $E_{\text{cut}} \sim 30\text{--}80\,\text{Ry}$ (400–1000 eV)

**4.2 Localized Basis (LCAO/Tight-Binding)**

Linear Combination of Atomic Orbitals:

$$
\psi_{n\mathbf{k}}(\mathbf{r}) = \sum_{\alpha} c_{n\alpha\mathbf{k}} \sum_{\mathbf{R}} e^{i\mathbf{k}\cdot\mathbf{R}}\phi_\alpha(\mathbf{r} - \mathbf{R} - \mathbf{d}_\alpha)
$$

This yields a **generalized eigenvalue problem**:

$$
H(\mathbf{k})\,\mathbf{c} = E(\mathbf{k})\,S(\mathbf{k})\,\mathbf{c}
$$

where:

- $H_{ij}(\mathbf{k}) = \sum_{\mathbf{R}} e^{i\mathbf{k}\cdot\mathbf{R}}\langle\phi_i(\mathbf{r})|\hat{H}|\phi_j(\mathbf{r}-\mathbf{R})\rangle$ — Hamiltonian matrix
- $S_{ij}(\mathbf{k}) = \sum_{\mathbf{R}} e^{i\mathbf{k}\cdot\mathbf{R}}\langle\phi_i(\mathbf{r})|\phi_j(\mathbf{r}-\mathbf{R})\rangle$ — Overlap matrix

**4.3 Slater-Koster Parameters**

For empirical tight-binding with direction cosines $(l, m, n)$:

$$
\begin{aligned}
E_{s,s} &= V_{ss\sigma} \\
E_{s,x} &= l \cdot V_{sp\sigma} \\
E_{x,x} &= l^2 V_{pp\sigma} + (1-l^2) V_{pp\pi} \\
E_{x,y} &= lm(V_{pp\sigma} - V_{pp\pi})
\end{aligned}
$$

**Harrison's universal parameters:**

| Integral | Formula |
|----------|---------|
| $V_{ss\sigma}$ | $-1.40 \dfrac{\hbar^2}{md^2}$ |
| $V_{sp\sigma}$ | $1.84 \dfrac{\hbar^2}{md^2}$ |
| $V_{pp\sigma}$ | $3.24 \dfrac{\hbar^2}{md^2}$ |
| $V_{pp\pi}$ | $-0.81 \dfrac{\hbar^2}{md^2}$ |



**5. Pseudopotential Theory**

Core electrons are chemically inert but computationally expensive. Replace true potential with smooth pseudopotential.

**5.1 Norm-Conserving Conditions**

(Hamann, Schlüter, Chiang):

1. **Matching**: $\psi^{\text{PS}}(r) = \psi^{\text{AE}}(r)$ for $r > r_c$
2. **Norm conservation**:
   $$
   \int_0^{r_c}|\psi^{\text{PS}}(r)|^2 r^2 dr = \int_0^{r_c}|\psi^{\text{AE}}(r)|^2 r^2 dr
   $$
3. **Eigenvalue matching**: $\epsilon^{\text{PS}} = \epsilon^{\text{AE}}$
4. **Log-derivative matching**:
   $$
   \left.\frac{d}{dr}\ln\psi^{\text{PS}}\right|_{r_c} = \left.\frac{d}{dr}\ln\psi^{\text{AE}}\right|_{r_c}
   $$

**5.2 Ultrasoft Pseudopotentials (Vanderbilt)**

Relaxes norm conservation for smoother potentials:

$$
\hat{H}|\psi_i\rangle = \epsilon_i\hat{S}|\psi_i\rangle
$$

where:

$$
\hat{S} = 1 + \sum_{ij}q_{ij}|\beta_i\rangle\langle\beta_j|
$$

**5.3 Projector Augmented Wave (PAW) Method**

Linear transformation connecting pseudo and all-electron wavefunctions:

$$
|\psi\rangle = |\tilde{\psi}\rangle + \sum_i \left(|\phi_i\rangle - |\tilde{\phi}_i\rangle\right)\langle\tilde{p}_i|\tilde{\psi}\rangle
$$

**Components:**

- $|\tilde{\psi}\rangle$ — smooth pseudo-wavefunction
- $|\phi_i\rangle$ — all-electron partial waves
- $|\tilde{\phi}_i\rangle$ — pseudo partial waves
- $|\tilde{p}_i\rangle$ — projector functions



**6. Brillouin Zone Integration**

Physical observables require integration over $\mathbf{k}$-space:

$$
\langle A \rangle = \frac{1}{\Omega_{BZ}}\int_{BZ} A(\mathbf{k})\,d\mathbf{k}
$$

**6.1 Monkhorst-Pack Grid**

Systematic $\mathbf{k}$-point sampling:

$$
\mathbf{k}_{n_1,n_2,n_3} = \sum_{i=1}^{3} \frac{2n_i - N_i - 1}{2N_i}\mathbf{b}_i
$$

where:

- $n_i = 1, 2, \ldots, N_i$
- $\mathbf{b}_i$ are reciprocal lattice vectors
- Grid specified as $N_1 \times N_2 \times N_3$

**6.2 Density of States**

The tetrahedron method improves integration accuracy:

$$
g(E) = \frac{1}{\Omega_{BZ}}\int_{BZ}\delta(E - E_{n\mathbf{k}})\,d\mathbf{k}
$$

**Practical evaluation:**

- Divide Brillouin zone into tetrahedra
- Linear interpolation of $E_n(\mathbf{k})$ within each tetrahedron
- Analytical integration of $\delta$-function



**7. Self-Consistent Field (SCF) Iteration**

**7.1 Algorithm**

1. Initialize density $n^{(0)}(\mathbf{r})$
2. Construct $V_{\text{eff}}[n]$
3. Diagonalize Kohn-Sham equations → obtain $\{\psi_i, \epsilon_i\}$
4. Compute new density:
   $$
   n^{\text{new}}(\mathbf{r}) = \sum_i^{\text{occ}}|\psi_i(\mathbf{r})|^2
   $$
5. Mix densities:
   $$
   n^{\text{in}} = (1-\alpha)n^{\text{old}} + \alpha n^{\text{new}}
   $$
6. Repeat until $\|n^{\text{new}} - n^{\text{old}}\| < \epsilon$

**7.2 Mixing Schemes**

- **Linear mixing**: Simple but slow convergence
  $$
  n^{(i+1)} = (1-\alpha)n^{(i)} + \alpha n^{\text{out},[i]}
  $$

- **Pulay mixing (DIIS)**: Minimizes residual over history
  $$
  n^{\text{in}} = \sum_j c_j n^{(j)}, \quad \text{where } \{c_j\} \text{ minimize } \left\|\sum_j c_j R^{(j)}\right\|
  $$

- **Broyden mixing**: Quasi-Newton approach
  $$
  n^{(i+1)} = n^{(i)} - \alpha B^{(i)} R^{(i)}
  $$



**8. Beyond DFT: The Band Gap Problem**

DFT-LDA/GGA systematically underestimates band gaps.

**Typical underestimation:**

| Material | Expt. Gap (eV) | LDA Gap (eV) | Error |
|----------|----------------|--------------|-------|
| Si | 1.17 | 0.52 | -56% |
| GaAs | 1.52 | 0.30 | -80% |
| Ge | 0.74 | 0.00 | -100% |

**8.1 GW Approximation**

The self-energy captures many-body corrections:

$$
\Sigma(\mathbf{r}, \mathbf{r}'; \omega) = \frac{i}{2\pi}\int G(\mathbf{r}, \mathbf{r}'; \omega+\omega')\,W(\mathbf{r}, \mathbf{r}'; \omega')\,d\omega'
$$

**Components:**

- $G$ — single-particle Green's function
- $W$ — screened Coulomb interaction:
  $$
  W = \epsilon^{-1}v
  $$

**Dielectric function (RPA):**

$$
\epsilon(\mathbf{r}, \mathbf{r}'; \omega) = \delta(\mathbf{r} - \mathbf{r}') - \int v(\mathbf{r} - \mathbf{r}'')P^0(\mathbf{r}'', \mathbf{r}'; \omega)\,d\mathbf{r}''
$$

**Quasiparticle correction:**

$$
E_{n\mathbf{k}}^{\text{QP}} = E_{n\mathbf{k}}^{\text{DFT}} + \langle\psi_{n\mathbf{k}}|\Sigma(E^{\text{QP}}) - V_{xc}|\psi_{n\mathbf{k}}\rangle
$$

This typically adds 0.5–2 eV to band gaps.



**9. Effective Mass and k·p Theory**

Near band extrema, expand energy to quadratic order:

$$
E_n(\mathbf{k}) \approx E_n(\mathbf{k}_0) + \frac{\hbar^2}{2}\sum_{ij}k_i\left(\frac{1}{m^*}\right)_{ij}k_j
$$

**9.1 Effective Mass Tensor**

From second-order perturbation theory:

$$
\left(\frac{1}{m^*}\right)_{ij} = \frac{1}{m}\delta_{ij} + \frac{2}{m^2}\sum_{n'
eq n}\frac{\langle n|\hat{p}_i|n'\rangle\langle n'|\hat{p}_j|n\rangle}{E_n - E_{n'}}
$$

**Alternate form using band curvature:**

$$
\left(\frac{1}{m^*}\right)_{ij} = \frac{1}{\hbar^2}\frac{\partial^2 E_n}{\partial k_i \partial k_j}
$$

**9.2 8-Band Kane Model**

For zincblende semiconductors (GaAs, InP, etc.):

$$
H_{\text{Kane}} = \begin{pmatrix}
E_c + \frac{\hbar^2k^2}{2m_0} & \frac{P}{\sqrt{2}}k_+ & -\sqrt{\frac{2}{3}}Pk_z & \cdots \\
\frac{P}{\sqrt{2}}k_- & E_v - \frac{\hbar^2k^2}{2m_0} & \cdots & \cdots \\
\vdots & \vdots & \ddots & \vdots
\end{pmatrix}
$$

where:

- $k_\pm = k_x \pm ik_y$
- $P = \langle S|\hat{p}_x|X\rangle$ is the Kane momentum matrix element
- Includes: conduction band, heavy hole, light hole, split-off bands



**10. Spin-Orbit Coupling**

For heavier elements (Ge, GaAs, InSb):

$$
H_{\text{SO}} = \frac{\hbar}{4m^2c^2}(
abla V \times \mathbf{p})\cdot\boldsymbol{\sigma}
$$

**10.1 Effects**

- **Lifts degeneracies**: Valence band splitting ~0.34 eV in GaAs
- **Essential for**:
  - Topological insulators
  - Spintronics
  - Optical selection rules

**10.2 Matrix Form**

The Hamiltonian becomes a $2 \times 2$ spinor structure:

$$
H = \begin{pmatrix}
H_0 + H_{\text{SO}}^{zz} & H_{\text{SO}}^{+-} \\
H_{\text{SO}}^{-+} & H_0 - H_{\text{SO}}^{zz}
\end{pmatrix}
$$

where:

- $H_{\text{SO}}^{zz} = \lambda L_z S_z$
- $H_{\text{SO}}^{+-} = \lambda L_+ S_-$



**11. Semiconductor Manufacturing Applications**

**11.1 Strain Engineering**

Biaxial strain modifies band structure via **deformation potentials**:

$$
\Delta E_c = \Xi_d \cdot \text{Tr}(\boldsymbol{\epsilon}) + \Xi_u \cdot \epsilon_{zz}
$$

**Strain tensor components:**

$$
\boldsymbol{\epsilon} = \begin{pmatrix}
\epsilon_{xx} & \epsilon_{xy} & \epsilon_{xz} \\
\epsilon_{yx} & \epsilon_{yy} & \epsilon_{yz} \\
\epsilon_{zx} & \epsilon_{zy} & \epsilon_{zz}
\end{pmatrix}
$$

**Valence band (Bir-Pikus Hamiltonian):**

$$
H_{\epsilon} = a(\epsilon_{xx} + \epsilon_{yy} + \epsilon_{zz}) + 3b\left[(L_x^2 - \frac{1}{3}L^2)\epsilon_{xx} + \text{c.p.}\right]
$$

**Manufacturing application:**

- Strained Si channels: ~30–50% mobility enhancement
- SiGe virtual substrates for strain control

**11.2 Heterostructures and Quantum Wells**

At interfaces, the **envelope function approximation**:

$$
\left[-\frac{\hbar^2}{2}
abla\cdot\frac{1}{m^*(\mathbf{r})}
abla + V(\mathbf{r})\right]F(\mathbf{r}) = EF(\mathbf{r})
$$

**Ben Daniel-Duke boundary conditions:**

$$
\begin{aligned}
F_A(z_0) &= F_B(z_0) \\
\frac{1}{m_A^*}\left.\frac{\partial F}{\partial z}\right|_A &= \frac{1}{m_B^*}\left.\frac{\partial F}{\partial z}\right|_B
\end{aligned}
$$

**Band alignment types:**

- **Type I (straddling)**: Both carriers confined in same layer (e.g., GaAs/AlGaAs)
- **Type II (staggered)**: Electrons and holes in different layers (e.g., InAs/GaSb)
- **Type III (broken gap)**: Conduction and valence bands overlap

**11.3 Defects and Dopants**

Supercell approach — create periodic array of defects.

**Formation energy:**

$$
E_f[D^q] = E_{\text{tot}}[D^q] - E_{\text{tot}}[\text{bulk}] - \sum_i n_i\mu_i + q(E_F + E_V + \Delta V)
$$

where:

- $D^q$ — defect in charge state $q$
- $n_i$ — number of atoms of species $i$ added/removed
- $\mu_i$ — chemical potential of species $i$
- $E_F$ — Fermi level referenced to valence band maximum $E_V$
- $\Delta V$ — potential alignment correction

**Charge transition levels:**

$$
\epsilon(q/q') = \frac{E_f[D^q; E_F=0] - E_f[D^{q'}; E_F=0]}{q' - q}
$$

**Classification:**

- **Shallow donors/acceptors**: $\epsilon$ near band edges
- **Deep levels**: $\epsilon$ in mid-gap (recombination centers)

**11.4 Alloy Effects**

**Virtual Crystal Approximation (VCA):**

$$
V_{\text{VCA}} = xV_A + (1-x)V_B
$$

**Bowing parameter:**

$$
E_g(x) = xE_g^A + (1-x)E_g^B - bx(1-x)
$$

**Advanced methods:**

- Coherent Potential Approximation (CPA) for disorder
- Special Quasirandom Structures (SQS) for explicit alloy supercells



**12. Computational Complexity**

| Method | Scaling | Typical System Size |
|--------|---------|---------------------|
| Exact diagonalization | $O(N^3)$ | ~$10^2$ atoms |
| Iterative (Davidson/Lanczos) | $O(N^2)$ per eigenvalue | ~$10^3$ atoms |
| Linear-scaling DFT | $O(N)$ | ~$10^4$ atoms |
| Tight-binding | $O(N)$ to $O(N^2)$ | ~$10^5$ atoms |

**12.1 Parallelization Strategies**

- **k-point parallelism**: Different k-points on different processors
- **Band parallelism**: Different bands distributed across processors
- **Real-space decomposition**: Domain decomposition for large systems
- **FFT parallelism**: Distributed 3D FFTs for plane-wave methods

**12.2 Key Software Packages**

| Package | Method | Primary Use |
|---------|--------|-------------|
| VASP | PAW/PW | Production DFT |
| Quantum ESPRESSO | NC/US/PAW-PW | Open-source DFT |
| WIEN2k | LAPW | Accurate all-electron |
| Gaussian | Localized basis | Molecular systems |
| SIESTA | Numerical AO | Large-scale O(N) |



**13. Workflow**

```text
┌─────────────────────────────────────────────────────────────┐
│                    INPUT: Crystal Structure                 │
│            (atomic positions, lattice vectors)              │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│              SELECT METHOD                                  │
│   • DFT (LDA/GGA/Hybrid) for accuracy                       │
│   • Tight-binding for speed                                 │
│   • GW for accurate band gaps                               │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│              COMPUTATIONAL SETUP                            │
│   • Choose k-point grid (Monkhorst-Pack)                    │
│   • Set energy cutoff (plane waves)                         │
│   • Select pseudopotentials                                 │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│              SELF-CONSISTENT CALCULATION                    │
│   • Iterate until density converges                         │
│   • Obtain ground-state energy                              │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│              POST-PROCESSING                                │
│   • Band structure along high-symmetry paths                │
│   • Density of states                                       │
│   • Effective masses                                        │
│   • Optical properties                                      │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│              VALIDATION & APPLICATION                       │
│   • Compare with ARPES, optical data                        │
│   • Extract parameters for device simulation (TCAD)         │
└─────────────────────────────────────────────────────────────┘
```


**14. Key Equations Reference Card**

**Schrödinger Equation**
$$
\hat{H}\psi = E\psi
$$

**Bloch Theorem**
$$
\psi_{n\mathbf{k}}(\mathbf{r}) = e^{i\mathbf{k}\cdot\mathbf{r}}u_{n\mathbf{k}}(\mathbf{r})
$$

**Kohn-Sham Equation**
$$
\left[-\frac{\hbar^2}{2m}
abla^2 + V_{\text{eff}}[n]\right]\psi_i = \epsilon_i\psi_i
$$

**Effective Mass**
$$
\frac{1}{m^*_{ij}} = \frac{1}{\hbar^2}\frac{\partial^2 E}{\partial k_i \partial k_j}
$$

**GW Self-Energy**
$$
\Sigma = iGW
$$

**Formation Energy**
$$
E_f = E_{\text{tot}}[\text{defect}] - E_{\text{tot}}[\text{bulk}] - \sum_i n_i\mu_i + qE_F
$$

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/band-structure-calculations) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
