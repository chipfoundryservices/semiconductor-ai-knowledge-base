# SAT solving

**Keywords**: sat solving

---

**SAT solving** is the problem of **determining whether a boolean formula can be satisfied** — finding an assignment of true/false values to variables that makes the formula true, or proving that no such assignment exists, serving as the foundation for many automated reasoning and verification tasks.

**What Is SAT?**

- **Boolean Formula**: Logical expression with variables, AND (∧), OR (∨), NOT (¬).
  - Example: (x ∨ y) ∧ (¬x ∨ z) ∧ (¬y ∨ ¬z)

- **Satisfiability**: Can we assign true/false to variables to make the formula true?

- **SAT**: Formula is satisfiable — there exists a satisfying assignment.

- **UNSAT**: Formula is unsatisfiable — no assignment makes it true.

**Why SAT Solving?**

- **Fundamental Problem**: SAT is the first problem proven NP-complete — many problems reduce to SAT.
- **Practical Importance**: Despite NP-completeness, modern SAT solvers are remarkably efficient on real-world instances.
- **Versatility**: SAT solving is used in verification, testing, planning, scheduling, and more.

**CNF (Conjunctive Normal Form)**

- **Standard Form**: Formula is AND of clauses, each clause is OR of literals.
  - Clause: (x ∨ ¬y ∨ z)
  - CNF: (x ∨ y) ∧ (¬x ∨ z) ∧ (¬y ∨ ¬z)

- **Conversion**: Any boolean formula can be converted to CNF.

- **Why CNF?**: SAT solvers work on CNF formulas — standard input format.

**Example: SAT Problem**

```
Formula: (x ∨ y) ∧ (¬x ∨ z) ∧ (¬y ∨ ¬z)

Try x=true:
  Clause 1: (true ∨ y) = true ✓
  Clause 2: (¬true ∨ z) = (false ∨ z) = z
    → Must have z=true
  Clause 3: (¬y ∨ ¬true) = (¬y ∨ false) = ¬y
    → Must have y=false

Check: (true ∨ false) ∧ (false ∨ true) ∧ (true ∨ false)
     = true ∧ true ∧ true = true ✓

Solution: x=true, y=false, z=true (SAT)
```

**DPLL Algorithm**

- **Classic SAT Algorithm**: Backtracking search with optimizations.

- **Steps**:
  1. **Unit Propagation**: If clause has only one unassigned literal, assign it to satisfy the clause.
  2. **Pure Literal Elimination**: If variable appears only positive (or only negative), assign it to satisfy all clauses.
  3. **Branching**: Pick unassigned variable, try both true and false.
  4. **Backtrack**: If conflict, undo assignments and try alternative.

**CDCL (Conflict-Driven Clause Learning)**

- **Modern SAT Solvers**: Extend DPLL with learning.

- **Key Idea**: When conflict found, analyze to learn new clause preventing same conflict.

- **Process**:
  1. Make decisions and propagate.
  2. If conflict: Analyze conflict, learn clause, backtrack.
  3. Learned clause prevents repeating same mistake.
  4. Continue until SAT or UNSAT proven.

**Example: CDCL Learning**

```
Formula: (x ∨ y) ∧ (¬x ∨ z) ∧ (¬y ∨ ¬z) ∧ (¬z ∨ w) ∧ (¬w)

Decisions: x=true, y=true
Propagation:
  From (¬x ∨ z): z=true
  From (¬y ∨ ¬z): conflict! (y=true and z=true violate this)

Conflict Analysis:
  Why conflict? Because x=true → z=true and y=true → ¬z
  Learn clause: (¬x ∨ ¬y)  # x and y can't both be true

Add learned clause to formula, backtrack, continue.
```

**Applications**

- **Hardware Verification**: Verify chip designs — equivalence checking, property verification.
- **Software Verification**: Bounded model checking, symbolic execution.
- **Planning**: AI planning problems encoded as SAT.
- **Scheduling**: Resource allocation, timetabling.
- **Cryptanalysis**: Breaking cryptographic systems.
- **Bioinformatics**: Haplotype inference, phylogeny.

**SAT Solvers**

- **MiniSat**: Small, efficient, widely used as baseline.
- **Glucose**: Focuses on learned clause management.
- **CryptoMiniSat**: Specialized for cryptographic problems.
- **Lingeling**: Competition-winning solver.
- **CaDiCaL**: Modern, efficient solver.

**Example: Encoding Graph Coloring as SAT**

```
Problem: Color graph with 3 colors such that adjacent nodes have different colors.

Variables: x_i_c = "node i has color c"
  For 3 nodes, 3 colors: x_1_1, x_1_2, x_1_3, x_2_1, x_2_2, x_2_3, x_3_1, x_3_2, x_3_3

Constraints:
  1. Each node has exactly one color:
     (x_1_1 ∨ x_1_2 ∨ x_1_3) ∧ (¬x_1_1 ∨ ¬x_1_2) ∧ (¬x_1_1 ∨ ¬x_1_3) ∧ (¬x_1_2 ∨ ¬x_1_3)
     ... (similar for nodes 2 and 3)

  2. Adjacent nodes have different colors:
     If nodes 1 and 2 are adjacent:
     (¬x_1_1 ∨ ¬x_2_1) ∧ (¬x_1_2 ∨ ¬x_2_2) ∧ (¬x_1_3 ∨ ¬x_2_3)

SAT solver finds satisfying assignment → valid coloring.
```

**MaxSAT**

- **Optimization Variant**: Maximize number of satisfied clauses.
- **Partial MaxSAT**: Some clauses are hard (must be satisfied), others are soft (prefer to satisfy).
- **Applications**: Optimization problems where not all constraints can be satisfied.

**Incremental SAT**

- **Idea**: Solve sequence of related SAT problems efficiently.
- **Technique**: Reuse learned clauses and solver state across problems.
- **Applications**: Bounded model checking, iterative refinement.

**Challenges**

- **NP-Completeness**: Worst-case exponential time.
- **Hard Instances**: Some formulas are extremely difficult for all known solvers.
- **Encoding Quality**: Efficiency depends on how problem is encoded as SAT.

**SAT Solver Heuristics**

- **Variable Selection**: Which variable to branch on? (VSIDS, EVSIDS)
- **Phase Selection**: Try true or false first? (Phase saving)
- **Restart Strategy**: When to restart search? (Luby, geometric)
- **Clause Deletion**: Which learned clauses to keep? (LBD, activity)

**LLMs and SAT Solving**

- **Problem Encoding**: LLMs can help translate problems into SAT formulas.
- **Result Interpretation**: LLMs can explain SAT solver results.
- **Debugging UNSAT**: LLMs can help identify conflicting constraints.
- **Heuristic Tuning**: LLMs can suggest solver configurations for specific problem types.

**Benefits**

- **Automation**: Automatically finds solutions or proves unsatisfiability.
- **Efficiency**: Modern solvers handle millions of variables and clauses.
- **Versatility**: Applicable to diverse problems via encoding.
- **Mature Technology**: Decades of research and engineering.

**Limitations**

- **Exponential Worst Case**: Some instances are intractable.
- **Encoding Overhead**: Translating problems to SAT can be complex.
- **Black Box**: Solvers don't explain why formula is UNSAT (though some provide UNSAT cores).

SAT solving is a **cornerstone of automated reasoning** — despite being NP-complete, modern SAT solvers are remarkably effective on real-world problems, making SAT solving essential for verification, testing, planning, and many other applications.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/sat-solving) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
