# Semiconductor Manufacturing Process: Layout Mathematical Modeling

**Keywords**: layout mathematics

---

**Semiconductor Manufacturing Process: Layout Mathematical Modeling**

**1. Problem Context**

A modern semiconductor fabrication facility (fab) involves:

**Process Complexity**

- **500–1000+ individual process steps per wafer**
- **Multiple product types with different process routes**
- **Strict process sequencing and timing requirements**

**Re-entrant Flow Characteristics**

- **Wafers revisit the same tool types** (e.g., lithography) 30–80 times
- **Creates complex dependencies** between process stages
- **Traditional flow-shop models are inadequate**

**Stochastic Elements**

- **Tool failures and unplanned maintenance**
- **Variable processing times**
- **Yield loss at various process steps**
- **Operator availability fluctuations**

**Economic Scale**

- **Leading-edge fab costs**: $15–20+ billion
- **Equipment costs**: $50M–$150M per lithography tool
- **High cost of WIP** (work-in-process) inventory

**2. Core Mathematical Formulations**

**2.1 Quadratic Assignment Problem (QAP)**

The foundational model for facility layout optimization:

$$
\min \sum_{i=1}^{n} \sum_{j=1}^{n} \sum_{k=1}^{n} \sum_{l=1}^{n} f_{ij} \cdot d_{kl} \cdot x_{ik} \cdot x_{jl}
$$

**Subject to:**

$$
\sum_{k=1}^{n} x_{ik} = 1 \quad \forall i \in \{1, \ldots, n\}
$$

$$
\sum_{i=1}^{n} x_{ik} = 1 \quad \forall k \in \{1, \ldots, n\}
$$
$$
x_{ik} \in \{0, 1\} \quad \forall i, k
$$

**Variables:**

| Symbol | Description |
|--------|-------------|
| $f_{ij}$ | Material flow frequency between tool groups $i$ and $j$ |
| $d_{kl}$ | Distance between locations $k$ and $l$ |
| $x_{ik}$ | Binary: 1 if tool group $i$ assigned to location $k$, 0 otherwise |
| $n$ | Number of departments/locations |

**Complexity Analysis:**

- **Problem Class**: NP-hard
- **Practical Limit**: Exact solutions feasible for $n \leq 30$
- **Large Instances**: Require heuristic/metaheuristic approaches

**2.2 Mixed-Integer Linear Programming (MILP) Extension**

For realistic industrial constraints:

$$
\min \sum_{i,j} c_{ij} \cdot f_{ij} \cdot z_{ij} + \sum_{k} F_k \cdot y_k
$$

**Capacity Constraint:**

$$
\sum_{p \in \mathcal{P}} d_p \cdot t_{pk} \leq C_k \cdot A_k \cdot y_k \quad \forall k
$$

**Space Constraint:**

$$
\sum_{i} a_i \cdot x_{ik} \leq S_k \quad \forall k
$$

**Adjacency Requirement (linearized):**

$$
x_{ik} + x_{jl} \leq 1 + M \cdot (1 - \text{adj}_{kl}) \quad \forall (i,j) \in \mathcal{R}
$$

**Variables:**

| Symbol | Description |
|--------|-------------|
| $c_{ij}$ | Unit transport cost between $i$ and $j$ |
| $z_{ij}$ | Distance variable (linearized) |
| $y_k$ | Binary: tool purchase decision for type $k$ |
| $F_k$ | Fixed cost for tool type $k$ |
| $d_p$ | Demand for product $p$ |
| $t_{pk}$ | Processing time for product $p$ on tool $k$ |
| $C_k$ | Capacity of tool type $k$ |
| $A_k$ | Availability factor for tool $k$ |
| $a_i$ | Floor area required by department $i$ |
| $S_k$ | Available space in zone $k$ |
| $M$ | Big-M constant |
| $\mathcal{R}$ | Set of required adjacency pairs |

**2.3 Network Flow Formulation**

Wafer flow modeled as a **multi-commodity network flow problem**:

$$
\min \sum_{(i,j) \in E} \sum_{p \in \mathcal{P}} c_{ij} \cdot x_{ij}^p
$$

**Flow Conservation Constraint:**

$$
\sum_{j:(i,j) \in E} x_{ij}^p - \sum_{j:(j,i) \in E} x_{ji}^p = b_i^p \quad \forall i \in V, \forall p \in \mathcal{P}
$$

