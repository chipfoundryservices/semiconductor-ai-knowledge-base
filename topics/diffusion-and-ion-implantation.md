# Mathematical Modeling of Diffusion and Ion Implantation in Semiconductor Manufacturing

**Keywords**: diffusion and ion implantation,diffusion,ion implantation,dopant diffusion,fick law,implant profile,gaussian profile,pearson distribution,ted,transient enhanced diffusion,thermal budget,semiconductor doping

---

**Mathematical Modeling of Diffusion and Ion Implantation in Semiconductor Manufacturing**

   Part I: Diffusion Modeling

    Fundamental Equations

Dopant redistribution in silicon at elevated temperatures is governed by  Fick's Laws .

     Fick's First Law

Relates flux to concentration gradient:

$$
J = -D \frac{\partial C}{\partial x}
$$

 Where: 

- $J$ — Atomic flux (atoms/cm²·s)
- $D$ — Diffusion coefficient (cm²/s)
- $C$ — Concentration (atoms/cm³)
- $x$ — Position (cm)

     Fick's Second Law

The diffusion equation follows from continuity:

$$
\frac{\partial C}{\partial t} = D \frac{\partial^2 C}{\partial x^2}
$$

This parabolic PDE admits analytical solutions for idealized boundary conditions.



    Temperature Dependence

The diffusion coefficient follows an  Arrhenius relationship :

$$
D(T) = D_0 \exp\left(-\frac{E_a}{kT}\right)
$$

 Parameters: 

- $D_0$ — Pre-exponential factor (cm²/s)
- $E_a$ — Activation energy (eV)
- $k$ — Boltzmann's constant ($8.617 \times 10^{-5}$ eV/K)
- $T$ — Absolute temperature (K)

 Typical Values for Phosphorus in Silicon: 

| Parameter | Value |
|-----------|-------|
| $D_0$ | $3.85$ cm²/s |
| $E_a$ | $3.66$ eV |

Diffusion approximately doubles every 10–15°C near typical process temperatures (900–1100°C).



    Classical Analytical Solutions

     Case 1: Constant Surface Concentration (Predeposition)

 Boundary Conditions: 

- $C(0, t) = C_s$ (constant surface concentration)
- $C(\infty, t) = 0$ (zero at infinite depth)
- $C(x, 0) = 0$ (initially undoped)

 Solution: 

$$
C(x,t) = C_s \cdot \text{erfc}\left(\frac{x}{2\sqrt{Dt}}\right)
$$

 Complementary Error Function: 

$$
\text{erfc}(z) = 1 - \text{erf}(z) = \frac{2}{\sqrt{\pi}} \int_z^{\infty} e^{-u^2} \, du
$$

 Total Incorporated Dose: 

$$
Q(t) = \frac{2 C_s \sqrt{Dt}}{\sqrt{\pi}}
$$



     Case 2: Fixed Dose (Drive-in Diffusion)

 Boundary Conditions: 

- $\displaystyle\int_0^{\infty} C \, dx = Q$ (constant total dose)
- $\displaystyle\frac{\partial C}{\partial x}\bigg|_{x=0} = 0$ (no flux at surface)

 Solution (Gaussian Profile): 

$$
C(x,t) = \frac{Q}{\sqrt{\pi Dt}} \exp\left(-\frac{x^2}{4Dt}\right)
$$

 Peak Surface Concentration: 

$$
C(0,t) = \frac{Q}{\sqrt{\pi Dt}}
$$



    Junction Depth Calculation

The metallurgical junction forms where dopant concentration equals background doping $C_B$.

     For erfc Profile:

$$
x_j = 2\sqrt{Dt} \cdot \text{erfc}^{-1}\left(\frac{C_B}{C_s}\right)
$$

     For Gaussian Profile:

$$
x_j = 2\sqrt{Dt \cdot \ln\left(\frac{Q}{C_B \sqrt{\pi Dt}}\right)}
$$



    Concentration-Dependent Diffusion

At high doping concentrations (approaching or exceeding intrinsic carrier concentration $n_i$), diffusivity becomes concentration-dependent.

 Generalized Model: 

$$
D = D^0 + D^{-}\frac{n}{n_i} + D^{+}\frac{p}{n_i} + D^{=}\left(\frac{n}{n_i}\right)^2
$$

 Physical Interpretation: 

