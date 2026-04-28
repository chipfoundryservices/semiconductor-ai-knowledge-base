# Fab Scheduling: Mathematical Modeling

**Keywords**: economic and scheduling mathematics,fab scheduling,queuing theory,little law,dispatching rules,stochastic optimization,capacity planning,cycle time,wip,throughput,oee

---

**Fab Scheduling: Mathematical Modeling**

A comprehensive technical reference on mathematical optimization, queueing theory, and computational methods for semiconductor manufacturing process scheduling.



   1. Problem Characteristics

Semiconductor fabrication (fab) scheduling is among the most complex scheduling problems in manufacturing. Key characteristics include:

-  Reentrant Flow : Wafers visit the same workstations multiple times (e.g., photolithography visited 30+ times at different "layers")
-  Scale : 
  - 400–800 processing steps per wafer
  - Hundreds of machines across dozens of workstations
  - Thousands of active lots representing hundreds of product types
  - Cycle times of 4–8 weeks
-  Sequence-Dependent Setup Times : Changeover time varies based on the product sequence
-  Batch Processing : Some machines (diffusion furnaces, wet etch) process multiple lots simultaneously
-  Machine Qualification : Not all machines can process all products—qualification restrictions apply
-  Queue Time Constraints : Maximum time limits between certain operations due to contamination risk
-  Rework : Defective wafers may require reprocessing
-  Hot Lots : Emergency/priority lots requiring expedited processing



   2. Mixed Integer Programming Formulations

    2.1 Sets and Indices

| Symbol | Description |
|--------|-------------|
| $J$ | Set of jobs (lots) |
| $O_j$ | Set of operations for job $j$ |
| $M$ | Set of machines |
| $M_{jo}$ | Set of machines capable of processing operation $o$ of job $j$ |

    2.2 Parameters

| Symbol | Description |
|--------|-------------|
| $p_{jom}$ | Processing time of operation $o$ of job $j$ on machine $m$ |
| $d_j$ | Due date of job $j$ |
| $w_j$ | Weight (priority) of job $j$ |
| $s_{jo,j'o'}^m$ | Setup time on machine $m$ when switching from $(j,o)$ to $(j',o')$ |

    2.3 Decision Variables

| Variable | Description |
|----------|-------------|
| $x_{jom} \in \{0,1\}$ | 1 if operation $o$ of job $j$ is assigned to machine $m$ |
| $y_{jo,j'o'}^m \in \{0,1\}$ | 1 if $(j,o)$ immediately precedes $(j',o')$ on machine $m$ |
| $S_{jo} \geq 0$ | Start time of operation $o$ of job $j$ |
| $C_{jo} \geq 0$ | Completion time of operation $o$ of job $j$ |

    2.4 Objective Function

 Minimize Weighted Tardiness: 

$$
\min \sum_{j \in J} w_j \cdot \max\left(0, \; C_{j,|O_j|} - d_j\right)
$$

 Alternative Objectives: 

- Minimize makespan: $\displaystyle \min \max_{j \in J} C_{j,|O_j|}$
- Maximize throughput: $\displaystyle \max \sum_{j \in J} \mathbf{1}_{[C_j \leq T]}$
- Minimize average cycle time: $\displaystyle \min \frac{1}{|J|} \sum_{j \in J} \left(C_{j,|O_j|} - r_j\right)$

    2.5 Constraints

 Machine Assignment  — Each operation assigned to exactly one qualified machine:

$$
\sum_{m \in M_{jo}} x_{jom} = 1 \quad \forall j \in J, \; \forall o \in O_j
$$

 Precedence  — Operations within a job follow sequence:

$$
C_{j,o-1} + \sum_{m \in M_{jo}} p_{jom} \cdot x_{jom} \leq C_{jo} \quad \forall j \in J, \; \forall o \in O_j, \; o > 1
$$

 Processing Time Relationship: 

$$
C_{jo} = S_{jo} + \sum_{m \in M_{jo}} p_{jom} \cdot x_{jom}
$$

 Disjunctive Constraints  — No overlap on machines (big-M formulation):

$$
C_{jo} + s_{jo,j'o'}^m + p_{j'o'm} \leq C_{j'o'} + M \cdot \left(1 - y_{jo,j'o'}^m\right)
$$

$$
C_{j'o'} + s_{j'o',jo}^m + p_{jom} \leq C_{jo} + M \cdot y_{jo,j'o'}^m
$$

 Queue Time Constraints: 

$$
S_{i,j+1} - C_{ij} \leq Q_{\max}^{(j)} \quad \text{for critical operation pairs}
$$

    2.6 Scalability Challenge

