# Model checking

**Keywords**: model checking,software engineering

---

**Model checking** is a formal verification technique that **exhaustively verifies system properties by exploring all possible states** — building a mathematical model of the system and systematically checking whether specified properties (expressed in temporal logic) hold in all reachable states, providing definitive yes/no answers about correctness.

**What Is Model Checking?**

- **Model**: Mathematical representation of the system — states, transitions, behaviors.
- **Property**: Specification of desired behavior — expressed in temporal logic (LTL, CTL).
- **Checking**: Exhaustive exploration of all reachable states to verify the property.
- **Result**: Either "property holds" (verified) or counterexample showing violation.

**Why Model Checking?**

- **Exhaustive**: Checks all possible behaviors — no missed cases.
- **Automatic**: Fully automated — no manual proof construction.
- **Counterexamples**: When property fails, provides concrete execution trace showing the violation.
- **Formal Guarantee**: Mathematical proof that property holds (or doesn't).

**How Model Checking Works**

1. **Model Construction**: Build finite state machine representing the system.
   - States: All possible configurations.
   - Transitions: How system moves between states.

2. **Property Specification**: Express desired property in temporal logic.
   - Example: "Every request eventually receives a response."

3. **State Space Exploration**: Systematically explore all reachable states.
   - BFS, DFS, or specialized algorithms.

4. **Property Verification**: Check if property holds in all states.

5. **Result**:
   - **Success**: Property holds — system is correct.
   - **Failure**: Property violated — counterexample provided.

**Example: Model Checking a Traffic Light**

```
States: {Red, Yellow, Green}
Transitions:
  Red → Green
  Green → Yellow
  Yellow → Red

Property: "Red and Green are never both active"
  (Safety property)

Model checking:
  - Explore all states: {Red}, {Yellow}, {Green}
  - Check property in each state
  - Result: Property holds ✓ (Red and Green never coexist)

Property: "Eventually, Green will be active"
  (Liveness property)

Model checking:
  - From any state, can we reach Green?
  - Red → Green ✓
  - Yellow → Red → Green ✓
  - Green → Green ✓
  - Result: Property holds ✓
```

**Temporal Logic**

- **Linear Temporal Logic (LTL)**: Properties about sequences of states.
  - **G p**: "Globally p" — p holds in all states.
  - **F p**: "Finally p" — p holds in some future state.
  - **X p**: "Next p" — p holds in the next state.
  - **p U q**: "p Until q" — p holds until q becomes true.

- **Computation Tree Logic (CTL)**: Properties about branching time.
  - **AG p**: "All paths, Globally p" — p holds in all states on all paths.
  - **EF p**: "Exists path, Finally p" — there exists a path where p eventually holds.

**Example: LTL Properties**

```
System: Mutex lock

Property 1: "Mutual exclusion"
  G(¬(process1_in_critical ∧ process2_in_critical))
  "Globally, both processes are never in critical section simultaneously"

Property 2: "No deadlock"
  G(request → F grant)
  "Globally, every request is eventually granted"

Property 3: "Fairness"
  G F process1_in_critical
  "Globally, process1 eventually enters critical section infinitely often"
```

**State Space Explosion**

- **Problem**: Number of states grows exponentially with system size.
  - n boolean variables → 2^n states
  - 100 variables → 2^100 ≈ 10^30 states (infeasible!)

- **Mitigation Techniques**:
  - **Abstraction**: Reduce state space by abstracting details.
  - **Symmetry Reduction**: Exploit symmetry to reduce equivalent states.
  - **Partial Order Reduction**: Avoid exploring equivalent interleavings.
  - **Symbolic Model Checking**: Represent state sets symbolically (BDDs).
  - **Bounded Model Checking**: Check property up to depth k.

**Symbolic Model Checking**

- **Binary Decision Diagrams (BDDs)**: Compact representation of boolean functions.
- **Idea**: Represent sets of states symbolically, not explicitly.
- **Advantage**: Can handle much larger state spaces — millions or billions of states.

**Bounded Model Checking (BMC)**

- **Idea**: Check property only up to depth k.
- **Encoding**: Translate to SAT problem — use SAT solver.
- **Advantage**: Finds bugs quickly if they exist within bound k.
- **Limitation**: Cannot prove property holds for all depths (unless k is sufficient).

**Applications**

- **Hardware Verification**: Verify chip designs — processors, memory controllers.
  - Intel, AMD use model checking extensively.

- **Protocol Verification**: Verify communication protocols — TCP, cache coherence.

- **Software Verification**: Verify concurrent programs — detect deadlocks, race conditions.

- **Embedded Systems**: Verify control systems — automotive, aerospace.

- **Security**: Verify security protocols — authentication, encryption.

**Model Checking Tools**

- **SPIN**: Model checker for concurrent systems — uses LTL.
- **NuSMV**: Symbolic model checker — uses BDDs.
- **UPPAAL**: Model checker for timed systems.
- **CBMC**: Bounded model checker for C programs.
- **Java PathFinder (JPF)**: Model checker for Java programs.

**Example: Finding Deadlock**

```c
// Two processes with two locks
Process 1:
  lock(A);
  lock(B);
  // critical section
  unlock(B);
  unlock(A);

Process 2:
  lock(B);
  lock(A);
  // critical section
  unlock(A);
  unlock(B);

// Model checking:
// State 1: P1 holds A, P2 holds B
// P1 waits for B (held by P2)
// P2 waits for A (held by P1)
// Deadlock detected!
// Counterexample: P1:lock(A) → P2:lock(B) → deadlock
```

**Counterexample-Guided Abstraction Refinement (CEGAR)**

- **Idea**: Start with coarse abstraction, refine if spurious counterexample found.
- **Process**:
  1. Check property on abstract model.
  2. If property holds: Done (verified).
  3. If property fails: Check if counterexample is real or spurious.
  4. If real: Bug found.
  5. If spurious: Refine abstraction, repeat.

**LLMs and Model Checking**

- **Model Generation**: LLMs can help generate models from code or specifications.
- **Property Specification**: LLMs can translate natural language requirements into temporal logic.
- **Counterexample Explanation**: LLMs can explain counterexamples in natural language.
- **Abstraction Guidance**: LLMs can suggest appropriate abstractions.

**Benefits**

- **Exhaustive**: Checks all possible behaviors — no missed bugs.
- **Automatic**: No manual proof construction.
- **Counterexamples**: Provides concrete bug demonstrations.
- **Formal Guarantee**: Mathematical proof of correctness.

**Limitations**

- **State Explosion**: Limited to systems with manageable state spaces.
- **Modeling Effort**: Requires building accurate models.
- **Property Specification**: Requires expressing properties in temporal logic.
- **Scalability**: Difficult to scale to very large systems.

Model checking is a **powerful formal verification technique** — it provides exhaustive verification with automatic counterexample generation, making it essential for verifying critical systems where correctness must be guaranteed.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/model-checking) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