| Term | Mechanism |
|------|-----------|
| $D^0$ | Neutral vacancy diffusion |
| $D^{-}$ | Singly negative vacancy diffusion |
| $D^{+}$ | Positive vacancy diffusion |
| $D^{=}$ | Doubly negative vacancy diffusion |

 Resulting Nonlinear PDE: 

$$
\frac{\partial C}{\partial t} = \frac{\partial}{\partial x}\left(D(C) \frac{\partial C}{\partial x}\right)
$$

This requires numerical solution methods.



    Point Defect Mediated Diffusion

Modern process modeling couples dopant diffusion to point defect dynamics.

 Governing System of PDEs: 

$$
\frac{\partial C_I}{\partial t} = 
abla \cdot (D_I 
abla C_I) - k_{IV} C_I C_V + G_I - R_I
$$

$$
\frac{\partial C_V}{\partial t} = 
abla \cdot (D_V 
abla C_V) - k_{IV} C_I C_V + G_V - R_V
$$

$$
\frac{\partial C_A}{\partial t} = 
abla \cdot (D_{AI} C_I 
abla C_A) + \text{(clustering terms)}
$$

 Variable Definitions: 

- $C_I$ — Interstitial concentration
- $C_V$ — Vacancy concentration
- $C_A$ — Dopant atom concentration
- $k_{IV}$ — Interstitial-vacancy recombination rate
- $G$ — Generation rate
- $R$ — Surface recombination rate



   Part II: Ion Implantation Modeling

    Energy Loss Mechanisms

Implanted ions lose energy through two mechanisms:

 Total Stopping Power: 

$$
S(E) = -\frac{dE}{dx} = S_n(E) + S_e(E)
$$

     Nuclear Stopping (Elastic Collisions)

Dominates at  low energies :

$$
S_n(E) = \frac{\pi a^2 \gamma E \cdot s_n(\varepsilon)}{1 + M_2/M_1}
$$

 Where: 

- $\gamma = \displaystyle\frac{4 M_1 M_2}{(M_1 + M_2)^2}$ — Energy transfer factor
- $a$ — Screening length
- $s_n(\varepsilon)$ — Reduced nuclear stopping

     Electronic Stopping (Inelastic Interactions)

Dominates at  high energies :

$$
S_e(E) \propto \sqrt{E}
$$

(at intermediate energies)



    LSS Theory

Lindhard, Scharff, and Schiøtt developed universal scaling using reduced units.

 Reduced Energy: 

$$
\varepsilon = \frac{a M_2 E}{Z_1 Z_2 e^2 (M_1 + M_2)}
$$

 Reduced Path Length: 

$$
\rho = 4\pi a^2 N \frac{M_1 M_2}{(M_1 + M_2)^2} \cdot x
$$

This allows tabulation of universal range curves applicable across ion-target combinations.



    Gaussian Profile Approximation

 First-Order Implant Profile: 

$$
C(x) = \frac{\Phi}{\sqrt{2\pi} \, \Delta R_p} \exp\left(-\frac{(x - R_p)^2}{2 \Delta R_p^2}\right)
$$

 Parameters: 

| Symbol | Name | Units |
|--------|------|-------|
| $\Phi$ | Dose | ions/cm² |
| $R_p$ | Projected range (mean stopping depth) | cm |
| $\Delta R_p$ | Range straggle (standard deviation) | cm |

 Peak Concentration: 

$$
C_{\text{peak}} = \frac{\Phi}{\sqrt{2\pi} \, \Delta R_p} \approx \frac{0.4 \, \Phi}{\Delta R_p}
$$



    Higher-Order Moment Distributions

The Gaussian approximation fails for many practical cases. The  Pearson IV distribution  uses four statistical moments:

| Moment | Symbol | Physical Meaning |
|--------|--------|------------------|
| 1st | $R_p$ | Projected range |
| 2nd | $\Delta R_p$ | Range straggle |
| 3rd | $\gamma$ | Skewness |
| 4th | $\beta$ | Kurtosis |

 Pearson IV Form: 

