# MOSFET: Mathematical Modeling

**Keywords**: mosfet equations,mosfet modeling,threshold voltage,drain current,NMOS PMOS,short channel effects,subthreshold,device physics equations

---

**MOSFET: Mathematical Modeling**

Metal-Oxide-Semiconductor Field-Effect Transistor (MOSFET)   
Comprehensive equations, mathematical modeling, and process-parameter relationships



   1. Fundamental Device Structure

    1.1 MOSFET Components

A MOSFET is a four-terminal semiconductor device consisting of:

-  Source (S) : Heavily doped region where carriers originate
-  Drain (D) : Heavily doped region where carriers are collected
-  Gate (G) : Control electrode separated from channel by dielectric
-  Body/Substrate (B) : Semiconductor bulk (p-type for NMOS, n-type for PMOS)

    1.2 Operating Principle

The gate voltage modulates channel conductivity through field effect:

$$
\text{Gate Voltage} \rightarrow \text{Electric Field} \rightarrow \text{Channel Formation} \rightarrow \text{Current Flow}
$$

    1.3 Device Types

| Type | Substrate | Channel Carriers | Threshold |
|------|-----------|------------------|-----------|
| NMOS | p-type | Electrons | $V_{th} > 0$ (enhancement) |
| PMOS | n-type | Holes | $V_{th} < 0$ (enhancement) |



   2. Core MOSFET Equations

    2.1 Threshold Voltage

The threshold voltage $V_{th}$ determines device turn-on and is highly process-dependent:

$$
V_{th} = V_{FB} + 2\phi_F + \frac{\sqrt{2\varepsilon_{Si} \cdot q \cdot N_A \cdot 2\phi_F}}{C_{ox}}
$$

     Component Equations

-  Flat-band voltage :

$$
V_{FB} = \phi_{ms} - \frac{Q_{ox}}{C_{ox}}
$$

-  Fermi potential :

$$
\phi_F = \frac{kT}{q} \ln\left(\frac{N_A}{n_i}\right)
$$

-  Oxide capacitance per unit area :

$$
C_{ox} = \frac{\varepsilon_{ox}}{t_{ox}} = \frac{\kappa \cdot \varepsilon_0}{t_{ox}}
$$

-  Work function difference :

$$
\phi_{ms} = \phi_m - \phi_s = \phi_m - \left(\chi + \frac{E_g}{2q} + \phi_F\right)
$$

     Parameter Definitions

| Symbol | Description | Typical Value/Unit |
|--------|-------------|-------------------|
| $V_{FB}$ | Flat-band voltage | $-0.5$ to $-1.0$ V |
| $\phi_F$ | Fermi potential | $0.3$ to $0.4$ V |
| $\phi_{ms}$ | Work function difference | $-0.5$ to $-1.0$ V |
| $C_{ox}$ | Oxide capacitance | $\sim 10^{-2}$ F/m² |
| $Q_{ox}$ | Fixed oxide charge | $\sim 10^{10}$ q/cm² |
| $N_A$ | Acceptor concentration | $10^{15}$ to $10^{18}$ cm⁻³ |
| $n_i$ | Intrinsic carrier concentration | $1.5 \times 10^{10}$ cm⁻³ (Si, 300K) |
| $\varepsilon_{Si}$ | Silicon permittivity | $11.7 \varepsilon_0$ |
| $\varepsilon_{ox}$ | SiO₂ permittivity | $3.9 \varepsilon_0$ |



    2.2 Drain Current Equations

     2.2.1 Linear (Triode) Region

 Condition : $V_{DS} < V_{GS} - V_{th}$ (channel not pinched off)

$$
I_D = \mu_n C_{ox} \frac{W}{L} \left[ (V_{GS} - V_{th}) V_{DS} - \frac{V_{DS}^2}{2} \right]
$$

 Simplified form  (for small $V_{DS}$):

$$
I_D \approx \mu_n C_{ox} \frac{W}{L} (V_{GS} - V_{th}) V_{DS}
$$

 Channel resistance :

$$
R_{ch} = \frac{V_{DS}}{I_D} = \frac{L}{\mu_n C_{ox} W (V_{GS} - V_{th})}
$$

     2.2.2 Saturation Region

 Condition : $V_{DS} \geq V_{GS} - V_{th}$ (channel pinched off)

$$
I_D = \frac{1}{2} \mu_n C_{ox} \frac{W}{L} (V_{GS} - V_{th})^2 (1 + \lambda V_{DS})
$$

 Without channel-length modulation  ($\lambda = 0$):

