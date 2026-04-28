# Semiconductor chip manufacturing

**Keywords**: chip,semiconductor chip,chip manufacturing,how to make a chip,semiconductor manufacturing,chip fabrication,wafer processing

---

**Semiconductor chip manufacturing** is one of the most sophisticated and precise manufacturing processes ever developed. This document provides a comprehensive guide following the complete fabrication flow from raw silicon wafer to finished integrated circuit.

---

**Manufacturing Process Flow (18 Steps)**

**FRONT-END-OF-LINE (FEOL) — Transistor Fabrication**

```
┌─────────────────────────────────────────────────────────────────┐
│  STEP 1: WAFER START & CLEANING                                 │
│  • Incoming QC inspection                                       │
│  • RCA clean (SC-1, SC-2, DHF)                                  │
│  • Surface preparation                                          │
└─────────────────────────────────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────┐
│  STEP 2: EPITAXY (EPI)                                          │
│  • Grow single-crystal Si layer                                 │
│  • In-situ doping control                                       │
│  • Strained SiGe for mobility                                   │
└─────────────────────────────────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────┐
│  STEP 3: OXIDATION / DIFFUSION                                  │
│  • Thermal gate oxide growth                                    │
│  • STI pad oxide                                                │
│  • High-κ dielectric (HfO₂)                                     │
└─────────────────────────────────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────┐
│  STEP 4: CVD (FEOL)                                             │
│  • STI trench fill (HDP-CVD)                                    │
│  • Hard masks (Si₃N₄)                                           │
│  • Spacer deposition                                            │
└─────────────────────────────────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────┐
│  STEP 5: PHOTOLITHOGRAPHY                                       │
│  • Coat → Expose (EUV/DUV) → Develop                            │
│  • Pattern transfer to resist                                   │
│  • Overlay alignment < 2 nm                                     │
└─────────────────────────────────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────┐
│  STEP 6: ETCHING                                                │
│  • RIE / Plasma etch                                            │
│  • Resist strip (ashing)                                        │
│  • Post-etch clean                                              │
└─────────────────────────────────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────┐
│  STEP 7: ION IMPLANTATION                                       │
│  • Source/Drain doping                                          │
│  • Well implants                                                │
│  • Threshold voltage adjust                                     │
└─────────────────────────────────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────┐
│  STEP 8: RAPID THERMAL PROCESSING (RTP)                         │
│  • Dopant activation                                            │
│  • Damage annealing                                             │
│  • Silicidation (NiSi)                                          │
└─────────────────────────────────────────────────────────────────┘
```

**BACK-END-OF-LINE (BEOL) — Interconnect Fabrication**

```
┌─────────────────────────────────────────────────────────────────┐
│  STEP 9: DEPOSITION (CVD / ALD)                                 │
│  • ILD dielectrics (low-κ)                                      │
│  • Tungsten plugs (W-CVD)                                       │
│  • Etch stop layers                                             │
└─────────────────────────────────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────┐
│  STEP 10: DEPOSITION (PVD)                                      │
│  • Barrier layers (TaN/Ta)                                      │
│  • Cu seed layer                                                │
│  • Liner films                                                  │
└─────────────────────────────────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────┐
│  STEP 11: ELECTROPLATING (ECP)                                  │
│  • Copper bulk fill                                             │
│  • Bottom-up superfill                                          │
│  • Dual damascene process                                       │
└─────────────────────────────────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────┐
│  STEP 12: CHEMICAL MECHANICAL POLISHING (CMP)                   │
│  • Planarization                                                │
│  • Excess metal removal                                         │
│  • Multi-step (Cu → Barrier → Buff)                             │
└─────────────────────────────────────────────────────────────────┘
```

**TESTING & ASSEMBLY — Backend Operations**

```
┌─────────────────────────────────────────────────────────────────┐
│  STEP 13: WAFER PROBE TEST (EDS)                                │
│  • Die-level electrical test                                    │
│  • Parametric & functional test                                 │
│  • Bad die inking / mapping                                     │
└─────────────────────────────────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────┐
│  STEP 14: BACKGRINDING & DICING                                 │
│  • Wafer thinning                                               │
│  • Blade / Laser / Stealth dicing                               │
│  • Die singulation                                              │
└─────────────────────────────────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────┐
│  STEP 15: DIE ATTACH                                            │
│  • Pick & place                                                 │
│  • Epoxy / Eutectic / Solder bond                               │
│  • Cure cycle                                                   │
└─────────────────────────────────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────┐
│  STEP 16: WIRE BONDING / FLIP CHIP                              │
│  • Au/Cu wire bonding                                           │
│  • Flip chip C4 / Cu pillar bumps                               │
│  • Underfill dispensing                                         │
└─────────────────────────────────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────┐
│  STEP 17: ENCAPSULATION                                         │
│  • Transfer molding                                             │
│  • Mold compound injection                                      │
│  • Post-mold cure                                               │
└─────────────────────────────────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────┐
│  STEP 18: FINAL TEST → PACKING & SHIP                           │
│  • Burn-in testing                                              │
│  • Speed binning & class test                                   │
│  • Tape & reel packaging                                        │
└─────────────────────────────────────────────────────────────────┘
```

---

# FRONT-END-OF-LINE (FEOL)

**Step 1: Wafer Start & Cleaning**

**1.1 Incoming Quality Control**

- **Wafer Specifications:**
  - Diameter: $300 	ext{ mm}$ (standard) or $200 	ext{ mm}$ (legacy)
  - Thickness: $775 pm 20 	ext{ μm}$
  - Resistivity: $1-20 	ext{ Ω·cm}$
  - Crystal orientation: $langle 100 
angle$ or $langle 111 
angle$

- **Inspection Parameters:**
  - Total Thickness Variation (TTV): $< 5 	ext{ μm}$
  - Surface roughness: $R_a < 0.5 	ext{ nm}$
  - Particle count: $< 0.1 	ext{ particles/cm}^2$ at $geq 0.1 	ext{ μm}$

**1.2 RCA Cleaning**

The industry-standard RCA clean removes organic, ionic, and metallic contaminants:

**SC-1 (Standard Clean 1) — Organic/Particle Removal:**
$$
NH_4OH : H_2O_2 : H_2O = 1:1:5 quad @ quad 70-80°C
$$

**SC-2 (Standard Clean 2) — Metal Ion Removal:**
$$
HCl : H_2O_2 : H_2O = 1:1:6 quad @ quad 70-80°C
$$

