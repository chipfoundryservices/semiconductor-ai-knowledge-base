# Semiconductor Manufacturing Etch Process

**Keywords**: etch process, etching, dry etch, wet etch, plasma etch, RIE, reactive ion etch, etch selectivity, anisotropic etch

---

**Semiconductor Manufacturing Etch Process**



**1. Overview**

Etching is a critical pattern transfer process in semiconductor fabrication. After lithography defines a pattern using photoresist, etching selectively removes material to create transistors, interconnects, and other IC structures.

**1.1 Fundamental Equation**

The etch process can be characterized by the **etch rate** $R$:

$$
R = \frac{\Delta d}{\Delta t} \quad \text{[nm/min]}
$$

where:

- $\Delta d$ = thickness removed (nm)
- $\Delta t$ = etch time (min)



**2. Etch Categories**

**2.1 Wet Etching**

Uses liquid chemicals to dissolve material isotropically.

- **Characteristics**:
  - Isotropic (etches equally in all directions)
  - High selectivity achievable
  - Simple and low cost
  - Batch processing capable

- **Common Chemistries**:
  - $\text{SiO}_2$ etching: $\text{HF}$ (hydrofluoric acid)
  - Si etching: $\text{HNO}_3 / \text{HF} / \text{CH}_3\text{COOH}$

- **Etch Rate Model** (for $\text{SiO}_2$ in HF):

$$
R_{\text{wet}} = A \cdot [\text{HF}]^n \cdot e^{-E_a / k_B T}
$$

where:

- $A$ = pre-exponential factor
- $[\text{HF}]$ = HF concentration
- $n$ = reaction order
- $E_a$ = activation energy
- $k_B$ = Boltzmann constant ($1.38 \times 10^{-23}$ J/K)
- $T$ = temperature (K)

**2.2 Dry Etching (Plasma Etching)**

Uses plasma containing ions and reactive radicals for anisotropic etching.

- **Sub-types**:
  - Physical Etching (Ion Milling)
  - Chemical Plasma Etching
  - Reactive Ion Etching (RIE)
  - High-Density Plasma (ICP, ECR)
  - Atomic Layer Etching (ALE)



**3. Reactive Ion Etching (RIE)**

**3.1 Plasma Generation**

RF power ionizes feed gas creating:

- **Ions** ($\text{Cl}^+$, $\text{F}^+$, $\text{Ar}^+$) → directional bombardment
- **Radicals** ($\text{Cl}^*$, $\text{F}^*$) → chemical reaction
- **Electrons** ($e^-$) → sustain plasma
- **Neutrals** → background species

**3.2 Ion Energy**

The ion energy at the wafer is determined by the **plasma potential** $V_p$ and **DC bias** $V_{dc}$:

$$
E_{\text{ion}} = e \cdot (V_p - V_{dc})
$$

where:

- $e$ = electron charge ($1.6 \times 10^{-19}$ C)
- $V_p$ = plasma potential (V)
- $V_{dc}$ = DC self-bias voltage (V)

**3.3 Ion-Enhanced Etching Model**

The synergistic etch rate combines physical and chemical components:

$$
R_{\text{total}} = R_{\text{chem}} + R_{\text{phys}} + R_{\text{synergy}}
$$

where typically:

$$
R_{\text{synergy}} \gg R_{\text{chem}} + R_{\text{phys}}
$$

This **ion-radical synergy** is the foundation of anisotropic plasma etching.



**4. Key Performance Metrics**

**4.1 Selectivity**

**Definition**: Ratio of etch rates between target material and mask/stop layer.

$$
S = \frac{R_{\text{target}}}{R_{\text{mask}}}
$$

- **Example Requirements**:
  - $\text{Si} : \text{SiO}_2$ selectivity $> 50:1$
  - Photoresist selectivity $> 10:1$
  - Etch stop selectivity $> 100:1$ (for thin films)

**4.2 Anisotropy**

**Definition**: Measure of directional etching preference.

$$
A = 1 - \frac{R_{\text{lateral}}}{R_{\text{vertical}}}
$$

where:

- $A = 1$ → perfectly anisotropic (vertical only)
- $A = 0$ → perfectly isotropic
- $0 < A < 1$ → partially anisotropic

**4.3 Uniformity**