$$
I_{D,sat} = \frac{1}{2} \mu_n C_{ox} \frac{W}{L} (V_{GS} - V_{th})^2
$$

 Saturation voltage :

$$
V_{DS,sat} = V_{GS} - V_{th}
$$

     2.2.3 Channel-Length Modulation

The parameter $\lambda$ captures output resistance degradation:

$$
\lambda = \frac{1}{L \cdot E_{crit}} \approx \frac{1}{V_A}
$$

 Output resistance :

$$
r_o = \frac{\partial V_{DS}}{\partial I_D} = \frac{1}{\lambda I_D} = \frac{V_A + V_{DS}}{I_D}
$$

Where $V_A$ is the Early voltage (typically $5$ to $50$ V/μm × L).



    2.3 Subthreshold Conduction

     2.3.1 Weak Inversion Current

 Condition : $V_{GS} < V_{th}$ (exponential behavior)

$$
I_D = I_0 \exp\left(\frac{V_{GS} - V_{th}}{n \cdot V_T}\right) \left[1 - \exp\left(-\frac{V_{DS}}{V_T}\right)\right]
$$

 Characteristic current :

$$
I_0 = \mu_n C_{ox} \frac{W}{L} (n-1) V_T^2
$$

 Thermal voltage :

$$
V_T = \frac{kT}{q} \approx 26 \text{ mV at } T = 300\text{K}
$$

     2.3.2 Subthreshold Swing

The subthreshold swing $S$ quantifies turn-off sharpness:

$$
S = \frac{\partial V_{GS}}{\partial (\log_{10} I_D)} = n \cdot V_T \cdot \ln(10) = 2.3 \cdot n \cdot V_T
$$

 Numerical values :

- Ideal minimum: $S_{min} = 60$ mV/decade (at 300K, $n = 1$)
- Typical range: $S = 70$ to $100$ mV/decade
- $n = 1 + \frac{C_{dep}}{C_{ox}}$ (subthreshold ideality factor)

     2.3.3 Depletion Capacitance

$$
C_{dep} = \frac{\varepsilon_{Si}}{W_{dep}} = \sqrt{\frac{q \varepsilon_{Si} N_A}{4 \phi_F}}
$$



    2.4 Body Effect

When source-to-body voltage $V_{SB} 
eq 0$:

$$
V_{th}(V_{SB}) = V_{th0} + \gamma \left(\sqrt{2\phi_F + V_{SB}} - \sqrt{2\phi_F}\right)
$$

 Body effect coefficient :

$$
\gamma = \frac{\sqrt{2 q \varepsilon_{Si} N_A}}{C_{ox}}
$$

 Typical values : $\gamma = 0.3$ to $1.0$ V$^{1/2}$



    2.5 Transconductance and Output Conductance

     2.5.1 Transconductance

 Saturation region :

$$
g_m = \frac{\partial I_D}{\partial V_{GS}} = \mu_n C_{ox} \frac{W}{L} (V_{GS} - V_{th}) = \sqrt{2 \mu_n C_{ox} \frac{W}{L} I_D}
$$

 Alternative form :

$$
g_m = \frac{2 I_D}{V_{GS} - V_{th}}
$$

     2.5.2 Output Conductance

$$
g_{ds} = \frac{\partial I_D}{\partial V_{DS}} = \lambda I_D = \frac{I_D}{V_A}
$$

     2.5.3 Intrinsic Gain

$$
A_v = \frac{g_m}{g_{ds}} = \frac{2}{\lambda(V_{GS} - V_{th})} = \frac{2 V_A}{V_{GS} - V_{th}}
$$



   3. Short-Channel Effects

    3.1 Velocity Saturation

At high lateral electric fields ($E > E_{crit} \approx 10^4$ V/cm):

$$
v_d = \frac{\mu_n E}{1 + E/E_{crit}}
$$

 Saturation velocity :

$$
v_{sat} = \mu_n E_{crit} \approx 10^7 \text{ cm/s (electrons in Si)}
$$

     3.1.1 Modified Saturation Current

$$
I_{D,sat} = W C_{ox} v_{sat} (V_{GS} - V_{th})
$$

Note: Linear (not quadratic) dependence on gate overdrive.

     3.1.2 Critical Length

Velocity saturation dominates when:

$$
L < L_{crit} = \frac{\mu_n (V_{GS} - V_{th})}{2 v_{sat}}
$$



    3.2 Drain-Induced Barrier Lowering (DIBL)

The drain field reduces the source-side barrier:

$$
V_{th} = V_{th,long} - \eta \cdot V_{DS}
$$

 DIBL coefficient :

