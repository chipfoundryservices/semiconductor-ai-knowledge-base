# AI-Driven Floorplanning

**Keywords**: ai floorplanning,ml chip floorplan,automated macro placement,neural network floorplan optimization,reinforcement learning floorplanning

---

**AI-Driven Floorplanning** is **the automated placement of large blocks and macros on chip floorplan using reinforcement learning and graph neural networks** — where RL agents learn optimal placement policies that minimize wirelength, congestion, and timing violations while meeting area and aspect ratio constraints, achieving 10-25% better quality of results than manual floorplanning in 6-24 hours vs weeks of expert effort, as demonstrated by Google's Nature 2021 paper where RL designed TPU floorplans with superhuman performance, using edge-based GNNs to encode block connectivity and spatial relationships, policy networks to select placement locations, and curriculum learning to transfer knowledge across designs, enabling automated floorplanning for complex SoCs with 100-1000 macros where manual exploration of 10⁵⁰+ possible placements is impossible and early floorplan decisions determine 60-80% of final PPA.

**Floorplanning Problem:**
- **Inputs**: macro blocks (hard blocks with fixed size), soft blocks (flexible size), I/O pads, area constraint, aspect ratio
- **Objectives**: minimize wirelength, congestion, timing violations; maximize routability; meet area and aspect ratio constraints
- **Complexity**: 100-1000 macros; 10⁵⁰+ possible placements; NP-hard problem; manual exploration takes weeks
- **Impact**: floorplan determines 60-80% of final PPA; early decisions critical; difficult to fix later

**Google's RL Approach:**
- **Representation**: floorplan as sequence of macro placements; edge-based GNN encodes connectivity
- **Policy Network**: GNN encoder + fully connected layers; outputs placement location for each macro
- **Value Network**: estimates quality of partial floorplan; guides search; shares encoder with policy
- **Training**: 10000 chip blocks; curriculum learning from simple to complex; 6-24 hours on TPU cluster

**RL Formulation:**
- **State**: current partial floorplan; placed and unplaced macros; connectivity graph; utilization map
- **Action**: place next macro at specific location; grid-based (32×32 to 128×128) or continuous
- **Reward**: weighted sum of wirelength (-), congestion (-), timing violations (-), area utilization (+)
- **Episode**: complete floorplan; 100-1000 steps (one per macro); 10-60 minutes per episode

**GNN for Connectivity:**
- **Graph**: nodes are macros and I/O pads; edges are nets; node features (area, aspect ratio, timing criticality)
- **Edge Features**: net weight, timing criticality, fanout; captures connectivity importance
- **Message Passing**: 5-10 GNN layers; aggregates neighborhood information; learns placement dependencies
- **Embedding**: 128-512 dimensional embeddings; captures both local and global context

**Placement Strategies:**
- **Sequential**: place macros one by one; RL selects order and location; most common approach
- **Hierarchical**: partition into regions; place regions first; then macros within regions; scales to large designs
- **Iterative Refinement**: initial placement; RL refines iteratively; 10-100 iterations; improves quality
- **Parallel**: place multiple macros simultaneously; faster but more complex; research phase

**Objectives and Constraints:**
- **Wirelength**: half-perimeter wirelength (HPWL); minimize total; reduces delay and power
- **Congestion**: routing congestion; predict from placement; avoid hotspots; ensures routability
- **Timing**: critical path delay; minimize; requires timing-aware placement; 10-30% impact on frequency
- **Area**: total area and aspect ratio; hard constraints; must fit within die; utilization 60-80% target

**Training Process:**
- **Data**: 1000-10000 chip blocks; diverse sizes and topologies; synthetic and real designs
- **Curriculum**: start with small blocks (10-50 macros); gradually increase complexity; 2-5 difficulty levels
- **Transfer Learning**: pre-train on diverse blocks; fine-tune for specific design; 10-100× faster
- **Convergence**: 10⁵-10⁶ episodes; 1-7 days on GPU/TPU cluster; early stopping when improvement plateaus

