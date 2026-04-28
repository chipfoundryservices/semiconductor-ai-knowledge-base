# Semiconductor Manufacturing Plasma Processes

**Keywords**: plasma, plasma process, semiconductor plasma, plasma processes

---

**Semiconductor Manufacturing Plasma Processes**

Plasma processes are foundational to modern semiconductor fabrication—nearly 40-50% of all processing steps in advanced chip manufacturing involve plasma in some form.



**1. What is Plasma in Semiconductor Manufacturing?**

In semiconductor manufacturing, plasma refers to a **partially ionized gas** containing:

- Free electrons ($e^-$)
- Positive ions ($\text{Ar}^+$, $\text{Cl}^+$, etc.)
- Neutral atoms and molecules
- Highly reactive radicals ($\text{F}^{\bullet}$, $\text{Cl}^{\bullet}$, $\text{O}^{\bullet}$)

**Plasma Characteristics**

These are typically **"cold" or non-equilibrium plasmas**:

| Parameter | Symbol | Typical Value |
|-----------|--------|---------------|
| Electron Temperature | $T_e$ | $1-10 \text{ eV}$ $(10^4 - 10^5 \text{ K})$ |
| Ion/Gas Temperature | $T_i$ | $\sim 300-500 \text{ K}$ |
| Electron Density | $n_e$ | $10^9 - 10^{12} \text{ cm}^{-3}$ |
| Pressure | $P$ | $1-100 \text{ mTorr}$ |

The electron temperature is related to thermal energy by:

$$T_e [\text{eV}] = \frac{k_B T}{e} \approx \frac{T[\text{K}]}{11600}$$

**Debye Length**

The characteristic shielding distance in plasma:

$$\lambda_D = \sqrt{\frac{\varepsilon_0 k_B T_e}{n_e e^2}} = 743 \sqrt{\frac{T_e [\text{eV}]}{n_e [\text{cm}^{-3}]}} \text{ cm}$$

For typical process plasmas: $\lambda_D \approx 10-100 \text{ μm}$

**Plasma Frequency**

The characteristic oscillation frequency of electrons:

$$\omega_{pe} = \sqrt{\frac{n_e e^2}{m_e \varepsilon_0}} \approx 9000 \sqrt{n_e [\text{cm}^{-3}]} \text{ rad/s}$$



**2. Major Plasma Processes**

**2.1 Plasma Etching**

The most critical plasma application—removes material in precisely defined patterns.

**2.1.1 Reactive Ion Etching (RIE)**

Combines **chemical attack** from radicals with **directional ion bombardment**.

**Key Mechanism - Ion-Enhanced Etching:**

$$\text{Etch Rate}_{total} >> \text{Etch Rate}_{chemical} + \text{Etch Rate}_{physical}$$

The synergistic enhancement factor:

$$\eta = \frac{R_{ion+neutral}}{R_{ion} + R_{neutral}}$$

Typically $\eta = 5-20$ for common etch processes.

**Common Chemistries:**

- **Silicon etching:**
  - $\text{SF}_6 \rightarrow \text{SF}_x + \text{F}^{\bullet}$ (isotropic)
  - $\text{Cl}_2 \rightarrow 2\text{Cl}^{\bullet}$ (anisotropic with sidewall passivation)
  - $\text{HBr} \rightarrow \text{H}^{\bullet} + \text{Br}^{\bullet}$ (high selectivity)

- **Silicon dioxide etching:**
  - $\text{CF}_4 + \text{O}_2 \rightarrow \text{CF}_x + \text{F}^{\bullet} + \text{CO}_2$
  - $\text{C}_4\text{F}_8 \rightarrow \text{CF}_2 + \text{C}_2\text{F}_4$ (polymerizing)
  - $\text{CHF}_3$ (selective to Si)

- **Metal etching:**
  - $\text{Cl}_2/\text{BCl}_3$ for Al, W
  - $\text{Cl}_2/\text{O}_2$ for Ti, TiN

**Silicon Etch Reaction:**

