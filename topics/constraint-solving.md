# Constraint solving

**Keywords**: constraint solving

---

**Constraint solving** is the process of **finding values for variables that satisfy a set of constraints** — determining assignments that make all specified conditions true, or proving that no such assignment exists, enabling automated problem-solving across diverse domains from scheduling to program verification.

**What Is Constraint Solving?**

- **Variables**: Unknowns to be determined — x, y, z, etc.
- **Domains**: Possible values for variables — integers, reals, booleans, finite sets.
- **Constraints**: Conditions that must be satisfied — equations, inequalities, logical formulas.
- **Solution**: Assignment of values to variables satisfying all constraints.

**Types of Constraint Problems**

- **Boolean Satisfiability (SAT)**: Variables are boolean, constraints are logical formulas.
  - Example: (x ∨ y) ∧ (¬x ∨ z)

- **Constraint Satisfaction Problem (CSP)**: Variables have finite domains, constraints are relations.
  - Example: Sudoku, graph coloring, scheduling.

- **Integer Linear Programming (ILP)**: Variables are integers, constraints are linear inequalities.
  - Example: Optimization problems with integer variables.

- **SMT**: Satisfiability Modulo Theories — combines boolean logic with theories.
  - Example: (x + y > 10) ∧ (x < 5)

**Constraint Solving Techniques**

- **Backtracking Search**: Try assignments, backtrack on conflicts.
  - Assign variable → check constraints → if conflict, backtrack and try different value.

- **Constraint Propagation**: Deduce implications of constraints.
  - If x < y and y < 5, then x < 5.
  - Reduce search space by eliminating impossible values.

- **Local Search**: Start with random assignment, iteratively improve.
  - Hill climbing, simulated annealing, genetic algorithms.

- **Systematic Search**: Exhaustively explore search space with pruning.
  - Branch and bound, DPLL for SAT.

**Example: Sudoku as CSP**

```
Variables: cells[i][j] for i,j in 1..9
Domains: {1, 2, 3, 4, 5, 6, 7, 8, 9}

Constraints:
  - All different in each row
  - All different in each column
  - All different in each 3x3 box
  - Given clues must be satisfied

Constraint solver finds assignment satisfying all constraints.
```

**SAT Solving**

- **Problem**: Given boolean formula, find satisfying assignment or prove unsatisfiable.

- **DPLL Algorithm**: Backtracking search with unit propagation and pure literal elimination.

- **CDCL (Conflict-Driven Clause Learning)**: Modern SAT solvers learn from conflicts.
  - When conflict found, analyze to learn new clause.
  - Prevents repeating same mistakes.

**Example: SAT Problem**

```
Formula: (x ∨ y) ∧ (¬x ∨ z) ∧ (¬y ∨ ¬z)

SAT solver:
  Try x=true:
    (true ∨ y) = true ✓
    (¬true ∨ z) = z → must have z=true
    (¬y ∨ ¬true) = ¬y → must have y=false
    Check: (true ∨ false) ∧ (false ∨ true) ∧ (true ∨ false) = true ✓
  Solution: x=true, y=false, z=true
```

**Constraint Propagation**

- **Idea**: Use constraints to reduce variable domains.

```
Variables: x, y, z ∈ {1, 2, 3, 4, 5}
Constraints:
  - x < y
  - y < z
  - z < 4

Propagation:
  - z < 4 → z ∈ {1, 2, 3}
  - y < z and z ≤ 3 → y ≤ 2 → y ∈ {1, 2}
  - x < y and y ≤ 2 → x ≤ 1 → x ∈ {1}
  - x = 1, y ∈ {2}, z ∈ {3}
  - Solution: x=1, y=2, z=3
```

**Applications**

- **Scheduling**: Assign tasks to time slots satisfying constraints.
  - Course scheduling, employee shifts, project planning.

- **Resource Allocation**: Assign resources to tasks.
  - Cloud computing, manufacturing, logistics.

- **Configuration**: Find valid product configurations.
  - Software configuration, hardware design.

- **Planning**: Find sequence of actions achieving goal.
  - Robot planning, logistics, game AI.

- **Verification**: Prove program properties.
  - Symbolic execution, model checking.

- **Optimization**: Find best solution among feasible ones.
  - Minimize cost, maximize profit, optimize performance.

**Constraint Solvers**

- **SAT Solvers**: MiniSat, Glucose, CryptoMiniSat.
- **SMT Solvers**: Z3, CVC5, Yices.
- **CSP Solvers**: Gecode, Choco, OR-Tools.
- **ILP Solvers**: CPLEX, Gurobi, SCIP.

**Example: Scheduling with Constraints**

```python
from z3 import *

# Variables: start times for 3 tasks
t1, t2, t3 = Ints('t1 t2 t3')

solver = Solver()

# Constraints:
solver.add(t1 >= 0)  # Tasks start at non-negative times
solver.add(t2 >= 0)
solver.add(t3 >= 0)
solver.add(t2 >= t1 + 2)  # Task 2 starts after task 1 finishes (duration 2)
solver.add(t3 >= t1 + 2)  # Task 3 starts after task 1 finishes
solver.add(t3 >= t2 + 3)  # Task 3 starts after task 2 finishes (duration 3)

if solver.check() == sat:
    model = solver.model()
    print(f"Schedule: t1={model[t1]}, t2={model[t2]}, t3={model[t3]}")
# Output: Schedule: t1=0, t2=2, t3=5
```

**Optimization**

- **Constraint Optimization**: Find solution optimizing objective function.
  - Minimize makespan in scheduling.
  - Maximize profit in resource allocation.

- **Techniques**:
  - Branch and bound: Prune suboptimal branches.
  - Linear programming relaxation: Solve relaxed problem for bounds.
  - Iterative solving: Find solution, add constraint to find better one.

**Challenges**

- **NP-Completeness**: Many constraint problems are NP-complete — exponential worst case.
- **Scalability**: Large problems with many variables and constraints are hard.
- **Modeling**: Expressing problems as constraints requires skill.
- **Solver Selection**: Different solvers excel at different problem types.

**LLMs and Constraint Solving**

- **Problem Formulation**: LLMs can help translate natural language problems into constraints.
- **Solver Selection**: LLMs can suggest appropriate solvers for problem types.
- **Result Interpretation**: LLMs can explain solutions in natural language.
- **Debugging**: LLMs can help identify why constraints are unsatisfiable.

**Benefits**

- **Automation**: Automatically finds solutions — no manual search.
- **Optimality**: Can find optimal solutions, not just feasible ones.
- **Declarative**: Specify what you want, not how to compute it.
- **Versatility**: Applicable to diverse problems across many domains.

**Limitations**

- **Complexity**: Hard problems may take exponential time.
- **Modeling Effort**: Requires translating problems into constraints.
- **Solver Limitations**: Not all problems are efficiently solvable.

Constraint solving is a **fundamental technique for automated problem-solving** — it provides declarative, automated solutions to complex problems across scheduling, planning, verification, and optimization, making it essential for both practical applications and theoretical computer science.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/constraint-solving) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