$$
\eta = -\frac{\partial V_{th}}{\partial V_{DS}}
$$

 Typical values : $\eta = 20$ to $100$ mV/V for short channels

     3.2.1 Modified Threshold Equation

$$
V_{th}(V_{DS}, V_{SB}) = V_{th0} + \gamma(\sqrt{2\phi_F + V_{SB}} - \sqrt{2\phi_F}) - \eta V_{DS}
$$



    3.3 Mobility Degradation

     3.3.1 Vertical Field Effect

$$
\mu_{eff} = \frac{\mu_0}{1 + \theta (V_{GS} - V_{th})}
$$

 Alternative form  (surface roughness scattering):

$$
\mu_{eff} = \frac{\mu_0}{1 + (\theta_1 + \theta_2 V_{SB})(V_{GS} - V_{th})}
$$

     3.3.2 Universal Mobility Model

$$
\mu_{eff} = \frac{\mu_0}{\left[1 + \left(\frac{E_{eff}}{E_0}\right)^
u + \left(\frac{E_{eff}}{E_1}\right)^\beta\right]}
$$

Where $E_{eff}$ is the effective vertical field:

$$
E_{eff} = \frac{Q_b + \eta_s Q_i}{\varepsilon_{Si}}
$$



    3.4 Hot Carrier Effects

     3.4.1 Impact Ionization Current

$$
I_{sub} = \frac{I_D}{M - 1}
$$

 Multiplication factor :

$$
M = \frac{1}{1 - \int_0^{L_{dep}} \alpha(E) dx}
$$

     3.4.2 Ionization Rate

$$
\alpha = \alpha_\infty \exp\left(-\frac{E_{crit}}{E}\right)
$$



    3.5 Gate Leakage

     3.5.1 Direct Tunneling Current

$$
J_g = A \cdot E_{ox}^2 \exp\left(-\frac{B}{\vert E_{ox} \vert}\right)
$$

Where:

$$
A = \frac{q^3}{16\pi^2 \hbar \phi_b}
$$

$$
B = \frac{4\sqrt{2m^* \phi_b^3}}{3\hbar q}
$$

     3.5.2 Gate Oxide Field

$$
E_{ox} = \frac{V_{GS} - V_{FB} - \psi_s}{t_{ox}}
$$



   4. Parameters

    4.1 Gate Oxide Engineering

     4.1.1 Oxide Capacitance

$$
C_{ox} = \frac{\varepsilon_0 \cdot \kappa}{t_{ox}}
$$

| Dielectric | $\kappa$ | EOT for $t_{phys} = 3$ nm |
|------------|----------|---------------------------|
| SiO₂ | 3.9 | 3.0 nm |
| Si₃N₄ | 7.5 | 1.56 nm |
| Al₂O₃ | 9 | 1.30 nm |
| HfO₂ | 20-25 | 0.47-0.59 nm |
| ZrO₂ | 25 | 0.47 nm |

     4.1.2 Equivalent Oxide Thickness (EOT)

$$
EOT = t_{high-\kappa} \times \frac{\varepsilon_{SiO_2}}{\varepsilon_{high-\kappa}} = t_{high-\kappa} \times \frac{3.9}{\kappa}
$$

     4.1.3 Capacitance Equivalent Thickness (CET)

Including quantum effects and poly depletion:

$$
CET = EOT + \Delta t_{QM} + \Delta t_{poly}
$$

Where:

- $\Delta t_{QM} \approx 0.3$ to $0.5$ nm (quantum mechanical)
- $\Delta t_{poly} \approx 0.3$ to $0.5$ nm (polysilicon depletion)



    4.2 Channel Doping

     4.2.1 Doping Profile Impact

$$
V_{th} \propto \sqrt{N_A}
$$

$$
\mu \propto \frac{1}{N_A^{0.3}} \text{ (ionized impurity scattering)}
$$

     4.2.2 Depletion Width

$$
W_{dep} = \sqrt{\frac{2\varepsilon_{Si}(2\phi_F + V_{SB})}{qN_A}}
$$

     4.2.3 Junction Capacitance

$$
C_j = C_{j0}\left(1 + \frac{V_R}{\phi_{bi}}\right)^{-m}
$$

Where:

- $C_{j0}$ = zero-bias capacitance
- $\phi_{bi}$ = built-in potential
- $m = 0.5$ (abrupt junction), $m = 0.33$ (graded junction)



    4.3 Gate Material Engineering

     4.3.1 Work Function Values

