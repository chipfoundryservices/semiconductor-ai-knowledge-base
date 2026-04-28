# Semiconductor Manufacturing Process Cost Modeling

**Keywords**: cost modeling, semiconductor economics, manufacturing cost, wafer cost, die cost, yield economics, fab economics

---

**Semiconductor Manufacturing Process Cost Modeling**

**Overview**

Semiconductor cost modeling quantifies the expenses of fabricating integrated circuits—from raw wafer to tested die. It informs technology roadmap decisions, fab investments, product pricing, and yield improvement prioritization.



**1. Major Cost Components**

**1.1 Capital Equipment (40–50% of Total Cost)**

This dominates leading-edge economics. A modern advanced-node fab costs **$20–30 billion** to construct.

**Key equipment categories and approximate costs:**

- **EUV lithography scanners**: $150–380M each (a fab may need 15–20)
- **DUV immersion scanners**: $50–80M
- **Deposition tools (CVD, PVD, ALD)**: $3–10M each
- **Etch systems**: $3–8M each
- **Ion implanters**: $5–15M
- **Metrology/inspection**: $2–20M per tool
- **CMP systems**: $3–5M

**Capital cost allocation formula:**

$$
\text{Cost per wafer pass} = \frac{\text{Tool cost} \times \text{Depreciation rate}}{\text{Throughput} \times \text{Utilization} \times \text{Uptime} \times \text{Hours/year}}
$$

Where:

- **Depreciation**: Typically 5–7 years
- **Utilization targets**: 85–95% for expensive tools

**1.2 Masks/Reticles**

A complete mask set for a leading-edge process (7nm and below) costs **$10–15 million** or more.

**EUV mask cost drivers:**

- Reflective multilayer blanks (not transmissive glass)
- Defect-free requirements at smaller dimensions
- Complex pellicle technology

**Mask cost per die:**

$$
\text{Mask cost per die} = \frac{\text{Total mask set cost}}{\text{Total production volume}}
$$

**1.3 Materials and Consumables (15–25%)**

- **Process gases**: Silane, ammonia, fluorine chemistries, noble gases
- **Chemicals**: Photoresists (EUV resists are expensive), developers, CMP slurries, cleaning chemistries
- **Substrates**: 300mm wafers ($100–500+ depending on spec)
  - SOI wafers: Higher cost
  - Epitaxial wafers: Additional processing cost
- **Targets/precursors**: For deposition processes

**1.4 Facilities (10–15%)**

- **Cleanroom**: Class 1 or better for critical areas
- **Ultrapure water**: 18.2 MΩ·cm resistivity requirement
- **HVAC and vibration control**: Critical for lithography
- **Power consumption**: 100–150+ MW continuously for leading fabs
- **Waste treatment**: Environmental compliance costs

**1.5 Labor (10–15%)**

Varies significantly by geography:

- Direct fab operators and technicians
- Process and equipment engineers
- Maintenance, quality, and yield engineers



**2. Yield Modeling**

Yield is the most critical variable, converting wafer cost into die cost:

$$
\text{Cost per die} = \frac{\text{Cost per wafer}}{\text{Dies per wafer} \times Y}
$$

Where $Y$ is the yield (fraction of good dies).

**2.1 Yield Models**

**Poisson Model (Random Defects):**

$$
Y = e^{-D_0 \times A}
$$

Where:

- $D_0$ = Defect density (defects/cm²)
- $A$ = Die area (cm²)

**Negative Binomial Model (Clustered Defects):**

$$
Y = \left(1 + \frac{D_0 \times A}{\alpha}\right)^{-\alpha}
$$

Where:

- $\alpha$ = Clustering parameter (higher values approach Poisson)

**Murphy's Model:**

$$
Y = \left(\frac{1 - e^{-D_0 \times A}}{D_0 \times A}\right)^2
$$

**2.2 Yield Components**

- **Random defect yield ($Y_{\text{random}}$)**: Particles, contamination
- **Systematic yield ($Y_{\text{systematic}}$)**: Design-process interactions, hotspots
- **Parametric yield ($Y_{\text{parametric}}$)**: Devices failing electrical specs

**Combined yield:**

$$
Y_{\text{total}} = Y_{\text{random}} \times Y_{\text{systematic}} \times Y_{\text{parametric}}
$$

**2.3 Yield Benchmarks**

- **Mature processes**: 90%+ yields
- **New leading-edge**: Start at 30–50%, ramp over 12–24 months



**3. Dies Per Wafer Calculation**

**Gross dies per wafer (rectangular approximation):**

$$
\text{Dies}_{\text{gross}} = \frac{\pi \times \left(\frac{D}{2}\right)^2}{A_{\text{die}}}
$$

Where:

- $D$ = Wafer diameter (mm)
- $A_{\text{die}}$ = Die area (mm²)

**More accurate formula (accounting for edge loss):**

$$
\text{Dies}_{\text{good}} = \frac{\pi \times D^2}{4 \times A_{\text{die}}} - \frac{\pi \times D}{\sqrt{2 \times A_{\text{die}}}}
$$

**For 300mm wafer:**

- Usable area: ~70,000 mm² (after edge exclusion)



**4. Cost Scaling by Technology Node**