$$\text{Si}_{(s)} + 4\text{F}^{\bullet} \xrightarrow{\text{ion assist}} \text{SiF}_{4(g)} \uparrow$$

**Oxide Etch Reaction:**

$$\text{SiO}_2 + \text{CF}_x \xrightarrow{\text{ion bombardment}} \text{SiF}_4 \uparrow + \text{CO}_2 \uparrow$$

**2.1.2 Deep Reactive Ion Etching (DRIE)**

Creates **high-aspect-ratio structures** using the Bosch process.

**Bosch Process Cycle:**

1. **Etch step** (typically 5-15 seconds):
   $$\text{SF}_6 \rightarrow \text{SF}_5^+ + \text{F}^{\bullet} + e^-$$
   $$\text{Si} + 4\text{F}^{\bullet} \rightarrow \text{SiF}_4 \uparrow$$

2. **Passivation step** (typically 2-5 seconds):
   $$\text{C}_4\text{F}_8 \rightarrow n\text{CF}_2 \rightarrow (\text{CF}_2)_n \text{ polymer}$$

**Achievable Parameters:**

- Aspect ratio: $> 50:1$
- Etch depth: $> 500 \text{ μm}$
- Sidewall angle: $90° \pm 0.5°$
- Scallop size: $< 50 \text{ nm}$ (optimized)

**2.1.3 Atomic Layer Etching (ALE)**

Provides **angstrom-level precision** through self-limiting reactions.

**Two-Step ALE Cycle:**

1. **Surface modification** (self-limiting):
   $$\text{Surface} + \text{Reactant} \rightarrow \text{Modified Layer}$$
   
2. **Modified layer removal** (self-limiting):
   $$\text{Modified Layer} \xrightarrow{\text{ion/thermal}} \text{Volatile Products} \uparrow$$

**Example - Silicon ALE with Cl₂/Ar:**

- Step 1: $\text{Si} + \text{Cl}_2 \rightarrow \text{SiCl}_x$ (surface chlorination)
- Step 2: $\text{SiCl}_x + \text{Ar}^+ \rightarrow \text{SiCl}_y \uparrow$ (ion-assisted removal)

**Etch per Cycle (EPC):**

$$\text{EPC} \approx 0.5 - 2 \text{ Å/cycle}$$

**Total Etch Depth:**

$$d = N \times \text{EPC}$$

where $N$ = number of cycles.

**2.2 Plasma-Enhanced Chemical Vapor Deposition (PECVD)**

Deposits thin films at **lower temperatures** than thermal CVD.

**Temperature Advantage:**

$$T_{PECVD} \approx 200-400°\text{C} \quad \text{vs} \quad T_{thermal CVD} \approx 700-900°\text{C}$$

**Deposition Rate Model (simplified):**

$$R_{dep} = k_0 \exp\left(-\frac{E_a}{k_B T}\right) \cdot f(n_e, P, \text{flow})$$

Where plasma activation effectively reduces $E_a$.

**Common PECVD Films**

**Silicon Dioxide:**

$$\text{SiH}_4 + \text{N}_2\text{O} \xrightarrow{\text{plasma}} \text{SiO}_2 + \text{H}_2 + \text{N}_2$$

or using TEOS:

$$\text{Si(OC}_2\text{H}_5)_4 + \text{O}_2 \xrightarrow{\text{plasma}} \text{SiO}_2 + \text{CO}_2 + \text{H}_2\text{O}$$

**Silicon Nitride:**

$$3\text{SiH}_4 + 4\text{NH}_3 \xrightarrow{\text{plasma}} \text{Si}_3\text{N}_4 + 12\text{H}_2$$

Film composition varies: $\text{SiN}_x\text{H}_y$ where $x \approx 0.8-1.3$

**Film Properties (Typical):**

| Film | Refractive Index | Stress (MPa) | Density (g/cm³) |
|------|------------------|--------------|-----------------|
| $\text{SiO}_2$ | $1.46-1.47$ | $-100$ to $+200$ | $2.1-2.3$ |
| $\text{SiN}_x$ | $1.8-2.1$ | $-200$ to $+500$ | $2.4-2.8$ |

