# Simulated Annealing for Placement

**Keywords**: simulated annealing placement,sa optimization algorithm,temperature schedule annealing,metropolis criterion acceptance,annealing convergence chip

---

**Simulated Annealing for Placement** is **the probabilistic optimization algorithm inspired by metallurgical annealing that iteratively improves chip placement by accepting both beneficial and occasionally detrimental moves with temperature-controlled probability — enabling escape from local optima through controlled randomness that decreases over time, making it the dominant algorithm for standard cell placement in commercial EDA tools for over three decades**.

**Annealing Algorithm Framework:**
- **Initial Solution**: random placement or constructive heuristic (quadratic placement, min-cut partitioning); initial temperature T₀ set high enough to accept 80-95% of moves; ensures thorough exploration of design space in early iterations
- **Move Generation**: randomly select cell or cell pair; propose new position (random location, swap with another cell, or small perturbation); move types include single-cell moves, cell swaps, region-based moves, and window-based optimization
- **Cost Function**: evaluates placement quality; typically weighted sum of half-perimeter wirelength (HPWL), timing slack violations, density violations, and routing congestion estimates; incremental cost computation updates only affected nets for efficiency
- **Acceptance Criterion (Metropolis)**: accept move if ΔCost < 0 (improvement); accept with probability exp(-ΔCost/T) if ΔCost > 0 (degradation); higher temperature T allows more uphill moves; enables escape from local minima

**Temperature Schedule:**
- **Geometric Cooling**: T_{k+1} = α·T_k where α = 0.85-0.95; simple and widely used; cooling rate α controls exploration-exploitation trade-off; slower cooling (α closer to 1) improves solution quality but increases runtime
- **Adaptive Cooling**: adjust cooling rate based on acceptance ratio; slow cooling when acceptance ratio is high (still exploring); fast cooling when acceptance ratio drops (converging); maintains effective search throughout optimization
- **Reheating**: periodically increase temperature when stuck in local optimum; acceptance ratio drops below threshold triggers reheat; enables escape from poor local minima; multiple cooling-reheating cycles improve robustness
- **Stopping Criteria**: terminate when temperature drops below threshold (T < 0.01·T₀), acceptance ratio falls below 1-5%, or maximum iterations reached; typical SA run performs 10⁶-10⁹ moves depending on design size

**Placement-Specific Optimizations:**
- **Range Limiting**: restrict move distance based on temperature; large moves at high temperature (global exploration); small moves at low temperature (local refinement); move range proportional to √T or exponentially decreasing
- **Net Weighting**: critical timing paths assigned higher weights in cost function; timing-driven SA focuses optimization effort on critical nets; weights updated periodically based on timing analysis
- **Density Management**: divide die into bins; track cell density per bin; penalize moves that create high-density regions; prevents routing congestion by maintaining uniform cell distribution
- **Incremental Timing**: fast incremental timing analysis after each move; avoids full static timing analysis (too expensive per move); Elmore delay model or lookup-table-based delay estimation provides quick timing estimates

**Hybrid and Parallel SA:**
- **Hierarchical SA**: partition design into regions; optimize each region independently; global SA optimizes region-level placement; local SA refines within regions; reduces problem size and enables parallelization
- **Parallel SA**: multiple independent SA runs with different random seeds; select best result; embarrassingly parallel; linear speedup with number of processors; alternative: parallel moves with conflict detection
- **SA + Analytical Placement**: analytical placement (quadratic wirelength minimization) provides initial solution; SA refines to legalize overlaps and optimize discrete objectives; combines speed of analytical methods with quality of SA
- **SA + Partitioning**: min-cut partitioning creates coarse placement; SA refines within partitions; reduces search space while maintaining global structure; faster convergence than pure SA

**Commercial Tool Implementations:**
- **Cadence Innovus**: simulated annealing for detailed placement refinement; follows analytical global placement; SA optimizes timing, power, and routability; adaptive temperature schedule based on design characteristics
- **Synopsys IC Compiler**: SA-based incremental placement optimization; handles ECOs and timing-driven optimization; parallel SA across multiple cores; integrated with timing and power analysis engines
- **Academic Tools (Capo, FastPlace)**: research implementations demonstrate SA effectiveness; open-source availability enables algorithm research; competitive with commercial tools on academic benchmarks
- **Analog Placement**: SA widely used for analog layout where precise device matching and symmetry constraints are critical; handles complex constraints better than analytical methods

**Performance Characteristics:**
- **Solution Quality**: SA consistently produces high-quality placements; within 2-5% of optimal for small designs where optimal is known; outperforms greedy heuristics by 10-30% on complex designs
- **Runtime**: SA runtime scales as O(n·log n) to O(n²) depending on move strategy and cost function; typical runtime 30 minutes to 4 hours for million-cell designs; slower than analytical placement but produces better final quality
- **Tuning Sensitivity**: performance depends on temperature schedule, move types, and cost function weights; requires expert tuning for optimal results; modern tools use adaptive parameters to reduce tuning burden
- **Convergence Guarantees**: SA provably converges to global optimum with infinitely slow cooling (impractical); practical cooling schedules find near-optimal solutions with high probability; multiple runs with different seeds improve robustness

**Modern Alternatives and Comparisons:**
- **Analytical Placement**: faster than SA (minutes vs hours); produces good initial placement but may have legalization issues; often used as SA initialization
- **Machine Learning Placement**: RL-based placement shows promise; currently slower than SA but improving; may eventually replace SA for certain design types
- **Hybrid Approaches**: modern placers combine analytical global placement, SA-based detailed placement, and ML-guided optimization; leverages strengths of each method

Simulated annealing for placement represents **the gold standard of placement optimization for decades — its ability to escape local optima through controlled randomness, handle arbitrary cost functions including discrete constraints, and consistently produce high-quality results has made it the algorithm of choice for detailed placement refinement in virtually every commercial EDA tool despite the emergence of newer optimization paradigms**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/simulated-annealing-placement) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
