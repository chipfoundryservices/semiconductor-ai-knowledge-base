# Semiconductor Manufacturing: Ion Implantation Mathematical Modeling

**Keywords**: implant modeling, ion implantation, doping, dopant diffusion, range straggling, damage

---

**Semiconductor Manufacturing: Ion Implantation Mathematical Modeling**



**1. Introduction**

Ion implantation is a critical process in semiconductor fabrication where dopant ions (B, P, As, Sb) are accelerated and embedded into silicon substrates to precisely control electrical properties.

**Key Process Parameters:**

- **Energy (keV)**: Controls implant depth ($R_p$)
- **Dose (ions/cm²)**: Controls peak concentration
- **Tilt angle (°)**: Minimizes channeling effects
- **Twist angle (°)**: Avoids major crystal planes
- **Beam current (mA)**: Affects dose rate and wafer heating



**2. Foundational Physics: Ion Stopping**

When an energetic ion enters a solid, it loses energy through two primary mechanisms.

**2.1 Total Stopping Power**

$$
\frac{dE}{dx} = N \left[ S_n(E) + S_e(E) \right]
$$

Where:

- $N$ = atomic density of target ($\approx 5 \times 10^{22}$ atoms/cm³ for Si)
- $S_n(E)$ = nuclear stopping cross-section (elastic collisions with nuclei)
- $S_e(E)$ = electronic stopping cross-section (inelastic energy loss to electrons)

**2.2 Nuclear Stopping: ZBL Universal Potential**

The Ziegler-Biersack-Littmark (ZBL) universal screening function:

$$
\phi(x) = 0.1818 e^{-3.2x} + 0.5099 e^{-0.9423x} + 0.2802 e^{-0.4028x} + 0.02817 e^{-0.2016x}
$$

Where $x = r/a_u$ is the reduced interatomic distance.

**Universal screening length:**

$$
a_u = \frac{0.8854 \, a_0}{Z_1^{0.23} + Z_2^{0.23}}
$$

Where:

- $a_0$ = Bohr radius (0.529 Å)
- $Z_1$ = atomic number of incident ion
- $Z_2$ = atomic number of target atom

**2.3 Electronic Stopping**

**Low energy regime** (velocity-proportional, Lindhard-Scharff):

$$
S_e = k_e \sqrt{E}
$$

Where:

$$
k_e = \frac{1.212 \, Z_1^{7/6} \, Z_2}{(Z_1^{2/3} + Z_2^{2/3})^{3/2} \, M_1^{1/2}}
$$

**High energy regime** (Bethe-Bloch formula):

$$
S_e = \frac{4\pi Z_1^2 e^4 N Z_2}{m_e v^2} \ln\left(\frac{2 m_e v^2}{I}\right)
$$

Where:

- $m_e$ = electron mass
- $v$ = ion velocity
- $I$ = mean ionization potential of target



**3. Range Statistics and Profile Models**

**3.1 Gaussian Approximation (First Order)**

For amorphous targets, the as-implanted profile:

$$
C(x) = \frac{\Phi}{\sqrt{2\pi} \, \Delta R_p} \exp\left[ -\frac{(x - R_p)^2}{2 \Delta R_p^2} \right]
$$

| Symbol | Definition | Units |
|--------|------------|-------|
| $\Phi$ | Implant dose | ions/cm² |
| $R_p$ | Projected range (mean depth) | nm or cm |
| $\Delta R_p$ | Range straggle (standard deviation) | nm or cm |

**Peak concentration:**

$$
C_{max} = \frac{\Phi}{\sqrt{2\pi} \, \Delta R_p} \approx \frac{0.4 \, \Phi}{\Delta R_p}
$$

**3.2 Pearson IV Distribution (Industry Standard)**

Real profiles exhibit asymmetry. The Pearson IV distribution uses four statistical moments:

$$
f(x) = K \left[ 1 + \left( \frac{x - \lambda}{a} \right)^2 \right]^{-m} \exp\left[ -
u \arctan\left( \frac{x - \lambda}{a} \right) \right]
$$

**Four Moments:**

1. **First Moment (Mean)**: $R_p$ — projected range
2. **Second Moment (Variance)**: $\Delta R_p^2$ — spread
3. **Third Moment (Skewness)**: $\gamma$ — asymmetry
   - $\gamma < 0$: tail extends deeper into substrate (light ions: B)
   - $\gamma > 0$: tail extends toward surface (heavy ions: As)
4. **Fourth Moment (Kurtosis)**: $\beta$ — peakedness relative to Gaussian

**Typical values for Si:**

