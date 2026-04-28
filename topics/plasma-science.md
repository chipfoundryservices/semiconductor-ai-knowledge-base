# Semiconductor Manufacturing Plasma Science

**Keywords**: plasma science, semiconductor plasma science, plasma technology, plasma fundamentals, plasma generation, plasma diagnostics, plasma processing

---

**Semiconductor Manufacturing Plasma Science**

**Overview**

This document covers the physics, chemistry, and engineering of plasma processes in semiconductor manufacturing—the foundation of modern chip fabrication.



**1. Fundamentals of Plasma Physics**

**1.1 What is Plasma?**

Plasma is the **fourth state of matter**—an ionized gas containing:

- Free electrons ($e^-$)
- Positive ions ($\text{Ar}^+$, $\text{Cl}^+$, $\text{F}^+$, etc.)
- Neutral species (atoms, molecules, radicals)

In semiconductor processing, we use **non-equilibrium** or **cold** plasmas where:

$$
T_e \gg T_i \approx T_n \approx T_{\text{room}}
$$

Where:
- $T_e$ = electron temperature (~1–10 eV, equivalent to $10^4$–$10^5$ K)
- $T_i$ = ion temperature (~0.025–0.1 eV)
- $T_n$ = neutral temperature (~300 K)

This asymmetry allows chemically reactive species to be generated without thermally damaging the substrate.

**1.2 Key Plasma Parameters**

| Parameter | Symbol | Typical Value | Description |
|-----------|--------|---------------|-------------|
| Electron density | $n_e$ | $10^9$–$10^{12}$ cm$^{-3}$ | Number of electrons per unit volume |
| Electron temperature | $T_e$ | 1–10 eV | Mean kinetic energy of electrons |
| Ion temperature | $T_i$ | 0.025–0.1 eV | Mean kinetic energy of ions |
| Debye length | $\lambda_D$ | 10–100 μm | Characteristic shielding distance |
| Plasma frequency | $\omega_{pe}$ | ~GHz | Characteristic oscillation frequency |

**1.3 Debye Length**

The **Debye length** characterizes the distance over which charge separation can occur:

$$
\lambda_D = \sqrt{\frac{\varepsilon_0 k_B T_e}{n_e e^2}}
$$

Where:
- $\varepsilon_0$ = permittivity of free space ($8.85 \times 10^{-12}$ F/m)
- $k_B$ = Boltzmann constant ($1.38 \times 10^{-23}$ J/K)
- $T_e$ = electron temperature (K)
- $n_e$ = electron density (m$^{-3}$)
- $e$ = electron charge ($1.6 \times 10^{-19}$ C)

**1.4 Plasma Frequency**

The **plasma frequency** is the natural oscillation frequency of electrons:

$$
\omega_{pe} = \sqrt{\frac{n_e e^2}{\varepsilon_0 m_e}}
$$

Or in practical units:

$$
f_{pe} \approx 9 \sqrt{n_e} \text{ Hz} \quad \text{(with } n_e \text{ in m}^{-3}\text{)}
$$



**2. The Plasma Sheath**

**2.1 Sheath Formation**

The **plasma sheath** is the most critical region for semiconductor processing. At any surface in contact with plasma:

1. Electrons (lighter, faster) escape more readily than ions
2. A positive space charge region forms adjacent to the surface
3. This creates a potential drop that accelerates ions toward the substrate

**2.2 Sheath Potential**

The **Bohm criterion** requires ions entering the sheath to have a minimum velocity:

$$
v_{\text{Bohm}} = \sqrt{\frac{k_B T_e}{M_i}}
$$

Where $M_i$ is the ion mass.

The **floating potential** (potential of an isolated surface) is approximately:

$$
V_f \approx -\frac{k_B T_e}{2e} \ln\left(\frac{M_i}{2\pi m_e}\right)
$$

For argon plasma with $T_e = 3$ eV:

$$
V_f \approx -15 \text{ V}
$$

**2.3 Child-Langmuir Law**

The **ion current density** through a collisionless sheath is given by:

$$
J_i = \frac{4\varepsilon_0}{9} \sqrt{\frac{2e}{M_i}} \frac{V^{3/2}}{d^2}
$$

Where:
- $V$ = sheath voltage
- $d$ = sheath thickness

**2.4 Sheath Thickness**

The sheath thickness scales approximately as:

$$
s \approx \lambda_D \left(\frac{2eV_s}{k_B T_e}\right)^{3/4}
$$

Where $V_s$ is the sheath voltage.



**3. Plasma Etching**

**3.1 Etching Mechanisms**

Three primary mechanisms contribute to plasma etching:

1. **Chemical etching** (isotropic):
   $$
   \text{Rate}_{\text{chem}} \propto \Gamma_n \cdot S \cdot \exp\left(-\frac{E_a}{k_B T_s}\right)
   $$
   Where $\Gamma_n$ is neutral flux, $S$ is sticking coefficient, $E_a$ is activation energy

2. **Physical sputtering** (anisotropic):
   $$
   Y(E) = \frac{0.042 \cdot Q \cdot \alpha^* \cdot S_n(E)}{U_s}
   $$
   Where $Y$ is sputter yield, $E$ is ion energy, $U_s$ is surface binding energy

3. **Ion-enhanced etching** (synergistic):
   $$
   \text{Rate}_{\text{total}} > \text{Rate}_{\text{chem}} + \text{Rate}_{\text{phys}}
   $$

**3.2 Etch Rate Equation**

A general expression for ion-enhanced etch rate:

$$
\text{ER} = \frac{1}{n} \left[ k_s \Gamma_n \theta + Y_{\text{phys}} \Gamma_i + Y_{\text{ion}} \Gamma_i (1-\theta) + Y_{\text{chem}} \Gamma_i \theta \right]
$$

Where:
- $n$ = atomic density of material
- $\Gamma_n$ = neutral flux
- $\Gamma_i$ = ion flux
- $\theta$ = surface coverage of reactive species
- $Y$ = yield coefficients

**3.3 Ion Energy Distribution Function (IEDF)**

For sinusoidal RF bias, the IEDF is bimodal with peaks at:

$$
E_{\pm} = eV_{dc} \pm eV_{rf} \cdot \frac{\omega_{pi}}{\omega_{rf}}
$$

Where:
- $V_{dc}$ = DC self-bias voltage
- $V_{rf}$ = RF amplitude
- $\omega_{pi}$ = ion plasma frequency
- $\omega_{rf}$ = RF frequency

The peak separation:

$$
\Delta E = 2eV_{rf} \cdot \frac{\omega_{pi}}{\omega_{rf}}
$$

**3.4 Common Etch Chemistries**

| Material | Chemistry | Key Radicals | Byproducts |
|----------|-----------|--------------|------------|
| Silicon | SF$_6$, Cl$_2$, HBr | F*, Cl*, Br* | SiF$_4$, SiCl$_4$ |
| SiO$_2$ | CF$_4$, CHF$_3$, C$_4$F$_8$ | CF$_x$*, F* | SiF$_4$, CO, CO$_2$ |
| Si$_3$N$_4$ | CF$_4$/O$_2$ | F*, O* | SiF$_4$, N$_2$ |
| Al | Cl$_2$/BCl$_3$ | Cl* | AlCl$_3$ |
| Photoresist | O$_2$ | O* | CO, CO$_2$, H$_2$O |

**3.5 Selectivity**

**Selectivity** is the ratio of etch rates between target and mask (or underlayer):

$$
S = \frac{\text{ER}_{\text{target}}}{\text{ER}_{\text{mask}}}
$$

For oxide-to-nitride selectivity in fluorocarbon plasmas:

$$
S_{\text{ox/nit}} = \frac{\text{ER}_{\text{SiO}_2}}{\text{ER}_{\text{Si}_3\text{N}_4}} \propto \frac{[\text{F}]}{[\text{CF}_x]}
$$



**4. Plasma Sources**

**4.1 Capacitively Coupled Plasma (CCP)**

**Configuration**: Parallel plate electrodes with RF power

**Power absorption**: Primarily through stochastic (collisionless) heating:

$$
P_{\text{stoch}} \propto \frac{m_e v_e^2 \omega_{rf}^2 s_0^2}{v_{th,e}}
$$

Where $s_0$ is the sheath oscillation amplitude.

**Dual-frequency operation**:
- High frequency (27–100 MHz): Controls plasma density
- Low frequency (100 kHz–13 MHz): Controls ion energy

Ion energy scaling:

$$
\langle E_i \rangle \propto \frac{V_{rf}^2}{n_e^{0.5}}
$$

**4.2 Inductively Coupled Plasma (ICP)**

