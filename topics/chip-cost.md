# Semiconductor Economics: Chip, Wafer, and Fab Costs

**Keywords**: chip cost,wafer cost,fab cost,economics

---

**Semiconductor Economics: Chip, Wafer, and Fab Costs**

**Overview**

Semiconductor economics operates across three interconnected cost levels, each driving the next in a hierarchical structure that determines the final price of every chip.

---

**1. Fab (Fabrication Plant) Cost**

The foundation of semiconductor economics—the capital expenditure required to build and equip a fabrication facility.

**Capital Expenditure Breakdown**

- **Modern leading-edge fabs (3nm/2nm):** $15–25+ billion to construct
- **Historical comparison:**
  - Year 2000: ~$1–2 billion per fab
  - Year 2010: ~$3–5 billion per fab
  - Year 2020: ~$10–15 billion per fab
  - Year 2024+: ~$20–30 billion per fab

**Cost Components**

- **Equipment (70–80% of capital cost):**
  - ASML EUV lithography machines: ~$350–400 million each
  - Deposition tools (CVD, PVD): $5–20 million each
  - Etching systems: $5–15 million each
  - Metrology and inspection: $2–10 million each
  - Ion implantation: $3–8 million each

- **Facility construction (20–30% of capital cost):**
  - Cleanroom (Class 1-10): $3,000–5,000 per square foot
  - Ultra-pure water systems: $100–500 million
  - Vibration isolation foundations
  - Chemical delivery systems
  - HVAC and air filtration

**Depreciation Model**

Fab equipment is typically depreciated over 5–7 years:

$$
\text{Annual Depreciation} = \frac{\text{Fab Capital Cost}}{\text{Depreciation Period}}
$$

**Example:**

$$
\text{Annual Depreciation} = \frac{\$20 \text{ billion}}{5 \text{ years}} = \$4 \text{ billion/year}
$$

---

**2. Wafer Cost**

The cost to process a single silicon wafer (typically 300mm diameter) through hundreds of manufacturing steps.

**Wafer Cost by Process Node**

| Node | Approximate Wafer Cost | Typical Applications |
|------|------------------------|---------------------|
| 3nm | $18,000–$22,000 | Flagship mobile SoCs, high-end GPUs |
| 5nm | $16,000–$18,000 | Premium smartphones, AI accelerators |
| 7nm | $10,000–$12,000 | Gaming consoles, data center CPUs |
| 14nm | $5,000–$7,000 | Mid-range processors, FPGAs |
| 28nm | $3,000–$4,000 | Automotive, WiFi, Bluetooth |
| 65nm | $2,000–$2,500 | MCUs, power management |
| 180nm | $1,000–$1,500 | Analog, sensors, legacy |

**Wafer Cost Formula**

$$
C_{\text{wafer}} = C_{\text{depreciation}} + C_{\text{materials}} + C_{\text{labor}} + C_{\text{utilities}} + C_{\text{overhead}}
$$

Where:

- $C_{\text{depreciation}}$ = Equipment depreciation per wafer
- $C_{\text{materials}}$ = Silicon, photoresists, gases, chemicals, CMP slurries
- $C_{\text{labor}}$ = Engineering and technician costs
- $C_{\text{utilities}}$ = Electricity, ultra-pure water, gases
- $C_{\text{overhead}}$ = Maintenance, yield engineering, facility costs

**Wafer Throughput Economics**

$$
C_{\text{depreciation/wafer}} = \frac{\text{Annual Depreciation}}{\text{Wafers per Year}}
$$

**Example for a $20B fab producing 100,000 wafers/month:**

$$
C_{\text{depreciation/wafer}} = \frac{\$4 \text{ billion/year}}{1.2 \text{ million wafers/year}} \approx \$3,333 \text{ per wafer}
$$

---

**3. Chip (Die) Cost**

The cost per individual chip, derived from wafer economics and manufacturing yield.

**Fundamental Die Cost Equation**

$$
C_{\text{die}} = \frac{C_{\text{wafer}}}{N_{\text{dies}} \times Y}
$$

Where:

- $C_{\text{die}}$ = Cost per good die
- $C_{\text{wafer}}$ = Total wafer processing cost
- $N_{\text{dies}}$ = Number of dies per wafer (gross)
- $Y$ = Yield (fraction of functional dies)

**Dies Per Wafer Calculation**

For a circular wafer with rectangular dies:

$$
N_{\text{dies}} \approx \frac{\pi \times D^2}{4 \times A_{\text{die}}} - \frac{\pi \times D}{\sqrt{2 \times A_{\text{die}}}}
$$

Where:

- $D$ = Wafer diameter (300mm for modern fabs)
- $A_{\text{die}}$ = Die area in mm²

**Simplified approximation:**

$$
N_{\text{dies}} \approx \frac{\pi \times (150)^2}{A_{\text{die}}} \times 0.85
$$

The 0.85 factor accounts for edge losses and scribe lines.

**Dies Per Wafer Examples**

| Die Size (mm²) | Approximate Dies/Wafer | Example Chips |
|----------------|------------------------|---------------|
| 5 | ~12,000 | Small MCUs, sensors |
| 25 | ~2,400 | Bluetooth, WiFi chips |
| 100 | ~600 | Mobile SoCs, mid-range GPUs |
| 300 | ~200 | Desktop CPUs, gaming GPUs |
| 600 | ~90 | Data center GPUs |
| 800 | ~60 | Large AI accelerators (H100) |
| 1,200 | ~35 | Largest monolithic dies |

**Yield Models**

**Murphy's Yield Model**

$$
Y = \left( \frac{1 - e^{-D_0 \times A}}{D_0 \times A} \right)^2
$$

**Poisson Yield Model (simpler)**

$$
Y = e^{-D_0 \times A}
$$

Where:

- $Y$ = Die yield (fraction)
- $D_0$ = Defect density (defects per cm²)
- $A$ = Die area (cm²)

**Typical defect densities:**

- Mature process: $D_0 \approx 0.05–0.1$ defects/cm²
- New process (early): $D_0 \approx 0.3–0.5$ defects/cm²
- New process (ramping): $D_0 \approx 0.1–0.2$ defects/cm²

**Yield Impact Examples**

For a 600mm² die ($A = 6$ cm²):

**Mature process** ($D_0 = 0.1$):

$$
Y = e^{-0.1 \times 6} = e^{-0.6} \approx 0.55 = 55\%
$$

**Early production** ($D_0 = 0.3$):

$$
Y = e^{-0.3 \times 6} = e^{-1.8} \approx 0.17 = 17\%
$$

---

**4. Complete Cost Model**

**Total Manufacturing Cost Per Chip**

$$
C_{\text{total}} = C_{\text{die}} + C_{\text{packaging}} + C_{\text{testing}} + C_{\text{design\_amort}}
$$

Where:

$$
C_{\text{design\_amort}} = \frac{C_{\text{NRE}}}{\text{Total Units Produced}}
$$

- $C_{\text{NRE}}$ = Non-Recurring Engineering costs (design, masks, validation)

**NRE Costs by Node**

| Node | Approximate NRE Cost |
|------|---------------------|
| 3nm | $500M – $1B+ |
| 5nm | $400M – $700M |
| 7nm | $250M – $400M |
| 14nm | $100M – $200M |
| 28nm | $50M – $100M |
| 65nm | $20M – $40M |

**Packaging Costs**

- **Standard wire bond:** $0.10 – $1.00
- **Flip chip BGA:** $2 – $10
- **Advanced fan-out (InFO):** $10 – $50
- **2.5D interposer (CoWoS):** $100 – $400
- **3D stacking:** $200 – $600+

---

**5. Worked Examples**

**Example 1: AI Accelerator Chip**

**Parameters:**

- Node: TSMC 5nm
- Die size: 600mm²
- Wafer cost: $17,000
- Defect density: $D_0 = 0.12$ /cm²

**Calculations:**

**Dies per wafer:**

$$
N_{\text{dies}} = \frac{\pi \times 150^2}{600} \times 0.85 \approx 100 \text{ dies}
$$

**Yield:**

$$
Y = e^{-0.12 \times 6} \approx e^{-0.72} \approx 0.49 = 49\%
$$

**Die cost:**

$$
C_{\text{die}} = \frac{\$17,000}{100 \times 0.49} = \frac{\$17,000}{49} \approx \$347
$$

**Total chip cost:**

$$
C_{\text{total}} = \$347 + \$250_{\text{(CoWoS)}} + \$30_{\text{(test)}} + \$50_{\text{(design)}} \approx \$677
$$

---

**Example 2: IoT Microcontroller**

**Parameters:**

- Node: 40nm
- Die size: 5mm²
- Wafer cost: $3,000
- Defect density: $D_0 = 0.05$ /cm²

**Calculations:**

