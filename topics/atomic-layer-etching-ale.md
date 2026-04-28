# Atomic Layer Etching (ALE)

**Keywords**: atomic layer etching ale,layer by layer etching,self limiting etch,isotropic ale,anisotropic ale

---

**Atomic Layer Etching (ALE)** is **the self-limiting etch process that removes material one atomic layer at a time through cyclic surface modification and removal steps** — providing angstrom-level etch control, excellent uniformity (±0.5Å across wafer), and minimal damage for critical applications including gate recess, fin reveal, spacer formation, and contact opening at 7nm, 5nm, 3nm nodes where conventional RIE lacks precision.

**ALE Process Fundamentals:**
- **Two-Step Cycle**: Step 1 (Modification): chemisorb reactive species on surface, forms self-limiting modified layer (typically 1-3Å thick); Step 2 (Removal): remove modified layer via ion bombardment, thermal desorption, or chemical reaction; repeat cycles until target depth reached
- **Self-Limiting**: modification step saturates at monolayer coverage; prevents runaway etching; provides atomic-level control; key advantage over continuous plasma etching
- **Etch Per Cycle (EPC)**: typical EPC 0.5-2Å depending on material and chemistry; silicon EPC ~1Å, SiO₂ EPC ~0.8Å; precise control enables <1nm total etch depth accuracy
- **Cycle Count**: etch depth = EPC × number of cycles; 10nm etch requires 50-100 cycles at 1-2Å EPC; process time 5-15 minutes; slower than RIE but necessary for critical steps

**Thermal ALE (Isotropic):**
- **Process**: alternating exposure to reactant gas (e.g., Cl₂, HF) and inert purge; thermal energy drives reactions; no plasma; isotropic etch (equal in all directions)
- **Silicon Thermal ALE**: Cl₂ adsorption forms SiClₓ surface layer; Ar purge removes excess Cl₂; heat (300-500°C) desorbs SiCl₄; EPC ~1Å; used for Si surface cleaning, defect removal
- **SiO₂ Thermal ALE**: HF vapor forms SiF₄; trimethylaluminum (TMA) ligand exchange; alternating HF/TMA cycles; EPC ~0.8Å; room temperature process; used for oxide recess, gate oxide thinning
- **Applications**: isotropic etch for surface preparation, defect removal, oxide thinning; not suitable for anisotropic features (trenches, vias)

**Plasma ALE (Anisotropic):**
- **Process**: alternating plasma modification and ion bombardment removal; directional etch; anisotropic profile; used for high aspect ratio features
- **Modification Step**: plasma generates reactive radicals (Cl, F, O); chemisorb on surface; form modified layer (oxide, fluoride, chloride); self-limiting at monolayer; typical 1-5 seconds
- **Removal Step**: low-energy ion bombardment (20-100eV Ar⁺); removes modified layer; minimal damage to underlying material; directional removal; typical 1-5 seconds
- **Cycle Optimization**: balance modification and removal; incomplete modification leaves residue; excessive removal damages substrate; process window ±10-20%

**Material Selectivity:**
- **Si:SiO₂ Selectivity**: >50:1 achievable with optimized chemistry; Cl-based chemistry etches Si, stops on SiO₂; critical for fin reveal, gate recess
- **SiN:SiO₂ Selectivity**: >20:1 with fluorocarbon chemistry; enables spacer formation, contact opening; selectivity higher than RIE (5-10:1)
- **Metal Selectivity**: TiN, TaN, W selective etch demonstrated; <5:1 selectivity typical; challenging due to similar chemistry; active research area
- **Damage Reduction**: low ion energy (<100eV) minimizes subsurface damage; <1nm damaged layer vs 3-5nm for RIE; critical for maintaining device performance

**Equipment and Implementation:**
- **ALE Reactors**: modified plasma etch tools (Lam Research, Applied Materials, Tokyo Electron); fast gas switching (<0.5s); precise ion energy control; temperature control (20-400°C)
- **Lam Syndion**: dedicated ALE platform; <0.3s gas switching; 20-1000eV ion energy; in-situ metrology; production-proven for 7nm/5nm
- **Applied Materials Selectra**: selective etch platform with ALE capability; optimized for high selectivity applications; integrated metrology
- **Throughput**: 30-60 wafers/hour depending on cycle count; slower than RIE (60-120 WPH) but acceptable for critical steps; 5-10% of total etch steps use ALE

**Process Control and Metrology:**
- **Endpoint Detection**: optical emission spectroscopy (OES) monitors etch progress; interferometry for film thickness; challenging due to small EPC; cycle counting primary method
- **Uniformity**: ±0.5Å (3σ) across 300mm wafer; 5-10× better than RIE (±2-5Å); enabled by self-limiting chemistry; critical for device matching
- **Repeatability**: ±0.3Å wafer-to-wafer; excellent process control; deterministic cycle-based process; minimal drift
- **In-Situ Monitoring**: ellipsometry, reflectometry track film thickness real-time; enables adaptive process control; compensates for incoming variation

**Applications at Advanced Nodes:**
- **Fin Reveal**: etch sacrificial oxide to expose Si fins; requires <1nm depth control; Si:SiO₂ selectivity >50:1; ALE standard process for 7nm/5nm FinFET
- **Gate Recess**: etch poly-Si gate to precise depth; ±0.5nm tolerance; critical for threshold voltage control; ALE enables <1nm depth accuracy
- **Spacer Formation**: selective etch of SiN spacer; high SiN:SiO₂ selectivity; anisotropic profile; ALE provides better profile control than RIE
- **Contact Opening**: etch through ILD to contact; stop on metal or Si; high selectivity required; ALE reduces contact resistance by minimizing damage

**Challenges and Limitations:**
- **Throughput**: 5-15 minutes per wafer vs 1-3 minutes for RIE; limits adoption to critical steps; cost-performance trade-off
- **Chemistry Development**: each material requires unique chemistry; limited chemistries available; extensive development needed for new materials
- **Aspect Ratio**: ion bombardment step can cause aspect ratio dependent etching (ARDE); limits application to <20:1 aspect ratio; higher AR requires optimization
- **Cost of Ownership**: slower throughput increases CoO; offset by improved yield and device performance; justified for critical steps

**Future Developments:**
- **Selective ALE**: area-selective ALE that etches only specific materials or regions; eliminates masking steps; active research; potential for self-aligned processes
- **High Aspect Ratio ALE**: improved ion directionality for >50:1 aspect ratio; required for 3D NAND, DRAM; neutral beam ALE under development
- **Metal ALE**: precise metal etch for advanced interconnects (Co, Ru); challenging chemistry; critical for future nodes
- **Faster Cycles**: <1 second per cycle target; requires faster gas switching and pumping; would improve throughput 2-3×

**Industry Adoption:**
- **Logic**: Intel, TSMC, Samsung use ALE for fin reveal, gate recess at 7nm and below; 5-10 ALE steps per device; critical for yield
- **DRAM**: SK Hynix, Samsung, Micron use ALE for capacitor contact opening; 18nm DRAM and below; high selectivity essential
- **3D NAND**: ALE for channel hole etch, slit etch; high aspect ratio challenges; limited adoption; conventional RIE still dominant
- **Market**: ALE equipment market $500M-1B annually; growing 15-20% per year; driven by advanced node adoption

Atomic Layer Etching is **the precision tool that enables atomic-scale manufacturing** — by removing material one layer at a time with self-limiting chemistry, ALE provides the angstrom-level control and minimal damage required for critical process steps at 7nm and beyond, where conventional etching techniques lack the precision to maintain device performance and yield.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/atomic-layer-etching-ale) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