**Within-Wafer Non-Uniformity (WIWNU)**:

$$
\text{WIWNU} = \frac{\sigma}{\bar{R}} \times 100\%
$$

where:

- $\sigma$ = standard deviation of etch rate
- $\bar{R}$ = mean etch rate

**Target**: WIWNU $< 2\%$ for advanced nodes

**4.4 Aspect Ratio**

$$
AR = \frac{H}{W}
$$

where:

- $H$ = feature depth/height
- $W$ = feature width

- **Current Challenges**:
  - Logic contacts: AR $\approx 10:1$ to $20:1$
  - 3D NAND channels: AR $> 60:1$ (trending toward $100:1$)
  - DRAM capacitors: AR $> 50:1$



**5. Etch Chemistry**

**5.1 Silicon Etching**

- **Primary Chemistries**:
  - $\text{Cl}_2 / \text{HBr}$ — high anisotropy
  - $\text{SF}_6$ — high rate, more isotropic
  - $\text{Cl}_2 / \text{HBr} / \text{O}_2$ — with sidewall passivation

- **Reaction Mechanism** (Chlorine-based):

$$
\text{Si}_{(s)} + 4\text{Cl}^* \rightarrow \text{SiCl}_{4(g)} \uparrow
$$

**5.2 Silicon Dioxide Etching**

- **Primary Chemistries**:
  - $\text{CF}_4$, $\text{C}_4\text{F}_8$, $\text{C}_4\text{F}_6$, $\text{CHF}_3$

- **Reaction Mechanism**:

$$
\text{SiO}_{2(s)} + \text{CF}_x^* \rightarrow \text{SiF}_{4(g)} + \text{CO}_{(g)} + \text{CO}_{2(g)}
$$

- **Selectivity Control**: C/F ratio in plasma
  - Higher C/F → more polymer → higher selectivity to Si
  - Lower C/F → less polymer → faster oxide etch

**5.3 Metal Etching**

- **Aluminum**: $\text{Cl}_2 / \text{BCl}_3$ (BCl₃ scavenges H₂O and Al₂O₃)
- **Tungsten**: $\text{SF}_6$, $\text{NF}_3$
- **Copper**: Not plasma etchable (damascene process instead)



**6. High-Density Plasma Sources**

**6.1 Inductively Coupled Plasma (ICP)**

- **Plasma Density**: $n_e \approx 10^{11} - 10^{12}$ cm⁻³
- **Advantages**:
  - Independent control of ion flux and ion energy
  - Higher density than capacitive RIE
  - Lower operating pressure (1-50 mTorr)

**6.2 Power Relations**

**Ion Flux** (proportional to plasma density):

$$
\Gamma_i = n_i \cdot v_{\text{Bohm}} = n_i \sqrt{\frac{k_B T_e}{m_i}}
$$

where:

- $n_i$ = ion density
- $T_e$ = electron temperature
- $m_i$ = ion mass

**Source Power** controls plasma density:

$$
n_e \propto \sqrt{P_{\text{source}}}
$$

**Bias Power** controls ion energy:

$$
E_{\text{ion}} \propto V_{\text{bias}} \propto \sqrt{P_{\text{bias}}}
$$



**7. Atomic Layer Etching (ALE)**

**7.1 Process Cycle**

```
-
┌─────────────────────────────────────────────────────┐
│  Step 1: Surface Modification (Self-limiting)       │
│          Cl₂ adsorption → Si-Cl surface bonds       │
├─────────────────────────────────────────────────────┤
│  Step 2: Purge                                      │
│          Remove excess Cl₂                          │
├─────────────────────────────────────────────────────┤
│  Step 3: Removal (Self-limiting)                    │
│          Low-energy Ar⁺ bombardment                 │
│          E_ion < E_threshold(Si), > E_threshold(SiCl)│
├─────────────────────────────────────────────────────┤
│  Step 4: Purge                                      │
│          Remove SiClₓ products                      │
└─────────────────────────────────────────────────────┘
         ↓ Repeat ↓
```

**7.2 Etch Per Cycle (EPC)**

$$
\text{EPC} \approx 0.3 - 0.5 \text{ nm/cycle} \approx 1 \text{ monolayer}
$$

**7.3 Energy Window**

For self-limiting removal, ion energy must satisfy:

$$
E_{\text{threshold}}^{\text{modified}} < E_{\text{ion}} < E_{\text{threshold}}^{\text{unmodified}}
$$

- **Example for Si ALE**:
  - $E_{\text{threshold}}(\text{Si-Cl}) \approx 12-15$ eV
  - $E_{\text{threshold}}(\text{Si}) \approx 25-30$ eV
  - **Operating window**: $15 < E_{\text{ion}} < 25$ eV



**8. Etch Challenges at Advanced Nodes**

**8.1 High Aspect Ratio Etching (HARE)**

- **Ion Angular Distribution Broadening**:

$$
\Delta\theta \propto \sqrt{\frac{T_i}{E_{\text{ion}}}}
$$

where $T_i$ is ion temperature.

- **Knudsen Transport Limitation**:

$$
\Gamma_{\text{bottom}} = \Gamma_{\text{top}} \cdot \frac{W}{2H} = \frac{\Gamma_{\text{top}}}{2 \cdot AR}
$$

**8.2 Aspect Ratio Dependent Etching (ARDE)**

Etch rate decreases with aspect ratio:

$$
R(AR) = R_0 \cdot f(AR)
$$

where typically:

$$
f(AR) \approx \frac{1}{1 + \beta \cdot AR}
$$

with $\beta$ being a process-dependent constant.

**8.3 Line Edge Roughness (LER)**

**3σ LER Specification**:

$$
\text{LER}_{3\sigma} < 0.1 \times \text{CD}
$$

For 20 nm CD: LER $< 2$ nm (3σ)



**9. Process Control**

**9.1 Endpoint Detection Methods**

| Method | Principle | Application |
|--------|-----------|-------------|
| **OES** | Optical Emission Spectroscopy | Monitor plasma species |
| **Interferometry** | Laser reflection interference | Real-time thickness |
| **RGA** | Residual Gas Analysis | Etch product detection |
| **Bias Monitoring** | DC bias change | Layer transition |

**9.2 OES Endpoint Signal**

For layer clearing:

$$
I_{\text{product}}(t) = I_0 \cdot e^{-t/\tau} \quad \text{(during clear)}
$$

where $\tau$ is the decay time constant related to etch rate.



**10. Key Equations Reference**

| Parameter | Equation | Units |
|-----------|----------|-------|
| Etch Rate | $R = \Delta d / \Delta t$ | nm/min |
| Selectivity | $S = R_{\text{target}} / R_{\text{mask}}$ | ratio |
| Anisotropy | $A = 1 - R_{\text{lat}} / R_{\text{vert}}$ | 0-1 |
| Aspect Ratio | $AR = H / W$ | ratio |
| Ion Energy | $E = e(V_p - V_{dc})$ | eV |
| Uniformity | $\text{WIWNU} = \sigma / \bar{R} \times 100\%$ | % |
| Ion Flux | $\Gamma_i = n_i \sqrt{k_B T_e / m_i}$ | cm⁻²s⁻¹ |






**Physical Constants**

| Constant | Symbol | Value |
|----------|--------|-------|
| Electron charge | $e$ | $1.602 \times 10^{-19}$ C |
| Boltzmann constant | $k_B$ | $1.381 \times 10^{-23}$ J/K |
| Electron mass | $m_e$ | $9.109 \times 10^{-31}$ kg |
| Avogadro's number | $N_A$ | $6.022 \times 10^{23}$ mol⁻¹ |



**Common Etch Gases**

- **Silicon Etch**: $\text{Cl}_2$, $\text{HBr}$, $\text{SF}_6$, $\text{NF}_3$
- **Oxide Etch**: $\text{CF}_4$, $\text{CHF}_3$, $\text{C}_4\text{F}_8$, $\text{C}_4\text{F}_6$
- **Nitride Etch**: $\text{CHF}_3$, $\text{CH}_2\text{F}_2$, $\text{CH}_3\text{F}$
- **Metal Etch**: $\text{Cl}_2$, $\text{BCl}_3$, $\text{SF}_6$
- **Passivation**: $\text{O}_2$, $\text{N}_2$, $\text{He}$
- **Carrier/Dilution**: $\text{Ar}$, $\text{He}$, $\text{N}_2$

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/etch-process) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