**Dies per wafer:**

$$
N_{\text{dies}} = \frac{\pi \times 150^2}{5} \times 0.85 \approx 12,000 \text{ dies}
$$

**Yield:**

$$
Y = e^{-0.05 \times 0.05} \approx e^{-0.0025} \approx 0.997 = 99.7\%
$$

**Die cost:**

$$
C_{\text{die}} = \frac{\$3,000}{12,000 \times 0.997} \approx \$0.25
$$

**Total chip cost:**

$$
C_{\text{total}} = \$0.25 + \$0.15_{\text{(pkg)}} + \$0.05_{\text{(test)}} + \$0.05_{\text{(design)}} \approx \$0.50
$$

---

**6. Economic Dynamics**

**Learning Curve Effect**

Manufacturing cost decreases with cumulative volume:

$$
C_n = C_1 \times n^{-b}
$$

Where:

- $C_n$ = Cost at cumulative unit $n$
- $C_1$ = Cost of first unit
- $b$ = Learning exponent (typically 0.1–0.3 for semiconductors)
- Learning rate = $2^{-b}$ (typically 85–95%)

**Economies of Scale**

**Fab utilization impact:**

$$
C_{\text{wafer}}(\text{util}) = \frac{C_{\text{fixed}}}{\text{util}} + C_{\text{variable}}
$$

- At 50% utilization: costs ~1.5× baseline
- At 90% utilization: costs ~1.05× baseline
- At 100% utilization: minimum cost achieved

**Cost Sensitivity Analysis**

**Die cost sensitivity to yield:**

$$
\frac{\partial C_{\text{die}}}{\partial Y} = -\frac{C_{\text{wafer}}}{N_{\text{dies}} \times Y^2}
$$

For large, expensive dies, yield improvements have dramatic cost impacts.

---

**7. Industry Structure Implications**

**Why Only 3 Companies at Leading Edge**

**Minimum efficient scale calculation:**

$$
\text{Revenue Required} = \frac{\text{Annual CapEx} + \text{R\&D}}{\text{Margin}}
$$

$$
\text{Revenue Required} \approx \frac{\$15B + \$5B}{0.40} = \$50B+ \text{ annually}
$$

Only TSMC, Samsung, and Intel can sustain this investment level.

**Foundry Model Economics**

**Fabless company advantage:**

$$
\text{ROI}_{\text{fabless}} = \frac{\text{Chip Revenue} - \text{Foundry Cost} - \text{Design Cost}}{\text{Design Cost}}
$$

**IDM (Integrated Device Manufacturer):**

$$
\text{ROI}_{\text{IDM}} = \frac{\text{Chip Revenue} - \text{Mfg Cost} - \text{Design Cost}}{\text{Fab CapEx} + \text{Design Cost}}
$$

The fabless model eliminates fab capital from the denominator, enabling higher ROI for design-focused companies.

---

**8. Summary Equations**

**Core Formulas Reference**

| Metric | Formula |
|--------|---------|
| Die Cost | $C_{\text{die}} = \frac{C_{\text{wafer}}}{N_{\text{dies}} \times Y}$ |
| Dies per Wafer | $N \approx \frac{\pi r^2}{A_{\text{die}}} \times 0.85$ |
| Poisson Yield | $Y = e^{-D_0 \times A}$ |
| Total Cost | $C_{\text{total}} = C_{\text{die}} + C_{\text{pkg}} + C_{\text{test}} + C_{\text{NRE}}$ |
| Depreciation/Wafer | $C_{\text{dep}} = \frac{\text{CapEx}/t}{\text{WPY}}$ |
| Learning Curve | $C_n = C_1 \times n^{-b}$ |

---

**9. Current Market Dynamics (2024–2025)**

**Key Trends**

- **AI demand:** Consuming 20%+ of advanced node capacity
- **Geopolitical reshoring:** Adding 20–30% cost premium for non-Taiwan fabs
- **EUV bottleneck:** ASML's monopoly constrains expansion
- **Advanced packaging:** Becoming equal cost driver to node shrinks
- **Chiplet economics:** Enabling yield improvement through smaller dies

**Government Subsidies Impact**

- **US CHIPS Act:** $52B in subsidies
- **EU Chips Act:** €43B in public/private investment
- **Effect:** Artificially reducing effective CapEx for new fabs

---

*Document generated: January 2025*
*Data sources: Industry reports, foundry pricing estimates, public financial disclosures*

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/chip-cost) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
