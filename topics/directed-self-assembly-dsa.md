# Directed Self-Assembly (DSA)

**Keywords**: directed self assembly dsa,block copolymer lithography,dsa patterning,self aligned patterning,bcp lithography

---

**Directed Self-Assembly (DSA)** is **the patterning technique that uses block copolymer phase separation guided by lithographically-defined templates to create sub-lithographic features with 2-4× pitch multiplication** — enabling 10-20nm pitch patterns from 40-80nm lithography, providing cost-effective alternative to multi-patterning for contact holes, line-space patterns, and via layers at 7nm, 5nm nodes.

**Block Copolymer Fundamentals:**
- **Phase Separation**: block copolymers (BCP) consist of two immiscible polymer blocks (e.g., PS-PMMA: polystyrene-polymethylmethacrylate); anneal to form ordered nanostructures; lamellar (line-space) or cylindrical (contact hole) morphologies
- **Natural Pitch**: L0 = characteristic period determined by polymer molecular weight and Flory-Huggins parameter χ; typical L0 = 20-40nm; independent of lithography; enables sub-lithographic features
- **Pattern Transfer**: after self-assembly, selectively remove one block (e.g., PMMA by UV exposure or wet etch); remaining block (PS) serves as etch mask; transfer pattern to substrate
- **Pitch Multiplication**: lithography defines guide patterns at 2-4× BCP pitch; BCP fills and self-assembles; achieves 2-4× density multiplication; cost-effective vs multi-patterning

**DSA Process Flows:**
- **Graphoepitaxy**: lithography creates topographic templates (trenches or posts); BCP fills templates; sidewalls guide orientation; used for line-space patterns; trench width = N × L0 where N is integer
- **Chemoepitaxy**: lithography patterns chemical contrast on flat surface (alternating wetting regions); BCP assembles on chemical template; used for contact holes and lines; requires precise surface chemistry control
- **Hybrid Methods**: combine topographic and chemical guiding; improves defectivity and placement accuracy; used in production for critical layers
- **Anneal Process**: thermal anneal (200-250°C, 2-5 minutes) or solvent vapor anneal; drives phase separation; forms ordered structures; anneal conditions critical for defect density

**Applications and Integration:**
- **Contact Holes**: cylindrical BCP morphology creates hexagonal array of holes; 20-30nm diameter holes at 40-60nm pitch; used for DRAM capacitor contacts, logic via layers; 2-3× cost reduction vs EUV
- **Line-Space Patterns**: lamellar BCP creates alternating lines; 10-20nm half-pitch; used for fin patterning, metal lines; competes with SAQP (self-aligned quadruple patterning)
- **Via Layers**: random via placement challenging for DSA; hybrid approach: lithography for via position, DSA for size control; improves CD uniformity
- **DRAM**: DSA widely adopted for DRAM capacitor contact patterning; 18nm DRAM and beyond; cost-effective; mature process; high-volume production

**Defectivity and Yield:**
- **Defect Types**: dislocations (missing or extra features), disclinations (orientation defects), bridging (merged features); typical defect density 0.1-10 defects/cm² depending on application
- **Defect Reduction**: optimized anneal conditions, improved BCP materials, better template design; defect density <0.1/cm² achieved for DRAM; <1/cm² for logic
- **Inspection**: optical inspection insufficient for sub-20nm features; CD-SEM required; time-consuming; inline monitoring challenges; statistical sampling used
- **Repair**: defect repair difficult due to small feature size; focus on defect prevention; process optimization critical; yield learning curve steep

**Materials Development:**
- **High-χ BCP**: higher χ enables smaller L0 (down to 10nm); materials like PS-PDMS, PS-P4VP; challenges in etch contrast and processing
- **Etch Selectivity**: need high etch selectivity between blocks; PS-PMMA has moderate selectivity (3:1); sequential infiltration synthesis (SIS) improves selectivity to >10:1
- **Thermal Budget**: anneal temperature must be compatible with underlying layers; <250°C typical; limits material choices; solvent anneal alternative but adds complexity
- **Suppliers**: JSR, Tokyo Ohka, Merck, Brewer Science developing DSA materials; continuous improvement in defectivity and process window

**Metrology and Process Control:**
- **CD Uniformity**: BCP self-assembly provides excellent CD uniformity (±1-2nm, 3σ); better than lithography alone; key advantage for critical dimensions
- **Placement Accuracy**: limited by template lithography; ±3-5nm typical; sufficient for many applications; tighter control requires advanced lithography
- **Defect Inspection**: CD-SEM for defect review; optical inspection for gross defects; inline monitoring limited; end-of-line inspection standard
- **Process Window**: anneal time, temperature, BCP thickness must be tightly controlled; ±5°C temperature, ±10% thickness; automated process control essential

**Cost and Throughput:**
- **Cost Advantage**: DSA single patterning vs SAQP (4 litho steps) or EUV; 50-70% cost reduction for contact holes; significant for high-volume production
- **Throughput**: BCP coat and anneal add 2-5 minutes per wafer; acceptable for cost savings; throughput 30-60 wafers/hour; comparable to multi-patterning
- **Equipment**: standard coat/develop tracks with anneal module; Tokyo Electron, SCREEN, SEMES supply equipment; capital cost <$5M; low barrier to adoption
- **Consumables**: BCP materials cost $500-1000 per liter; usage 1-2mL per wafer; material cost <$1 per wafer; negligible vs lithography cost

**Industry Adoption:**
- **DRAM**: SK Hynix, Samsung, Micron use DSA for 18nm and below; high-volume production; proven technology; cost-effective
- **Logic**: Intel explored DSA for fin patterning; TSMC evaluated for via layers; limited adoption due to defectivity concerns; niche applications
- **3D NAND**: potential for word line patterning; under development; challenges in thick film patterning; future opportunity
- **Future Outlook**: DSA niche technology for cost-sensitive applications; EUV adoption reduces DSA need for logic; DRAM remains strong application

**Challenges and Limitations:**
- **Defectivity**: achieving <0.01 defects/cm² for logic remains challenging; DRAM tolerates higher defect density; limits logic adoption
- **Design Restrictions**: DSA favors regular patterns; random logic layouts difficult; design-technology co-optimization required
- **Placement Accuracy**: limited by template lithography; insufficient for tightest overlay requirements (<2nm); restricts applications
- **Scalability**: L0 scaling limited by polymer physics; <10nm challenging; high-χ materials needed; materials development ongoing

Directed Self-Assembly is **the cost-effective patterning solution for regular, high-density structures** — by leveraging block copolymer self-assembly to achieve sub-lithographic features, DSA provides 2-4× pitch multiplication at 50-70% cost reduction vs multi-patterning, enabling economical production of DRAM and selected logic layers while complementing advanced lithography technologies.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/directed-self-assembly-dsa) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