For a fab with:
- 100 machines
- 1,000 lots
- 500 operations per lot

The problem has approximately:

$$
\text{Binary variables} \approx 100 \times 1000 \times 500 = 5 \times 10^7
$$

This exceeds the capability of commercial MIP solvers, necessitating decomposition and heuristic methods.



   3. Batching Subproblem

    3.1 Additional Variables

| Variable | Description |
|----------|-------------|
| $z_{job} \in \{0,1\}$ | 1 if operation $o$ of job $j$ is assigned to batch $b$ |
| $B$ | Set of potential batches |
| $\text{cap}_m$ | Capacity of batch machine $m$ |

    3.2 Batching Constraints

 Unique Batch Assignment: 

$$
\sum_{b \in B} z_{job} = 1 \quad \forall j, o
$$

 Capacity Limit: 

$$
\sum_{j,o} z_{job} \leq \text{cap}_m \quad \forall b \in B
$$

 Simultaneous Completion  — All jobs in a batch complete together:

$$
C_{jo} = C_b \quad \text{if } z_{job} = 1
$$

 Compatibility  — Jobs in the same batch must have compatible recipes:

$$
z_{job} + z_{j'ob} \leq 1 \quad \text{if } \text{recipe}_j 
eq \text{recipe}_{j'}
$$

    3.3 Complexity

The batch scheduling subproblem is related to  bin packing  and is  NP-hard .



   4. Photolithography Scheduling

Photolithography (stepper/scanner tools) often forms the  bottleneck  workstation.

    4.1 Characteristics

- Each product-layer combination requires a specific  reticle 
- Reticle changes take 10–30 minutes
- Setup time matrix: $s_{ij}$ = time to switch from product $i$ to product $j$

    4.2 TSP-Like Formulation

Let $x_{ij} = 1$ if product $j$ immediately follows product $i$ in the schedule.

 Objective — Minimize Total Setup Time: 

$$
\min \sum_{i} \sum_{j} s_{ij} \cdot x_{ij}
$$

 Constraints: 

$$
\sum_{j} x_{ij} = 1 \quad \forall i \quad \text{(exactly one successor)}
$$

$$
\sum_{i} x_{ij} = 1 \quad \forall j \quad \text{(exactly one predecessor)}
$$

 Subtour Elimination (MTZ formulation): 

$$
u_i - u_j + n \cdot x_{ij} \leq n - 1 \quad \forall i 
eq j
$$

where $u_i$ is the position of product $i$ in the sequence.



   5. Queueing Network Models

    5.1 Open Queueing Network Approximation

Model each workstation $k$ as a queue:

| Parameter | Definition |
|-----------|------------|
| $\lambda_k$ | Arrival rate to station $k$ |
| $\mu_k$ | Service rate per machine at station $k$ |
| $c_k$ | Number of parallel machines at station $k$ |
| $\rho_k$ | Utilization: $\displaystyle \rho_k = \frac{\lambda_k}{c_k \cdot \mu_k}$ |

 Stability Condition: 

$$
\rho_k < 1 \quad \forall k
$$

    5.2 Little's Law

$$
L = \lambda \cdot W
$$

where:
- $L$ = average number in system (WIP)
- $\lambda$ = throughput
- $W$ = average time in system (cycle time)

 Implication:  $\text{Cycle Time} = \dfrac{\text{WIP}}{\text{Throughput}}$

    5.3 Kingman's Formula (G/G/1 Approximation)

For a single-server queue with general arrival and service distributions:

$$
W_q \approx \frac{\rho}{1 - \rho} \cdot \frac{C_a^2 + C_s^2}{2} \cdot \frac{1}{\mu}
$$

where:
- $C_a$ = coefficient of variation of inter-arrival times
- $C_s$ = coefficient of variation of service times
- $\rho$ = utilization
- $\mu$ = service rate

 Key Insights: 
- Waiting time  explodes  as $\rho \to 1$
-  Variability multiplies  waiting time (the $(C_a^2 + C_s^2)/2$ term)

    5.4 Multi-Server Approximation (G/G/c)

For $c$ parallel servers (heavy traffic):

$$
W_q \approx \frac{\rho^{\sqrt{2(c+1)} - 1}}{c \cdot \mu \cdot (1 - \rho)} \cdot \frac{C_a^2 + C_s^2}{2}
$$

    5.5 Total Cycle Time

Summing over all $K$ workstations:

$$
CT = \sum_{k=1}^{K} \left( W_{q,k} + \frac{1}{\mu_k} \right)
$$

    5.6 Fluid Model Dynamics

Approximate WIP levels $w_k(t)$ as continuous:

$$
\frac{dw_k}{dt} = \lambda_k(t) - \mu_k(t) \cdot \mathbf{1}_{[w_k(t) > 0]}
$$

    5.7 Diffusion Approximation

In heavy traffic, WIP fluctuates around the fluid solution:

$$
W_k(t) \approx \bar{W}_k + \sigma_k \cdot B(t)
$$

where $B(t)$ is standard Brownian motion.



   6. Hierarchical Planning Framework

| Level | Time Horizon | Decisions | Methods |
|-------|--------------|-----------|---------|
|  Strategic  | Months–Quarters | Capacity, product mix | LP, MIP |
|  Tactical  | Weeks | Lot release, target WIP | Queueing models, LP |
|  Operational  | Days | Machine allocation, batching | CP, decomposition, heuristics |
|  Real-Time  | Minutes | Dispatching | Rules, RL |

    6.1 Lot Release Control (CONWIP)

Maintain constant WIP level $W^*$:

$$
\text{Release rate} = \text{min}\left(\text{Demand rate}, \; \frac{W^* - \text{Current WIP}}{\text{Target CT}}\right)
$$


   7. Dispatching Rules

    7.1 Standard Rules

| Rule | Priority Metric | Strengths | Weaknesses |
|------|-----------------|-----------|------------|
|  FIFO  | Arrival time | Simple, fair | Ignores urgency |
|  SPT  | Processing time $p_j$ | Minimizes avg. CT | Starves long jobs |
|  EDD  | Due date $d_j$ | Reduces tardiness | Ignores processing time |
|  CR  | $\dfrac{d_j - t}{w_j}$ (slack/work) | Balances urgency | Complex to compute |
|  SRPT  | Remaining work $\sum_{o' \geq o} p_{jo'}$ | Minimizes WIP | Requires global info |

    7.2 Composite Rule

$$
\text{Priority}_j = w_1 \cdot \text{slack}_j + w_2 \cdot p_j + w_3 \cdot Q_{\text{remaining}} + w_4 \cdot \mathbf{1}_{[\text{bottleneck}]}
$$

where weights $w_1, w_2, w_3, w_4$ are tuned via simulation.

    7.3 Critical Ratio

$$
CR_j = \frac{d_j - t}{\sum_{o' \geq o} p_{jo'}}
$$

- $CR < 1$: Job is behind schedule (high priority)
- $CR = 1$: Job is on schedule
- $CR > 1$: Job is ahead of schedule



   8. Decomposition Methods

    8.1 Lagrangian Relaxation

 Original Problem: 

$$
\min \; f(x) \quad \text{s.t.} \quad g(x) \leq 0, \; h(x) = 0
$$

 Relaxed Problem  (dualize capacity constraints):

$$
L(\lambda) = \min_x \left\{ f(x) + \lambda^T g(x) \right\}
$$

 Subgradient Update: 

$$
\lambda^{(k+1)} = \max\left(0, \; \lambda^{(k)} + \alpha_k \cdot g(x^{(k)})\right)
$$

where $\alpha_k$ is the step size.

    8.2 Benders Decomposition

 Master Problem  (integer variables):

$$
\min \; c^T x + \theta \quad \text{s.t.} \quad Ax \geq b, \; \theta \geq \text{cuts}
$$

 Subproblem  (continuous variables, fixed $\bar{x}$):

$$
\min \; d^T y \quad \text{s.t.} \quad Wy \geq r - T\bar{x}
$$

 Benders Cut  (from dual solution $\pi$):

$$
\theta \geq \pi^T (r - Tx)
$$

    8.3 Column Generation

 Master Problem: 

$$
\min \sum_{s \in S'} c_s \lambda_s \quad \text{s.t.} \quad \sum_{s \in S'} a_s \lambda_s = b
$$

 Pricing Subproblem: 

$$
\min \; c_s - \pi^T a_s \quad \text{over feasible columns } s
$$

Add column $s$ to $S'$ if reduced cost < 0.



   9. Stochastic and Robust Optimization

    9.1 Two-Stage Stochastic Program

$$
\min_{x} \; c^T x + \mathbb{E}_{\xi}\left[Q(x, \xi)\right]
$$

where:
- $x$ = first-stage decisions (before uncertainty)
- $Q(x, \xi)$ = optimal recourse cost under scenario $\xi$

 Scenario Approximation: 

$$
\min_{x} \; c^T x + \frac{1}{N} \sum_{n=1}^{N} Q(x, \xi_n)
$$

    9.2 Robust Optimization

 Uncertainty Set: 

$$
\mathcal{U} = \left\{ p : |p - \bar{p}| \leq \Gamma \cdot \hat{p} \right\}
$$

 Robust Formulation: 

$$
\min_x \max_{\xi \in \mathcal{U}} f(x, \xi)
$$

 Tractable Reformulation  (for polyhedral uncertainty):

$$
\min_x \; c^T x + \Gamma \cdot \|d\|_1 \quad \text{s.t.} \quad Ax \geq b + Du
$$



   10. Machine Learning Approaches

    10.1 Reinforcement Learning for Dispatching

 MDP Formulation: 

| Component | Definition |
|-----------|------------|
|  State  $s_t$ | WIP by location, machine status, queue lengths, lot attributes |
|  Action  $a_t$ | Which lot to dispatch to available machine |
|  Reward  $r_t$ | Throughput bonus, tardiness penalty, queue violation penalty |
|  Transition  $P(s_{t+1} \| s_t, a_t)$ | Determined by processing times and arrivals |

 Q-Learning Update: 

$$
Q(s, a) \leftarrow Q(s, a) + \alpha \left[ r + \gamma \max_{a'} Q(s', a') - Q(s, a) \right]
$$

 Deep Q-Network (DQN): 

$$
\mathcal{L}(\theta) = \mathbb{E}\left[ \left( r + \gamma \max_{a'} Q(s', a'; \theta^-) - Q(s, a; \theta) \right)^2 \right]
$$

    10.2 Graph Neural Networks

Represent fab as graph $G = (V, E)$:
-  Nodes : Machines, buffers, lots
-  Edges : Material flow, machine-buffer connections

 Message Passing: 

$$
h_v^{(l+1)} = \sigma \left( W^{(l)} \cdot \text{AGGREGATE}\left( \{ h_u^{(l)} : u \in \mathcal{N}(v) \} \right) \right)
$$



   11. Performance Metrics

    11.1 Key Performance Indicators

| Metric | Formula | Target |
|--------|---------|--------|
|  Cycle Time  | $CT = C_j - r_j$ | Minimize |
|  Throughput  | $TH = \dfrac{\text{Lots completed}}{\text{Time period}}$ | Maximize |
|  WIP  | $\text{WIP} = \sum_k w_k$ | Control to target |
|  On-Time Delivery  | $OTD = \dfrac{|\{j : C_j \leq d_j\}|}{|J|}$ | $\geq 95\%$ |
|  Utilization  | $U_m = \dfrac{\text{Busy time}}{\text{Available time}}$ | 85–95% |

    11.2 Cycle Time Components

$$
CT = \underbrace{\sum_o p_o}_{\text{Raw Process Time}} + \underbrace{\sum_o W_{q,o}}_{\text{Queue Time}} + \underbrace{\sum_o s_o}_{\text{Setup Time}} + \underbrace{T_{\text{wait}}}_{\text{Batch Wait}}
$$

    11.3 X-Factor

$$
X = \frac{\text{Actual Cycle Time}}{\text{Raw Process Time}}
$$

- Typical fab: $X \in [2, 4]$
- World-class: $X < 2$

    11.4 Multi-Objective Pareto Analysis

 ε-Constraint Method: 

$$
\min f_1(x) \quad \text{s.t.} \quad f_2(x) \leq \epsilon_2, \; f_3(x) \leq \epsilon_3, \ldots
$$

Vary $\epsilon$ to trace the Pareto frontier.



   12. Computational Complexity

    12.1 Complexity Results

| Problem Variant | Complexity |
|-----------------|------------|
| Single machine, sequence-dependent setup |  NP-hard  |
| Flow shop with reentrant routing |  NP-hard  |
| Batch scheduling with incompatibilities |  NP-hard  |
| Parallel machine with eligibility |  NP-hard  |
| General job shop |  NP-hard  (strongly) |

    12.2 Approximation Guarantees

For single-machine weighted completion time:

$$
\text{WSPT rule achieves} \quad \frac{OPT}{ALG} \geq \frac{1}{2}
$$

For parallel machines (LPT rule, makespan):

$$
\frac{ALG}{OPT} \leq \frac{4}{3} - \frac{1}{3m}
$$



   Principles:

1.  Variability is the enemy  — Reducing $C_a$ and $C_s$ shrinks cycle time more than adding capacity

2.  Bottleneck management dominates  — Optimize the constraining resource; non-bottleneck optimization often has zero effect

3.  WIP control matters  — Lower WIP (via CONWIP or caps) reduces cycle time even if utilization drops slightly

4.  Hierarchical decomposition is essential  — No single model spans strategic to real-time decisions

5.  Validation requires simulation  — Analytical models provide insight; DES captures full complexity

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/economic-and-scheduling-mathematics) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