**DHF Dip (Dilute HF) — Native Oxide Removal:**
$$
HF : H_2O = 1:50 quad @ quad 25°C
$$

**1.3 Surface Preparation**

- **Megasonic cleaning**: $0.8-1.5 	ext{ MHz}$ frequency
- **DI water rinse**: Resistivity $> 18 	ext{ MΩ·cm}$
- **Spin-rinse-dry (SRD)**: $< 1000 	ext{ rpm}$ final spin

---

**Step 2: Epitaxy (EPI)**

**2.1 Purpose**

Grows a thin, high-quality single-crystal silicon layer with precisely controlled doping on the substrate.

**Why Epitaxy?**
- Better crystal quality than bulk wafer
- Independent doping control
- Reduced latch-up in CMOS
- Enables strained silicon (SiGe)

**2.2 Epitaxial Growth Methods**

**Chemical Vapor Deposition (CVD) Epitaxy:**
$$
SiH_4 xrightarrow{Delta} Si + 2H_2 quad (Silane)
$$
$$
SiH_2Cl_2 xrightarrow{Delta} Si + 2HCl quad (Dichlorosilane)
$$
$$
SiHCl_3 + H_2 xrightarrow{Delta} Si + 3HCl quad (Trichlorosilane)
$$

**2.3 Growth Rate**

The epitaxial growth rate depends on temperature and precursor:

$$
R_{growth} = k_0 cdot P_{precursor} cdot expleft(-frac{E_a}{k_B T}
ight)
$$

| Precursor | Temperature | Growth Rate |
|-----------|-------------|-------------|
| $SiH_4$ | $550-700°C$ | $0.01-0.1 	ext{ μm/min}$ |
| $SiH_2Cl_2$ | $900-1050°C$ | $0.1-1 	ext{ μm/min}$ |
| $SiHCl_3$ | $1050-1150°C$ | $0.5-2 	ext{ μm/min}$ |
| $SiCl_4$ | $1150-1250°C$ | $1-3 	ext{ μm/min}$ |

**2.4 In-Situ Doping**

Dopant gases are introduced during epitaxy:

- **N-type**: $PH_3$ (phosphine), $AsH_3$ (arsine)
- **P-type**: $B_2H_6$ (diborane)

**Doping Concentration:**
$$
N_d = frac{P_{dopant}}{P_{Si}} cdot frac{k_{seg}}{1 + k_{seg}} cdot N_{Si}
$$

Where $k_{seg}$ is the segregation coefficient.

**2.5 Strained Silicon (SiGe)**

Modern transistors use SiGe for strain engineering:

$$
Si_{1-x}Ge_x quad 	ext{where} quad x = 0.2-0.4
$$

**Lattice Mismatch:**
$$
frac{Delta a}{a} = frac{a_{SiGe} - a_{Si}}{a_{Si}} approx 0.042x
$$

**Strain-induced mobility enhancement:**
- Hole mobility: $+50-100\%$
- Electron mobility: $+20-40\%$

---

**Step 3: Oxidation / Diffusion**

**3.1 Thermal Oxidation**

**Dry Oxidation (Higher Quality, Slower):**
$$
Si + O_2 xrightarrow{900-1200°C} SiO_2
$$

**Wet Oxidation (Lower Quality, Faster):**
$$
Si + 2H_2O xrightarrow{900-1100°C} SiO_2 + 2H_2
$$

**3.2 Deal-Grove Model**

Oxide thickness follows:

$$
x_{ox}^2 + A cdot x_{ox} = B(t + 	au)
$$

**Linear Rate Constant:**
$$
frac{B}{A} = frac{h cdot C^*}{N_1}
$$

**Parabolic Rate Constant:**
$$
B = frac{2D_{eff} cdot C^*}{N_1}
$$

Where:
- $C^*$ = equilibrium oxidant concentration
- $N_1$ = number of oxidant molecules per unit volume of oxide
- $D_{eff}$ = effective diffusion coefficient
- $h$ = surface reaction rate constant

**3.3 Oxide Types in CMOS**

| Oxide Type | Thickness | Purpose |
|------------|-----------|---------|
| Gate Oxide | $1-5 	ext{ nm}$ | Transistor gate dielectric |
| STI Pad Oxide | $10-20 	ext{ nm}$ | Stress buffer for STI |
| Tunnel Oxide | $8-10 	ext{ nm}$ | Flash memory |
| Sacrificial Oxide | $10-50 	ext{ nm}$ | Surface damage removal |

**3.4 High-κ Dielectrics**

Modern nodes use high-κ materials instead of $SiO_2$:

**Equivalent Oxide Thickness (EOT):**
$$
EOT = t_{high-kappa} cdot frac{kappa_{SiO_2}}{kappa_{high-kappa}} = t_{high-kappa} cdot frac{3.9}{kappa_{high-kappa}}
$$

| Material | Dielectric Constant ($kappa$) | Bandgap (eV) |
|----------|-------------------------------|--------------|
| $SiO_2$ | $3.9$ | $9.0$ |
| $Si_3N_4$ | $7.5$ | $5.3$ |
| $Al_2O_3$ | $9$ | $8.8$ |
| $HfO_2$ | $20-25$ | $5.8$ |
| $ZrO_2$ | $25$ | $5.8$ |

---

**Step 4: CVD (FEOL) — Dielectrics, Hard Masks, Spacers**

**4.1 Purpose in FEOL**

CVD in FEOL is critical for depositing:
- **STI (Shallow Trench Isolation)** fill oxide
- **Gate hard masks** ($Si_3N_4$, $SiO_2$)
- **Spacer materials** ($Si_3N_4$, $SiCO$)
- **Pre-metal dielectric (ILD₀)**
- **Etch stop layers**

**4.2 CVD Methods**

**LPCVD (Low Pressure CVD):**
- Pressure: $0.1-10 	ext{ Torr}$
- Temperature: $400-900°C$
- Excellent uniformity
- Batch processing

**PECVD (Plasma Enhanced CVD):**
- Pressure: $0.1-10 	ext{ Torr}$
- Temperature: $200-400°C$
- Lower thermal budget
- Single wafer processing

**HDPCVD (High Density Plasma CVD):**
- Simultaneous deposition and sputtering
- Superior gap fill for STI
- Pressure: $1-10 	ext{ mTorr}$

**SACVD (Sub-Atmospheric CVD):**
- Pressure: $200-600 	ext{ Torr}$
- Good conformality
- Used for BPSG, USG

**4.3 Key FEOL CVD Films**