**Arc Capacity Constraint:**

$$
\sum_{p \in \mathcal{P}} x_{ij}^p \leq u_{ij} \quad \forall (i,j) \in E
$$

**Variables:**

| Symbol | Description |
|--------|-------------|
| $E$ | Set of arcs (edges) in the network |
| $V$ | Set of nodes (vertices) |
| $\mathcal{P}$ | Set of product types (commodities) |
| $x_{ij}^p$ | Flow of product $p$ on arc $(i,j)$ |
| $c_{ij}$ | Cost per unit flow on arc $(i,j)$ |
| $b_i^p$ | Net supply/demand of product $p$ at node $i$ |
| $u_{ij}$ | Capacity of arc $(i,j)$ |

**3. Queuing Network Models**
**3.1 Fundamental Performance Metrics**

**Little's Law** (fundamental relationship):

$$
L = \lambda \cdot W
$$

Equivalently:

$$
\text{WIP} = \text{Throughput} \times \text{Cycle Time}
$$

**Station Utilization:**

$$
\rho_k = \frac{\lambda \cdot v_k}{\mu_k \cdot m_k}
$$

**Definitions:**

- $L$ — Average number in system (WIP)
- $\lambda$ — Arrival rate (throughput)
- $W$ — Average time in system (cycle time)
- $\rho_k$ — Utilization of station $k$
- $v_k$ — Average number of visits to station $k$ per wafer
- $\mu_k$ — Service rate at station $k$
- $m_k$ — Number of parallel tools at station $k$

**3.2 Cycle Time Approximation**

**Kingman's Formula (GI/G/1 approximation):**

$$
W_q \approx \left( \frac{C_a^2 + C_s^2}{2} \right) \cdot \left( \frac{\rho}{1 - \rho} \right) \cdot \bar{s}
$$

**Extended GI/G/m Approximation:**

$$
CT_k \approx t_k \cdot \left[ 1 + \frac{C_a^2 + C_s^2}{2} \cdot \frac{\rho_k^{\sqrt{2(m_k+1)}-1}}{m_k \cdot (1-\rho_k)} \right]
$$

**Total Cycle Time:**

$$
CT_{\text{total}} = \sum_{k \in \mathcal{K}} v_k \cdot CT_k + \sum_{\text{moves}} T_{\text{transport}}
$$

**Variables:**

| Symbol | Description |
|--------|-------------|
| $W_q$ | Average waiting time in queue |
| $C_a^2$ | Squared coefficient of variation of inter-arrival times |
| $C_s^2$ | Squared coefficient of variation of service times |
| $\bar{s}$ | Mean service time |
| $t_k$ | Mean processing time at station $k$ |
| $CT_k$ | Cycle time at station $k$ |
| $\mathcal{K}$ | Set of all stations |
| $T_{\text{transport}}$ | Transport time between stations |

**3.3 Re-entrant Flow Complexity**

**Characteristics of Re-entrant Systems:**

- **Variability Propagation**: Variance accumulates through network
- **Correlation Effects**: Successive visits to same station are correlated
- **Priority Inversions**: Lots at different stages compete for same resources

**Variability Propagation (Linking Equation):**

$$
C_{a,j}^2 = 1 + \sum_{i} p_{ij}^2 \cdot \frac{\lambda_i}{\lambda_j} \cdot (C_{d,i}^2 - 1)
$$

**Departure Variability:**

$$
C_{d,k}^2 = 1 + (1 - \rho_k^2) \cdot (C_{a,k}^2 - 1) + \rho_k^2 \cdot (C_{s,k}^2 - 1)
$$

Where:

- $p_{ij}$ — Routing probability from station $i$ to $j$
- $C_{d,k}^2$ — Squared CV of departures from station $k$

**4. Stochastic Modeling**

**4.1 Random Variable Distributions**

| Element | Typical Distribution | Parameters |
|---------|---------------------|------------|
| Processing time | Log-normal | $\mu, \sigma$ (log-scale) |
| Tool failure (TTF) | Exponential / Weibull | $\lambda$ or $(\eta, \beta)$ |
| Repair time (TTR) | Log-normal | $\mu, \sigma$ |
| Yield | Beta / Truncated Normal | $(\alpha, \beta)$ or $(\mu, \sigma, a, b)$ |
| Batch size | Discrete (Poisson) | $\lambda$ |

**Log-normal PDF:**