**Power transfer**: Through induced electric field from RF current in coil:

$$
E_\theta = -\frac{\partial A_\theta}{\partial t} = j\omega A_\theta
$$

**Skin depth** (characteristic penetration depth of fields):

$$
\delta = \sqrt{\frac{2}{\omega \mu_0 \sigma_p}}
$$

Where $\sigma_p$ is plasma conductivity:

$$
\sigma_p = \frac{n_e e^2}{m_e 
u_m}
$$

**Power density**:

$$
P = \frac{1}{2} \text{Re}(\sigma_p) |E|^2
$$

**Advantages**:
- Higher plasma density: $10^{11}$–$10^{12}$ cm$^{-3}$
- Lower operating pressure: 1–50 mTorr
- Independent control of ion flux and energy

**4.3 Plasma Density Comparison**

| Source Type | Density (cm$^{-3}$) | Pressure Range | Ion Energy Control |
|-------------|---------------------|----------------|-------------------|
| CCP | $10^9$–$10^{10}$ | 10–1000 mTorr | Coupled |
| ICP | $10^{11}$–$10^{12}$ | 1–50 mTorr | Independent |
| ECR | $10^{11}$–$10^{12}$ | 0.1–10 mTorr | Independent |
| Helicon | $10^{12}$–$10^{13}$ | 0.1–10 mTorr | Independent |



**5. Plasma-Enhanced Deposition**

**5.1 PECVD Fundamentals**

**Reaction rate** in PECVD:

$$
R = k_0 \exp\left(-\frac{E_a}{k_B T_{eff}}\right) [A]^a [B]^b
$$

Where $T_{eff}$ is an effective temperature combining gas and electron contributions.

The plasma reduces the effective activation energy by providing:
- Electron-impact dissociation
- Ion bombardment energy
- Radical species

**5.2 Common PECVD Reactions**

**Silicon dioxide** from silane and nitrous oxide:

$$
\text{SiH}_4 + 2\text{N}_2\text{O} \xrightarrow{\text{plasma}} \text{SiO}_2 + 2\text{N}_2 + 2\text{H}_2
$$

**Silicon nitride** from silane and ammonia:

$$
3\text{SiH}_4 + 4\text{NH}_3 \xrightarrow{\text{plasma}} \text{Si}_3\text{N}_4 + 12\text{H}_2
$$

**Amorphous silicon**:

$$
\text{SiH}_4 \xrightarrow{\text{plasma}} a\text{-Si:H} + 2\text{H}_2
$$

**5.3 Film Quality Parameters**

Film stress in PECVD films:

$$
\sigma = \frac{E_f}{1-
u_f} \left( \alpha_s - \alpha_f \right) \Delta T + \sigma_{\text{intrinsic}}
$$

Where:
- $E_f$ = film Young's modulus
- $
u_f$ = film Poisson's ratio
- $\alpha_s, \alpha_f$ = thermal expansion coefficients (substrate, film)
- $\sigma_{\text{intrinsic}}$ = intrinsic stress from deposition process

**5.4 Plasma-Enhanced ALD (PEALD)**

**Growth per cycle (GPC)**:

$$
\text{GPC} = \frac{\theta_{\text{sat}} \cdot \Omega}{A_{\text{site}}}
$$

Where:
- $\theta_{\text{sat}}$ = saturation coverage
- $\Omega$ = molecular volume
- $A_{\text{site}}$ = area per reactive site

**Self-limiting behavior** requires:

$$
\Gamma_{\text{precursor}} \cdot t_{\text{pulse}} > \frac{N_{\text{sites}}}{S_0}
$$

Where $S_0$ is the initial sticking coefficient.



**6. Advanced Topics**

**6.1 Aspect Ratio Dependent Etching (ARDE)**

Etch rate decreases with increasing aspect ratio due to:

1. **Ion shadowing**: Reduced ion flux at feature bottom
2. **Neutral transport**: Knudsen diffusion limitation
3. **Product redeposition**: Reduced volatile product escape

**Knudsen number** for feature transport:

$$
Kn = \frac{\lambda}{w}
$$

Where $\lambda$ is mean free path, $w$ is feature width.

For $Kn > 1$ (molecular flow regime):

$$
\Gamma_{\text{bottom}} = \Gamma_{\text{top}} \cdot K(\text{AR})
$$

