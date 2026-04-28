# Micro-Bump Technology

**Keywords**: micro bump technology,copper pillar bump,solder bump formation,bump pitch scaling,bump interconnect reliability

---

**Micro-Bump Technology** is **the fine-pitch solder interconnect system that connects stacked dies in 3D packages — featuring Cu pillar bumps (10-50μm diameter) with solder caps (Sn-Ag or Pb-Sn) on 40-150μm pitch, providing electrical connection, mechanical bonding, and thermal conduction with resistance 20-50 mΩ per bump and current carrying capacity 0.1-0.5 A per bump**.

**Copper Pillar Bump Structure:**
- **Cu Pillar**: electroplated Cu column 10-40μm height, 15-50μm diameter; provides mechanical standoff and low electrical resistance (1.7 μΩ·cm); pillar height controls final gap between dies (typically 15-30μm after bonding)
- **Solder Cap**: Sn-Ag (96.5Sn-3.5Ag), Sn-Ag-Cu (SAC305: 96.5Sn-3Ag-0.5Cu), or Pb-Sn (37Pb-63Sn for legacy) electroplated on Cu pillar; thickness 5-15μm; melts during reflow forming metallurgical bond; solder volume controls joint height and reliability
- **Under-Bump Metallization (UBM)**: Ti/Cu or Ti/Ni/Cu seed layer (50/500nm or 50/200/500nm) on Al bond pad; provides adhesion, diffusion barrier, and wettable surface for Cu electroplating; patterned by photolithography and wet etch
- **Passivation Opening**: polyimide or BCB passivation opened to expose Al pads; opening diameter 20-60μm for 40-150μm pitch bumps; passivation thickness 5-15μm provides electrical isolation and mechanical protection

**Fabrication Process:**
- **UBM Deposition**: PVD Ti/Cu sputtered on wafer; Ti (50nm) provides adhesion to Al and passivation; Cu (500nm) provides seed layer for electroplating; Applied Materials Endura or Singulus TIMARIS PVD tools
- **Photoresist Patterning**: thick photoresist (20-50μm) spin-coated and patterned to define bump locations; openings 15-50μm diameter; Tokyo Electron CLEAN TRACK or SUSS MicroTec ACS200 coat/develop systems
- **Cu Electroplating**: Cu plated in photoresist openings; acid Cu sulfate bath with organic additives; current density 10-30 mA/cm²; plating time 30-90 minutes for 20-40μm height; Lam Research SABRE or Applied Materials Raider plating tools
- **Solder Electroplating**: Sn-Ag or SAC solder plated on Cu pillar; alkaline or methanesulfonic acid (MSA) bath; current density 5-15 mA/cm²; plating time 10-30 minutes for 5-15μm thickness; composition control ±0.5% critical for melting point and reliability

**Bump Pitch Scaling:**
- **Current State**: production micro-bumps at 40-55μm pitch for HBM (High Bandwidth Memory); 50-80μm pitch for logic-on-logic stacking; 100-150μm pitch for interposer connections
- **Scaling Challenges**: <40μm pitch requires <30μm bump diameter; solder volume decreases with diameter³ causing insufficient joint formation; alignment tolerance must be <±5μm (vs ±10μm at 55μm pitch)
- **Hybrid Bonding Transition**: <20μm pitch requires hybrid bonding (direct Cu-Cu bonding without solder); micro-bumps limited to >40μm pitch by solder volume and alignment constraints
- **Pitch Roadmap**: 55μm (HBM2), 40μm (HBM3), 25-30μm (future HBM), <10μm (hybrid bonding only); pitch scaling driven by bandwidth requirements (1 TB/s requires >10,000 connections per mm²)

**Reflow and Bonding:**
- **Flux Application**: no-clean flux dispensed or printed on bumps; activates solder surface, removes oxides, improves wetting; flux residue <50μm thickness remains after reflow
- **Die Placement**: pick-and-place equipment positions top die on bottom die with ±5-10μm accuracy; Besi Esec 3100 or ASM AMICRA NOVA die bonder; vision-based alignment to fiducial marks
- **Reflow**: heating to 240-260°C (Sn-Ag) or 180-200°C (Pb-Sn) in N₂ or forming gas atmosphere; solder melts, wets Cu pillar and UBM, forms intermetallic compounds (Cu₆Sn₅, Cu₃Sn); cooling solidifies joint
- **Underfill**: capillary underfill (CUF) dispensed at die edge; flows between dies by capillarity; cures at 150-180°C for 30-90 minutes; provides mechanical support, stress relief, and moisture barrier; typical materials: epoxy with silica filler (60-70 wt%)

**Electrical and Thermal Performance:**
- **Resistance**: Cu pillar 5-15 mΩ, solder joint 10-30 mΩ, UBM and pad 5-10 mΩ; total bump resistance 20-50 mΩ; resistance increases 10-20% after thermal cycling due to intermetallic growth
- **Inductance**: 10-50 pH per bump depending on height and diameter; lower than wire bonds (1-5 nH) enabling higher frequency operation (>10 GHz); critical for high-speed interfaces
- **Current Capacity**: 0.1-0.5 A per bump limited by electromigration in solder joint; current density <10⁴ A/cm² for 10-year lifetime at 100°C; power delivery requires 100-500 bumps per die
- **Thermal Conductivity**: Cu pillar 400 W/m·K, solder 50-60 W/m·K, underfill 0.5-1 W/m·K; thermal resistance 5-20 K/W per bump; parallel bumps reduce effective thermal resistance; heat extraction through bumps supplements through-silicon cooling

**Reliability:**
- **Thermal Cycling**: JEDEC JESD22-A104 (-40°C to 125°C, 1000 cycles); failure mechanism: solder fatigue at Cu-solder interface; characteristic life 2000-5000 cycles for SAC305; Pb-Sn more ductile with 3000-8000 cycles
- **Electromigration**: current-induced atomic migration in solder; voids form at cathode, hillocks at anode; mean time to failure (MTTF) = A·j⁻ⁿ·exp(Ea/kT) where j is current density, n≈2, Ea≈0.8 eV for Sn-Ag
- **Intermetallic Growth**: Cu₆Sn₅ and Cu₃Sn intermetallics grow at Cu-solder interface; growth rate proportional to √t; excessive growth (>5μm) causes brittle fracture; high-temperature storage (150°C, 1000 hours) accelerates growth for reliability testing
- **Underfill Delamination**: moisture absorption causes underfill swelling and delamination; JEDEC moisture sensitivity level (MSL) testing; proper surface preparation (plasma clean) and adhesion promoters prevent delamination

**Inspection and Test:**
- **Optical Inspection**: automated optical inspection (AOI) checks bump height, diameter, and coplanarity; Camtek Falcon or KLA 8 series; resolution 1-2μm; detects missing bumps, bridging, and dimensional defects
- **X-Ray Inspection**: 2D or 3D X-ray (computed tomography) inspects bump-to-pad alignment and solder joint quality after reflow; Nordson Dage XD7600 or Zeiss Xradia; detects voids, non-wetting, and misalignment
- **Electrical Test**: 4-wire Kelvin measurement of bump resistance; typical specification 20-50 mΩ; >100 mΩ indicates poor contact or high intermetallic resistance; daisy-chain test structures enable continuity testing

Micro-bump technology is **the workhorse interconnect for 3D packaging — providing the electrical, mechanical, and thermal connections that enable high-bandwidth memory stacking, logic-on-logic integration, and heterogeneous chiplet systems, balancing the competing requirements of fine pitch, low resistance, high reliability, and manufacturing cost that make 3D integration practical for high-volume production**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/micro-bump-technology-13686) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