**Silicon Nitride ($Si_3N_4$):**
$$
3SiH_4 + 4NH_3 xrightarrow{LPCVD, 750°C} Si_3N_4 + 12H_2
$$

$$
3SiH_2Cl_2 + 4NH_3 xrightarrow{LPCVD, 750°C} Si_3N_4 + 6HCl + 6H_2
$$

**TEOS Oxide ($SiO_2$):**
$$
Si(OC_2H_5)_4 xrightarrow{PECVD, 400°C} SiO_2 + 	ext{byproducts}
$$

**HDP Oxide (STI Fill):**
$$
SiH_4 + O_2 xrightarrow{HDP-CVD} SiO_2 + 2H_2
$$

**4.4 CVD Process Parameters**

| Parameter | LPCVD | PECVD | HDPCVD |
|-----------|-------|-------|--------|
| Pressure | $0.1-10$ Torr | $0.1-10$ Torr | $1-10$ mTorr |
| Temperature | $400-900°C$ | $200-400°C$ | $300-450°C$ |
| Uniformity | $< 2\%$ | $< 3\%$ | $< 3\%$ |
| Step Coverage | Conformal | $50-80\%$ | Gap fill |
| Throughput | High (batch) | Medium | Medium |

**4.5 Film Properties**

| Film | Stress | Density | Application |
|------|--------|---------|-------------|
| LPCVD $Si_3N_4$ | $1.0-1.2$ GPa (tensile) | $3.1 	ext{ g/cm}^3$ | Hard mask, spacer |
| PECVD $Si_3N_4$ | $-200$ to $+200$ MPa | $2.5-2.8 	ext{ g/cm}^3$ | Passivation |
| LPCVD $SiO_2$ | $-300$ MPa (compressive) | $2.2 	ext{ g/cm}^3$ | Spacer |
| HDP $SiO_2$ | $-100$ to $-300$ MPa | $2.2 	ext{ g/cm}^3$ | STI fill |

---

**Step 5: Photolithography**

**5.1 Process Sequence**

```
HMDS Prime → Spin Coat → Soft Bake → Align → Expose → PEB → Develop → Hard Bake
```

**5.2 Resolution Limits**

**Rayleigh Criterion:**
$$
CD_{min} = k_1 cdot frac{lambda}{NA}
$$

**Depth of Focus:**
$$
DOF = k_2 cdot frac{lambda}{NA^2}
$$

Where:
- $CD_{min}$ = minimum critical dimension
- $k_1$ = process factor ($0.25-0.4$ for advanced nodes)
- $k_2$ = depth of focus factor ($approx 0.5$)
- $lambda$ = wavelength
- $NA$ = numerical aperture

**5.3 Exposure Systems Evolution**

| Generation | $lambda$ (nm) | $NA$ | $k_1$ | Resolution |
|------------|----------------|------|-------|------------|
| G-line | $436$ | $0.4$ | $0.8$ | $870 	ext{ nm}$ |
| I-line | $365$ | $0.6$ | $0.7$ | $425 	ext{ nm}$ |
| KrF | $248$ | $0.8$ | $0.5$ | $155 	ext{ nm}$ |
| ArF Dry | $193$ | $0.85$ | $0.4$ | $90 	ext{ nm}$ |
| ArF Immersion | $193$ | $1.35$ | $0.35$ | $50 	ext{ nm}$ |
| EUV | $13.5$ | $0.33$ | $0.35$ | $14 	ext{ nm}$ |
| High-NA EUV | $13.5$ | $0.55$ | $0.30$ | $8 	ext{ nm}$ |

**5.4 Immersion Lithography**

Uses water ($n = 1.44$) between lens and wafer:

$$
NA_{immersion} = n_{fluid} cdot sin	heta_{max}
$$

**Maximum NA achievable:**
- Dry: $NA approx 0.93$
- Water immersion: $NA approx 1.35$

**5.5 EUV Lithography**

**Light Source:**
- Tin ($Sn$) plasma at $lambda = 13.5 	ext{ nm}$
- CO₂ laser ($10.6 	ext{ μm}$) hits Sn droplets
- Conversion efficiency: $eta approx 5\%$

**Power Requirements:**
$$
P_{source} = frac{P_{wafer}}{eta_{optics} cdot eta_{conversion}} approx frac{250W}{0.04 cdot 0.05} = 125 	ext{ kW}
$$

**Multilayer Mirror Reflectivity:**
- Mo/Si bilayer: $sim 70\%$ per reflection
- 6 mirrors: $(0.70)^6 approx 12\%$ total throughput

**5.6 Photoresist Chemistry**

**Chemically Amplified Resist (CAR):**
$$
	ext{PAG} xrightarrow{h
u} H^+ quad 	ext{(Photoacid Generator)}
$$
$$
	ext{Protected Polymer} + H^+ xrightarrow{PEB} 	ext{Deprotected Polymer} + H^+
$$

**Acid Diffusion Length:**
$$
L_D = sqrt{D cdot t_{PEB}} approx 10-50 	ext{ nm}
$$

**5.7 Overlay Control**

**Overlay Budget:**
$$
sigma_{overlay} = sqrt{sigma_{tool}^2 + sigma_{process}^2 + sigma_{wafer}^2}
$$

Modern requirement: $< 2 	ext{ nm}$ (3σ)

---

**Step 6: Etching**

**6.1 Etch Methods Comparison**

| Property | Wet Etch | Dry Etch (RIE) |
|----------|----------|----------------|
| Profile | Isotropic | Anisotropic |
| Selectivity | High ($>100:1$) | Moderate ($10-50:1$) |
| Damage | None | Ion damage possible |
| Resolution | $> 1 	ext{ μm}$ | $< 10 	ext{ nm}$ |
| Throughput | High | Lower |

**6.2 Dry Etch Mechanisms**

**Physical Sputtering:**
$$
Y_{sputter} = frac{	ext{Atoms removed}}{	ext{Incident ion}}
$$

**Chemical Etching:**
$$
	ext{Material} + 	ext{Reactive Species} 
ightarrow 	ext{Volatile Products}
$$

**Reactive Ion Etching (RIE):**
Combines both mechanisms for anisotropic profiles.

**6.3 Plasma Chemistry**

**Silicon Etching:**
$$
Si + 4F^* 
ightarrow SiF_4 uparrow
$$
$$
Si + 2Cl^* 
ightarrow SiCl_2 uparrow
$$

**Oxide Etching:**
$$
SiO_2 + 4F^* + C^* 
ightarrow SiF_4 uparrow + CO_2 uparrow
$$

