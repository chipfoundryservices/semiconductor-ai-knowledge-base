# Parametric design

**Keywords**: parametric design,engineering

---

**Parametric design** is a **design approach where geometry is defined by parameters and relationships** — creating models that automatically update when parameters change, enabling rapid design exploration, variation generation, and rule-based design systems that capture design intent and enable intelligent, flexible design workflows.

**What Is Parametric Design?**

- **Definition**: Design controlled by parameters, equations, and relationships.
- **Key Concept**: Change parameters → geometry updates automatically.
- **Philosophy**: Define rules and relationships, not just geometry.
- **Output**: Flexible, intelligent models that adapt to changes.

**Parametric vs. Direct Modeling**

**Direct Modeling**:
- **Approach**: Directly manipulate geometry (push, pull, move).
- **Flexibility**: Quick changes, intuitive interaction.
- **Limitation**: No design history, changes don't propagate.
- **Use Case**: Conceptual design, imported geometry editing.

**Parametric Modeling**:
- **Approach**: Define parameters, constraints, relationships.
- **Flexibility**: Changes propagate through model automatically.
- **Limitation**: More complex, requires planning.
- **Use Case**: Engineering design, product families, design automation.

**Parametric Design Components**

**Parameters**:
- **Dimensions**: Length, width, height, diameter, angle.
- **Variables**: Named values that control geometry.
- **User Parameters**: Custom variables defined by designer.

**Relationships**:
- **Equations**: Mathematical relationships between parameters.
  - `Height = 2 * Width`
  - `Volume = π * Radius² * Length`

- **Constraints**: Geometric relationships.
  - Parallel, perpendicular, tangent, concentric.
  - Equal length, symmetric, fixed distance.

**Design Intent**:
- **Capture**: How design should behave when modified.
- **Example**: "Hole should always be centered on face."
- **Benefit**: Model updates correctly when dimensions change.

**Parametric Design Tools**

**CAD Software**:
- **SolidWorks**: Feature-based parametric modeling.
- **Autodesk Inventor**: Parametric solid modeling.
- **Fusion 360**: Cloud-based parametric CAD.
- **Siemens NX**: Advanced parametric design.
- **CATIA**: High-end parametric modeling.
- **FreeCAD**: Open-source parametric CAD.

**Visual Programming**:
- **Grasshopper**: Visual programming for Rhino.
- **Dynamo**: Visual programming for Revit.
- **Houdini**: Procedural 3D modeling and animation.

**Code-Based**:
- **OpenSCAD**: Script-based parametric modeling.
- **CadQuery**: Python library for parametric CAD.
- **ImplicitCAD**: Functional programming for CAD.

**Parametric Design Process**

1. **Define Parameters**: Identify key dimensions and variables.
2. **Establish Relationships**: Define equations and constraints.
3. **Create Geometry**: Build model using parameters.
4. **Test**: Change parameters, verify model updates correctly.
5. **Refine**: Adjust relationships for desired behavior.
6. **Document**: Explain parameters and design intent.

**Example: Parametric Bracket**

```
Parameters:
- Width = 100mm
- Height = 150mm
- Thickness = 10mm
- Hole_Diameter = 12mm
- Fillet_Radius = Thickness / 2

Relationships:
- Hole_Spacing = Width / 2
- Hole_Position_X = Width / 4
- Hole_Position_Y = Height - 20mm

Design Intent:
- Holes always centered on width
- Holes always 20mm from top edge
- Fillets always half of thickness
- All features update when Width, Height, or Thickness change

Result:
- Change Width to 120mm → Holes reposition automatically
- Change Thickness to 15mm → Fillets update to 7.5mm
- Model maintains design intent through all changes
```

**Applications**

**Product Design**:
- **Product Families**: Create variations from single parametric model.
  - Small, medium, large sizes from one design.
  - Different configurations (left-hand, right-hand).

**Architecture**:
- **Building Design**: Parametric facades, structures, spaces.
  - Adjust building dimensions, all elements update.
  - Explore design variations quickly.

**Manufacturing**:
- **Tooling**: Parametric molds, dies, fixtures.
  - Adapt tooling for different part sizes.

**Engineering**:
- **Optimization**: Link parameters to optimization algorithms.
  - Automatically find optimal dimensions.

**Customization**:
- **Mass Customization**: Generate custom products from parameters.
  - Customer specifies dimensions, model generates automatically.