$$
C(x) = \frac{K}{\left[(x-a)^2 + b^2\right]^m} \exp\left(-
u \arctan\frac{x-a}{b}\right)
$$

 Parameters  $(a, b, m, 
u, K)$ are derived from the four moments through algebraic relations.

 Skewness Behavior: 

-  Light ions (B)  in heavy substrates → Negative skewness (tail toward surface)
-  Heavy ions (As, Sb)  in silicon → Positive skewness (tail toward bulk)



    Dual Pearson Model

For channeling tails or complex profiles:

$$
C(x) = f \cdot C_1(x) + (1-f) \cdot C_2(x)
$$

 Where: 

- $C_1(x)$, $C_2(x)$ — Two Pearson distributions with different parameters
- $f$ — Weight fraction



    Lateral Distribution

Ions scatter laterally as well:

$$
C(x, r) = C(x) \cdot \frac{1}{2\pi \Delta R_{\perp}^2} \exp\left(-\frac{r^2}{2 \Delta R_{\perp}^2}\right)
$$

 For Amorphous Targets: 

$$
\Delta R_{\perp} \approx \frac{\Delta R_p}{\sqrt{3}}
$$

Lateral straggle is critical for device scaling—it limits minimum feature sizes.



    Monte Carlo Simulation (TRIM/SRIM)

For accurate profiles, especially in multilayer or crystalline structures, Monte Carlo methods track individual ion trajectories.

 Algorithm: 

1. Initialize ion position, direction, energy
2. Select free flight path: $\lambda = 1/(N\pi a^2)$
3. Calculate impact parameter and scattering angle via screened Coulomb potential
4. Energy transfer to recoil:
   $$T = T_m \sin^2\left(\frac{\theta}{2}\right)$$
   where $T_m = \gamma E$
5. Apply electronic energy loss over path segment
6. Update ion position/direction; cascade recoils if $T > E_d$ (displacement energy)
7. Repeat until $E < E_{\text{cutoff}}$
8. Accumulate statistics over $10^4 - 10^6$ ion histories

 ZBL Interatomic Potential: 

$$
V(r) = \frac{Z_1 Z_2 e^2}{r} \, \phi(r/a)
$$

Where $\phi$ is the screening function tabulated from quantum mechanical calculations.



    Channeling

In crystalline silicon, ions aligned with crystal axes experience reduced stopping.

 Critical Angle for Channeling: 

$$
\psi_c \approx \sqrt{\frac{2 Z_1 Z_2 e^2}{E \, d}}
$$

 Where: 

- $d$ — Atomic spacing along the channel
- $E$ — Ion energy

 Effects: 

- Channeled ions penetrate  2–10× deeper 
- Creates extended tails in profiles
- Modern implants use  7° tilt  or random-equivalent conditions to minimize



    Damage Accumulation

Implant damage is quantified by:

$$
D(x) = \Phi \int_0^{\infty} 
u(E) \cdot F(x, E) \, dE
$$

 Where: 

- $
u(E)$ — Kinchin-Pease damage function (displaced atoms per ion)
- $F(x, E)$ — Energy deposition profile

 Amorphization Threshold for Silicon: 

$$
\sim 10^{22} \text{ displacements/cm}^3
$$

(approximately 10–15% of atoms displaced)



   Part III: Post-Implant Diffusion and Transient Enhanced Diffusion

    Transient Enhanced Diffusion (TED)

After implantation, excess interstitials dramatically enhance diffusion until they anneal:

$$
D_{\text{eff}} = D^* \left(1 + \frac{C_I}{C_I^*}\right)
$$

 Where: 

- $C_I^*$ — Equilibrium interstitial concentration

 "+1" Model for Boron: 

$$
\frac{\partial C_B}{\partial t} = \frac{\partial}{\partial x}\left[D_B \left(1 + \frac{C_I}{C_I^*}\right) \frac{\partial C_B}{\partial x}\right]
$$

Impact:  TED can cause junction depths  2–5× deeper  than equilibrium diffusion would predict—critical for modern shallow junctions.



    {311} Defect Dissolution Kinetics

Interstitials cluster into rod-like {311} defects that slowly dissolve:

$$
\frac{dN_{311}}{dt} = -
u_0 \exp\left(-\frac{E_a}{kT}\right) N_{311}
$$