**Nitride Etching:**
$$
Si_3N_4 + 12F^* 
ightarrow 3SiF_4 uparrow + 2N_2 uparrow
$$

**6.4 Etch Parameters**

**Etch Rate:**
$$
ER = frac{Delta h}{Delta t} quad [	ext{nm/min}]
$$

**Selectivity:**
$$
S = frac{ER_{target}}{ER_{mask}}
$$

**Anisotropy:**
$$
A = 1 - frac{ER_{lateral}}{ER_{vertical}}
$$

$A = 1$ is perfectly anisotropic (vertical sidewalls)

**Aspect Ratio:**
$$
AR = frac{	ext{Depth}}{	ext{Width}}
$$

Modern HAR (High Aspect Ratio) etching: $AR > 100:1$

**6.5 Etch Gas Chemistry**

| Material | Primary Etch Gas | Additives | Products |
|----------|------------------|-----------|----------|
| Si | $SF_6$, $Cl_2$, $HBr$ | $O_2$ | $SiF_4$, $SiCl_4$, $SiBr_4$ |
| $SiO_2$ | $CF_4$, $C_4F_8$ | $CHF_3$, $O_2$ | $SiF_4$, $CO$, $CO_2$ |
| $Si_3N_4$ | $CF_4$, $CHF_3$ | $O_2$ | $SiF_4$, $N_2$, $CO$ |
| Poly-Si | $Cl_2$, $HBr$ | $O_2$ | $SiCl_4$, $SiBr_4$ |
| W | $SF_6$ | $N_2$ | $WF_6$ |
| Cu | Not practical | Use CMP | — |

**6.6 Post-Etch Processing**

**Resist Strip (Ashing):**
$$
	ext{Photoresist} + O^* xrightarrow{plasma} CO_2 + H_2O
$$

**Wet Clean (Post-Etch Residue Removal):**
- Dilute HF for polymer residue
- SC-1 for particles
- Proprietary etch residue removers

---

**Step 7: Ion Implantation**

**7.1 Purpose**

Introduces dopant atoms into silicon with precise control of:
- Dose (atoms/cm²)
- Energy (depth)
- Species (n-type or p-type)

**7.2 Implanter Components**

```
Ion Source → Mass Analyzer → Acceleration → Beam Scanning → Target Wafer
```

**7.3 Dopant Selection**

**N-type (Donors):**

| Dopant | Mass (amu) | $E_d$ (meV) | Application |
|--------|------------|-------------|-------------|
| $P$ | $31$ | $45$ | NMOS S/D, wells |
| $As$ | $75$ | $54$ | NMOS S/D (shallow) |
| $Sb$ | $122$ | $39$ | Buried layers |

**P-type (Acceptors):**

| Dopant | Mass (amu) | $E_a$ (meV) | Application |
|--------|------------|-------------|-------------|
| $B$ | $11$ | $45$ | PMOS S/D, wells |
| $BF_2$ | $49$ | — | Ultra-shallow junctions |
| $In$ | $115$ | $160$ | Halo implants |

**7.4 Implantation Physics**

**Ion Energy:**
$$
E = qV_{acc}
$$

Typical range: $0.2 	ext{ keV} - 3 	ext{ MeV}$

**Dose:**
$$
Phi = frac{I_{beam} cdot t}{q cdot A}
$$

Where:
- $Phi$ = dose (ions/cm²), typical: $10^{11} - 10^{16}$
- $I_{beam}$ = beam current
- $t$ = implant time
- $A$ = implanted area

**Beam Current Requirements:**
- High dose (S/D): $1-20 	ext{ mA}$
- Medium dose (wells): $100 	ext{ μA} - 1 	ext{ mA}$
- Low dose (threshold adjust): $1-100 	ext{ μA}$

**7.5 Depth Distribution**

**Gaussian Profile (First Order):**
$$
N(x) = frac{Phi}{sqrt{2pi} cdot Delta R_p} cdot expleft[-frac{(x - R_p)^2}{2(Delta R_p)^2}
ight]
$$

Where:
- $R_p$ = projected range (mean depth)
- $Delta R_p$ = straggle (standard deviation)

**Peak Concentration:**
$$
N_{peak} = frac{Phi}{sqrt{2pi} cdot Delta R_p} approx frac{0.4 cdot Phi}{Delta R_p}
$$

**7.6 Range Tables (in Silicon)**

| Ion | Energy (keV) | $R_p$ (nm) | $Delta R_p$ (nm) |
|-----|--------------|------------|-------------------|
| $B$ | $10$ | $35$ | $15$ |
| $B$ | $50$ | $160$ | $55$ |
| $P$ | $30$ | $40$ | $15$ |
| $P$ | $100$ | $120$ | $45$ |
| $As$ | $50$ | $35$ | $12$ |
| $As$ | $150$ | $95$ | $35$ |

**7.7 Channeling**

When ions align with crystal axes, they penetrate deeper (channeling).

**Prevention Methods:**
- Tilt wafer $7°$ off-axis
- Rotate wafer during implant
- Pre-amorphization implant (PAI)
- Screen oxide

**7.8 Implant Damage**

**Damage Density:**
$$
N_{damage} propto Phi cdot frac{dE}{dx}_{nuclear}
$$

**Amorphization Threshold:**
- Si becomes amorphous above critical dose
- For As at RT: $Phi_{crit} approx 10^{14} 	ext{ cm}^{-2}$

---

**Step 8: Rapid Thermal Processing (RTP)**

**8.1 Purpose**

- **Dopant Activation**: Move implanted atoms to substitutional sites
- **Damage Annealing**: Repair crystal damage from implantation
- **Silicidation**: Form metal silicides for contacts

**8.2 RTP Methods**

| Method | Temperature | Time | Application |
|--------|-------------|------|-------------|
| Furnace Anneal | $800-1100°C$ | $30-60$ min | Diffusion, oxidation |
| Spike RTA | $1000-1100°C$ | $1-5$ s | Dopant activation |
| Flash Anneal | $1100-1350°C$ | $1-10$ ms | USJ activation |
| Laser Anneal | $>1300°C$ | $100$ ns - $1$ μs | Surface activation |

**8.3 Dopant Activation**

**Electrical Activation:**
$$
n_{active} = N_d cdot left(1 - expleft(-frac{t}{	au}
ight)
ight)
$$

Where $	au$ = activation time constant

**Solid Solubility Limit:**
Maximum electrically active concentration at given temperature.

