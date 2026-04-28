# Raised Source/Drain (RSD)

**Keywords**: raised source drain structure,raised sd epitaxy,elevated source drain,rsd contact resistance,raised sd integration

---

**Raised Source/Drain (RSD)** is **the structural enhancement where selective epitaxial silicon growth elevates the source/drain surface 20-80nm above the original silicon level — providing increased volume for silicide formation, reduced contact resistance, lower parasitic resistance, and improved contact landing tolerance, while serving as a platform for stress engineering through SiGe epitaxy in PMOS devices**.

**RSD Formation Process:**
- **Selective Epitaxy**: after source/drain implantation and before silicidation, selective silicon epitaxy grows only on exposed silicon surfaces (S/D regions), not on gate or spacer dielectrics
- **Growth Chemistry**: SiH₄ or SiH₂Cl₂ precursor with HCl at 600-750°C; HCl etches nucleation on oxide/nitride surfaces, ensuring selectivity; growth rate 5-20nm/min
- **Raised Height**: typical RSD height 30-60nm for logic processes; taller structures provide more silicide volume but increase topography and contact aspect ratio
- **In-Situ Doping**: phosphorus (PH₃) for NMOS or boron (B₂H₆) for PMOS added during growth; active doping >10²⁰ cm⁻³ provides low contact resistance without additional implantation

**Facet Control:**
- **Crystal Planes**: epitaxial silicon naturally grows with {111} and {311} facets; facet angles 54.7° for {111}, 25° for {311} relative to (100) surface
- **Growth Conditions**: temperature, pressure, and precursor ratios control facet formation; higher temperature favors {111} facets, lower temperature produces more {311}
- **Facet Uniformity**: uniform facets ensure consistent silicide thickness across the S/D region; non-uniform facets cause silicide thickness variation and contact resistance variation
- **Lateral Growth**: some lateral epitaxy occurs under spacer edges; controlled lateral growth can reduce S/D-to-gate spacing and series resistance; excessive growth causes gate shorts

**Contact Resistance Reduction:**
- **Silicide Volume**: raised S/D provides 2-3× more silicon volume for silicide formation; thicker NiSi (20-30nm vs 10-15nm on flat S/D) reduces contact resistance
- **Contact Area**: raised surface improves contact landing; misaligned contacts still land on raised S/D rather than spacer or STI; improves yield and reduces resistance variation
- **Specific Contact Resistivity**: ρc = 1-3×10⁻⁸ Ω·cm² for NiSi on heavily-doped raised S/D; 30-50% lower than flat S/D due to better silicide quality and thickness
- **Total Contact Resistance**: Rc reduced 40-60% with RSD vs flat S/D; particularly important at advanced nodes where contact resistance dominates total resistance

**Parasitic Resistance Benefits:**
- **Series Resistance**: raised S/D reduces total series resistance (Rsd) by 20-40%; more conductive volume between contact and channel reduces spreading resistance
- **Sheet Resistance**: heavily-doped epitaxial layer has sheet resistance 50-100 Ω/sq vs 200-400 Ω/sq for implanted S/D; lower Rsh reduces lateral resistance
- **Resistance Scaling**: as devices shrink, parasitic resistance becomes larger fraction of total; RSD maintains acceptable Ron even as channel resistance decreases
- **Performance Impact**: 10-15% drive current improvement from reduced parasitic resistance; enables meeting performance targets without aggressive channel scaling

**Integration with Strain Engineering:**
- **SiGe Raised S/D**: for PMOS, grow Si₁₋ₓGeₓ instead of Si; combines raised S/D benefits (low resistance) with strain engineering (compressive channel stress)
- **Dual Benefits**: SiGe RSD provides both 20-30% mobility enhancement (from stress) and 30-40% resistance reduction (from raised structure); total performance improvement 40-60%
- **Process Simplification**: single epitaxy step provides both strain and raised S/D; eliminates need for separate recess etch and raised epi steps
- **NMOS Options**: some processes use raised Si:C (silicon-carbon) for NMOS to provide tensile stress; carbon content 0.5-2% induces tensile strain

**Topography Management:**
- **CMP Challenges**: raised S/D creates 30-60nm topography; subsequent contact CMP must handle this step height without dishing or erosion
- **Planarization**: thick interlayer dielectric (ILD) deposition and CMP planarizes surface before contact formation; requires 200-400nm ILD overburden
- **Contact Aspect Ratio**: raised S/D increases contact depth by the raised height; 50nm raised S/D adds 50nm to contact depth; affects contact etch and fill processes
- **Design Rules**: raised S/D topography affects lithography focus; design rules may restrict dense S/D patterns or require dummy fills for planarization

**Process Optimization:**
- **Temperature**: 650-700°C provides good selectivity and growth rate; lower temperature (<600°C) improves selectivity but reduces throughput; higher temperature (>750°C) risks loss of selectivity
- **HCl/Precursor Ratio**: ratio 0.1-0.3 optimizes selectivity vs growth rate; higher HCl improves selectivity but reduces growth rate and can etch silicon
- **Pressure**: 10-100 Torr; lower pressure improves uniformity and selectivity; higher pressure increases growth rate
- **Doping Uniformity**: in-situ doping must be uniform throughout raised region; doping gradients cause contact resistance variation; requires stable gas flow and temperature

**Advanced RSD Techniques:**
- **Multi-Layer RSD**: bottom layer high-doping Si for low resistance, top layer SiGe for stress; provides optimized resistance and strain
- **Selective RSD**: raised S/D only on critical devices (minimum gate length); longer gates use flat S/D; reduces process complexity while optimizing performance
- **Ultra-Raised S/D**: 80-120nm raised height for maximum contact area and resistance reduction; used in some high-performance processes despite topography challenges
- **Facet Engineering**: controlled facet angles optimize stress transfer to channel; steeper facets provide more vertical stress component

**Reliability Considerations:**
- **Silicide Uniformity**: non-uniform raised S/D causes non-uniform silicide; thin silicide regions have high resistance and poor reliability
- **Defect Density**: epitaxial defects (dislocations, stacking faults) degrade junction leakage and reliability; defect density <10⁴ cm⁻² required
- **Stress Effects**: raised SiGe S/D creates high stress at gate edge; stress concentration can affect gate dielectric reliability; requires careful stress management
- **Electromigration**: current crowding at contact-to-raised-S/D interface affects electromigration; contact design must account for current density

**Scaling Considerations:**
- **FinFET Transition**: raised S/D becomes essential in FinFET structures; provides landing area for contacts on narrow fins (7-10nm wide)
- **Contact Scaling**: as contact size shrinks below 40nm, raised S/D becomes mandatory for acceptable contact resistance; flat S/D cannot meet resistance targets
- **Epitaxy Challenges**: selective epitaxy on narrow structures (<20nm) is challenging; requires advanced precursors and process control
- **Alternative Materials**: cobalt or ruthenium replacing tungsten in contacts benefits from raised S/D landing area; enables aggressive contact scaling

Raised source/drain structures are **the essential enabler of low contact resistance in scaled CMOS — by providing increased volume for silicide formation and improved contact landing tolerance, RSD reduces parasitic resistance by 30-50% while serving as the platform for strain engineering, making it indispensable from 65nm planar CMOS through 5nm FinFET technologies**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/raised-source-drain-structure) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
