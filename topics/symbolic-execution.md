# Symbolic execution

**Keywords**: symbolic execution,software engineering

---

**Symbolic execution** is a program analysis technique that **executes programs with symbolic inputs rather than concrete values** — exploring multiple execution paths simultaneously by representing inputs as symbols and tracking constraints on those symbols, enabling systematic path exploration and automated test generation.

**What Is Symbolic Execution?**

- **Symbolic Inputs**: Instead of concrete values (e.g., x = 5), use symbols (e.g., x = α).
- **Symbolic State**: Track symbolic expressions for variables — e.g., y = α + 10.
- **Path Constraints**: Collect conditions that must hold for each path — e.g., α > 0.
- **Path Exploration**: Systematically explore all feasible paths through the program.

**How Symbolic Execution Works**

1. **Initialize**: Start with symbolic inputs (α, β, γ, ...).

2. **Execute Symbolically**: Interpret program operations symbolically.
   - `y = x + 5` becomes `y = α + 5`
   - `z = y * 2` becomes `z = (α + 5) * 2`

3. **Branch Handling**: At conditional branches, fork execution.
   - For `if (x > 10)`: Fork into two paths.
   - Path 1: Assume `α > 10`, continue with true branch.
   - Path 2: Assume `α <= 10`, continue with false branch.

4. **Constraint Collection**: Accumulate path constraints.
   - Path 1 constraints: `α > 10`
   - Path 2 constraints: `α <= 10`

5. **Constraint Solving**: Use SMT solver to check satisfiability.
   - If satisfiable: Path is feasible, solver provides concrete input.
   - If unsatisfiable: Path is infeasible, prune it.

6. **Test Generation**: For each feasible path, generate concrete test input.

**Example: Symbolic Execution**

```python
def test_function(x, y):
    z = x + y
    if z > 10:
        if x > 5:
            return "A"  # Path 1
        else:
            return "B"  # Path 2
    else:
        return "C"  # Path 3

# Symbolic execution with inputs x=α, y=β:

# Path 1: z > 10 AND x > 5
# Constraints: α + β > 10 AND α > 5
# Solver finds: α=6, β=5 → test_function(6, 5) = "A"

# Path 2: z > 10 AND x <= 5
# Constraints: α + β > 10 AND α <= 5
# Solver finds: α=5, β=6 → test_function(5, 6) = "B"

# Path 3: z <= 10
# Constraints: α + β <= 10
# Solver finds: α=3, β=2 → test_function(3, 2) = "C"

# Result: 3 test cases covering all paths!
```

**Applications**

- **Automated Test Generation**: Generate test inputs that cover all paths.
- **Bug Finding**: Explore paths to find crashes, assertion violations, security vulnerabilities.
- **Verification**: Prove that certain paths are infeasible or that properties hold on all paths.
- **Exploit Generation**: Find inputs that trigger vulnerabilities.
- **Program Understanding**: Understand all possible behaviors of a program.

**Symbolic Execution Tools**

- **KLEE**: Symbolic execution for C/C++ programs.
- **Angr**: Binary analysis and symbolic execution framework.
- **S2E**: Selective symbolic execution for binaries.
- **Java PathFinder (JPF)**: Symbolic execution for Java.
- **Pex / IntelliTest**: Symbolic execution for .NET.

**Challenges**

- **Path Explosion**: Programs with many branches have exponentially many paths.
  - **Example**: 20 independent if-statements → 2^20 = 1 million paths.
  - **Mitigation**: Path pruning, path merging, selective exploration.

- **Constraint Complexity**: Symbolic expressions can become very complex.
  - **Example**: Nested loops, recursive functions, complex arithmetic.
  - **Mitigation**: Simplification, approximation, timeouts.

- **Environment Modeling**: Symbolic execution needs models of external systems.
  - **Example**: File I/O, network, system calls.
  - **Mitigation**: Provide symbolic models or concrete stubs.

- **Scalability**: Analyzing large programs is computationally expensive.
  - **Mitigation**: Focus on specific functions or modules.

**Optimization Techniques**

- **Path Pruning**: Discard infeasible or uninteresting paths early.
- **Path Merging**: Merge similar paths to reduce path explosion.
- **Lazy Constraint Solving**: Delay constraint solving until necessary.
- **Caching**: Reuse constraint solving results for similar queries.
- **Heuristic Search**: Prioritize paths likely to find bugs or achieve coverage.

**Concolic Execution (Concrete + Symbolic)**

- **Hybrid Approach**: Combine concrete and symbolic execution.
- **Process**:
  1. Execute program concretely with random input.
  2. Collect path constraints symbolically during execution.
  3. Negate one constraint to explore alternative path.
  4. Solve constraints to generate new input.
  5. Repeat with new input.

- **Benefits**: More scalable than pure symbolic execution — concrete execution handles complex operations.

**Example: Finding Buffer Overflow**

```c
void vulnerable(char *input) {
    char buffer[10];
    if (strlen(input) > 10) {
        return;  // Safe path
    }
    strcpy(buffer, input);  // Potential overflow
}

// Symbolic execution:
// Input: input = symbolic string α
// Path 1: strlen(α) > 10 → return (safe)
// Path 2: strlen(α) <= 10 → strcpy(buffer, α)
//   - If strlen(α) == 10, strcpy writes 11 bytes (including null)
//   - Buffer overflow detected!
// Generated test: input = "0123456789" (10 chars)
// Triggers overflow!
```

**LLMs and Symbolic Execution**

- **Path Selection**: LLMs can suggest which paths to explore first.
- **Constraint Simplification**: LLMs can help simplify complex symbolic expressions.
- **Environment Modeling**: LLMs can generate models for external functions.
- **Bug Explanation**: LLMs can explain bugs found by symbolic execution.

**Benefits**

- **Systematic Exploration**: Explores all feasible paths — no random guessing.
- **High Coverage**: Generates tests that achieve high code coverage.
- **Bug Finding**: Effective at finding deep bugs requiring specific inputs.
- **No False Positives**: Generated tests demonstrate real bugs.

**Limitations**

- **Path Explosion**: Cannot explore all paths in large programs.
- **Constraint Solving**: Complex constraints may be unsolvable or slow.
- **Environment Dependencies**: Requires modeling external systems.
- **Scalability**: Limited to relatively small programs or functions.

Symbolic execution is a **powerful program analysis technique** — it systematically explores program paths to generate tests, find bugs, and verify properties, providing deeper analysis than random testing but with scalability challenges that require careful engineering.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/symbolic-execution) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