The released interstitials sustain TED, explaining why TED persists for times much longer than point defect diffusion would suggest.



   Part IV: Numerical Methods

    Finite Difference Discretization

For the diffusion equation on uniform grid $(x_i, t_n)$:

     Explicit (Forward Euler)

$$
\frac{C_i^{n+1} - C_i^n}{\Delta t} = D \frac{C_{i+1}^n - 2C_i^n + C_{i-1}^n}{\Delta x^2}
$$

 Stability Requirement (CFL Condition): 

$$
\Delta t < \frac{\Delta x^2}{2D}
$$

     Implicit (Backward Euler)

$$
\frac{C_i^{n+1} - C_i^n}{\Delta t} = D \frac{C_{i+1}^{n+1} - 2C_i^{n+1} + C_{i-1}^{n+1}}{\Delta x^2}
$$

-  Unconditionally stable 
- Requires solving tridiagonal system each timestep

     Crank-Nicolson Method

- Average of explicit and implicit schemes
-  Second-order accurate  in time
- Results in tridiagonal system



    Adaptive Meshing

Concentration gradients vary by orders of magnitude. Adaptive grids refine near:

- Junctions
- Surface
- Implant peaks
- Moving interfaces

 Grid Spacing Scaling: 

$$
\Delta x \propto \frac{C}{|
abla C|}
$$



    Process Simulation Flow (TCAD)

Modern simulators (Sentaurus Process, ATHENA, FLOOPS) integrate:

1.  Implantation  → Monte Carlo or analytical tables
2.  Damage model  → Amorphization, defect clustering
3.  Annealing  → Coupled dopant-defect PDEs
4.  Oxidation  → Deal-Grove kinetics, stress effects, OED
5.  Silicidation, epitaxy, etc.  → Specialized models

Output feeds device simulation (drift-diffusion, Monte Carlo transport).



   Part V: Key Process Design Equations

    Thermal Budget

The characteristic diffusion length after multiple thermal steps:

$$
\sqrt{Dt}_{\text{total}} = \sqrt{\sum_i D_i t_i}
$$

 For Varying Temperature $T(t)$: 

$$
Dt = \int_0^{t_f} D_0 \exp\left(-\frac{E_a}{kT(t')}\right) dt'
$$



    Sheet Resistance

$$
R_s = \frac{1}{q \displaystyle\int_0^{x_j} \mu(C) \cdot C(x) \, dx}
$$

 For Uniform Mobility Approximation: 

$$
R_s \approx \frac{1}{q \mu Q}
$$

Electrical measurements to profile parameters.



    Implant Dose-Energy Selection

 Target Peak Concentration: 

$$
C_{\text{peak}} = \frac{0.4 \, \Phi}{\Delta R_p(E)}
$$

 Target Depth (Empirical): 

$$
R_p(E) \approx A \cdot E^n
$$

 Where: 

- $n \approx 0.6 - 0.8$ (depending on energy regime)
- $A$ — Ion-target dependent constant



   Key Mathematical Tools:

| Process | Core Equation | Solution Method |
|---------|---------------|-----------------|
| Thermal diffusion | $\displaystyle\frac{\partial C}{\partial t} = 
abla \cdot (D 
abla C)$ | Analytical (erfc, Gaussian) or FEM/FDM |
| Implant profile | 4-moment Pearson distribution | Lookup tables or Monte Carlo |
| Damage evolution | Coupled defect-dopant kinetics | Stiff ODE solvers |
| TED | $D_{\text{eff}} = D^*(1 + C_I/C_I^*)$ | Coupled PDEs |
| 2D/3D profiles | $
abla \cdot (D 
abla C)$ in 2D/3D | Finite element methods |



   Common Dopant Properties in Silicon:

| Dopant | Type | $D_0$ (cm²/s) | $E_a$ (eV) | Typical Use |
|--------|------|---------------|------------|-------------|
| Boron (B) | p-type | 0.76 | 3.46 | Source/drain, channel doping |
| Phosphorus (P) | n-type | 3.85 | 3.66 | Source/drain, n-well |
| Arsenic (As) | n-type | 0.32 | 3.56 | Shallow junctions |
| Antimony (Sb) | n-type | 0.214 | 3.65 | Buried layers |

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/diffusion-and-ion-implantation) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