| Dopant | Solubility at $1000°C$ (cm⁻³) |
|--------|-------------------------------|
| $B$ | $2 	imes 10^{20}$ |
| $P$ | $1.2 	imes 10^{21}$ |
| $As$ | $1.5 	imes 10^{21}$ |

**8.4 Diffusion During Annealing**

**Fick's Second Law:**
$$
frac{partial C}{partial t} = D cdot frac{partial^2 C}{partial x^2}
$$

**Diffusion Coefficient:**
$$
D = D_0 cdot expleft(-frac{E_a}{k_B T}
ight)
$$

**Diffusion Length:**
$$
L_D = 2sqrt{D cdot t}
$$

**8.5 Transient Enhanced Diffusion (TED)**

Implant damage creates excess interstitials that enhance diffusion:

$$
D_{TED} = D_{intrinsic} cdot left(1 + frac{C_I}{C_I^*}
ight)
$$

Where:
- $C_I$ = interstitial concentration
- $C_I^*$ = equilibrium interstitial concentration

**TED Mitigation:**
- Low-temperature annealing first
- Carbon co-implantation
- Millisecond annealing

**8.6 Silicidation**

**Self-Aligned Silicide (Salicide) Process:**

$$
M + Si xrightarrow{Delta} M_xSi_y
$$

| Silicide | Formation Temp | Resistivity (μΩ·cm) | Consumption Ratio |
|----------|----------------|---------------------|-------------------|
| $TiSi_2$ | $700-850°C$ | $13-20$ | 2.27 nm Si/nm Ti |
| $CoSi_2$ | $600-800°C$ | $15-20$ | 3.64 nm Si/nm Co |
| $NiSi$ | $400-600°C$ | $15-20$ | 1.83 nm Si/nm Ni |

**Modern Choice: NiSi**
- Lower formation temperature
- Less silicon consumption
- Compatible with SiGe

---

# BACK-END-OF-LINE (BEOL)

**Step 9: Deposition (CVD / ALD) — ILD, Tungsten Plugs**

**9.1 Inter-Layer Dielectric (ILD)**

**Purpose:**
- Electrical isolation between metal layers
- Planarization base
- Capacitance control

**ILD Materials Evolution:**

| Generation | Material | $kappa$ | Application |
|------------|----------|----------|-------------|
| Al era | $SiO_2$ | $4.0$ | 0.25 μm+ |
| Early Cu | FSG ($SiO_xF_y$) | $3.5$ | 180-130 nm |
| Low-κ | SiCOH | $2.7-3.0$ | 90-45 nm |
| ULK | Porous SiCOH | $2.2-2.5$ | 32 nm+ |
| Air gap | Air/$SiO_2$ | $< 2.0$ | 14 nm+ |

**9.2 CVD Oxide Processes**

**PECVD TEOS:**
$$
Si(OC_2H_5)_4 + O_2 xrightarrow{plasma} SiO_2 + 	ext{byproducts}
$$

**SACVD TEOS/Ozone:**
$$
Si(OC_2H_5)_4 + O_3 xrightarrow{400°C} SiO_2 + 	ext{byproducts}
$$

**9.3 ALD (Atomic Layer Deposition)**

**Characteristics:**
- Self-limiting surface reactions
- Atomic-level thickness control
- Excellent conformality (100%)
- Essential for advanced nodes

**Growth Per Cycle (GPC):**
$$
GPC approx 0.5-2 	ext{ Å/cycle}
$$

**ALD $Al_2O_3$ Example:**
```
Cycle:
1. TMA pulse: Al(CH₃)₃ + surface-OH → surface-O-Al(CH₃)₂ + CH₄
2. Purge
3. H₂O pulse: surface-O-Al(CH₃)₂ + H₂O → surface-O-Al-OH + CH₄
4. Purge
→ Repeat
```

**ALD $HfO_2$ (High-κ Gate):**
- Precursor: $Hf(N(CH_3)_2)_4$ (TDMAH) or $HfCl_4$
- Oxidant: $H_2O$ or $O_3$
- Temperature: $250-350°C$
- GPC: $sim 1 	ext{ Å/cycle}$

**9.4 Tungsten CVD (Contact Plugs)**

**Nucleation Layer:**
$$
WF_6 + SiH_4 
ightarrow W + SiF_4 + 3H_2
$$

**Bulk Fill:**
$$
WF_6 + 3H_2 xrightarrow{300-450°C} W + 6HF
$$

**Process Parameters:**
- Temperature: $400-450°C$
- Pressure: $30-90 	ext{ Torr}$
- Deposition rate: $100-400 	ext{ nm/min}$
- Resistivity: $8-15 	ext{ μΩ·cm}$

**9.5 Etch Stop Layers**

**Silicon Carbide ($SiC$) / Nitrogen-doped $SiC$:**
$$
	ext{Precursor: } (CH_3)_3SiH 	ext{ (Trimethylsilane)}
$$

- $kappa approx 4-5$
- Provides etch selectivity to oxide
- Acts as Cu diffusion barrier

---

**Step 10: Deposition (PVD) — Barriers, Seed Layers**

**10.1 PVD Sputtering Fundamentals**

**Sputter Yield:**
$$
Y = frac{	ext{Target atoms ejected}}{	ext{Incident ion}}
$$

| Target | Yield (Ar⁺ at 500 eV) |
|--------|----------------------|
| Al | 1.2 |
| Cu | 2.3 |
| Ti | 0.6 |
| Ta | 0.6 |
| W | 0.6 |

**10.2 Barrier Layers**

**Purpose:**
- Prevent Cu diffusion into dielectric
- Promote adhesion
- Provide nucleation for seed layer

**TaN/Ta Bilayer (Standard):**
- TaN: Cu diffusion barrier, $
ho approx 200 	ext{ μΩ·cm}$
- Ta: Adhesion/nucleation, $
ho approx 15 	ext{ μΩ·cm}$
- Total thickness: $3-10 	ext{ nm}$

**Advanced Barriers:**
- TiN: Compatible with W plugs
- Ru: Enables direct Cu plating
- Co: Next-generation contacts

**10.3 PVD Methods**

**DC Magnetron Sputtering:**
- For conductive targets (Ta, Ti, Cu)
- High deposition rates

**RF Magnetron Sputtering:**
- For insulating targets
- Lower rates

**Ionized PVD (iPVD):**
- High ion fraction for improved step coverage
- Essential for high aspect ratio features

**Collimated PVD:**
- Physical collimator for directionality
- Reduced deposition rate

**10.4 Copper Seed Layer**

