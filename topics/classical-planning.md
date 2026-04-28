# Classical planning

**Keywords**: classical planning,ai agent

---

**Classical planning** is the AI approach to **automated planning using formal action representations and search algorithms** — typically using languages like STRIPS or PDDL to specify states, actions, and goals, then employing systematic search to find action sequences that achieve objectives with logical correctness guarantees.

**What Is Classical Planning?**

- **Formal Representation**: States, actions, and goals are precisely defined in logical formalism.
- **Deterministic**: Actions have predictable effects — no uncertainty.
- **Fully Observable**: Complete knowledge of current state.
- **Sequential**: Actions are executed one at a time.
- **Goal-Directed**: Find action sequence transforming initial state to goal state.

**STRIPS (Stanford Research Institute Problem Solver)**

- **Classic Planning Language**: Defines actions with preconditions and effects.

- **Components**:
  - **States**: Sets of logical propositions (facts).
  - **Actions**: Defined by preconditions (what must be true) and effects (what changes).
  - **Goal**: Set of propositions that must be true.

**STRIPS Example: Blocks World**

```
State: on(A, Table), on(B, Table), on(C, B), clear(A), clear(C)

Action: pickup(X)
  Preconditions: on(X, Table), clear(X), handempty
  Effects: holding(X), ¬on(X, Table), ¬clear(X), ¬handempty

Action: putdown(X)
  Preconditions: holding(X)
  Effects: on(X, Table), clear(X), handempty, ¬holding(X)

Action: stack(X, Y)
  Preconditions: holding(X), clear(Y)
  Effects: on(X, Y), clear(X), handempty, ¬holding(X), ¬clear(Y)

Goal: on(A, B), on(B, C)

Plan:
1. pickup(A)
2. stack(A, B)
3. pickup(C)
4. putdown(C)
5. pickup(B)
6. stack(B, C)
7. pickup(A)
8. stack(A, B)
```

**PDDL (Planning Domain Definition Language)**

- **Modern Standard**: More expressive than STRIPS.
- **Features**: Typing, conditional effects, quantifiers, durative actions, numeric fluents.

**PDDL Example**

```lisp
(define (domain logistics)
  (:requirements :strips :typing)
  (:types truck package location)
  
  (:predicates
    (at ?obj - (either truck package) ?loc - location)
    (in ?pkg - package ?truck - truck))
  
  (:action load
    :parameters (?pkg - package ?truck - truck ?loc - location)
    :precondition (and (at ?pkg ?loc) (at ?truck ?loc))
    :effect (and (in ?pkg ?truck) (not (at ?pkg ?loc))))
  
  (:action unload
    :parameters (?pkg - package ?truck - truck ?loc - location)
    :precondition (and (in ?pkg ?truck) (at ?truck ?loc))
    :effect (and (at ?pkg ?loc) (not (in ?pkg ?truck))))
  
  (:action drive
    :parameters (?truck - truck ?from - location ?to - location)
    :precondition (at ?truck ?from)
    :effect (and (at ?truck ?to) (not (at ?truck ?from)))))
```

**Planning Algorithms**

- **Forward Search (Progression)**: Start from initial state, apply actions, search toward goal.
  - Breadth-first, depth-first, A* with heuristics.

- **Backward Search (Regression)**: Start from goal, work backward to initial state.
  - Identify actions that achieve goal, recursively plan for their preconditions.

- **Partial-Order Planning**: Build plan incrementally, ordering actions only when necessary.
  - More flexible than total-order plans.

- **GraphPlan**: Build planning graph, extract solution.
  - Efficient for certain problem classes.

- **SAT-Based Planning**: Encode planning problem as SAT formula, use SAT solver.
  - Bounded planning — find plan of length k.

**Heuristics for Planning**

- **Delete Relaxation**: Ignore delete effects of actions — optimistic estimate of plan length.
- **Pattern Databases**: Precompute costs for abstracted problems.
- **Landmarks**: Identify facts that must be achieved in any valid plan.
- **Causal Graph**: Analyze dependencies between state variables.

**Example: Forward Search with Heuristic**

```
Initial: at(robot, A), at(package, B)
Goal: at(package, C)

Actions:
  move(robot, X, Y): robot moves from X to Y
  pickup(robot, package, X): robot picks up package at X
  putdown(robot, package, X): robot puts down package at X

Forward search with h = distance to goal:
1. move(robot, A, B) → at(robot, B), at(package, B)
2. pickup(robot, package, B) → at(robot, B), holding(robot, package)
3. move(robot, B, C) → at(robot, C), holding(robot, package)
4. putdown(robot, package, C) → at(robot, C), at(package, C) ✓ Goal!
```

**Applications**

- **Robotics**: Plan robot actions for navigation, manipulation, assembly.
- **Logistics**: Plan delivery routes, warehouse operations.
- **Manufacturing**: Plan production schedules, resource allocation.
- **Game AI**: Plan NPC behaviors, strategy games.
- **Space Missions**: Plan spacecraft operations, rover activities.

**Classical Planning Tools**

- **Fast Downward**: State-of-the-art planner, winner of many competitions.
- **FF (Fast Forward)**: Classic heuristic planner.
- **LAMA**: Landmark-based planner.
- **Madagascar**: SAT-based planner.
- **Metric-FF**: Handles numeric planning.

**Limitations of Classical Planning**

- **Deterministic Assumption**: Real world has uncertainty — actions may fail.
- **Full Observability**: May not know complete state.
- **Static World**: World doesn't change during planning.
- **Discrete Actions**: Continuous actions (motion) not directly supported.
- **Scalability**: Large state spaces are challenging.

**Extensions**

- **Probabilistic Planning**: Handle uncertainty with MDPs, POMDPs.
- **Temporal Planning**: Actions have durations, concurrent execution.
- **Conformant Planning**: Plan without full observability.
- **Contingent Planning**: Plan with sensing actions and conditional branches.

**Classical Planning vs. LLM Planning**

- **Classical Planning**:
  - Pros: Correctness guarantees, optimal solutions, handles complex constraints.
  - Cons: Requires formal specifications, limited flexibility.

- **LLM Planning**:
  - Pros: Natural language interface, common sense, flexible.
  - Cons: No guarantees, may generate infeasible plans.

- **Hybrid**: Use LLM to generate high-level plan, classical planner to refine and verify.

**Benefits**

- **Correctness**: Plans are guaranteed to achieve goals (if solution exists).
- **Optimality**: Can find shortest or least-cost plans.
- **Generality**: Works across diverse domains with appropriate domain models.
- **Formal Verification**: Plans can be formally verified.

Classical planning is a **mature and rigorous approach to automated planning** — it provides formal guarantees and optimal solutions, making it essential for applications where correctness and reliability are critical, though it requires careful domain modeling and may need augmentation with learning or heuristics for scalability.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/classical-planning) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