| Gate Material | Work Function $\phi_m$ (eV) | Application |
|--------------|----------------------------|-------------|
| n+ Polysilicon | 4.05 | Legacy NMOS |
| p+ Polysilicon | 5.15 | Legacy PMOS |
| TiN | 4.5-4.7 | NMOS (midgap) |
| TaN | 4.0-4.4 | NMOS |
| TiAl | 4.2-4.3 | NMOS |
| TiAlN | 4.7-4.8 | PMOS |

     4.3.2 Flat-Band Voltage Engineering

For symmetric CMOS threshold voltages:

$$
V_{FB,NMOS} + V_{FB,PMOS} \approx -E_g/q
$$



    4.4 Channel Length Scaling

     4.4.1 Characteristic Length

$$
\lambda = \sqrt{\frac{\varepsilon_{Si}}{\varepsilon_{ox}} \cdot t_{ox} \cdot x_j}
$$

For good short-channel control: $L > 5\lambda$ to $10\lambda$

     4.4.2 Scale Length (FinFET/GAA)

$$
\lambda_{GAA} = \sqrt{\frac{\varepsilon_{Si} \cdot t_{Si}^2}{2 \varepsilon_{ox} \cdot t_{ox}}}
$$



    4.5 Strain Engineering

     4.5.1 Mobility Enhancement

$$
\mu_{strained} = \mu_0 (1 + \Pi \cdot \sigma)
$$

Where:

- $\Pi$ = piezoresistive coefficient
- $\sigma$ = applied stress

 Enhancement factors :

- NMOS (tensile): $+30\%$ to $+70\%$ mobility gain
- PMOS (compressive): $+50\%$ to $+100\%$ mobility gain

     4.5.2 Stress Impact on Threshold

$$
\Delta V_{th} = \alpha_{th} \cdot \sigma
$$

Where $\alpha_{th} \approx 1$ to $5$ mV/GPa



   5. Advanced Compact Models

    5.1 BSIM4 Model

     5.1.1 Unified Current Equation

$$
I_{DS} = I_{DS0} \cdot \left(1 + \frac{V_{DS} - V_{DS,eff}}{V_A}\right) \cdot \frac{1}{1 + R_S \cdot G_{DS0}}
$$

     5.1.2 Effective Overdrive

$$
V_{GS,eff} - V_{th} = \frac{2nV_T \cdot \ln\left[1 + \exp\left(\frac{V_{GS} - V_{th}}{2nV_T}\right)\right]}{1 + 2n\sqrt{\delta + \left(\frac{V_{GS}-V_{th}}{2nV_T} - \delta\right)^2}}
$$

     5.1.3 Effective Saturation Voltage

$$
V_{DS,eff} = V_{DS,sat} - \frac{V_T}{2}\ln\left(\frac{V_{DS,sat} + \sqrt{V_{DS,sat}^2 + 4V_T^2}}{V_{DS} + \sqrt{V_{DS}^2 + 4V_T^2}}\right)
$$



    5.2 Surface Potential Model (PSP)

     5.2.1 Implicit Surface Potential Equation

$$
V_{GB} - V_{FB} = \psi_s + \gamma\sqrt{\psi_s + V_T e^{(\psi_s - 2\phi_F - V_{SB})/V_T} - V_T}
$$

     5.2.2 Charge-Based Current

$$
I_D = \mu W \frac{Q_i(0) - Q_i(L)}{L} \cdot \frac{V_{DS}}{V_{DS,eff}}
$$

Where $Q_i$ is the inversion charge density:

$$
Q_i = -C_{ox}\left[\psi_s - 2\phi_F - V_{ch} + V_T\left(e^{(\psi_s - 2\phi_F - V_{ch})/V_T} - 1\right)\right]^{1/2}
$$



    5.3 FinFET Equations

     5.3.1 Effective Width

$$
W_{eff} = 2H_{fin} + W_{fin}
$$

For multiple fins:

$$
W_{total} = N_{fin} \cdot (2H_{fin} + W_{fin})
$$

     5.3.2 Multi-Gate Scale Length

 Double-gate :

$$
\lambda_{DG} = \sqrt{\frac{\varepsilon_{Si} \cdot t_{Si} \cdot t_{ox}}{2\varepsilon_{ox}}}
$$

 Gate-all-around (GAA) :

$$
\lambda_{GAA} = \sqrt{\frac{\varepsilon_{Si} \cdot r^2}{4\varepsilon_{ox}} \cdot \ln\left(1 + \frac{t_{ox}}{r}\right)}
$$

Where $r$ = nanowire radius

     5.3.3 FinFET Threshold Voltage

