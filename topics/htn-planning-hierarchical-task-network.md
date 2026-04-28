# HTN planning (Hierarchical Task Network)

**Keywords**: htn planning (hierarchical task network),htn planning,hierarchical task network,ai agent

---

**HTN planning (Hierarchical Task Network)** is a planning approach that **decomposes high-level tasks into networks of subtasks hierarchically** — using domain-specific knowledge about how complex tasks break down into simpler ones, enabling efficient planning for complex domains by exploiting task structure and procedural knowledge.

**What Is HTN Planning?**

- **Hierarchical**: Tasks are organized in a hierarchy from abstract to concrete.
- **Task Network**: Tasks are connected by ordering constraints and dependencies.
- **Decomposition**: High-level tasks are recursively decomposed into subtasks until primitive actions are reached.
- **Domain Knowledge**: Decomposition methods encode expert knowledge about how to accomplish tasks.

**HTN Components**

- **Primitive Tasks**: Directly executable actions (like STRIPS actions).
- **Compound Tasks**: High-level tasks that must be decomposed.
- **Methods**: Recipes for decomposing compound tasks into subtasks.
- **Ordering Constraints**: Specify execution order of subtasks.

**HTN Example: Making Dinner**

```
Compound Task: make_dinner

Method 1: cook_pasta_dinner
  Subtasks:
    1. boil_water
    2. cook_pasta
    3. make_sauce
    4. combine_pasta_and_sauce
  Ordering: 1 < 2, 3 < 4, 2 < 4

Method 2: order_takeout
  Subtasks:
    1. choose_restaurant
    2. place_order
    3. wait_for_delivery
  Ordering: 1 < 2 < 3

Planner chooses method based on context (time, ingredients available, etc.)
```

**HTN Planning Process**

1. **Start with Goal**: High-level task to accomplish.

2. **Select Method**: Choose decomposition method for current task.

3. **Decompose**: Replace task with subtasks from method.

4. **Recurse**: Repeat for each compound subtask.

5. **Primitive Actions**: When all tasks are primitive, plan is complete.

6. **Backtrack**: If decomposition fails, try alternative method.

**Example: Robot Assembly Task**

```
Task: assemble_chair

Method: standard_assembly
  Subtasks:
    1. attach_legs_to_seat
    2. attach_backrest_to_seat
    3. tighten_all_screws
  Ordering: 1 < 3, 2 < 3

Task: attach_legs_to_seat
Method: four_leg_attachment
  Subtasks:
    1. attach_leg(leg1)
    2. attach_leg(leg2)
    3. attach_leg(leg3)
    4. attach_leg(leg4)
  Ordering: none (can be done in any order)

Task: attach_leg(L)
  Primitive action: screw(L, seat)
```

**HTN vs. Classical Planning**

- **Classical Planning (STRIPS/PDDL)**:
  - **Search**: Searches through state space.
  - **Domain-Independent**: General search algorithms.
  - **Flexibility**: Can find novel solutions.
  - **Scalability**: May struggle with large state spaces.

- **HTN Planning**:
  - **Decomposition**: Decomposes tasks hierarchically.
  - **Domain-Specific**: Uses expert knowledge in methods.
  - **Efficiency**: Exploits task structure for faster planning.
  - **Constraints**: Limited to decompositions defined in methods.

**Advantages of HTN Planning**

- **Efficiency**: Hierarchical decomposition reduces search space dramatically.
- **Domain Knowledge**: Encodes expert knowledge about how tasks are typically accomplished.
- **Natural Representation**: Matches how humans think about complex tasks.
- **Scalability**: Handles complex domains that classical planning struggles with.

**HTN Planning Algorithms**

- **SHOP (Simple Hierarchical Ordered Planner)**: Total-order HTN planner.
- **SHOP2**: Extension with more expressive methods.
- **SIADEX**: HTN planner for real-world applications.
- **PANDA**: Partial-order HTN planner.

**Applications**

- **Manufacturing**: Plan assembly sequences, production workflows.
- **Military Operations**: Plan missions with hierarchical command structure.
- **Game AI**: Plan NPC behaviors with complex goal hierarchies.
- **Robotics**: Plan manipulation tasks with subtask structure.
- **Business Process Management**: Plan workflows with task decomposition.

**Example: Military Mission Planning**

```
Task: conduct_reconnaissance_mission

Method: aerial_reconnaissance
  Subtasks:
    1. prepare_aircraft
    2. fly_to_target_area
    3. perform_surveillance
    4. return_to_base
    5. debrief
  Ordering: 1 < 2 < 3 < 4 < 5

Task: prepare_aircraft
Method: standard_preflight
  Subtasks:
    1. inspect_aircraft
    2. fuel_aircraft
    3. load_equipment
    4. brief_crew
  Ordering: 1 < 2, 1 < 3, 4 < (all others complete)
```

**Partial-Order HTN Planning**

- **Flexibility**: Subtasks can be partially ordered — only specify necessary orderings.
- **Advantage**: More flexible than total-order plans — allows parallel execution.
- **Example**: attach_leg(leg1) and attach_leg(leg2) can be done in any order or in parallel.

**HTN with Preconditions and Effects**

- **Hybrid Approach**: Combine HTN decomposition with STRIPS-style preconditions and effects.
- **Benefit**: Ensures plan feasibility while exploiting hierarchical structure.
- **Example**: Check that preconditions are satisfied when selecting methods.

**Challenges**

- **Method Engineering**: Defining good decomposition methods requires domain expertise.
- **Completeness**: HTN planning may miss solutions not captured by defined methods.
- **Flexibility**: Limited to predefined decompositions — less flexible than classical planning.
- **Verification**: Ensuring methods are correct and complete is challenging.

**LLMs and HTN Planning**

- **Method Generation**: LLMs can generate decomposition methods from natural language descriptions.
- **Task Understanding**: LLMs can interpret high-level tasks and suggest decompositions.
- **Method Refinement**: LLMs can refine methods based on execution feedback.

**Example: LLM Generating HTN Method**

```
User: "How do I organize a conference?"

LLM generates HTN method:
Task: organize_conference
Method: standard_conference_organization
  Subtasks:
    1. select_venue
    2. invite_speakers
    3. promote_event
    4. manage_registrations
    5. arrange_catering
    6. conduct_conference
    7. follow_up
  Ordering: 1 < 3, 1 < 4, 2 < 6, 5 < 6, 6 < 7
```

**Benefits**

- **Efficiency**: Dramatically reduces search space through hierarchical decomposition.
- **Knowledge Encoding**: Captures expert knowledge about task structure.
- **Scalability**: Handles complex domains with many actions.
- **Natural**: Matches human problem-solving approach.

**Limitations**

- **Method Dependency**: Quality depends on quality of decomposition methods.
- **Less Flexible**: Cannot find solutions outside defined methods.
- **Engineering Effort**: Requires significant effort to define methods.

HTN planning is a **powerful approach for complex, structured domains** — it exploits hierarchical task structure and domain knowledge to achieve efficient planning, making it particularly effective for real-world applications where expert knowledge about task decomposition is available.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/htn-planning-hierarchical-task-network) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