**High-Density Plasma CVD (HDP-CVD)**

Simultaneous deposition and sputtering for **gap fill**.

**Deposition-to-Sputter Ratio:**

$$D/S = \frac{R_{deposition}}{R_{sputter}}$$

Optimal gap fill: $D/S \approx 3-5$

**Gap Fill Mechanism:**

- Deposition occurs everywhere
- Sputtering preferentially removes material from corners/top
- Net result: bottom-up fill

**2.3 Physical Vapor Deposition (Sputtering)**

Argon ions bombard a solid target, ejecting atoms.

**Sputter Yield**

Number of target atoms ejected per incident ion:

$$Y = \frac{3\alpha}{4\pi^2} \cdot \frac{4M_1 M_2}{(M_1 + M_2)^2} \cdot \frac{E}{U_s}$$

Where:
- $M_1$ = ion mass
- $M_2$ = target atom mass
- $E$ = ion energy
- $U_s$ = surface binding energy
- $\alpha$ = dimensionless function of mass ratio

**Typical Sputter Yields** (500 eV Ar⁺):

| Target | Yield (atoms/ion) |
|--------|-------------------|
| Al | 1.2 |
| Cu | 2.3 |
| W | 0.6 |
| Ti | 0.6 |
| Ta | 0.6 |

**Ionized PVD (iPVD)**

Ionizes sputtered metal atoms for **directional deposition**.

**Ionization Fraction:**

$$f_{ion} = \frac{n_{M^+}}{n_{M^+} + n_M}$$

Modern iPVD: $f_{ion} > 70\%$

**Bottom Coverage Improvement:**

$$\text{BC} = \frac{t_{bottom}}{t_{field}}$$

iPVD achieves BC > 50% in features with AR > 5:1

**2.4 Plasma-Enhanced Atomic Layer Deposition (PEALD)**

Uses plasma as one of the reactants in the ALD cycle.

**Standard ALD Cycle:**

1. Precursor A exposure (self-limiting)
2. Purge
3. Precursor B exposure (self-limiting)
4. Purge

**PEALD Advantage:**

Plasma provides reactive species at lower temperatures:

$$\text{O}_2 \xrightarrow{\text{plasma}} 2\text{O}^{\bullet}$$

vs thermal:

$$\text{H}_2\text{O} \xrightarrow{T > 300°C} \text{OH}^{\bullet} + \text{H}^{\bullet}$$

**Example - HfO₂ PEALD:**

- Step 1: $\text{Hf(NMe}_2)_4 + \text{Surface-OH} \rightarrow \text{Surface-O-Hf(NMe}_2)_3 + \text{HNMe}_2$
- Step 2: $\text{Surface-O-Hf(NMe}_2)_3 + \text{O}^{\bullet} \rightarrow \text{Surface-HfO}_2\text{-OH}$

**Growth per Cycle (GPC):**

$$\text{GPC} \approx 0.5-1.5 \text{ Å/cycle}$$

**Film Thickness:**

$$t = N \times \text{GPC}$$



**3. Plasma Sources**

**3.1 Capacitively Coupled Plasma (CCP)**

Two parallel plate electrodes with RF power (typically 13.56 MHz).

**Sheath Voltage:**

$$V_{sh} \approx \frac{V_{RF}}{2}$$

**Ion Bombardment Energy:**

$$E_{ion} \approx eV_{sh} = \frac{eV_{RF}}{2}$$

For $V_{RF} = 500\text{ V}$: $E_{ion} \approx 250\text{ eV}$

**Plasma Density:**

$$n_e \propto P_{RF}^{0.5-1.0}$$

Typical: $n_e \approx 10^9 - 10^{10} \text{ cm}^{-3}$

**Limitations:**
- Ion flux and energy are coupled
- Lower density than ICP

**3.2 Inductively Coupled Plasma (ICP)**

RF coil induces plasma currents.

**Power Transfer:**

$$P_{plasma} = \frac{V_{ind}^2}{R_{plasma}}$$

Where induced voltage:

$$V_{ind} = -\frac{d\Phi}{dt} = \omega \cdot N \cdot B \cdot A$$

**Key Advantage - Independent Control:**

- **Source power** ($P_{source}$) → Ion flux ($\Gamma_i$)
  $$\Gamma_i \propto n_e \propto P_{source}^{0.5-1.0}$$

- **Bias power** ($P_{bias}$) → Ion energy ($E_i$)
  $$E_i \propto V_{bias} \propto \sqrt{P_{bias}}$$

**Typical Parameters:**

| Parameter | CCP | ICP |
|-----------|-----|-----|
| $n_e$ (cm⁻³) | $10^9-10^{10}$ | $10^{11}-10^{12}$ |
| Pressure (mTorr) | $50-500$ | $1-50$ |
| Ion energy control | Limited | Independent |

**3.3 Electron Cyclotron Resonance (ECR)**

Microwave power (2.45 GHz) + magnetic field.

**Resonance Condition:**

$$\omega = \omega_{ce} = \frac{eB}{m_e}$$

At 2.45 GHz: $B_{res} = 875 \text{ G}$

**Advantages:**
- Very high density: $n_e > 10^{12} \text{ cm}^{-3}$
- Low pressure operation: $< 1 \text{ mTorr}$
- Efficient power coupling

**3.4 Remote Plasma**

Plasma generated away from substrate—only **radicals** reach wafer.

**Radical Flux at Wafer:**

$$\Gamma_r = \Gamma_0 \exp\left(-\frac{L}{\lambda_{mfp}}\right) \cdot \exp\left(-\frac{t}{\tau_{recomb}}\right)$$

Where:
- $L$ = distance from plasma
- $\lambda_{mfp}$ = mean free path
- $\tau_{recomb}$ = recombination lifetime

**Benefits:**
- No ion bombardment damage
- Gentle surface treatment
- Ideal for cleaning and selective processes



**4. Plasma Sheath Physics**

The sheath is the region between bulk plasma and surfaces.

**4.1 Sheath Formation**

Electrons are faster than ions:

$$v_e = \sqrt{\frac{8k_BT_e}{\pi m_e}} >> v_i = \sqrt{\frac{8k_BT_i}{\pi m_i}}$$

Result: Surfaces charge **negatively**, forming a positive space-charge sheath.

**4.2 Bohm Criterion**

Ions must reach sheath edge with minimum velocity:

$$v_{Bohm} = \sqrt{\frac{k_B T_e}{m_i}}$$

**Ion flux to surface:**

$$\Gamma_i = n_s \cdot v_{Bohm} = n_s \sqrt{\frac{k_B T_e}{m_i}}$$

Where $n_s \approx 0.61 n_e$ at sheath edge.

**4.3 Child-Langmuir Law**

Ion current density through collisionless sheath:

$$J_i = \frac{4\varepsilon_0}{9} \sqrt{\frac{2e}{m_i}} \cdot \frac{V^{3/2}}{d^2}$$

**4.4 Sheath Thickness**

$$s = \frac{\sqrt{2}}{3} \lambda_D \left(\frac{2V_s}{T_e}\right)^{3/4}$$

For $V_s = 100\text{ V}$, $T_e = 3\text{ eV}$: $s \approx 10-100 \text{ μm}$

**4.5 Ion Angular Distribution**

**Without collisions** (low pressure):

$$\theta_{max} \approx \arctan\sqrt{\frac{T_i}{eV_s}}$$

Typically $\theta_{max} < 5°$ — highly directional!

**With collisions** (high pressure):

$$\theta \propto \frac{s}{\lambda_{mfp}}$$

Collisions broaden the angular distribution, reducing anisotropy.



**5. Etch Process Metrics**

**5.1 Etch Rate**

$$R = \frac{\Delta d}{\Delta t} \quad [\text{nm/min}]$$

Typical values:
- Si in $\text{SF}_6$: $200-1000$ nm/min
- $\text{SiO}_2$ in $\text{CF}_4$: $50-200$ nm/min
- Poly-Si in $\text{Cl}_2$: $100-500$ nm/min

