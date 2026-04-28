# Power Delivery Network (PDN)

**Keywords**: power delivery network pdn,voltage droop ir drop,decoupling capacitor placement,power integrity analysis,package power distribution

---

**Power Delivery Network (PDN)** is **the electrical distribution system that supplies stable voltage and current to semiconductor devices — comprising voltage regulators, package power planes, on-die power grids, and decoupling capacitors that must deliver 50-300A currents with <50mV voltage ripple across frequencies from DC to multi-GHz, preventing voltage droop that would cause timing failures and ensuring reliable operation despite rapidly switching loads that create current transients exceeding 100A/ns**.

**PDN Architecture:**
- **Voltage Regulator Module (VRM)**: converts 12V input to 0.8-1.2V core voltage; switching regulators (buck converters) at 200-1000 kHz; located on motherboard 5-20cm from package; provides bulk current (50-300A) but has limited high-frequency response due to distance and inductance
- **Package Power Distribution**: power planes in package substrate distribute current from VRM to die; copper planes 20-50μm thick with 0.1-1 mΩ resistance; multiple power and ground planes reduce inductance; ball grid array (BGA) connections provide 100-500 power/ground balls
- **On-Die Power Grid**: metal layers M1-M8+ form power grid on die; top metal layers (M6-M8) carry bulk current with 1-5μm width and 1-3μm thickness; lower layers distribute locally; grid resistance 10-100 mΩ, inductance 10-100 pH
- **Decoupling Capacitors**: placed at multiple levels (VRM, motherboard, package, die) to supply high-frequency current transients; form low-impedance path at different frequency ranges; total capacitance 1-10 mF distributed across frequency spectrum

**Voltage Droop and IR Drop:**
- **Static IR Drop**: voltage drop from DC current through resistive power grid; ΔV = I·R where I is average current, R is grid resistance; 50-200mV drop typical from VRM to die; compensated by setting VRM output voltage higher than target die voltage
- **Dynamic Voltage Droop**: transient voltage drop from di/dt through inductive power grid; ΔV = L·(di/dt) where L is grid inductance, di/dt is current slew rate; 100A/ns transients create 50-200mV droop with 0.5-2nH inductance
- **Resonance**: PDN has resonant frequency where impedance peaks; determined by package inductance and decoupling capacitance; f_res = 1/(2π√(LC)); typical resonance 10-100 MHz; impedance peak can exceed 10× DC resistance
- **Target Impedance**: maximum allowable PDN impedance to limit voltage droop; Z_target = ΔV_max / I_max; for 50mV droop with 100A transient, Z_target = 0.5 mΩ; must be maintained from DC to GHz frequencies

**Decoupling Capacitor Strategy:**
- **Bulk Capacitors**: 100-1000μF electrolytic or polymer capacitors on motherboard near VRM; provide low-frequency (1-100 kHz) decoupling; large capacitance but high ESR (equivalent series resistance) and ESL (equivalent series inductance)
- **Ceramic Capacitors**: 0.1-100μF multilayer ceramic capacitors (MLCC) on motherboard and package; provide mid-frequency (100 kHz-10 MHz) decoupling; low ESR/ESL but limited capacitance; placed close to package (1-10mm)
- **On-Package Capacitors**: 1-10μF capacitors embedded in package substrate or mounted on package surface; provide high-frequency (10-100 MHz) decoupling; minimize inductance by proximity to die
- **On-Die Capacitors**: MOS capacitors or trench capacitors integrated on die; provide ultra-high-frequency (100 MHz-1 GHz) decoupling; 1-100 nF/mm² capacitance density; consume die area but essential for advanced nodes

**Power Integrity Analysis:**
- **Frequency Domain Analysis**: measures or simulates PDN impedance vs frequency; identifies resonances and impedance peaks; validates impedance below target across all frequencies; vector network analyzer (VNA) measures impedance from 1 MHz to 10 GHz
- **Time Domain Analysis**: simulates voltage response to current transients; uses SPICE models of VRM, package, and die; validates voltage stays within specifications during worst-case switching; identifies critical transient scenarios
- **Current Signature Analysis**: measures die current vs time using current probes or VRM telemetry; identifies switching patterns and peak currents; validates PDN design assumptions; typical current waveforms show 10-100A transients with 1-10ns rise times
- **Electromagnetic Simulation**: 3D field solvers (Ansys Q3D, Cadence Clarity) extract resistance, inductance, and capacitance of power distribution structures; accounts for skin effect, proximity effect, and return path inductance