**Benefits of Parametric Design**

- **Flexibility**: Easy to modify and create variations.
  - Change one parameter, entire model updates.

- **Design Intent**: Captures how design should behave.
  - Relationships preserved through changes.

- **Automation**: Generate designs programmatically.
  - Scripts, spreadsheets, databases drive models.

- **Exploration**: Rapidly explore design space.
  - Try different dimensions, configurations, options.

- **Consistency**: Relationships ensure geometric consistency.
  - Features stay aligned, proportions maintained.

**Challenges**

- **Complexity**: Parametric models can become complex.
  - Many parameters, equations, constraints to manage.

- **Planning**: Requires upfront thinking about design intent.
  - Must anticipate how design will change.

- **Robustness**: Models can break if relationships are poorly defined.
  - Circular references, over-constrained sketches.

- **Learning Curve**: More complex than direct modeling.
  - Requires understanding of constraints and relationships.

- **Performance**: Complex parametric models can be slow.
  - Many features and relationships to recalculate.

**Advanced Parametric Techniques**

**Configurations**:
- **Definition**: Multiple variations within single model.
- **Use**: Part families, different sizes, optional features.
- **Example**: Bolt model with configurations for different lengths and diameters.

**Design Tables**:
- **Definition**: Spreadsheet controlling parameters.
- **Use**: Generate many variations from table.
- **Example**: Excel table with rows for each part size.

**Equations**:
- **Definition**: Mathematical relationships between parameters.
- **Use**: Complex dependencies, calculations.
- **Example**: `Spring_Force = Spring_Constant * Deflection`

**Global Variables**:
- **Definition**: Parameters shared across multiple parts.
- **Use**: Assembly-level control, synchronized changes.
- **Example**: Standard hole sizes used in all parts.

**Parametric Design Patterns**

**Proportional Scaling**:
- All dimensions scale proportionally.
- `Length = Base_Size * 2`
- `Width = Base_Size * 1.5`
- `Height = Base_Size`

**Adaptive Features**:
- Features adapt to changing geometry.
- Holes always centered on faces.
- Fillets always at intersections.

**Rule-Based Design**:
- Design follows engineering rules.
- `Wall_Thickness >= 2mm` (manufacturing constraint)
- `Safety_Factor >= 2.0` (engineering requirement)

**Quality Metrics**

- **Robustness**: Does model update correctly when parameters change?
- **Clarity**: Are parameters and relationships well-organized and documented?
- **Efficiency**: Does model recalculate quickly?
- **Flexibility**: Can model accommodate expected design changes?
- **Maintainability**: Can other designers understand and modify the model?

**Parametric Design Best Practices**

- **Plan Ahead**: Think about how design will change before modeling.
- **Name Parameters**: Use descriptive names, not default names.
- **Document Intent**: Add comments explaining relationships.
- **Test Extremes**: Try minimum and maximum parameter values.
- **Keep It Simple**: Don't over-constrain or create unnecessary complexity.
- **Use Equations**: Capture mathematical relationships explicitly.
- **Organize Features**: Logical feature tree, group related features.

**Generative Parametric Design**

**Combination**: Parametric design + AI optimization.

**Process**:
1. Define parametric model with key parameters.
2. Set parameter ranges and constraints.
3. Define optimization objectives.
4. AI explores parameter space, evaluates designs.
5. Optimal parameter values found automatically.

**Example**:
```
Parametric Beam Model:
- Width, Height, Wall_Thickness (parameters)

Optimization:
- Minimize: Weight
- Constraint: Stress < 200 MPa
- Constraint: Deflection < 5mm

Result: AI finds optimal Width=50mm, Height=80mm, Wall_Thickness=3mm
```

**Future of Parametric Design**

- **AI Integration**: AI suggests parameters and relationships.
- **Natural Language**: Define parameters with text descriptions.
- **Real-Time Optimization**: Instant feedback on parameter changes.
- **Cloud-Based**: Parametric models in the cloud, accessible anywhere.
- **Collaborative**: Multiple users editing parameters simultaneously.
- **Generative**: AI-driven parameter exploration and optimization.

Parametric design is a **powerful design methodology** — it transforms static geometry into intelligent, flexible models that capture design intent and enable rapid exploration, variation generation, and design automation, making it essential for modern engineering, architecture, and product design.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/parametric-design) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