$$
f(x; \mu, \sigma) = \frac{1}{x \sigma \sqrt{2\pi}} \exp\left( -\frac{(\ln x - \mu)^2}{2\sigma^2} \right), \quad x > 0
$$

**Weibull PDF (for reliability):**

$$
f(x; \eta, \beta) = \frac{\beta}{\eta} \left( \frac{x}{\eta} \right)^{\beta - 1} \exp\left( -\left( \frac{x}{\eta} \right)^\beta \right), \quad x \geq 0
$$

**4.2 Markov Decision Process (MDP) Formulation**

For sequential decision-making under uncertainty:

**Bellman Equation:**

$$
V^*(s) = \max_{a \in \mathcal{A}(s)} \left[ R(s, a) + \gamma \sum_{s' \in \mathcal{S}} P(s' | s, a) \cdot V^*(s') \right]
$$

**Optimal Policy:**

$$
\pi^*(s) = \arg\max_{a \in \mathcal{A}(s)} \left[ R(s, a) + \gamma \sum_{s' \in \mathcal{S}} P(s' | s, a) \cdot V^*(s') \right]
$$

**MDP Components:**

| Component | Description | Example in Fab Context |
|-----------|-------------|------------------------|
| $\mathcal{S}$ | State space | Queue lengths, tool status, lot positions |
| $\mathcal{A}(s)$ | Action set at state $s$ | Dispatch rules, maintenance decisions |
| $P(s' \| s, a)$ | Transition probability | Probability of tool failure/repair |
| $R(s, a)$ | Immediate reward | Negative cycle time, throughput |
| $\gamma$ | Discount factor | $\gamma \in [0, 1)$ |

**5. Hierarchical Layout Structure**

**5.1 Bay Layout Architecture**

Modern fabs use a hierarchical **bay layout**:
```text
│─────────────────────────────────────────────────────────────│
│    Bay 1      │    Bay 2      │    Bay 3      │    Bay 4    │
│  (Lithography)│    (Etch)     │  (Deposition) │    (CMP)    │
├───────────────┴───────────────┴───────────────┴─────────────┤
│              INTERBAY AMHS (Overhead Hoist Transport)       │
├───────────────┬───────────────┬───────────────┬─────────────┤
│    Bay 5      │    Bay 6      │    Bay 7      │    Bay 8    │
│  (Implant)    │  (Metrology)  │   (Diffusion) │   (Clean)   │
│───────────────┴───────────────┴───────────────┴─────────────│
```

**Two-Level Optimization:**

1. **Macro Level**: Assign tool groups to bays
   - Objective: Minimize interbay transport
   - Constraints: Bay capacity, cleanroom class requirements

2. **Micro Level**: Arrange tools within each bay
   - Objective: Minimize within-bay movement
   - Constraints: Tool footprint, utility access

**5.2 Distance Metrics**

**Rectilinear (Manhattan) Distance:**

$$
d(k, l) = |x_k - x_l| + |y_k - y_l|
$$

**Euclidean Distance:**

$$
d(k, l) = \sqrt{(x_k - x_l)^2 + (y_k - y_l)^2}
$$

**Actual AMHS Path Distance:**

$$
d_{\text{AMHS}}(k, l) = \sum_{(i,j) \in \text{path}(k,l)} d_{ij} + \sum_{\text{intersections}} \tau_{\text{delay}}
$$

Where $(x_k, y_k)$ and $(x_l, y_l)$ are coordinates of locations $k$ and $l$.

**6. Objective Functions**

**6.1 Multi-Objective Formulation**

$$
\min \mathbf{F}(\mathbf{x}) = \begin{bmatrix} f_1(\mathbf{x}) \\ f_2(\mathbf{x}) \\ f_3(\mathbf{x}) \\ f_4(\mathbf{x}) \end{bmatrix} = \begin{bmatrix} \text{Material Handling Cost} \\ \text{Cycle Time} \\ \text{Work-in-Process (WIP)} \\ -\text{Throughput} \end{bmatrix}
$$

**6.2 Individual Objective Functions**

**Material Handling Cost:**

$$
f_1(\mathbf{x}) = \sum_{i < j} f_{ij} \cdot d(\pi(i), \pi(j)) \cdot c_{\text{transport}}
$$

**Cycle Time:**

$$
f_2(\mathbf{x}) = \sum_{k \in \mathcal{K}} v_k \cdot \left[ t_k + W_{q,k}(\mathbf{x}) \right] + \sum_{\text{moves}} T_{\text{transport}}(\mathbf{x})
$$

**Work-in-Process:**

$$
f_3(\mathbf{x}) = \sum_{k \in \mathcal{K}} L_k(\mathbf{x}) = \sum_{k \in \mathcal{K}} \lambda_k \cdot W_k(\mathbf{x})
$$

**Throughput (bottleneck-constrained):**

$$
f_4(\mathbf{x}) = -X = -\min_{k \in \mathcal{K}} \left( \frac{\mu_k \cdot m_k}{v_k} \right)
$$

**Variables:**

| Symbol | Description |
|--------|-------------|
| $\pi(i)$ | Location assigned to department $i$ |
| $c_{\text{transport}}$ | Unit transport cost |
| $W_{q,k}$ | Waiting time at station $k$ |
| $L_k$ | Average queue length at station $k$ |
| $X$ | System throughput |

**6.3 Weighted-Sum Scalarization**

$$
\min F(\mathbf{x}) = \sum_{i=1}^{4} w_i \cdot \frac{f_i(\mathbf{x}) - f_i^{\min}}{f_i^{\max} - f_i^{\min}}
$$

Where:

- $w_i$ — Weight for objective $i$ (with $\sum_i w_i = 1$)
- $f_i^{\min}, f_i^{\max}$ — Normalization bounds for objective $i$

**7. Constraint Categories**

**7.1 Constraint Summary Table**

| Category | Mathematical Form | Description |
|----------|-------------------|-------------|
| **Space** | $\sum_i A_i \cdot x_{ik} \leq S_k$ | Total area in zone $k$ |
| **Adjacency (required)** | $\| \text{loc}(i) - \text{loc}(j) \| \leq \delta_{ij}$ | Tools must be close |
| **Separation (forbidden)** | $\| \text{loc}(i) - \text{loc}(j) \| \geq \Delta_{ij}$ | Tools must be apart |
| **Cleanroom class** | $\text{class}(\text{loc}(i)) \geq \text{req}_i$ | Cleanliness requirement |
| **Utility access** | $\sum_{i \in \text{zone}} \text{power}_i \leq P_{\text{zone}}$ | Power budget |
| **Aspect ratio** | $L/W \in [r_{\min}, r_{\max}]$ | Layout shape |

**7.2 Detailed Constraint Formulations**

**Non-Overlapping Constraint (for unequal areas):**

$$
x_i + w_i \leq x_j + M(1 - \alpha_{ij}) \quad \text{OR}
$$

$$
x_j + w_j \leq x_i + M(1 - \beta_{ij}) \quad \text{OR}
$$

$$
y_i + h_i \leq y_j + M(1 - \gamma_{ij}) \quad \text{OR}
$$

$$
y_j + h_j \leq y_i + M(1 - \delta_{ij})
$$

With:

$$
\alpha_{ij} + \beta_{ij} + \gamma_{ij} + \delta_{ij} \geq 1
$$

**Cleanroom Zone Assignment:**

$$
\sum_{k \in \mathcal{Z}_c} x_{ik} = 1 \quad \forall i \text{ with } \text{req}_i = c
$$

Where $\mathcal{Z}_c$ is the set of locations with cleanroom class $c$.

**8. Solution Methods**

**8.1 Exact Methods**

**Applicable for small instances ($n \leq 30$):**

- **Branch and Bound**:
  - Uses Gilmore-Lawler bound for pruning
  - Lower bound: $\text{LB} = \sum_{i} \min_k \{ \text{flow}_i \cdot \text{dist}_k \}$

- **Dynamic Programming**:
  - For special structures (e.g., single-row layout)
  - Complexity: $O(n^2 \cdot 2^n)$ for general case

- **Cutting Plane Methods**:
  - Linearize QAP using reformulation-linearization technique (RLT)

**8.2 Construction Heuristics**

**CRAFT (Computerized Relative Allocation of Facilities Technique):**

```text
│─────────────────────────────────────────────────────────────│
│ Algorithm CRAFT:                                            │
│ 1. Start with initial layout                                │
│ 2. Evaluate all pairwise exchanges                          │
│ 3. Select exchange with maximum cost reduction              │
│ 4. If improvement found, goto step 2                        │
│ 5. Return final layout                                      │
│─────────────────────────────────────────────────────────────│
```

**CORELAP (Computerized Relationship Layout Planning):**

```text
│────────────────────────────────────────────────────────────│
│ Algorithm CORELAP:                                         │
│ 1. Calculate Total Closeness Rating (TCR) for each dept    │
│ 2. Place department with highest TCR at center             │
│ 3. For remaining departments:                              │
│    a. Calculate placement score for candidate locations    │
│    b. Place dept at location maximizing adjacency          │
│ 4. Return layout                                           │
│────────────────────────────────────────────────────────────│
```

**ALDEP (Automated Layout Design Program):**

```text
│─────────────────────────────────────────────────────────────│
│ Algorithm ALDEP:                                            │
│ 1. Randomly select first department                         │
│ 2. Scan relationship matrix for high-rated pairs            │
│ 3. Place related departments in sequence                    │
│ 4. Repeat until all departments placed                      │
│ 5. Evaluate layout; repeat for multiple random starts       │
│─────────────────────────────────────────────────────────────│
```

**8.3 Metaheuristics**

**Genetic Algorithm (GA):**

```text
│────────────────────────────────────────────────────────────│
│ Algorithm GA_for_Layout:                                   │
│   Initialize population P of size N (random permutations)  │
│   Evaluate fitness f(x) for all x in P                     │
│                                                            │
│   While not converged:                                     │
│     Selection:                                             │
│       Parents = TournamentSelect(P, k=3)                   │
│     Crossover (PMX or OX for permutations):                │
│       Offspring = PMX_Crossover(Parents, p_c=0.8)          │
│     Mutation (swap or insertion):                          │
│       Offspring = SwapMutation(Offspring, p_m=0.1)         │
│     Evaluation:                                            │
│       Evaluate fitness for Offspring                       │
│     Replacement:                                           │
│       P = ElitistReplacement(P, Offspring)                 │
│                                                            │
│   Return best solution in P                                │
│────────────────────────────────────────────────────────────│
```

**Simulated Annealing (SA):**

$$
P(\text{accept worse solution}) = \exp\left( -\frac{\Delta f}{T} \right)
$$

```text
│────────────────────────────────────────────────────────────│
│ Algorithm SA_for_Layout:                                   │
│   x = initial_solution()                                   │
│   T = T_initial                                            │
│                                                            │
│   While T > T_final:                                       │
│     For i = 1 to iterations_per_temp:                      │
│       x' = neighbor(x)  (e.g., swap two departments)       │
│       Δf = f(x') - f(x)                                    │
│                                                            │
│       If Δf < 0:                                           │
│         x = x'                                             │
│       Else If random() < exp(-Δf / T):                     │
│         x = x'                                             │
│                                                            │
│     T = α × T  (Cooling, α ≈ 0.95)                         │
│                                                            │
│   Return x                                                 │
│────────────────────────────────────────────────────────────│
```

**Cooling Schedule:**

$$
T_{k+1} = \alpha \cdot T_k, \quad \alpha \in [0.9, 0.99]
$$

**8.4 Simulation-Optimization Framework**

```text
│─────────────│     │──────────────────│     │─────────────────│
│   Layout    │────▶│  Discrete-Event  │────▶│   Performance   │
│  Solution   │     │   Simulation     │     │    Metrics      │
│─────────────│     │──────────────────│     │────────┬────────│
      ▲                                              │
      │                                              │
      │         │──────────────────│                 │
      │─────────│   Optimization   │◀────────────────│
                │    Algorithm     │
                │──────────────────│
```

**Surrogate-Assisted Optimization:**

$$
\hat{f}(\mathbf{x}) \approx f(\mathbf{x})
$$

Where $\hat{f}$ is a surrogate model (e.g., Gaussian Process, Neural Network) trained on simulation evaluations.

**9. Advanced Topics**

**9.1 Digital Twin Integration**

**Real-Time Layout Performance:**

$$
\text{KPI}(t) = g\left( \mathbf{x}_{\text{layout}}, \mathbf{s}(t), \boldsymbol{\theta}(t) \right)
$$

Where:

- $\mathbf{s}(t)$ — System state at time $t$
- $\boldsymbol{\theta}(t)$ — Real-time parameter estimates

**Applications:**

- Real-time cycle time prediction
- Predictive maintenance scheduling
- Dynamic dispatching optimization

**9.2 Machine Learning Hybridization**

**Graph Neural Network (GNN) for Layout:**

$$
\mathbf{h}_v^{(l+1)} = \sigma\left( \mathbf{W}^{(l)} \cdot \text{AGGREGATE}\left( \{ \mathbf{h}_u^{(l)} : u \in \mathcal{N}(v) \} \right) \right)
$$

**Reinforcement Learning for Dispatching:**

$$
Q(s, a) \leftarrow Q(s, a) + \alpha \left[ r + \gamma \max_{a'} Q(s', a') - Q(s, a) \right]
$$

**Surrogate Model (Neural Network):**

$$
\hat{CT}(\mathbf{x}) = \text{NN}_\theta(\mathbf{x}) \approx \mathbb{E}[\text{Simulation}(\mathbf{x})]
$$

**9.3 Robust Optimization**

**Min-Max Formulation:**

$$
\min_{\mathbf{x} \in \mathcal{X}} \max_{\boldsymbol{\xi} \in \mathcal{U}} f(\mathbf{x}, \boldsymbol{\xi})
$$

**Uncertainty Set (Polyhedral):**

$$
\mathcal{U} = \left\{ \boldsymbol{\xi} : \| \boldsymbol{\xi} - \bar{\boldsymbol{\xi}} \| _\infty \leq \Gamma \right\}
$$

**Chance-Constrained Formulation:**

$$
\min_{\mathbf{x}} \mathbb{E}[f(\mathbf{x}, \boldsymbol{\xi})]
$$

$$
\text{s.t.} \quad P\left( g(\mathbf{x}, \boldsymbol{\xi}) \leq 0 \right) \geq 1 - \epsilon
$$

Where:

- $\boldsymbol{\xi}$ — Uncertain parameters (demand, yield, tool availability)
- $\mathcal{U}$ — Uncertainty set
- $\Gamma$ — Budget of uncertainty
- $\epsilon$ — Acceptable violation probability

**9.4 Multi-Objective Optimization**

**Pareto Optimality:**

Solution $\mathbf{x}^*$ is Pareto optimal if there exists no $\mathbf{x}$ such that:

$$
f_i(\mathbf{x}) \leq f_i(\mathbf{x}^*) \quad \forall i \quad \text{and} \quad f_j(\mathbf{x}) < f_j(\mathbf{x}^*) \quad \text{for some } j
$$

**NSGA-II Crowding Distance:**

$$
d_i = \sum_{m=1}^{M} \frac{f_m^{(i+1)} - f_m^{(i-1)}}{f_m^{\max} - f_m^{\min}}
$$

**10. Key Insights**

**10.1 Fundamental Observations**

1. **Multi-Scale Nature**:
   - Nanometer-scale process physics
   - Meter-scale equipment layout
   - Kilometer-scale supply chain

2. **Re-entrant Flow Complexity**:
   - Traditional queuing theory requires significant adaptation
   - Correlation effects are significant
   - Scheduling and layout are tightly coupled

3. **Simulation Necessity**:
   - Analytical models sacrifice too much fidelity
   - High-fidelity simulation essential for validation
   - Surrogate models bridge the gap

4. **Layout-Scheduling Interaction**:
   - Optimal layout depends on dispatch policy
   - Optimal dispatch depends on layout
   - Joint optimization is active research area

5. **Industry Trends Impact Modeling**:
   - EUV lithography changes bottleneck structure
   - 3D integration (chiplets, stacking) changes flow patterns
   - High-mix low-volume increases variability

**10.2 Practical Recommendations**

- **Start with QAP formulation** for initial layout
- **Use queuing models** for performance estimation
- **Validate with discrete-event simulation**
- **Apply metaheuristics** for large-scale instances
- **Consider multi-objective formulation** for trade-off analysis
- **Integrate digital twin** for real-time optimization

**Symbol Reference**

| Symbol | Description | Typical Units |
|--------|-------------|---------------|
| $n$ | Number of departments/tools | — |
| $f_{ij}$ | Flow frequency | lots/hour |
| $d_{kl}$ | Distance | meters |
| $\lambda$ | Arrival rate | lots/hour |
| $\mu$ | Service rate | lots/hour |
| $\rho$ | Utilization | — |
| $CT$ | Cycle time | hours |
| $WIP$ | Work-in-process | lots |
| $X$ | Throughput | lots/hour |
| $C^2$ | Squared coefficient of variation | — |
| $m$ | Number of parallel servers | — |

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/layout-mathematics) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
