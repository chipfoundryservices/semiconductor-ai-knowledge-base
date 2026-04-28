# HPC Data Center Power and Cooling: Liquid Cooling and Power Management — energy-efficient facility operation with PUE <1.1 and hot-water-cooled systems minimizing overhead

**Keywords**: hpc power management facility,data center pue,liquid cooling hpc,hot water cooling server,power capping hpc

---

**HPC Data Center Power and Cooling: Liquid Cooling and Power Management — energy-efficient facility operation with PUE <1.1 and hot-water-cooled systems minimizing overhead**

**PUE (Power Usage Effectiveness)**
- **Definition**: PUE = total facility power / IT equipment power, metric for data center efficiency
- **Target**: PUE <1.1 (10% overhead for cooling, power conversion, lighting), state-of-art systems achieve 1.05-1.08
- **Breakdown**: IT equipment 90% (compute ~60%, storage ~20%, network ~10%), overhead (cooling, UPS, lighting) 10%
- **Measurement**: enterprise data centers typically 1.5-2.0 PUE, HPC facility can achieve 1.1 with design optimization
- **Energy Cost Impact**: PUE 2.0 costs 2× electricity bill vs PUE 1.1 (same compute load), incentivizes optimization

**Liquid Cooling for HPC**
- **Air-Cooled Limitation**: air cooling maxes out ~50-100 kW/cabinet (heat transfer limited), air density low (requires high volume)
- **Liquid Cooled Advantage**: water 800× denser than air (excellent heat capacity), enables 500+ kW/cabinet, higher temperature tolerance
- **Direct Liquid Cooling (DLC)**: cold-water pipes routed directly to CPU/GPU (cold-plate attached), minimal air cooling needed
- **Cost**: liquid cooling infrastructure (manifolds, hoses, pumps) ~10-20% facility cost premium, offset by reduced cooling plant size + footprint

**Hot-Water-Cooled Supercomputers**
- **Inlet Water Temperature**: 20°C inlet water (vs standard 15°C), hotter inlet reduces cooling plant load
- **Outlet Temperature**: 50-60°C outlet (vs standard 30-35°C), hot water (not waste) useful for facility heating (office space, domestic hot water)
- **Efficiency Cascade**: hot water at 50°C can heat adjacent buildings (district heating), reuse thermal energy
- **Summit System**: 20°C inlet water, 95% HW cooled (direct liquid cooling on CPUs + GPUs), 90% liquid-cooled facility overall
- **Frontier System**: similar approach, 21 MW IT load with ~50 MW facility power (PUE ~2.4, but includes all facility infrastructure)

**Cooling Plant Efficiency**
- **Chiller Efficiency**: coefficient of performance (COP) depends on inlet/outlet temperature difference
- **High Temperature**: COP improves with hotter inlet (20°C vs 15°C = 20% COP improvement), offsets higher ambient
- **Free Cooling**: cooler climates (Finland, Iceland, Norway) enable free air cooling (outdoor air used directly), PUE <1.05 possible
- **Adiabatic Cooling**: hybrid approach (air + evaporative), reduces chiller duty 30-50%

**Power Distribution and Conversion**
- **UPS (Uninterruptible Power Supply)**: battery backup during power outage, continuous power ensures graceful shutdown
- **UPS Efficiency**: 85-95% (loss from inverter, battery charging), adds 5-15% facility overhead
- **PDU (Power Distribution Unit)**: distributes power to racks, metered PDU enables per-rack power monitoring
- **Power Factor Correction**: PFC circuits improve efficiency (99%+ modern systems), older systems ~90% (induces utility penalties)

**Power Capping for Budget Compliance**
- **Power Budget**: facility may contract 30 MW power (utility limit), hardware adds up to 35 MW (oversubscription assumed)
- **Capping Policy**: dynamically reduce performance (DVFS: dynamic voltage/frequency scaling) if total power approaches limit
- **Per-Node Monitoring**: CPU/GPU power monitored via on-chip sensors (RAPL: running average power limit), daemon enforces policy
- **Trade-off**: capping reduces performance (slower jobs) vs allowing power spike (risk facility shutdown)
- **Granularity**: coarse capping (per-node, 2-5 kW range) vs fine capping (per-core, 100-500 W range)

**Dynamic Voltage/Frequency Scaling (DVFS)**
- **Power Scaling**: dynamic power ∝ V²×f (voltage² × frequency), 10% frequency reduction = 30-40% power reduction
- **Performance Impact**: 10% frequency reduction = 10-12% performance reduction (not linear due to IPC scaling)
- **Energy Efficiency**: optimal frequency depends on workload (CPU-bound benefits from scaling, memory-bound indifferent)
- **Control**: OS-based governor (Linux cpufreq: ondemand, powersave), or hardware-based (RAPL)

**Carbon Footprint of HPC**
- **Frontier**: 21 MW power, 1.1 ExaFLOPS, carbon intensity varies by region (clean energy grid = low emissions)
- **Grid Mix**: US average ~0.9 lbs CO2/kWh, coal ~2 lbs, natural gas ~1 lbs, wind/solar ~0.05 lbs
- **Annual Emissions**: 21 MW × 24 h × 365 days × 0.9 lbs CO2/kWh ≈ 165,000 tons CO2/year (equivalent to 40,000 cars)
- **Green Computing**: data centers shifting to renewable energy (Google, Microsoft sign long-term solar/wind PPAs), HPC centers following
- **Sustainability**: exascale systems justify only with green energy + high utilization

**Cooling Technology Roadmap**
- **Immersion Cooling**: submerge electronics in non-conductive fluid (dielectric liquid), enables higher power density
- **Chip-Level Cooling**: microfluidic channels etched into chip (or interposer), liquid flows through substrate (advanced phase-change opportunities)
- **Phase-Change Cooling**: thermosiphon or vapor-chamber based cooling, exploits latent heat (efficient but complex)
- **Two-Phase Cooling**: boiling of coolant near hot spots (CPUs), condensation in radiator, 5-10× higher heat transfer than single-phase liquid

**Facility Design for HPC**
- **Redundancy**: N+1 cooling (backup chiller, dual power feeds), ensures uptime during maintenance
- **Airflow Management**: hot aisle/cold aisle containment, prevents mixing (reduces cooling load 10-20%)
- **Monitoring**: DCIM (data center infrastructure management) software tracks power, temperature, humidity (enables predictive analytics)
- **Space Efficiency**: co-location of compute + storage (minimize data movement), hierarchical facility layout

**Cost Analysis**
- **Capital**: facility $200-500M (site, building, infrastructure, IT equipment)
- **Operating**: ~$50M annually (electricity, maintenance, staffing)
- **Cooling**: 20-30% of operating budget (dominant cost after electricity in high-efficiency facilities)
- **ROI**: scientific breakthroughs (climate, fusion, materials) justify investment (not monetarily, socially)

**Future**: exascale systems pushing cooling technology limits, post-exascale will require fundamental innovations (efficiency + cooling breakthroughs), AI-driven facility optimization emerging.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/hpc-power-management-facility) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
