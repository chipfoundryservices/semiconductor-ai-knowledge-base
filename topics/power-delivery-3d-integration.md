# Power Delivery in 3D Integration

**Keywords**: power delivery 3d integration,power distribution network 3d,ir drop 3d stacks,decoupling capacitor placement,power grid design 3d

---

**Power Delivery in 3D Integration** is **the critical challenge of distributing clean, stable power to stacked dies through vertical interconnects — managing IR drop (<5% of supply voltage), minimizing power supply noise (<50 mV), providing sufficient decoupling capacitance (1-10 nF per mA of switching current), and delivering 10-100 A currents through thousands of micro-bumps or TSVs while maintaining power integrity across multiple voltage domains**.

**Power Distribution Network (PDN) Architecture:**
- **Vertical Power Delivery**: power supplied from package through bottom die; TSVs or micro-bumps carry power to upper dies; each interface adds resistance (10-50 mΩ per connection); total PDN resistance 50-200 mΩ for 4-die stack
- **Horizontal Power Distribution**: on-die power grid distributes power across each die; metal layers (M1-M8) form mesh or tree structure; grid resistance 10-50 mΩ depending on metal thickness and width
- **Backside Power Delivery**: Intel PowerVia and imec backside PDN deliver power through wafer backside; eliminates front-side power routing; reduces IR drop by 30-50%; frees front-side metals for signals
- **Hybrid PDN**: combines vertical TSVs (coarse power delivery) with on-die grids (fine distribution); optimizes area, resistance, and routing congestion

**IR Drop Analysis:**
- **Voltage Drop Budget**: total IR drop = I × R_PDN; for 10 A current and 100 mΩ resistance, IR drop = 1 V; specification typically <5% of supply voltage (50 mV for 1.0 V supply)
- **Static IR Drop**: DC voltage drop due to average current; calculated using DC resistance of PDN; worst-case analysis assumes all circuits switching simultaneously (unrealistic but conservative)
- **Dynamic IR Drop**: transient voltage drop due to current surges; L·di/dt component dominates at high frequencies; requires AC impedance analysis of PDN including inductance
- **IR Drop Mitigation**: increase metal width (reduces resistance), add more TSVs/bumps (parallel resistance), use thicker metals (M8-M9 for power), implement backside power delivery

**Power Supply Noise:**
- **Simultaneous Switching Noise (SSN)**: large number of circuits switching simultaneously causes current surge; L·di/dt voltage drop on power supply; noise amplitude 50-200 mV for poorly designed PDN
- **Resonance**: PDN has resonant frequency f_res = 1/(2π√(L·C)) where L is inductance and C is capacitance; resonance amplifies noise at specific frequencies; typical f_res 100 MHz - 1 GHz
- **Noise Specification**: power supply noise <50 mV (5% of 1.0 V supply) for reliable operation; >100 mV noise causes timing failures and functional errors
- **Noise Reduction**: increase decoupling capacitance (lowers impedance), reduce PDN inductance (shorter current loops), spread switching events in time (reduces di/dt)

**Decoupling Capacitors:**
- **On-Die Capacitance**: MOS capacitors (NMOS in n-well) or MIM capacitors provide 1-10 nF/mm²; placed near high-power blocks; response time <1 ns; effective for high-frequency noise (>100 MHz)
- **Package Capacitance**: ceramic capacitors (0.1-10 μF) mounted on package substrate; response time 1-10 ns; effective for mid-frequency noise (10-100 MHz); ESR 1-10 mΩ, ESL 100-500 pH
- **Board Capacitance**: bulk capacitors (10-1000 μF) on PCB; response time 10-100 ns; effective for low-frequency noise (<10 MHz); ESR 10-100 mΩ, ESL 1-5 nH
- **Capacitor Placement**: hierarchical placement at multiple levels; on-die caps for high-frequency, package caps for mid-frequency, board caps for low-frequency; total capacitance 1-10 nF per mA of switching current