| Node | Wafer Cost (USD) | Key Cost Drivers |
|------|------------------|------------------|
| 28nm | $3,000–4,000 | Mature, high yield |
| 14/16nm | $5,000–7,000 | FinFET transition |
| 7nm | $9,000–12,000 | EUV introduction (limited layers) |
| 5nm | $15,000–17,000 | More EUV layers |
| 3nm | $18,000–22,000 | GAA transistors, high EUV count |
| 2nm | $25,000+ | Backside power, nanosheet complexity |

**4.1 Cost Per Transistor Trend**

**Historical Moore's Law economics:**

$$
\text{Cost reduction per node} \approx 30\%
$$

**Current reality (sub-7nm):**

$$
\text{Cost reduction per node} \approx 10\text{–}20\%
$$



**5. Worked Example**

**5.1 Assumptions**

- **Wafer size**: 300mm
- **Wafer cost**: $15,000 (all-in manufacturing cost)
- **Die size**: 100 mm²
- **Usable wafer area**: ~70,000 mm²
- **Gross dies per wafer**: ~680 (including partial dies)
- **Good dies per wafer**: ~600 (after edge loss)
- **Yield**: 85%

**5.2 Calculation**

**Good dies:**

$$
\text{Good dies} = 600 \times 0.85 = 510
$$

**Cost per die:**

$$
  ext{Cost per die} = \frac{15{,}000}{510} \approx 29.41\ \text{USD}
$$

**5.3 Yield Sensitivity Analysis**

| Yield | Good Dies | Cost per Die |
|-------|-----------|--------------|
| 95% | 570 | $26.32 |
| 85% | 510 | $29.41 |
| 75% | 450 | $33.33 |
| 60% | 360 | $41.67 |
| 50% | 300 | $50.00 |

**Impact:** A 25-point yield drop (85% → 60%) increases unit cost by **42%**.



**6. Geographic Cost Variations**

| Factor | Taiwan/Korea | US | Europe | China |
|--------|-------------|-----|--------|-------|
| Labor | Moderate | High | High | Low |
| Power | Low-moderate | Varies | High | Low |
| Incentives | Moderate | High (CHIPS Act) | High | Very high |
| Supply chain | Dense | Developing | Limited | Developing |

**US cost premium:**

$$
\text{Premium}_{\text{US}} \approx 20\text{–}40\%
$$



**7. Advanced Packaging Economics**

**7.1 Packaging Options**

- **Interposers**: Silicon (expensive) vs. organic (cheaper)
- **Bonding**: Hybrid bonding enables fine pitch but has yield challenges
- **Technologies**: CoWoS, InFO, EMIB (each with different cost structures)

**7.2 Compound Yield**

For chiplet architectures with $N$ dies:

$$
Y_{\text{package}} = \prod_{i=1}^{N} Y_i
$$

**Example (N = 4 chiplets, each 95% yield):**

$$
Y_{\text{package}} = 0.95^4 = 0.814 = 81.4\%
$$



**8. Cost Modeling Methodologies**

**8.1 Activity-Based Costing (ABC)**

Maps costs to specific process operations, then aggregates:

$$
\text{Total Cost} = \sum_{i=1}^{n} (\text{Activity}_i \times \text{Cost Driver}_i)
$$

**8.2 Process-Based Cost Modeling (PBCM)**

Links technical parameters to equipment requirements:

$$
\text{Cost} = f(\text{deposition rate}, \text{etch selectivity}, \text{throughput}, ...)
$$

**8.3 Learning Curve Model**

Cost reduction with cumulative production:

$$
C_n = C_1 \times n^{-b}
$$

Where:

- $C_n$ = Cost of the $n$-th unit
- $C_1$ = Cost of the first unit
- $b$ = Learning exponent (typically 0.1–0.3 for semiconductors)



**9. Key Cost Metrics Summary**

| Metric | Formula |
|--------|---------|
| Cost per Wafer | $\sum \text{(CapEx + OpEx + Materials + Labor + Facilities)}$ |
| Cost per Die | $\frac{\text{Cost per Wafer}}{\text{Dies per Wafer} \times \text{Yield}}$ |
| Cost per Transistor | $\frac{\text{Cost per Die}}{\text{Transistors per Die}}$ |
| Cost per mm² | $\frac{\text{Cost per Wafer}}{\text{Usable Wafer Area} \times \text{Yield}}$ |



**10. Current Industry Trends**

1. **EUV cost trajectory**: More EUV layers per node; High-NA EUV (\$350M+ per tool) arriving for 2nm
2. **Sustainability costs**: Carbon neutrality requirements, water recycling mandates
3. **Supply chain reshoring**: Government subsidies changing cost calculus
4. **3D integration**: Shifts cost from transistor scaling to packaging
5. **Mature node scarcity**: 28nm–65nm capacity tightening, prices rising



**Reference Formulas**

**Yield Models**

```
Poisson:           Y = exp(-D₀ × A)
Negative Binomial: Y = (1 + D₀×A/α)^(-α)
Murphy:            Y = ((1 - exp(-D₀×A)) / (D₀×A))²
```

**Cost Equations**

```
Cost/Die     = Cost/Wafer ÷ (Dies/Wafer × Yield)
Cost/Wafer   = CapEx + Materials + Labor + Facilities + Overhead
CapEx/Pass   = (Tool Cost × Depreciation) ÷ (Throughput × Util × Uptime × Hours)
```

**Dies Per Wafer**

```
Gross Dies ≈ π × (D/2)² ÷ A_die
Net Dies   ≈ (π × D²)/(4 × A_die) - (π × D)/√(2 × A_die)
```

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/cost-modeling) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