$$
V_{th} = V_{FB} + 2\phi_F + \frac{qN_A W_{fin}}{2C_{ox}} - \Delta V_{th,SCE}
$$



   6. Process-Equation Coupling

    6.1 Parameter Sensitivity Analysis

| Process Parameter | Primary Equations Affected | Sensitivity |
|------------------|---------------------------|-------------|
| $t_{ox}$ (oxide thickness) | $C_{ox}$, $V_{th}$, $I_D$, $g_m$ | High |
| $N_A$ (channel doping) | $V_{th}$, $\gamma$, $\mu$, $W_{dep}$ | High |
| $L$ (channel length) | $I_D$, SCE, $\lambda$ | Very High |
| $W$ (channel width) | $I_D$, $g_m$ (linear) | Moderate |
| Gate work function | $V_{FB}$, $V_{th}$ | High |
| Junction depth $x_j$ | SCE, $R_{SD}$ | Moderate |
| Strain level | $\mu$, $I_D$ | Moderate |

    6.2 Variability Equations

     6.2.1 Random Dopant Fluctuation (RDF)

$$
\sigma_{V_{th}} = \frac{A_{VT}}{\sqrt{W \cdot L}}
$$

Where $A_{VT}$ is the Pelgrom coefficient (typically $1$ to $5$ mV·μm).

     6.2.2 Line Edge Roughness (LER)

$$
\sigma_{V_{th,LER}} \propto \frac{\sigma_{LER}}{L}
$$

     6.2.3 Oxide Thickness Variation

$$
\sigma_{V_{th,tox}} = \frac{\partial V_{th}}{\partial t_{ox}} \cdot \sigma_{t_{ox}} = \frac{V_{th} - V_{FB} - 2\phi_F}{t_{ox}} \cdot \sigma_{t_{ox}}
$$



    6.3 Equations:

     6.3.1 Drive Current

$$
I_{on} = \frac{W}{L} \cdot \mu_{eff} \cdot C_{ox} \cdot \frac{(V_{DD} - V_{th})^\alpha}{1 + (V_{DD} - V_{th})/E_{sat}L}
$$

Where $\alpha = 2$ (long channel) or $\alpha \rightarrow 1$ (velocity saturated).

     6.3.2 Leakage Current

$$
I_{off} = I_0 \cdot \frac{W}{L} \cdot \exp\left(\frac{-V_{th}}{nV_T}\right) \cdot \left(1 - \exp\left(\frac{-V_{DD}}{V_T}\right)\right)
$$

     6.3.3 CV/I Delay Metric

$$
\tau = \frac{C_L \cdot V_{DD}}{I_{on}} \propto \frac{L^2}{\mu (V_{DD} - V_{th})}
$$



   Constants:

| Constant | Symbol | Value |
|----------|--------|-------|
| Elementary charge | $q$ | $1.602 \times 10^{-19}$ C |
| Boltzmann constant | $k$ | $1.381 \times 10^{-23}$ J/K |
| Permittivity of free space | $\varepsilon_0$ | $8.854 \times 10^{-12}$ F/m |
| Planck constant | $\hbar$ | $1.055 \times 10^{-34}$ J·s |
| Electron mass | $m_0$ | $9.109 \times 10^{-31}$ kg |
| Thermal voltage (300K) | $V_T$ | $25.9$ mV |
| Silicon bandgap (300K) | $E_g$ | $1.12$ eV |
| Intrinsic carrier conc. (Si) | $n_i$ | $1.5 \times 10^{10}$ cm⁻³ |



   Equations:

    Threshold Voltage

$$
V_{th} = V_{FB} + 2\phi_F + \frac{\sqrt{2\varepsilon_{Si} q N_A (2\phi_F)}}{C_{ox}}
$$

    Linear Region Current

$$
I_D = \mu C_{ox} \frac{W}{L} \left[(V_{GS} - V_{th})V_{DS} - \frac{V_{DS}^2}{2}\right]
$$

    Saturation Current

$$
I_D = \frac{1}{2}\mu C_{ox}\frac{W}{L}(V_{GS} - V_{th})^2(1 + \lambda V_{DS})
$$

    Subthreshold Current

$$
I_D = I_0 \exp\left(\frac{V_{GS} - V_{th}}{nV_T}\right)
$$

    Transconductance

$$
g_m = \sqrt{2\mu C_{ox}\frac{W}{L}I_D}
$$

    Body Effect

$$
V_{th} = V_{th0} + \gamma\left(\sqrt{2\phi_F + V_{SB}} - \sqrt{2\phi_F}\right)
$$

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/mosfet-equations) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