**5.2 Selectivity**

Ratio of etch rates between two materials:

$$S_{A:B} = \frac{R_A}{R_B}$$

**Critical Selectivities:**

| Process | Target/Stop | Required Selectivity |
|---------|-------------|---------------------|
| Gate etch | Poly-Si / $\text{SiO}_2$ | $> 50:1$ |
| Contact etch | $\text{SiO}_2$ / Si | $> 20:1$ |
| Spacer etch | $\text{SiN}$ / Si | $> 100:1$ |

**5.3 Anisotropy**

$$A = 1 - \frac{R_{lateral}}{R_{vertical}}$$

- $A = 1$: Perfectly anisotropic (vertical sidewalls)
- $A = 0$: Perfectly isotropic (hemispherical profile)

**5.4 Uniformity**

$$U = \frac{R_{max} - R_{min}}{2 \cdot R_{avg}} \times 100\%$$

Target: $U < 3\%$ across 300mm wafer.

**5.5 Aspect Ratio Dependent Etching (ARDE)**

Etch rate decreases with aspect ratio:

$$R(AR) = R_0 \cdot f(AR)$$

**Knudsen Transport Model:**

$$\frac{R(AR)}{R_0} = \frac{1}{1 + \frac{AR}{K}}$$

Where $K$ is a chemistry-dependent constant (typically 5-20).



**6. Process Control Parameters**

**6.1 RF Power**

**Source Power** (ICP coil or CCP top electrode):
- Controls plasma density: $n_e \propto P^{0.5-1.0}$
- Controls radical production
- Typical: $100-3000$ W

**Bias Power** (substrate electrode):
- Controls ion energy: $E_i \propto \sqrt{P_{bias}}$
- Controls anisotropy
- Typical: $0-500$ W

**6.2 Pressure**

**Effects:**

| Pressure | Mean Free Path | Ion Directionality | Radical Density |
|----------|----------------|-------------------|-----------------|
| Low ($< 10$ mTorr) | Long | High | Lower |
| High ($> 100$ mTorr) | Short | Low | Higher |

**Mean Free Path:**

$$\lambda = \frac{k_B T}{P \cdot \sigma}$$

At 10 mTorr, 300K: $\lambda \approx 5 \text{ mm}$

**6.3 Gas Flow and Chemistry**

**Residence Time:**

$$\tau_{res} = \frac{P \cdot V}{Q}$$

Where $Q$ = flow rate (sccm), $V$ = chamber volume.

**Dissociation Fraction:**

$$\alpha = \frac{n_{dissociated}}{n_{total}}$$

Higher power → higher $\alpha$

**6.4 Temperature**

**Wafer Temperature Effects:**
- Reaction rates: $k \propto \exp(-E_a/k_BT)$
- Desorption rates
- Selectivity
- Film stress (PECVD)

Typical range: $-20°C$ to $400°C$



**7. Advanced Topics**

**7.1 Pulsed Plasmas**

Modulate RF power on/off with period $T_{pulse}$.

**Duty Cycle:**

$$D = \frac{t_{on}}{t_{on} + t_{off}} = \frac{t_{on}}{T_{pulse}}$$

**Benefits:**
- Narrower ion energy distribution
- Reduced charging damage
- Better selectivity control

**Ion Energy Distribution (IED):**

- CW plasma: Bimodal distribution
- Pulsed plasma: Controllable, narrower distribution

**7.2 Plasma-Induced Damage**

**Charging Damage:**

$$V_{gate} = \frac{Q_{accumulated}}{C_{gate}} = \frac{(J_e - J_i) \cdot t \cdot A}{C_{gate}}$$

When $V_{gate} > V_{BD}$ → oxide breakdown!

**Mitigation:**
- Pulsed plasmas
- Neutral beam sources
- Process optimization

**UV Damage:**

VUV photons ($E > 9$ eV) can break Si-O bonds.

$$\text{Si-O} + h
u \rightarrow \text{defects}$$

**7.3 Loading Effects**

**Macro-loading:**