**Quality Metrics:**
- **Wirelength**: 10-25% better than manual; through learned placement strategies
- **Congestion**: 15-30% lower overflow; better routability; fewer routing iterations
- **Timing**: 10-20% better slack; timing-aware placement; higher frequency
- **Design Time**: 6-24 hours vs weeks for manual; 10-100× faster; enables exploration

**Commercial Adoption:**
- **Google**: production use for TPU design; Nature 2021 paper; superhuman performance demonstrated
- **NVIDIA**: exploring RL for GPU floorplanning; internal research; early results promising
- **Synopsys**: RL in DSO.ai; automated floorplanning; 10-30% QoR improvement
- **Cadence**: researching RL for floorplanning; integration with Innovus; early development

**Integration with EDA Flow:**
- **Input**: netlist, macro dimensions, I/O locations, constraints; standard formats (LEF/DEF)
- **RL Floorplanning**: automated placement; 6-24 hours; generates initial floorplan
- **Refinement**: traditional tools refine placement; detailed placement and routing; 1-3 days
- **Iteration**: if QoR insufficient, adjust constraints and re-run; 2-5 iterations typical

**Handling Large Designs:**
- **Hierarchical**: partition design into blocks; floorplan each block; 100-1000 macros per block
- **Clustering**: group related macros; place clusters first; then macros within clusters; reduces complexity
- **Incremental**: place critical macros first; then remaining; focuses effort on important decisions
- **Distributed**: parallelize across multiple GPUs; 5-20× speedup; handles very large designs

**Comparison with Traditional Methods:**
- **Simulated Annealing**: RL 10-25% better QoR; learns from data; but requires training
- **Analytical**: RL handles discrete constraints better; analytical faster but less flexible
- **Manual**: RL 10-100× faster; comparable or better quality; but less interpretable
- **Hybrid**: combine RL with traditional; RL for initial placement, traditional for refinement; best results

**Challenges:**
- **Training Cost**: 1-7 days on GPU/TPU cluster; $1K-10K per training; amortized over designs
- **Generalization**: models trained on one design family may not transfer; requires fine-tuning
- **Interpretability**: difficult to understand why RL makes decisions; trust and debugging challenges
- **Constraints**: complex constraints (timing, power, thermal) difficult to encode; requires careful reward design

**Advanced Techniques:**
- **Multi-Objective**: Pareto front of floorplans; trade-offs between objectives; 10-100 solutions
- **Uncertainty**: RL handles uncertainty in estimates (wirelength, congestion); robust floorplans
- **Interactive**: designer provides feedback; RL adapts; personalized to design style
- **Explainable**: attention mechanisms show which connections influence placement; improves trust

**Best Practices:**
- **Start Simple**: begin with small blocks (10-50 macros); validate approach; scale gradually
- **Use Transfer Learning**: pre-train on diverse designs; fine-tune for specific; 10-100× faster
- **Hybrid Approach**: RL for initial placement; traditional for refinement; best of both worlds
- **Iterate**: floorplanning is iterative; refine constraints and objectives; 2-5 iterations typical

**Cost and ROI:**
- **Training Cost**: $1K-10K per training run; amortized over multiple designs; one-time per design family
- **Inference Cost**: 6-24 hours on GPU; $100-1000; negligible compared to manual effort
- **QoR Improvement**: 10-25% better PPA; translates to competitive advantage; $10M-100M value
- **Design Time**: 10-100× faster; reduces time-to-market by weeks; $1M-10M value

AI-Driven Floorplanning represents **the automation of early-stage physical design** — by using RL agents with GNN encoders to learn optimal macro placement policies, AI achieves 10-25% better QoR than manual floorplanning in 6-24 hours vs weeks, as demonstrated by Google's superhuman TPU design, making AI-driven floorplanning essential for complex SoCs with 100-1000 macros where manual exploration of 10⁵⁰+ possible placements is impossible and early floorplan decisions determine 60-80% of final PPA.');

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/ai-floorplanning) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
