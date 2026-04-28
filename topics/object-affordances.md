# Object affordances

**Keywords**: object affordances,robotics

---

**Object affordances** are the **action possibilities that objects offer to agents** — representing what actions can be performed with objects (grasp, push, pour, sit on, etc.), enabling robots to understand how to interact with objects based on their properties and the robot's capabilities, bridging perception and action.

**What Are Affordances?**

- **Definition**: Action possibilities offered by objects.
- **Origin**: Coined by psychologist James J. Gibson (1979).
- **Examples**:
  - **Chair**: Affords sitting.
  - **Cup**: Affords grasping, pouring, drinking.
  - **Door**: Affords opening, closing.
  - **Button**: Affords pushing.

**Key Concept**: Affordances are relationships between objects and agents.
- Same object may afford different actions to different agents.
- Cup affords grasping to human, but not to robot without gripper.

**Why Affordances for Robotics?**

- **Action-Oriented Perception**: Perceive objects in terms of what can be done with them.
  - Not just "this is a cup" but "I can grasp this cup here"

- **Generalization**: Transfer knowledge to novel objects.
  - Never seen this specific cup, but recognize graspable handle.

- **Task Planning**: Plan actions based on affordances.
  - "To pour, need object that affords grasping and pouring"

- **Interaction**: Enable robots to interact with objects purposefully.

**Types of Affordances**

**Manipulation Affordances**:
- **Graspability**: Where and how object can be grasped.
- **Pushability**: Where object can be pushed to move it.
- **Containment**: Object can contain other objects (bowl, box).
- **Support**: Object can support other objects (table, shelf).

**Functional Affordances**:
- **Pourability**: Object can pour liquids (cup, pitcher).
- **Cuttability**: Object can be cut (food, paper).
- **Openability**: Object can be opened (door, drawer, bottle).
- **Sittability**: Object can be sat on (chair, bench).

**Tool Affordances**:
- **Hammering**: Object can be used to hammer (hammer, rock).
- **Cutting**: Object can be used to cut (knife, scissors).
- **Scooping**: Object can be used to scoop (spoon, shovel).

**Affordance Representation**

**Geometric Affordances**:
- **Representation**: 3D regions or poses where actions can be performed.
- **Example**: Grasp affordance = set of gripper poses that achieve stable grasp.
- **Benefit**: Precise, actionable.

**Semantic Affordances**:
- **Representation**: High-level action labels.
- **Example**: "This object affords sitting"
- **Benefit**: Abstract, generalizable.

**Probabilistic Affordances**:
- **Representation**: Probability distributions over action success.
- **Example**: P(grasp succeeds | gripper pose, object)
- **Benefit**: Captures uncertainty.

**Affordance Learning**

**Supervised Learning**:
- **Data**: Labeled examples of affordances.
- **Example**: Images with annotated grasp points.
- **Method**: Train classifier or regressor.
- **Challenge**: Requires large labeled datasets.

**Self-Supervised Learning**:
- **Data**: Robot's own interaction experience.
- **Method**: Learn from trial and error.
- **Example**: Try grasping, learn what works.
- **Benefit**: No human labels needed.

**Transfer Learning**:
- **Method**: Pre-train on large datasets, fine-tune on robot tasks.
- **Example**: Pre-train on ImageNet, fine-tune on grasp detection.
- **Benefit**: Leverage large-scale data.

**Affordance Detection Methods**

**Grasp Affordance Detection**:
- **Input**: RGB or RGB-D image of object.
- **Output**: Grasp poses (position, orientation, gripper width).
- **Methods**:
  - **GraspNet**: Large-scale grasp detection.
  - **Contact-GraspNet**: Grasp detection from point clouds.
  - **6-DOF GraspNet**: Full 6-DOF grasp poses.

**Pushing Affordance**:
- **Input**: Object state, desired motion.
- **Output**: Push location and direction.
- **Methods**: Learn from pushing interactions.

**Containment Affordance**:
- **Input**: Object geometry.
- **Output**: Whether object can contain others, where.
- **Methods**: Geometric reasoning, learned models.

**Applications**

**Manipulation**:
- **Grasping**: Detect where to grasp objects.
- **Tool Use**: Understand how to use tools.
- **Assembly**: Identify how parts fit together.

**Navigation**:
- **Traversability**: Identify surfaces that afford walking.
- **Openability**: Detect doors that can be opened.

**Human-Robot Interaction**:
- **Shared Understanding**: Humans and robots understand affordances similarly.
- **Communication**: "Hand me something to cut with" — robot finds knife.

**Household Tasks**:
- **Cooking**: Understand utensil affordances.
- **Cleaning**: Identify surfaces that need cleaning.
- **Organization**: Place objects where they afford storage.

**Affordance-Based Planning**

**Task**: Pour water from pitcher to cup.

**Affordance Reasoning**:
1. **Identify**: Pitcher affords grasping (handle) and pouring (spout).
2. **Identify**: Cup affords grasping and containment.
3. **Plan**:
   - Grasp pitcher at handle.
   - Grasp cup.
   - Position cup under pitcher spout.
   - Tilt pitcher to pour.

**Benefit**: Plan based on what objects afford, not just object categories.

**Challenges**

**Perception**:
- Detecting affordances from visual observations.
- Occlusions, viewpoint variations, lighting.

**Generalization**:
- Transferring affordances to novel objects.
- "This object looks graspable like a cup, even though I've never seen it"

**Context-Dependence**:
- Affordances depend on context.
- Cup affords drinking when upright, not when upside down.

**Multi-Step Reasoning**:
- Complex tasks require reasoning about multiple affordances.
- "To pour, first need to grasp, then position, then tilt"

**Uncertainty**:
- Affordances are probabilistic, not deterministic.
- Grasp may fail due to friction, weight, shape.

**Affordance Datasets**

**UMD Affordance Dataset**: Objects with affordance annotations.
**ADE20K**: Scenes with affordance labels.
**EPIC-KITCHENS**: Videos of object interactions.
**Something-Something**: Videos of object manipulations.

**Affordance Models**

**Affordance Networks**:
- Neural networks that predict affordances from images.
- Input: RGB or RGB-D image.
- Output: Affordance heatmaps or poses.

**Physics-Based Models**:
- Use physics simulation to predict affordances.
- Simulate grasping, pushing, pouring to evaluate success.

**Hybrid Models**:
- Combine learned perception with physics-based reasoning.
- Learn to predict physics parameters, simulate to verify.

**Quality Metrics**

- **Detection Accuracy**: Correctly identify affordances.
- **Action Success Rate**: Actions based on affordances succeed.
- **Generalization**: Performance on novel objects.
- **Efficiency**: Speed of affordance detection.

**Future of Object Affordances**

- **Foundation Models**: Large models pre-trained on diverse interactions.
- **Zero-Shot Affordances**: Recognize affordances of novel objects.
- **Language-Grounded**: "Find something to cut with" — understand affordances from language.
- **Multi-Modal**: Combine vision, touch, audio for affordance understanding.
- **Lifelong Learning**: Continuously learn new affordances from experience.
- **Compositional**: Understand complex affordances from simpler ones.

Object affordances are **fundamental to intelligent robot interaction** — they enable robots to perceive objects in terms of action possibilities, supporting generalization to novel objects, task planning, and purposeful interaction with the physical world.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/object-affordances) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
