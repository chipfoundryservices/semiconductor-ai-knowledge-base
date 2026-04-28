# Planning with LLMs

**Keywords**: planning with llms,ai agent

---

**Planning with LLMs** involves using **large language models to generate action sequences that achieve specified goals** — leveraging LLMs' understanding of tasks, common sense, and procedural knowledge to create plans for robots, agents, and automated systems, bridging natural language goal specifications with executable action sequences.

**What Is AI Planning?**

- **Planning**: Finding a sequence of actions that transforms an initial state into a goal state.
- **Components**:
  - **Initial State**: Current situation.
  - **Goal**: Desired situation.
  - **Actions**: Operations that change state.
  - **Plan**: Sequence of actions achieving the goal.

**Why Use LLMs for Planning?**

- **Natural Language Goals**: LLMs can understand goals expressed in natural language — "make breakfast," "clean the room."
- **Common Sense**: LLMs have learned common-sense knowledge about how the world works.
- **Procedural Knowledge**: LLMs have seen many examples of plans and procedures in training data.
- **Flexibility**: LLMs can adapt plans to different contexts and constraints.

**How LLMs Generate Plans**

1. **Goal Understanding**: LLM interprets the natural language goal.

2. **Plan Generation**: LLM generates a sequence of actions.
   ```
   Goal: "Make a cup of coffee"
   
   LLM-generated plan:
   1. Fill kettle with water
   2. Boil water
   3. Put coffee grounds in filter
   4. Pour hot water over grounds
   5. Wait for brewing to complete
   6. Pour coffee into cup
   ```

3. **Refinement**: LLM can refine the plan based on feedback or constraints.

4. **Execution**: Actions are executed by a robot or system.

**LLM Planning Approaches**

- **Direct Generation**: LLM generates complete plan in one shot.
  - Fast but may not handle complex constraints.

- **Iterative Refinement**: LLM generates plan, checks feasibility, refines.
  - More robust for complex problems.

- **Hierarchical Planning**: LLM decomposes goal into subgoals, plans for each.
  - Handles complex tasks by breaking them down.

- **Reactive Planning**: LLM generates next action based on current state.
  - Adapts to dynamic environments.

**Example: Household Robot Planning**

```
Goal: "Set the table for dinner"

LLM-generated plan:
1. Navigate to kitchen
2. Open cabinet
3. Grasp plate
4. Place plate on table
5. Repeat steps 2-4 for additional plates
6. Grasp fork from drawer
7. Place fork next to plate
8. Repeat steps 6-7 for additional forks
9. Grasp knife from drawer
10. Place knife next to plate
11. Repeat steps 9-10 for additional knives
12. Grasp glass from cabinet
13. Place glass on table
14. Repeat steps 12-13 for additional glasses
```

**Challenges**

- **Feasibility**: LLM-generated plans may not be physically feasible.
  - Example: "Pick up the table" — table may be too heavy.
  - **Solution**: Verify plan with physics simulator or feasibility checker.

- **Completeness**: Plans may miss necessary steps.
  - Example: Forgetting to open door before walking through.
  - **Solution**: Use verification or execution feedback to identify gaps.

- **Optimality**: Plans may not be optimal — longer or more costly than necessary.
  - **Solution**: Use optimization or search to improve plans.

- **Grounding**: Mapping high-level actions to low-level robot commands.
  - Example: "Grasp cup" → specific motor commands.
  - **Solution**: Use motion planning and control systems.

**LLM + Classical Planning**

- **Hybrid Approach**: Combine LLM with classical planners (STRIPS, PDDL).
  - **LLM**: Generates high-level plan structure, handles natural language.
  - **Classical Planner**: Ensures logical correctness, handles constraints.

- **Process**:
  1. LLM translates natural language goal to formal specification (PDDL).
  2. Classical planner finds valid plan.
  3. LLM translates plan back to natural language or executable actions.

**Example: LLM Translating to PDDL**

```
Natural Language Goal: "Move all blocks from table A to table B"

LLM-generated PDDL:
(define (problem move-blocks)
  (:domain blocks-world)
  (:objects
    block1 block2 block3 - block
    tableA tableB - table)
  (:init
    (on block1 tableA)
    (on block2 tableA)
    (on block3 tableA))
  (:goal
    (and (on block1 tableB)
         (on block2 tableB)
         (on block3 tableB))))

Classical planner generates valid action sequence.
```

**Applications**

- **Robotics**: Plan robot actions for manipulation, navigation, assembly.
- **Virtual Assistants**: Plan sequences of API calls to accomplish user requests.
- **Game AI**: Plan NPC behaviors and strategies.
- **Workflow Automation**: Plan business process steps.
- **Smart Homes**: Plan device actions to achieve user goals.

**LLM Planning with Feedback**

- **Execution Monitoring**: Observe plan execution, detect failures.
- **Replanning**: If action fails, LLM generates alternative plan.
- **Learning**: LLM learns from failures to improve future plans.

**Example: Replanning**

```
Initial Plan: "Pick up cup from table"
Execution: Robot attempts to grasp cup → fails (cup is too slippery)

LLM Replanning:
"Cup is slippery. Alternative plan:
1. Get paper towel
2. Dry cup
3. Pick up cup with better grip"
```

**Evaluation**

- **Success Rate**: What percentage of plans achieve the goal?
- **Efficiency**: How many actions does the plan require?
- **Robustness**: Does the plan handle unexpected situations?
- **Generalization**: Does the planner work on novel tasks?

**LLMs vs. Classical Planning**

- **Classical Planning**:
  - Pros: Guarantees correctness, handles complex constraints, optimal solutions.
  - Cons: Requires formal specifications, limited to predefined action spaces.

- **LLM Planning**:
  - Pros: Natural language interface, common sense, flexible, handles novel tasks.
  - Cons: No correctness guarantees, may generate infeasible plans.

- **Best Practice**: Combine both — LLM for high-level reasoning, classical planner for correctness.

**Benefits**

- **Natural Language Interface**: Users specify goals in plain language.
- **Common Sense**: LLMs bring real-world knowledge to planning.
- **Flexibility**: Adapts to new tasks without reprogramming.
- **Rapid Prototyping**: Quickly generate plans for testing.

**Limitations**

- **No Guarantees**: Plans may be incorrect or infeasible.
- **Grounding Gap**: High-level plans need translation to low-level actions.
- **Context Limits**: LLMs have limited context — may not track complex state.

Planning with LLMs is an **emerging and promising approach** — it makes AI planning more accessible and flexible by leveraging natural language understanding and common sense, though it requires careful integration with verification and execution systems to ensure reliability.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/planning-with-llms) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
