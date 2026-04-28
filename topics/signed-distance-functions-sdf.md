# Signed Distance Functions (SDF)

**Keywords**: signed distance functions (sdf),signed distance functions,sdf,computer vision

---

**Signed Distance Functions (SDF)** are a mathematical representation of **3D geometry as distance to the nearest surface** — defining shapes by storing the signed distance from any point in space to the closest surface point, with negative values inside and positive outside, enabling powerful geometric operations and high-quality surface representation.

**What Is a Signed Distance Function?**

- **Definition**: Function SDF(x, y, z) → distance to nearest surface.
- **Sign Convention**: Negative inside object, positive outside, zero on surface.
- **Properties**: Continuous, differentiable, metric information.
- **Surface**: Defined by zero level set (SDF = 0).

**SDF Formula**:
```
SDF(p) = {
  -d  if p inside object
   0  if p on surface
  +d  if p outside object
}
where d = distance to nearest surface point
```

**Why Signed Distance Functions?**

- **Geometric Operations**: Easy boolean operations (union, intersection, difference).
- **Collision Detection**: Fast distance queries for physics.
- **Ray Marching**: Efficient rendering via sphere tracing.
- **Surface Reconstruction**: Robust to noise, handles topology changes.
- **Shape Representation**: Continuous, resolution-independent.
- **Gradient**: Surface normal = gradient of SDF.

**SDF Properties**

**Metric Information**:
- **Distance**: Exact distance to surface at any point.
- **Use**: Collision detection, proximity queries.

**Surface Normal**:
- **Formula**: n = ∇SDF / |∇SDF|
- **Benefit**: Normals available everywhere via gradient.

**Lipschitz Continuity**:
- **Property**: |SDF(p₁) - SDF(p₂)| ≤ |p₁ - p₂|
- **Benefit**: Bounded rate of change, stable numerics.

**Zero Level Set**:
- **Surface**: Points where SDF(p) = 0.
- **Extraction**: Marching Cubes, dual contouring.

**SDF Representations**

**Analytic SDF**:
- **Method**: Mathematical formulas for primitive shapes.
- **Examples**: Sphere, box, cylinder, torus.
- **Benefit**: Exact, efficient evaluation.
- **Use**: Procedural modeling, CSG.

**Discrete SDF**:
- **Method**: Store SDF values on grid (voxels).
- **Benefit**: Represent arbitrary shapes.
- **Use**: 3D reconstruction, simulation.

**Neural SDF**:
- **Method**: Neural network learns SDF as implicit function.
- **Examples**: DeepSDF, Neural SDF.
- **Benefit**: Compact, continuous, learnable.

**Truncated SDF (TSDF)**:
- **Method**: Truncate SDF to narrow band around surface.
- **Benefit**: Focus computation on surface region.
- **Use**: Real-time 3D reconstruction (KinectFusion).

**Analytic SDF Examples**

**Sphere**:
```
SDF(p) = |p - center| - radius
```

**Box**:
```
SDF(p) = max(|p.x| - size.x, |p.y| - size.y, |p.z| - size.z)
```

**Plane**:
```
SDF(p) = dot(p - point_on_plane, normal)
```

**Torus**:
```
q = (|p.xz| - major_radius, p.y)
SDF(p) = |q| - minor_radius
```

**SDF Operations**

**Boolean Operations**:
- **Union**: min(SDF₁, SDF₂)
- **Intersection**: max(SDF₁, SDF₂)
- **Difference**: max(SDF₁, -SDF₂)
- **Benefit**: Combine shapes easily.

**Smooth Boolean**:
- **Smooth Union**: Blend SDFs smoothly.
- **Formula**: -log(exp(-k·SDF₁) + exp(-k·SDF₂)) / k
- **Benefit**: Organic blending between shapes.

**Transformations**:
- **Translation**: SDF(p - offset)
- **Rotation**: SDF(R⁻¹ · p)
- **Scale**: SDF(p / scale) · scale
- **Benefit**: Transform shapes via coordinate transformation.

**Deformations**:
- **Twist, Bend, Taper**: Apply deformation to coordinates before SDF evaluation.
- **Benefit**: Complex shape variations.

**Applications**

**3D Reconstruction**:
- **Use**: Fuse depth maps into TSDF volume.
- **Method**: KinectFusion, Voxblox, BundleFusion.
- **Benefit**: Robust to noise, incremental updates.

**Ray Marching / Sphere Tracing**:
- **Use**: Render SDFs efficiently.
- **Method**: March along ray by SDF distance (safe step size).
- **Benefit**: Accurate rendering without discretization.

**Collision Detection**:
- **Use**: Fast distance queries for physics simulation.
- **Benefit**: Exact distance, penetration depth.

**Shape Generation**:
- **Use**: Neural networks learn SDF for shape synthesis.
- **Method**: DeepSDF — latent code → SDF.
- **Benefit**: Continuous, high-quality shapes.