Where $K(\text{AR})$ is the Clausing factor, approximately:

$$
K(\text{AR}) \approx \frac{1}{1 + \frac{3}{8}\text{AR}}
$$

For high aspect ratio features.

**6.2 Atomic Layer Etching (ALE)**

**Self-limiting surface modification**:

$$
\theta(t) = \theta_{\text{sat}} \left[1 - \exp\left(-\frac{t}{\tau}\right)\right]
$$

**Etch per cycle (EPC)**:

$$
\text{EPC} = \frac{N_{\text{modified}} \cdot a}{n_{\text{film}}}
$$

Where:
- $N_{\text{modified}}$ = surface density of modified atoms
- $a$ = atoms removed per modified site
- $n_{\text{film}}$ = atomic density of film

**6.3 Plasma-Induced Damage**

**Charging damage** occurs when:

$$
V_{\text{antenna}} = \frac{J_e - J_i}{C_{\text{gate}}/A_{\text{antenna}}} \cdot t > V_{\text{breakdown}}
$$

**Antenna ratio** limit:

$$
\text{AR}_{\text{antenna}} = \frac{A_{\text{antenna}}}{A_{\text{gate}}} < \text{AR}_{\text{critical}}
$$

**UV damage** from vacuum UV photons ($\lambda < 200$ nm):

$$
N_{\text{defects}} \propto \int I(\lambda) \cdot \sigma(\lambda) \cdot d\lambda
$$



**7. Plasma Diagnostics**

**7.1 Langmuir Probe Analysis**

**Electron density** from ion saturation current:

$$
n_e = \frac{I_{i,sat}}{0.61 \cdot e \cdot A_p \cdot \sqrt{\frac{k_B T_e}{M_i}}}
$$

**Electron temperature** from the exponential region:

$$
T_e = \frac{e}{k_B} \left( \frac{d(\ln I_e)}{dV} \right)^{-1}
$$

**EEDF** from second derivative of I-V curve:

$$
f(\varepsilon) = \frac{2m_e}{e^2 A_p} \sqrt{\frac{2\varepsilon}{m_e}} \frac{d^2 I}{dV^2}
$$

**7.2 Optical Emission Spectroscopy (OES)**

**Actinometry** for radical density measurement:

$$
\frac{n_X}{n_{\text{Ar}}} = \frac{I_X}{I_{\text{Ar}}} \cdot \frac{\sigma_{\text{Ar}} \cdot Q_{\text{Ar}}}{\sigma_X \cdot Q_X}
$$

Where:
- $I$ = emission intensity
- $\sigma$ = electron-impact excitation cross-section
- $Q$ = quantum efficiency



**8. Process Control Equations**

**8.1 Residence Time**

$$
\tau_{\text{res}} = \frac{p \cdot V}{Q \cdot k_B T}
$$

Where:
- $p$ = pressure
- $V$ = chamber volume
- $Q$ = gas flow rate (sccm converted to molecules/s)

**8.2 Mean Free Path**

$$
\lambda = \frac{k_B T}{\sqrt{2} \pi d^2 p}
$$

For argon at 10 mTorr and 300 K:

$$
\lambda \approx 0.5 \text{ cm}
$$

**8.3 Power Density**

**Effective power density** at wafer:

$$
P_{\text{eff}} = \frac{\eta \cdot P_{\text{source}}}{A_{\text{wafer}}}
$$

Where $\eta$ is power transfer efficiency (typically 0.3–0.7).



**9. Critical Equations**

| Application | Equation | Key Parameters |
|-------------|----------|----------------|
| Debye length | $\lambda_D = \sqrt{\frac{\varepsilon_0 k_B T_e}{n_e e^2}}$ | $T_e$, $n_e$ |
| Bohm velocity | $v_B = \sqrt{\frac{k_B T_e}{M_i}}$ | $T_e$, $M_i$ |
| Skin depth | $\delta = \sqrt{\frac{2}{\omega \mu_0 \sigma_p}}$ | $\omega$, $n_e$ |
| Selectivity | $S = \frac{\text{ER}_1}{\text{ER}_2}$ | Chemistry, energy |
| ARDE factor | $K \approx (1 + 0.375 \cdot \text{AR})^{-1}$ | Aspect ratio |
| Residence time | $\tau = \frac{pV}{Qk_B T}$ | $p$, $Q$, $V$ |

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/plasma-science) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