$$R = R_0 \cdot \frac{1}{1 + \frac{A_{etch}}{A_0}}$$

More exposed area → lower etch rate (radical consumption).

**Micro-loading:**

Local pattern density affects local etch rate.

$$\Delta R = R_{isolated} - R_{dense}$$

**7.4 Profile Control**

**Sidewall Passivation Model:**

$$\theta = \arctan\left(\frac{R_{lateral}}{R_{vertical}}\right) = \arctan\left(\frac{R_V - R_P}{R_V}\right)$$

Where:
- $R_V$ = vertical etch rate
- $R_P$ = passivation deposition rate

**Ideal Vertical Profile:** $R_P = R_{lateral}$ on sidewalls



**8. Equipment and Monitoring**

**8.1 Chamber Components**

- **Chuck/Pedestal:** Temperature-controlled substrate holder
  - Electrostatic chuck (ESC) for wafer clamping
  - He backside cooling for thermal contact

- **Gas Distribution:**
  - Showerhead or side injection
  - Mass flow controllers (MFCs): $\pm 1\%$ accuracy

- **Pumping System:**
  - Turbo-molecular pump: base pressure $< 10^{-6}$ Torr
  - Throttle valve for pressure control

- **RF System:**
  - Generator: 13.56 MHz, 2 MHz, 60 MHz common
  - Matching network: L-type or $\pi$-type

**8.2 In-Situ Monitoring**

**Optical Emission Spectroscopy (OES):**

Monitor plasma species by emission lines:

| Species | Wavelength (nm) |
|---------|-----------------|
| F | 703.7 |
| Cl | 837.6 |
| O | 777.4 |
| CO | 483.5 |
| Si | 288.2 |
| SiF | 440.0 |

**Endpoint Detection:**

$$\text{EPD Signal} = \frac{I_{product}}{I_{reference}}$$

Endpoint when signal changes (product species decrease).

**Interferometry:**

Film thickness from interference:

$$2nd\cos\theta = m\lambda$$

Real-time thickness monitoring during etch/deposition.



**9. Challenges at Advanced Nodes**

**9.1 Feature Dimensions**

At 3nm node:
- Gate length: $\sim 12$ nm ($\sim 50$ atoms)
- Fin width: $\sim 5-7$ nm
- Metal pitch: $\sim 20-24$ nm

**Precision Required:**

$$\sigma_{CD} < 0.5 \text{ nm}$$

**9.2 New Architectures**

**Gate-All-Around (GAA) FETs:**
- Requires isotropic etching for channel release
- Selective removal of SiGe vs Si
- Inner spacer formation

**3D NAND:**
- $> 200$ stacked layers
- High aspect ratio etching ($> 60:1$)
- Memory hole etch: $> 10$ μm deep

**9.3 New Materials**

| Material | Application | Etch Chemistry Challenge |
|----------|-------------|-------------------------|
| $\text{HfO}_2$ | High-k gate | Low volatility of Hf halides |
| $\text{Ru}$ | Contacts | RuO₄ volatility issues |
| $\text{Co}$ | Interconnects | Selectivity to Cu |
| $\text{SiGe}$ | Channel | Selectivity to Si |



**10. Key Equations**

**Plasma Parameters**

$$\lambda_D = \sqrt{\frac{\varepsilon_0 k_B T_e}{n_e e^2}}$$

$$v_{Bohm} = \sqrt{\frac{k_B T_e}{m_i}}$$

$$\Gamma_i = 0.61 \cdot n_e \cdot v_{Bohm}$$

**Etch Metrics**

$$S_{A:B} = \frac{R_A}{R_B}$$

$$A = 1 - \frac{R_{lateral}}{R_{vertical}}$$

$$U = \frac{R_{max} - R_{min}}{2R_{avg}} \times 100\%$$

**Process Dependencies**

$$n_e \propto P_{source}^{0.5-1.0}$$

$$E_i \propto \sqrt{P_{bias}}$$

$$R \propto \Gamma_i \cdot f(E_i) \cdot [X^{\bullet}]$$

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/plasma) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