| Dopant | Skewness ($\gamma$) | Kurtosis ($\beta$) |
|--------|---------------------|---------------------|
| Boron (B) | -0.5 to +0.5 | 2.5 to 4.0 |
| Phosphorus (P) | -0.3 to +0.3 | 2.5 to 3.5 |
| Arsenic (As) | +0.5 to +1.5 | 3.0 to 5.0 |
| Antimony (Sb) | +0.8 to +2.0 | 3.5 to 6.0 |

**3.3 Dual Pearson Model (Channeling Effects)**

For implants into crystalline silicon with channeling tails:

$$
C(x) = (1 - f_{ch}) \cdot P_{random}(x) + f_{ch} \cdot P_{channel}(x)
$$

Where:

- $P_{random}(x)$ = Pearson distribution for random (amorphous) stopping
- $P_{channel}(x)$ = Pearson distribution for channeled ions
- $f_{ch}$ = channeling fraction (depends on tilt, beam divergence, surface oxide)

**Channeling fraction dependencies:**

- Beam divergence: $f_{ch} \downarrow$ as divergence $\uparrow$
- Tilt angle: $f_{ch} \downarrow$ as tilt $\uparrow$ (typically 7° off-axis)
- Surface oxide: $f_{ch} \downarrow$ with screen oxide
- Pre-amorphization: $f_{ch} \approx 0$ with PAI



**4. Monte Carlo Simulation (BCA Method)**

The Binary Collision Approximation provides the highest accuracy for profile prediction.

**4.1 Algorithm Overview**

```
FOR each ion i = 1 to N_ions (typically 10⁵ - 10⁶):
    
    1. Initialize:
       - Energy: E = E₀
       - Position: (x, y, z) = (0, 0, 0)
       - Direction: (cos θ, sin θ cos φ, sin θ sin φ)
    
    2. WHILE E > E_cutoff:
       
       a. Calculate mean free path:
          $\lambda = 1 / (N \cdot \pi \cdot p_{max}^2)$
       
       b. Select random impact parameter:
          $p = p_{max} \cdot \sqrt{\text{random}[0,1]}$
       
       c. Solve scattering integral for deflection angle $\Theta$
       
       d. Calculate energy transfer to target atom:
          $T = T_{max} \cdot \sin^2(\Theta/2)$
       
       e. Update ion energy:
          $E \to E - T - \Delta E_{\text{electronic}}$
       
       f. IF T > E_displacement:
          Create recoil cascade (track secondary)
       
       g. Update position and direction vectors
    
    3. Record final ion position (x_final, y_final, z_final)

END FOR

4. Build histogram of final positions → Dopant profile
```

**4.2 Scattering Integral**

The classical scattering integral for deflection angle:

$$
\Theta = \pi - 2p \int_{r_{min}}^{\infty} \frac{dr}{r^2 \sqrt{1 - \frac{V(r)}{E_c} - \frac{p^2}{r^2}}}
$$

Where:

- $p$ = impact parameter
- $r_{min}$ = distance of closest approach
- $V(r)$ = interatomic potential (e.g., ZBL)
- $E_c$ = center-of-mass energy

**Center-of-mass energy:**

$$
E_c = \frac{M_2}{M_1 + M_2} E
$$

**4.3 Energy Transfer**

Maximum energy transfer in elastic collision:

$$
T_{max} = \frac{4 M_1 M_2}{(M_1 + M_2)^2} \cdot E = \gamma \cdot E
$$

Where $\gamma$ is the kinematic factor:

| Ion → Si | $M_1$ (amu) | $\gamma$ |
|----------|-------------|----------|
| B → Si | 11 | 0.702 |
| P → Si | 31 | 0.968 |
| As → Si | 75 | 0.746 |

**4.4 Electronic Energy Loss (Continuous)**

Along the free flight path:

$$
\Delta E_{electronic} = \int_0^{\lambda} S_e(E) \, dx \approx S_e(E) \cdot \lambda
$$



**5. Multi-Layer and Through-Film Implantation**

**5.1 Screen Oxide Implantation**

For implantation through oxide layer of thickness $t_{ox}$:

**Range correction:**

$$
R_p^{eff} = R_p^{Si} - t_{ox} \left( \frac{R_p^{Si} - R_p^{ox}}{R_p^{ox}} \right)
$$

**Straggle correction:**

$$
(\Delta R_p^{eff})^2 = (\Delta R_p^{Si})^2 - t_{ox} \left( \frac{(\Delta R_p^{Si})^2 - (\Delta R_p^{ox})^2}{R_p^{ox}} \right)
$$

**5.2 Moment Matching at Interfaces**

For multi-layer structures, use moment conservation:

$$
\langle x^n \rangle_{total} = \sum_i \langle x^n \rangle_i \cdot w_i
$$

Where $w_i$ is the weighting factor for layer $i$.



**6. Two-Dimensional Profile Modeling**

**6.1 Lateral Straggle**

The lateral distribution follows:

$$
C(x, y) = C(x) \cdot \frac{1}{\sqrt{2\pi} \, \Delta R_\perp} \exp\left[ -\frac{y^2}{2 \Delta R_\perp^2} \right]
$$

**Relationship between straggles:**

$$
\Delta R_\perp \approx (0.7 \text{ to } 1.0) \times \Delta R_p
$$

**6.2 Masked Implant with Edge Effects**

For a mask opening of width $W$:

$$
C(x, y) = C(x) \cdot \frac{1}{2} \left[ \text{erf}\left( \frac{y + W/2}{\sqrt{2} \, \Delta R_\perp} \right) - \text{erf}\left( \frac{y - W/2}{\sqrt{2} \, \Delta R_\perp} \right) \right]
$$

**6.3 Full 3D Distribution**

$$
C(x, y, z) = \frac{\Phi}{(2\pi)^{3/2} \Delta R_p \, \Delta R_\perp^2} \exp\left[ -\frac{(x - R_p)^2}{2 \Delta R_p^2} - \frac{y^2 + z^2}{2 \Delta R_\perp^2} \right]
$$



**7. Damage and Defect Modeling**

**7.1 Kinchin-Pease Model**

Number of displaced atoms per incident ion:

$$
N_d = 
\begin{cases}
0 & \text{if } E_D < E_d \\
1 & \text{if } E_d < E_D < 2E_d \\
\displaystyle\frac{E_D}{2E_d} & \text{if } E_D > 2E_d
\end{cases}
$$

Where:

- $E_D$ = damage energy (energy deposited into nuclear collisions)
- $E_d$ = displacement threshold energy ($\approx 15$ eV for Si)

**7.2 Modified NRT Model (Norgett-Robinson-Torrens)**

$$
N_d = \frac{0.8 \, E_D}{2 E_d}
$$

The factor 0.8 accounts for forward scattering efficiency.

**7.3 Damage Energy Partition**

Lindhard partition function:

$$
E_D = \frac{E_0}{1 + k \cdot g(\varepsilon)}
$$

Where:

$$
k = 0.1337 \, Z_1^{1/6} \left( \frac{Z_1}{Z_2} \right)^{1/2}
$$

$$
\varepsilon = \frac{32.53 \, M_2 \, E_0}{Z_1 Z_2 (M_1 + M_2)(Z_1^{0.23} + Z_2^{0.23})}
$$

**7.4 Amorphization Threshold**

Critical dose for amorphization:

$$
\Phi_c \approx \frac{N_0}{N_d \cdot \sigma_{damage}}
$$

**Typical values:**

| Ion | Critical Dose (cm⁻²) |
|-----|----------------------|
| B⁺ | $\sim 10^{15}$ |
| P⁺ | $\sim 5 \times 10^{14}$ |
| As⁺ | $\sim 10^{14}$ |
| Sb⁺ | $\sim 5 \times 10^{13}$ |

**7.5 Damage Profile**

The damage distribution differs from dopant distribution:

$$
D(x) = \frac{\Phi \cdot N_d(E)}{\sqrt{2\pi} \, \Delta R_d} \exp\left[ -\frac{(x - R_d)^2}{2 \Delta R_d^2} \right]
$$

Where $R_d < R_p$ (damage peaks shallower than dopant).



**8. Process-Relevant Calculations**

**8.1 Junction Depth**

For Gaussian profile meeting background concentration $C_B$:

$$
x_j = R_p + \Delta R_p \sqrt{2 \ln\left( \frac{C_{max}}{C_B} \right)}
$$

**For asymmetric Pearson profiles:**

$$
x_j = R_p + \Delta R_p \left[ \gamma + \sqrt{\gamma^2 + 2 \ln\left( \frac{C_{max}}{C_B} \right)} \right]
$$

**8.2 Sheet Resistance**

$$
R_s = \frac{1}{q \displaystyle\int_0^{x_j} \mu(C(x)) \cdot C(x) \, dx}
$$

**With concentration-dependent mobility (Masetti model):**

$$
\mu(C) = \mu_{min} + \frac{\mu_0}{1 + (C/C_r)^\alpha} - \frac{\mu_1}{1 + (C_s/C)^\beta}
$$

| Parameter | Electrons | Holes |
|-----------|-----------|-------|
| $\mu_{min}$ | 52.2 | 44.9 |
| $\mu_0$ | 1417 | 470.5 |
| $C_r$ | $9.68 \times 10^{16}$ | $2.23 \times 10^{17}$ |
| $\alpha$ | 0.68 | 0.719 |

**8.3 Threshold Voltage Shift**

For channel implant:

$$
\Delta V_T = \frac{q}{\varepsilon_{ox}} \int_0^{x_{max}} C(x) \cdot x \, dx
$$