**Requirements:**
- Continuous coverage (no voids)
- Thickness: $20-80 	ext{ nm}$
- Good adhesion to barrier
- Uniform grain structure

**Deposition:**
$$
	ext{Ar}^+ + 	ext{Cu}_{	ext{target}} 
ightarrow 	ext{Cu}_{	ext{atoms}} 
ightarrow 	ext{Cu}_{	ext{film}}
$$

**Step Coverage Challenge:**
$$
	ext{Step Coverage} = frac{t_{sidewall}}{t_{field}} 	imes 100\%
$$

For trenches with $AR > 3$, iPVD is required.

---

**Step 11: Electroplating (ECP) — Copper Fill**

**11.1 Electrochemical Fundamentals**

**Copper Reduction:**
$$
Cu^{2+} + 2e^- 
ightarrow Cu
$$

**Faraday's Law:**
$$
m = frac{I cdot t cdot M}{n cdot F}
$$

Where:
- $m$ = mass deposited
- $I$ = current
- $t$ = time
- $M$ = molar mass ($63.5 	ext{ g/mol}$ for Cu)
- $n$ = electrons transferred ($2$ for Cu)
- $F$ = Faraday constant ($96,485 	ext{ C/mol}$)

**Deposition Rate:**
$$
R = frac{I cdot M}{n cdot F cdot 
ho cdot A}
$$

**11.2 Superfilling (Bottom-Up Fill)**

**Additives Enable Void-Free Fill:**

| Additive Type | Function | Example |
|---------------|----------|---------|
| Accelerator | Promotes deposition at bottom | SPS (bis-3-sulfopropyl disulfide) |
| Suppressor | Inhibits deposition at top | PEG (polyethylene glycol) |
| Leveler | Controls shape | JGB (Janus Green B) |

**Superfilling Mechanism:**
1. Suppressor adsorbs on all surfaces
2. Accelerator concentrates at feature bottom
3. As feature fills, accelerator becomes more concentrated
4. Bottom-up fill achieved

**11.3 ECP Process Parameters**

| Parameter | Value |
|-----------|-------|
| Electrolyte | $CuSO_4$ (0.25-1.0 M) + $H_2SO_4$ |
| Temperature | $20-25°C$ |
| Current Density | $5-60 	ext{ mA/cm}^2$ |
| Deposition Rate | $100-600 	ext{ nm/min}$ |
| Bath pH | $< 1$ |

**11.4 Damascene Process**

**Single Damascene:**
1. Deposit ILD
2. Pattern and etch trenches
3. Deposit barrier (PVD TaN/Ta)
4. Deposit seed (PVD Cu)
5. Electroplate Cu
6. CMP to planarize

**Dual Damascene:**
1. Deposit ILD stack
2. Pattern and etch vias
3. Pattern and etch trenches
4. Single barrier + seed + plate step
5. CMP
- More efficient (fewer steps)
- Via-first or trench-first approaches

**11.5 Overburden Requirements**

$$
t_{overburden} = t_{trench} + t_{margin}
$$

Typical: $300-1000 	ext{ nm}$ over field

---

**Step 12: Chemical Mechanical Polishing (CMP)**

**12.1 Preston Equation**

$$
MRR = K_p cdot P cdot V
$$

Where:
- $MRR$ = Material Removal Rate (nm/min)
- $K_p$ = Preston coefficient
- $P$ = down pressure
- $V$ = relative velocity

**12.2 CMP Components**

**Slurry Composition:**

| Component | Function | Example |
|-----------|----------|---------|
| Abrasive | Mechanical removal | $SiO_2$, $Al_2O_3$, $CeO_2$ |
| Oxidizer | Chemical modification | $H_2O_2$, $KIO_3$ |
| Complexing agent | Metal dissolution | Glycine, citric acid |
| Surfactant | Particle dispersion | Various |
| Corrosion inhibitor | Protect Cu | BTA (benzotriazole) |

**Abrasive Particle Size:**
$$
d_{particle} = 20-200 	ext{ nm}
$$

**12.3 CMP Process Parameters**

| Parameter | Cu CMP | Oxide CMP | W CMP |
|-----------|--------|-----------|-------|
| Pressure | $1-3 	ext{ psi}$ | $3-7 	ext{ psi}$ | $3-5 	ext{ psi}$ |
| Platen speed | $50-100 	ext{ rpm}$ | $50-100 	ext{ rpm}$ | $50-100 	ext{ rpm}$ |
| Slurry flow | $150-300 	ext{ mL/min}$ | $150-300 	ext{ mL/min}$ | $150-300 	ext{ mL/min}$ |
| Removal rate | $300-800 	ext{ nm/min}$ | $100-300 	ext{ nm/min}$ | $200-400 	ext{ nm/min}$ |

**12.4 Planarization Metrics**

**Within-Wafer Non-Uniformity (WIWNU):**
$$
WIWNU = frac{sigma}{mean} 	imes 100\%
$$

Target: $< 3\%$

**Dishing (Cu):**
$$
D_{dish} = t_{field} - t_{trench}
$$

Occurs because Cu polishes faster than barrier.

**Erosion (Dielectric):**
$$
E_{erosion} = t_{oxide,initial} - t_{oxide,final}
$$

Occurs in dense pattern areas.

**12.5 Multi-Step Cu CMP**

**Step 1 (Bulk Cu removal):**
- High rate slurry
- Remove overburden
- Stop on barrier

**Step 2 (Barrier removal):**
- Different chemistry
- Remove TaN/Ta
- Stop on oxide

**Step 3 (Buff/clean):**
- Low pressure
- Remove residues
- Final surface preparation

---

# TESTING & ASSEMBLY

**Step 13: Wafer Probe Test (EDS)**

**13.1 Purpose**

- Test every die on wafer before dicing
- Identify defective dies (ink marking)
- Characterize process performance
- Bin dies by speed grade

**13.2 Test Types**

**Parametric Testing:**
- Threshold voltage: $V_{th}$
- Drive current: $I_{on}$
- Leakage current: $I_{off}$
- Contact resistance: $R_c$
- Sheet resistance: $R_s$

**Functional Testing:**
- Memory BIST (Built-In Self-Test)
- Logic pattern testing
- At-speed testing

**13.3 Key Device Equations**

**MOSFET On-Current (Saturation):**
$$
I_{DS,sat} = frac{W}{L} cdot mu cdot C_{ox} cdot frac{(V_{GS} - V_{th})^2}{2} cdot (1 + lambda V_{DS})
$$