**Procedural Modeling**:
- **Use**: Combine primitive SDFs for complex shapes.
- **Benefit**: Compact, parametric, editable.

**SDF-Based Rendering**

**Sphere Tracing**:
- **Method**: Ray marching using SDF as step size.
- **Algorithm**:
  1. Start at ray origin.
  2. Evaluate SDF at current position.
  3. Step forward by SDF distance (safe, won't overshoot surface).
  4. Repeat until close to surface (SDF ≈ 0) or max steps.
- **Benefit**: Efficient, accurate, no discretization.

**Soft Shadows**:
- **Method**: Accumulate minimum SDF along shadow ray.
- **Benefit**: Soft, realistic shadows.

**Ambient Occlusion**:
- **Method**: Sample SDF in hemisphere around point.
- **Benefit**: Approximate global illumination.

**Neural SDF**

**DeepSDF**:
- **Architecture**: MLP maps (x, y, z, latent) → SDF value.
- **Training**: Learn to predict SDF from ground truth.
- **Use**: Shape representation, generation, completion.
- **Benefit**: Compact (KB), continuous, interpolatable.

**Neural Implicit Surfaces**:
- **Method**: Neural network represents SDF.
- **Benefit**: Smooth, high-quality surfaces.
- **Examples**: DeepSDF, IGR, SAL.

**Conditional Neural SDF**:
- **Method**: Condition on observations (point cloud, image).
- **Use**: 3D reconstruction from partial data.

**Challenges**

**Computation**:
- **Problem**: Evaluating SDF at many points is expensive.
- **Solution**: Hierarchical evaluation, octrees, hash encoding.

**Learning**:
- **Problem**: Neural networks struggle with exact SDF (Lipschitz constraint).
- **Solution**: Eikonal loss, geometric regularization.

**Topology Changes**:
- **Problem**: Discrete SDFs struggle with topology changes.
- **Solution**: Adaptive grids, implicit representations.

**Thin Structures**:
- **Problem**: Thin features require high resolution.
- **Solution**: Adaptive resolution, multi-scale.

**TSDF (Truncated SDF)**

**Definition**:
- **Truncation**: Clamp SDF to [-τ, +τ] near surface.
- **Benefit**: Focus computation on surface region.

**KinectFusion**:
- **Method**: Fuse depth maps into TSDF volume.
- **Process**: 
  1. Align depth map to volume.
  2. Integrate depth into TSDF (weighted average).
  3. Extract mesh via Marching Cubes.
- **Benefit**: Real-time 3D reconstruction.

**Voxblox**:
- **Method**: Efficient TSDF for robotics.
- **Benefit**: Fast, memory-efficient, incremental.

**Quality Metrics**

- **Accuracy**: Distance to ground truth surface.
- **Completeness**: Coverage of object surface.
- **Lipschitz Property**: Verify |∇SDF| ≈ 1.
- **Surface Quality**: Smoothness, detail preservation.
- **Rendering Quality**: Visual realism of rendered SDF.

**SDF Tools**

**Analytic SDF**:
- **Shadertoy**: Online SDF ray marching demos.
- **Inigo Quilez**: Comprehensive SDF resource.

**Discrete SDF**:
- **Open3D**: TSDF integration and mesh extraction.
- **PCL**: Point cloud to SDF conversion.

**Neural SDF**:
- **DeepSDF**: Official implementation.
- **PyTorch3D**: Implicit function support.
- **Kaolin**: 3D deep learning with SDF.

**Rendering**:
- **ShaderToy**: Real-time SDF rendering.
- **Blender**: SDF-based procedural modeling.

**SDF vs. Other Representations**

**SDF vs. Occupancy**:
- **SDF**: Metric distance information.
- **Occupancy**: Binary inside/outside.
- **Benefit**: SDF provides more information.

**SDF vs. Meshes**:
- **SDF**: Implicit, continuous, easy boolean ops.
- **Meshes**: Explicit, efficient rendering.
- **Use**: SDF for modeling, mesh for rendering.

**SDF vs. Voxels**:
- **SDF**: Continuous, resolution-independent.
- **Voxels**: Discrete, fixed resolution.
- **Benefit**: SDF smoother, more accurate.

**Future of SDF**

- **Real-Time**: Fast neural SDF evaluation.
- **High-Resolution**: Capture fine geometric details.
- **Generalization**: Single model for all shapes.
- **Hybrid**: Combine with explicit representations.
- **Dynamic**: Represent deforming shapes.
- **Semantic**: Integrate semantic information with geometry.

Signed Distance Functions are a **powerful geometric representation** — they encode 3D shapes as continuous distance fields, enabling efficient rendering, robust reconstruction, and intuitive geometric operations, making them fundamental to modern computer graphics, vision, and robotics.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/signed-distance-functions-sdf) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
