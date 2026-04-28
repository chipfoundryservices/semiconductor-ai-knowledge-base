# Complementary FET (CFET)

**Keywords**: complementary fet cfet,cfet stacked transistor,cfet nmos pmos vertical,cfet 3d integration,cfet monolithic stacking

---

**Complementary FET (CFET)** is **the revolutionary 3D transistor architecture that vertically stacks NMOS devices directly on top of PMOS devices within a single logic gate footprint — achieving 2× logic density improvement over planar GAA by eliminating horizontal NMOS-PMOS separation, enabling continued scaling beyond the 1nm node when lateral dimensions reach fundamental limits imposed by lithography, materials, and quantum mechanics**.

**CFET Architecture Concepts:**
- **Vertical Stacking**: PMOS nanosheets occupy bottom tier (0-60nm height); dielectric isolation layer (10-20nm SiO₂ or low-k); NMOS nanosheets in top tier (70-130nm height); shared gate electrode wraps both tiers vertically; single gate contact controls both devices simultaneously
- **Monolithic Integration**: both tiers fabricated sequentially on same substrate without wafer bonding; bottom tier (PMOS) processed first including S/D formation and partial gate stack; top tier (NMOS) epitaxially grown on planarized bottom tier; eliminates alignment challenges of hybrid bonding approaches
- **Footprint Advantage**: CFET inverter occupies area of single GAA transistor; 2× logic density vs GAA; 4× density vs FinFET; enables 6-8 track standard cell height vs 10-12 tracks for GAA; critical for continued transistor count scaling when gate pitch cannot shrink further
- **Shared vs Independent Gates**: shared gate (both tiers connected) simplifies processing but limits circuit flexibility; independent gates (separate contacts to NMOS and PMOS) enables pass-gate logic and transmission gates but requires complex via structures through isolation layer

**Bottom Tier (PMOS) Fabrication:**
- **Substrate Preparation**: Si substrate with buried oxide (BOX) layer for bottom tier isolation; alternatively, bulk Si with deep trench isolation; starting material must support subsequent high-temperature processing (>1000°C) for top tier
- **PMOS Nanosheet Formation**: Si/SiGe superlattice epitaxy (3-4 layers, total height 50-60nm); fin patterning; dummy gate and spacer formation; S/D recess and SiGe:B epitaxial growth at 550-600°C; B concentration 1-2×10²¹ cm⁻³
- **Partial Gate Stack**: SiGe release etch; HfO₂ and work function metal (TiN) deposition wrapping PMOS nanosheets; gate fill metal (W or Co) deposited but not fully planarized; top surface of gate remains recessed 20-30nm below ILD level to accommodate top tier
- **Planarization and Passivation**: thick ILD (SiO₂ or low-k) deposited and CMP planarized; surface roughness <0.5nm RMS required for top tier epitaxy; passivation layer (SiN or SiCN, 5-10nm) protects bottom tier during top tier processing; thermal budget for all subsequent steps limited to <800°C to preserve bottom tier

**Top Tier (NMOS) Fabrication:**
- **Epitaxial Regrowth**: selective Si epitaxy on exposed bottom tier Si regions; growth temperature 600-700°C (below bottom tier degradation threshold); defect density <10⁴ cm⁻² required; threading dislocations from bottom tier must not propagate; buffer layer (10-20nm) improves crystal quality
- **NMOS Superlattice**: Si/SiGe stack epitaxy for top tier nanosheets (3-4 layers, height 50-60nm); alignment to bottom tier gates within ±3nm using advanced metrology; fin patterning with overlay to bottom tier <2nm; etch stop on isolation layer between tiers
- **S/D Formation**: dummy gate and spacer; S/D recess etch stops at inter-tier isolation; SiP epitaxial S/D at 650-700°C; P concentration 1-3×10²¹ cm⁻³; thermal budget management critical to prevent bottom tier dopant diffusion or silicide degradation
- **Gate Stack Completion**: SiGe release for top tier; HfO₂ and work function metal (TiAlC or TaN) deposition; gate fill metal connects top and bottom tier gates vertically; single gate contact accesses both tiers; CMP planarization to final ILD level

**Inter-Tier Isolation and Connectivity:**
- **Isolation Layer**: 10-20nm SiO₂ or low-k dielectric separates NMOS and PMOS tiers; must withstand top tier processing without degradation; prevents leakage between tiers (<1 pA/μm² at 1V); thermal conductivity important for heat dissipation (SiO₂: 1.4 W/m·K)
- **Vertical Interconnects**: through-isolation vias (TIVs) connect bottom tier S/D to top tier S/D or gates; via diameter 10-15nm; aspect ratio 1:1 to 2:1; metal fill (W or Co) by CVD; contact resistance <50Ω per via; alignment tolerance ±2nm
- **Power Delivery**: VDD connects to PMOS S/D (bottom tier); VSS connects to NMOS S/D (top tier); vertical power distribution through TIVs; buried power rails in substrate below bottom tier further reduce routing overhead; power grid resistance <1 mΩ per cell
- **Signal Routing**: M0 metal layer contacts both tiers; M1 and above for inter-cell routing; reduced metal layer count possible due to 2× logic density (fewer cells to connect); back-side power delivery network (BS-PDN) synergizes with CFET for optimal power/signal separation

**Thermal and Reliability Challenges:**
- **Thermal Management**: 2× power density from vertical stacking; heat generation in top tier must conduct through bottom tier to substrate; thermal resistance 2-3× higher than planar devices; requires enhanced cooling (backside cooling, microfluidic channels, or diamond heat spreaders)
- **Process-Induced Stress**: bottom tier experiences full top tier thermal budget; stress from top tier epitaxy and ILD deposition affects bottom tier channel mobility; stress engineering (SiGe composition, ILD choice) optimizes both tiers simultaneously
- **Reliability**: time-dependent dielectric breakdown (TDDB) of inter-tier isolation critical; 10-year lifetime at 0.7V requires breakdown field >8 MV/cm; bias temperature instability (BTI) for both tiers; top tier hot carrier injection (HCI) enhanced by vertical field from bottom tier
- **Yield**: defect in either tier kills the CFET; yield = Y_bottom × Y_top; requires >99.9% yield per tier for acceptable overall yield; defect density <0.01 cm⁻² target; in-line metrology and defect inspection after each tier critical

**Performance and Scaling:**
- **Drive Current**: NMOS 1.5-1.8 mA/μm, PMOS 1.2-1.5 mA/μm at Vdd=0.65V (1nm node); comparable to planar GAA but in half the footprint; series resistance from TIVs adds 10-20Ω per device
- **Switching Speed**: inverter delay 15-20% higher than planar GAA due to increased parasitic capacitance (inter-tier coupling, TIV capacitance); compensated by reduced interconnect delay from higher logic density
- **Power Efficiency**: 2× logic density enables 30-40% chip area reduction at constant transistor count; 20-30% power reduction from reduced interconnect capacitance and resistance; power density increases requiring voltage scaling to 0.6-0.65V
- **Scaling Roadmap**: CFET targets 1nm node (2028-2030); A10 (0.7nm) node may use dual-tier CFET (4 nanosheet tiers total); beyond A10, atomic-scale transistors (2D materials, carbon nanotubes) required as Si CMOS reaches fundamental limits

Complementary FET is **the ultimate expression of 3D transistor integration — vertically stacking NMOS and PMOS to double logic density and extend Moore's Law through the 1nm node and beyond, representing the culmination of 60 years of silicon CMOS scaling and the bridge to post-silicon device technologies in the 2030s**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/complementary-fet-cfet-13701) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