**Subthreshold Current:**
$$
I_{sub} = I_0 cdot expleft(frac{V_{GS} - V_{th}}{n cdot V_T}
ight) cdot left(1 - expleft(frac{-V_{DS}}{V_T}
ight)
ight)
$$

**Subthreshold Swing:**
$$
SS = n cdot frac{k_B T}{q} cdot ln(10) approx 60 	ext{ mV/dec} 	imes n quad @ quad 300K
$$

Ideal: $SS = 60 	ext{ mV/dec}$ ($n = 1$)

**On/Off Ratio:**
$$
frac{I_{on}}{I_{off}} > 10^6
$$

**13.4 Yield Models**

**Poisson Model:**
$$
Y = e^{-D_0 cdot A}
$$

**Murphy's Model:**
$$
Y = left(frac{1 - e^{-D_0 A}}{D_0 A}
ight)^2
$$

**Negative Binomial Model:**
$$
Y = left(1 + frac{D_0 A}{alpha}
ight)^{-alpha}
$$

Where:
- $Y$ = yield
- $D_0$ = defect density (defects/cm²)
- $A$ = die area
- $alpha$ = clustering parameter

**13.5 Speed Binning**

Dies sorted into performance grades:
- Bin 1: Highest speed (premium)
- Bin 2: Standard speed
- Bin 3: Lower speed (budget)
- Fail: Defective

---

**Step 14: Backgrinding & Dicing**

**14.1 Wafer Thinning (Backgrinding)**

**Purpose:**
- Reduce package height
- Improve thermal dissipation
- Enable TSV reveal
- Required for stacking

**Final Thickness:**
| Application | Thickness |
|-------------|-----------|
| Standard | $200-300 	ext{ μm}$ |
| Thin packages | $50-100 	ext{ μm}$ |
| 3D stacking | $20-50 	ext{ μm}$ |

**Process:**
1. Mount wafer face-down on tape/carrier
2. Coarse grind (diamond wheel)
3. Fine grind
4. Stress relief (CMP or dry polish)
5. Optional: Backside metallization

**14.2 Dicing Methods**

**Blade Dicing:**
- Diamond-coated blade
- Kerf width: $20-50 	ext{ μm}$
- Speed: $10-100 	ext{ mm/s}$
- Standard method

**Laser Dicing:**
- Ablation or stealth dicing
- Kerf width: $< 10 	ext{ μm}$
- Higher throughput
- Less chipping

**Stealth Dicing (SD):**
- Laser creates internal modification
- Expansion tape breaks wafer
- Zero kerf loss
- Best for thin wafers

**Plasma Dicing:**
- Deep RIE through streets
- Irregular die shapes possible
- No mechanical stress

**14.3 Dies Per Wafer**

**Gross Die Per Wafer:**
$$
GDW = frac{pi D^2}{4 cdot A_{die}} - frac{pi D}{sqrt{2 cdot A_{die}}}
$$

Where:
- $D$ = wafer diameter
- $A_{die}$ = die area (including scribe)

**Example (300mm wafer, 100mm² die):**
$$
GDW = frac{pi 	imes 300^2}{4 	imes 100} - frac{pi 	imes 300}{sqrt{200}} approx 640 	ext{ dies}
$$

---

**Step 15: Die Attach**

**15.1 Methods**

| Method | Material | Temperature | Application |
|--------|----------|-------------|-------------|
| Epoxy | Ag-filled epoxy | $150-175°C$ | Standard |
| Eutectic | Au-Si | $363°C$ | High reliability |
| Solder | SAC305 | $217-227°C$ | Power devices |
| Sintering | Ag paste | $250-300°C$ | High power |

**15.2 Thermal Performance**

**Thermal Resistance:**
$$
R_{th} = frac{t}{k cdot A}
$$

Where:
- $t$ = bond line thickness (BLT)
- $k$ = thermal conductivity
- $A$ = die area

| Material | $k$ (W/m·K) |
|----------|-------------|
| Ag-filled epoxy | $2-25$ |
| SAC solder | $60$ |
| Au-Si eutectic | $27$ |
| Sintered Ag | $200-250$ |

**15.3 Die Attach Requirements**

- **BLT uniformity**: $pm 5 	ext{ μm}$
- **Void content**: $< 5\%$ (power devices)
- **Die tilt**: $< 1°$
- **Placement accuracy**: $pm 25 	ext{ μm}$

---

**Step 16: Wire Bonding / Flip Chip**

**16.1 Wire Bonding**

**Wire Materials:**

| Material | Diameter | Resistivity | Application |
|----------|----------|-------------|-------------|
| Au | $15-50 	ext{ μm}$ | $2.2 	ext{ μΩ·cm}$ | Premium, RF |
| Cu | $15-50 	ext{ μm}$ | $1.7 	ext{ μΩ·cm}$ | Cost-effective |
| Ag | $15-25 	ext{ μm}$ | $1.6 	ext{ μΩ·cm}$ | LED, power |
| Al | $25-500 	ext{ μm}$ | $2.7 	ext{ μΩ·cm}$ | Power, ribbon |

**Thermosonic Ball Bonding:**
- Temperature: $150-220°C$
- Ultrasonic frequency: $60-140 	ext{ kHz}$
- Bond force: $15-100 	ext{ gf}$
- Bond time: $5-20 	ext{ ms}$

**Wire Resistance:**
$$
R_{wire} = 
ho cdot frac{L}{pi r^2}
$$

**16.2 Flip Chip**

**Advantages over Wire Bonding:**
- Higher I/O density
- Lower inductance
- Better thermal path
- Higher frequency capability

**Bump Types:**

| Type | Pitch | Material | Application |
|------|-------|----------|-------------|
| C4 (Controlled Collapse Chip Connection) | $150-250 	ext{ μm}$ | Pb-Sn, SAC | Standard |
| Cu pillar | $40-100 	ext{ μm}$ | Cu + solder cap | Fine pitch |
| Micro-bump | $10-40 	ext{ μm}$ | Cu + SnAg | 2.5D/3D |

**Bump Height:**
$$
h_{bump} approx 50-100 	ext{ μm} quad 	ext{(C4)}
$$
$$
h_{pillar} approx 30-50 	ext{ μm} quad 	ext{(Cu pillar)}
$$

**16.3 Underfill**

**Purpose:**
- Distribute thermal stress
- Protect bumps
- Improve reliability

**CTE Matching:**
$$
alpha_{underfill} approx 25-30 	ext{ ppm/°C}
$$

(Between Si at $3 	ext{ ppm/°C}$ and substrate at $17 	ext{ ppm/°C}$)