**Package PDN Design:**
- **Power Plane Pairs**: dedicated power and ground planes in package substrate; spacing 50-200μm minimizes inductance; multiple power domains (core, I/O, analog) require separate planes; plane thickness 20-50μm copper provides <1 mΩ resistance
- **Via Design**: power vias connect die bumps to package planes; via diameter 50-150μm, pitch 200-500μm; via inductance 50-200 pH each; parallel vias reduce effective inductance; target >100 power vias and >100 ground vias for high-power die
- **Ball Grid Array (BGA)**: power and ground balls connect package to motherboard; ball diameter 300-600μm, pitch 0.5-1.0mm; 20-40% of balls allocated to power/ground; peripheral balls have higher inductance than center balls
- **Embedded Capacitors**: thin dielectric layers (1-5μm) between power planes create distributed capacitance; 10-100 nF/cm² capacitance density; reduces package inductance and provides high-frequency decoupling

**On-Die PDN Design:**
- **Power Grid Topology**: mesh grid with horizontal and vertical metal stripes; top metals (M6-M8) carry bulk current; lower metals distribute locally; grid pitch 5-50μm balances resistance and routing congestion
- **IR Drop Analysis**: static timing analysis includes IR drop effects; voltage-dependent delay models account for reduced voltage at far corners; design margins ensure timing closure with worst-case IR drop
- **Electromigration**: current density limits (1-2 MA/cm² for copper) prevent metal migration; wider wires for high-current paths; redundant paths improve reliability; EM analysis validates 10-year lifetime
- **Power Gating**: switches disconnect power to unused blocks; reduces leakage power by 50-90%; power switches sized to handle block current (1-10A); distributed switches minimize voltage drop

**Advanced PDN Techniques:**
- **Adaptive Voltage Scaling (AVS)**: adjusts supply voltage based on workload and temperature; reduces power during low-performance periods; requires fast VRM response (<1μs) and on-die voltage sensors
- **Per-Core Power Domains**: separate voltage domains for each CPU core; enables independent voltage/frequency scaling; requires additional package routing and decoupling; improves power efficiency by 20-40%
- **Deep Trench Capacitors**: high-aspect-ratio trenches (depth 10-50μm, width 0.5-2μm) filled with dielectric and metal; provides 10-100 nF/mm² on-die capacitance; used in high-performance processors and FPGAs
- **Integrated Voltage Regulators (IVR)**: on-die switching regulators convert package voltage to core voltage; eliminates package inductance from high-frequency path; enables faster voltage transitions and finer-grained power management

**Measurement and Validation:**
- **Voltage Probing**: oscilloscope probes measure die voltage during operation; requires package modification or probe access points; validates voltage ripple and droop; typical measurements show 20-100mV ripple at 100-500 MHz
- **Thermal Test Die**: test die with integrated voltage sensors and current sources; generates controlled current transients; measures voltage response; characterizes PDN impedance in-situ
- **Latch-Up Testing**: validates PDN robustness against latch-up (parasitic thyristor triggering); applies voltage/current transients; ensures device survives without latch-up; critical for reliability
- **Power Integrity Correlation**: compares measured voltage waveforms to simulations; validates PDN models; identifies discrepancies; improves model accuracy for future designs

**Design Challenges:**
- **Scaling Trends**: voltage scaling (1.2V to 0.8V) reduces noise margin; current increasing (50A to 300A) increases IR drop; tighter specifications require better PDN design
- **High-Frequency Noise**: multi-GHz clock frequencies create high-frequency current transients; on-die decoupling essential; package and board capacitors ineffective above 100 MHz
- **Cost vs Performance**: more decoupling capacitors and power planes improve performance but increase cost; design optimization balances performance requirements with cost constraints
- **3D Integration**: through-silicon vias (TSVs) in 3D stacked die create new PDN challenges; TSV inductance and resistance impact power delivery; requires new design methodologies

Power delivery networks are **the electrical lifeline of modern processors — delivering hundreds of amperes with millivolt precision, suppressing voltage fluctuations that would cause timing failures, and enabling the aggressive voltage scaling that makes high-performance, power-efficient computing possible, operating invisibly but critically at every clock cycle**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/power-delivery-network-pdn) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