**Simplified (shallow implant):**

$$
\Delta V_T \approx \frac{q \, \Phi \, R_p}{\varepsilon_{ox}}
$$

**8.4 Dose Calculation from Profile**

$$
\Phi = \int_0^{\infty} C(x) \, dx
$$

**Verification:**

$$
\Phi_{measured} = \frac{I \cdot t}{q \cdot A}
$$

Where:

- $I$ = beam current
- $t$ = implant time
- $A$ = implanted area



**9. Advanced Effects**

**9.1 Transient Enhanced Diffusion (TED)**

The "+1 Model": Each implanted ion creates approximately one net interstitial.

**Enhanced diffusion equation:**

$$
\frac{\partial C}{\partial t} = \frac{\partial}{\partial x} \left[ D^* \frac{\partial C}{\partial x} \right]
$$

**Enhanced diffusivity:**

$$
D^* = D_i \cdot \left( 1 + \frac{C_I}{C_I^*} \right)
$$

Where:

- $D_i$ = intrinsic diffusivity
- $C_I$ = interstitial concentration
- $C_I^*$ = equilibrium interstitial concentration

**9.2 Dose Loss Mechanisms**

**Sputtering yield:**

$$
Y = \frac{0.042 \, \alpha \, S_n(E_0)}{U_0}
$$

Where:

- $\alpha$ = angular factor ($\approx 0.2$ for light ions, $\approx 0.4$ for heavy ions)
- $U_0$ = surface binding energy ($\approx 4.7$ eV for Si)

**Retained dose:**

$$
\Phi_{retained} = \Phi_{implanted} \cdot (1 - \eta_{sputter} - \eta_{backscatter})
$$

**9.3 High Dose Effects**

**Dose saturation:**

$$
C_{max}^{sat} = \frac{N_0}{\sqrt{2\pi} \, \Delta R_p}
$$

**Snow-plow effect** at very high doses pushes peak toward surface.

**9.4 Temperature Effects**

**Dynamic annealing:** Competes with damage accumulation

$$
\Phi_c(T) = \Phi_c(0) \exp\left( \frac{E_a}{k_B T} \right)
$$

Where $E_a \approx 0.3$ eV for Si self-interstitial migration.


**10. Summary Tables**

**10.1 Key Scaling Relationships**

| Parameter | Scaling with Energy |
|-----------|---------------------|
| Projected Range | $R_p \propto E^n$ where $n \approx 0.5 - 0.8$ |
| Range Straggle | $\Delta R_p \approx 0.4 R_p$ (light ions) to $0.2 R_p$ (heavy ions) |
| Lateral Straggle | $\Delta R_\perp \approx 0.7 - 1.0 \times \Delta R_p$ |
| Damage Energy | $E_D/E_0$ increases with ion mass |

**10.2 Common Implant Parameters in Si**

| Dopant | Type | Energy (keV) | $R_p$ (nm) | $\Delta R_p$ (nm) |
|--------|------|--------------|------------|-------------------|
| B | p | 10 | 35 | 14 |
| B | p | 50 | 160 | 52 |
| P | n | 30 | 40 | 15 |
| P | n | 100 | 120 | 40 |
| As | n | 50 | 35 | 12 |
| As | n | 150 | 95 | 28 |

**10.3 Simulation Tools Comparison**

| Approach | Speed | Accuracy | Primary Use |
|----------|-------|----------|-------------|
| Analytical (Gaussian) | ★★★★★ | ★★☆☆☆ | Quick estimates |
| Pearson IV Tables | ★★★★☆ | ★★★☆☆ | Process simulation |
| Monte Carlo (SRIM/TRIM) | ★★☆☆☆ | ★★★★☆ | Profile calibration |
| Molecular Dynamics | ★☆☆☆☆ | ★★★★★ | Damage cascade studies |





**Quick Reference Formulas**

**Essential Equations Card**

```
-
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│  GAUSSIAN PROFILE                                                                           │
│  $C(x) = \Phi/(\sqrt{2\pi} \cdot \Delta R_p) \cdot \exp[-(x-R_p)^2/(2\Delta R_p^2)]$        │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│  PEAK CONCENTRATION                                                                         │
│  $C_{max} \approx 0.4 \cdot \Phi/\Delta R_p$                                                │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│  JUNCTION DEPTH                                                                             │
│  $x_j = R_p + \Delta R_p \cdot \sqrt{2 \cdot \ln(C_{max}/C_B)}$                             │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│  SHEET RESISTANCE                                                                           │
│  $R_s = 1/(q \cdot \int \mu(C) \cdot C(x) dx)$                                              │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│  DISPLACEMENT DAMAGE                                                                        │
│  $N_d = 0.8 \cdot E_D/(2E_d)$                                                               │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/implant-modeling) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