---

**Step 17: Encapsulation**

**17.1 Mold Compound Properties**

| Property | Value | Unit |
|----------|-------|------|
| Filler content | $70-90$ | wt% ($SiO_2$) |
| CTE ($alpha_1$, below $T_g$) | $8-15$ | ppm/°C |
| CTE ($alpha_2$, above $T_g$) | $30-50$ | ppm/°C |
| Glass transition ($T_g$) | $150-175$ | °C |
| Thermal conductivity | $0.7-3$ | W/m·K |
| Flexural modulus | $15-25$ | GPa |
| Moisture absorption | $< 0.3$ | wt% |

**17.2 Transfer Molding Process**

**Parameters:**
- Mold temperature: $175-185°C$
- Transfer pressure: $5-10 	ext{ MPa}$
- Transfer time: $10-20 	ext{ s}$
- Cure time: $60-120 	ext{ s}$
- Post-mold cure: $4-8 	ext{ hrs}$ at $175°C$

**Cure Kinetics (Kamal Model):**
$$
frac{dalpha}{dt} = (k_1 + k_2 alpha^m)(1-alpha)^n
$$

Where:
- $alpha$ = degree of cure (0 to 1)
- $k_1, k_2$ = rate constants
- $m, n$ = reaction orders

**17.3 Package Types**

**Traditional:**
- DIP (Dual In-line Package)
- QFP (Quad Flat Package)
- QFN (Quad Flat No-lead)
- BGA (Ball Grid Array)

**Advanced:**
- WLCSP (Wafer Level Chip Scale Package)
- FCBGA (Flip Chip BGA)
- SiP (System in Package)
- 2.5D/3D IC

---

**Step 18: Final Test → Packing & Ship**

**18.1 Final Test**

**Test Levels:**
- **Hot Test**: $85-125°C$
- **Cold Test**: $-40$ to $0°C$
- **Room Temp Test**: $25°C$

**Burn-In:**
- Temperature: $125-150°C$
- Voltage: $V_{DD} + 10\%$
- Duration: $24-168 	ext{ hrs}$
- Accelerates infant mortality failures

**Acceleration Factor (Arrhenius):**
$$
AF = expleft[frac{E_a}{k_B}left(frac{1}{T_{use}} - frac{1}{T_{stress}}
ight)
ight]
$$

Where $E_a approx 0.7 	ext{ eV}$ (typical)

**18.2 Quality Metrics**

**DPPM (Defective Parts Per Million):**
$$
DPPM = frac{	ext{Failures}}{	ext{Units Shipped}} 	imes 10^6
$$

| Market | DPPM Target |
|--------|-------------|
| Consumer | $< 500$ |
| Industrial | $< 100$ |
| Automotive | $< 10$ |
| Medical | $< 1$ |

**18.3 Reliability Testing**

**Electromigration (Black's Equation):**
$$
MTTF = A cdot J^{-n} cdot expleft(frac{E_a}{k_B T}
ight)
$$

Where:
- $J$ = current density ($	ext{MA/cm}^2$)
- $n approx 2$ (current exponent)
- $E_a approx 0.7-0.9 	ext{ eV}$ (Cu)

**Current Density Limit:**
$$
J_{max} approx 1-2 	ext{ MA/cm}^2 quad 	ext{(Cu at 105°C)}
$$

**18.4 Packing & Ship**

**Tape & Reel:**
- Components in carrier tape
- 8mm, 12mm, 16mm tape widths
- Standard reel: 7" or 13"

**Tray Packing:**
- JEDEC standard trays
- For larger packages

**Moisture Sensitivity Level (MSL):**
| MSL | Floor Life | Storage |
|-----|------------|---------|
| 1 | Unlimited | Ambient |
| 2 | 1 year | $< 60\%$ RH |
| 3 | 168 hrs | Dry pack |
| 4 | 72 hrs | Dry pack |
| 5 | 48 hrs | Dry pack |
| 6 | 6 hrs | Dry pack |

---

**Appendix: Technology Scaling**

**Moore's Law**

$$
N_{transistors} = N_0 cdot 2^{t/T_2}
$$

Where $T_2 approx 2 	ext{ years}$ (doubling time)

**Node Naming vs. Physical Dimensions**

| "Node" | Gate Pitch | Metal Pitch | Fin Pitch |
|--------|------------|-------------|-----------|
| 14nm | $70 	ext{ nm}$ | $52 	ext{ nm}$ | $42 	ext{ nm}$ |
| 10nm | $54 	ext{ nm}$ | $36 	ext{ nm}$ | $34 	ext{ nm}$ |
| 7nm | $54 	ext{ nm}$ | $36 	ext{ nm}$ | $30 	ext{ nm}$ |
| 5nm | $48 	ext{ nm}$ | $28 	ext{ nm}$ | $25-30 	ext{ nm}$ |
| 3nm | $48 	ext{ nm}$ | $21 	ext{ nm}$ | GAA |

**Transistor Density**

$$

ho_{transistor} = frac{N_{transistors}}{A_{die}} quad [	ext{MTr/mm}^2]
$$

| Node | Density (MTr/mm²) |
|------|-------------------|
| 14nm | $sim 37$ |
| 10nm | $sim 100$ |
| 7nm | $sim 100$ |
| 5nm | $sim 170$ |
| 3nm | $sim 300$ |

---

**Key Equations Reference**

| Process | Equation |
|---------|----------|
| Oxidation (Deal-Grove) | $x^2 + Ax = B(t + 	au)$ |
| Lithography Resolution | $CD = k_1 cdot frac{lambda}{NA}$ |
| Depth of Focus | $DOF = k_2 cdot frac{lambda}{NA^2}$ |
| Implant Profile | $N(x) = frac{Phi}{sqrt{2pi}Delta R_p}expleft[-frac{(x-R_p)^2}{2Delta R_p^2}
ight]$ |
| Diffusion | $L_D = 2sqrt{Dt}$ |
| CMP (Preston) | $MRR = K_p cdot P cdot V$ |
| Electroplating (Faraday) | $m = frac{ItM}{nF}$ |
| Yield (Poisson) | $Y = e^{-D_0 A}$ |
| Thermal Resistance | $R_{th} = frac{t}{kA}$ |
| Electromigration (Black) | $MTTF = AJ^{-n}e^{E_a/k_BT}$ |

---

*Document Version 2.0 — Corrected and enhanced based on accurate 18-step process flow*
*Formatted for VS Code with KaTeX/LaTeX math support*

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/chip) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