**TSV and Micro-Bump Power Delivery:**
- **Current Capacity**: single TSV or micro-bump carries 0.1-0.5 A limited by electromigration; current density <10⁴ A/cm² for 10-year lifetime at 100°C
- **Power TSV/Bump Count**: 10 A total current requires 20-100 power TSVs/bumps; typically 30-50% of total TSVs/bumps allocated to power and ground; remaining for signals
- **Resistance**: TSV resistance 10-50 mΩ, micro-bump resistance 20-50 mΩ; parallel connection of N TSVs/bumps reduces resistance by N×; 100 power TSVs achieve 0.1-0.5 mΩ total resistance
- **Inductance**: TSV inductance 10-50 pH, micro-bump inductance 10-50 pH; parallel connection reduces inductance by N×; low inductance critical for high-frequency power integrity

**Voltage Domains:**
- **Multiple Voltage Domains**: different dies or blocks operate at different voltages (0.7-1.8 V); requires separate power distribution networks; increases PDN complexity and area
- **Voltage Regulators**: on-die or in-package voltage regulators convert package voltage to die voltage; reduces IR drop by placing regulator close to load; enables fine-grained voltage control
- **Power Gating**: unused blocks powered down to save energy; requires power switches (large transistors) and isolation cells; reduces average power by 30-70% but adds area and complexity
- **Dynamic Voltage and Frequency Scaling (DVFS)**: adjust voltage and frequency based on workload; reduces power during low-activity periods; requires fast voltage regulators (<1 μs response time)

**3D-Specific Challenges:**
- **Uneven Power Distribution**: bottom die has best power delivery (closest to package); top die has worst (farthest from package); IR drop varies 2-5× across dies; requires per-die power optimization
- **Thermal-Power Coupling**: high temperature increases resistance (Cu resistance increases 0.4%/°C); increased resistance causes more IR drop and heating; positive feedback loop requires careful design
- **Inter-Die Power Coupling**: switching in one die causes noise in other dies through shared PDN; requires isolation between dies or careful synchronization of switching events
- **Test and Debug**: measuring power integrity in 3D stacks difficult; embedded voltage sensors and current monitors enable in-situ measurement; critical for validation and debug

**Design and Simulation:**
- **PDN Extraction**: extract resistance, inductance, and capacitance of power grid from layout; Cadence Voltus, Synopsys PrimeRail, or Ansys RedHawk tools
- **IR Drop Simulation**: static and dynamic IR drop analysis; identifies worst-case voltage drop locations; guides power grid optimization; typical runtime 1-24 hours for full-chip analysis
- **Frequency-Domain Analysis**: calculate PDN impedance vs frequency; identify resonances; optimize decoupling capacitor placement; target impedance <1 mΩ at all frequencies
- **Co-Simulation**: combine power, thermal, and signal integrity simulation; captures coupling effects; enables holistic optimization; computationally expensive but necessary for 3D designs

**Measurement and Validation:**
- **Embedded Voltage Sensors**: on-die sensors measure local supply voltage; resolution 1-10 mV, sampling rate 1-100 MHz; distributed across die to capture spatial variation
- **Current Monitors**: measure current through power TSVs/bumps; resolution 1-100 mA; enables real-time power monitoring and dynamic power management
- **Power Integrity Test Structures**: dedicated test structures with controlled switching patterns; generate known current profiles; validate PDN design and simulation
- **Failure Analysis**: voltage contrast imaging (SEM) identifies regions with IR drop; thermal imaging correlates hot spots with power delivery issues; guides design improvements

**Production Examples:**
- **AMD 3D V-Cache**: 64 MB SRAM die stacked on CPU die; dedicated power TSVs for SRAM; IR drop <50 mV at 105 W TDP; production since 2021
- **Intel Foveros**: logic-on-logic stacking with micro-bump power delivery; 30% of bumps allocated to power/ground; IR drop <5% of supply voltage; production in Meteor Lake
- **SK Hynix HBM3**: 12 DRAM dies stacked on logic base; TSV-based power delivery; IR drop <100 mV at 300 GB/s bandwidth; production since 2022

Power delivery in 3D integration is **the fundamental enabler of high-performance stacked systems — requiring careful co-design of vertical interconnects, on-die power grids, and decoupling capacitors to deliver clean, stable power with minimal IR drop and noise, making possible the 100+ W power densities and multi-voltage-domain architectures that define modern 3D integrated circuits**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/power-delivery-3d-integration) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
