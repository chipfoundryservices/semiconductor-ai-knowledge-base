# Shape correspondence

**Keywords**: shape correspondence,computer vision

---

**Shape correspondence** is the problem of **finding matching points or regions across different 3D shapes** — establishing relationships between shapes to enable comparison, analysis, and transfer of properties, fundamental to shape matching, morphing, statistical modeling, and understanding shape variations.

**What Is Shape Correspondence?**

- **Definition**: Mapping between points/regions on different shapes.
- **Goal**: Find semantically or geometrically meaningful matches.
- **Types**: Point-to-point, region-to-region, dense or sparse.
- **Challenge**: Shapes may differ in pose, scale, topology, detail.

**Why Shape Correspondence?**

- **Shape Matching**: Determine if shapes are similar.
- **Deformation Transfer**: Transfer animation or edits between shapes.
- **Statistical Modeling**: Build shape models from aligned examples.
- **Morphing**: Smoothly interpolate between shapes.
- **Texture Transfer**: Map textures from one shape to another.
- **Shape Analysis**: Compare and understand shape variations.

**Types of Correspondence**

**Dense Correspondence**:
- **Definition**: Match every point on one shape to another.
- **Output**: Complete mapping between surfaces.
- **Use**: Morphing, texture transfer, detailed analysis.

**Sparse Correspondence**:
- **Definition**: Match key points or features.
- **Output**: Set of point pairs.
- **Use**: Shape retrieval, alignment, registration.

**Partial Correspondence**:
- **Definition**: Match subset of shapes (partial overlap).
- **Challenge**: Handle missing regions, occlusions.
- **Use**: Partial shape matching, scan alignment.

**Semantic Correspondence**:
- **Definition**: Match semantically similar regions (e.g., all "legs").
- **Benefit**: Meaningful across shape variations.
- **Use**: Shape understanding, part-based analysis.

**Correspondence Approaches**

**Geometric Methods**:
- **Method**: Match based on geometric properties (curvature, geodesics).
- **Examples**: Iterative Closest Point (ICP), geodesic distances.
- **Benefit**: No training data required.
- **Limitation**: Sensitive to pose, deformation.

**Feature-Based Methods**:
- **Method**: Extract features, match similar features.
- **Features**: Shape descriptors (SHOT, FPFH, HKS, WKS).
- **Benefit**: Robust to noise, partial data.

**Functional Maps**:
- **Method**: Represent correspondence as linear operator on function spaces.
- **Benefit**: Compact, handles symmetry, efficient optimization.
- **Use**: Non-rigid shape matching.

**Learning-Based Methods**:
- **Method**: Neural networks learn to predict correspondences.
- **Training**: Learn from datasets with ground truth correspondences.
- **Benefit**: Handle complex deformations, semantic matching.
- **Examples**: Deep Functional Maps, PointNet-based matching.

**Correspondence Techniques**

**Iterative Closest Point (ICP)**:
- **Method**: Iteratively find nearest neighbors and align.
- **Process**: Find correspondences → compute transformation → apply → repeat.
- **Use**: Rigid alignment, scan registration.
- **Limitation**: Local minima, requires good initialization.

**Geodesic Distance Matching**:
- **Method**: Match points with similar geodesic distance patterns.
- **Benefit**: Invariant to isometric deformations.
- **Use**: Non-rigid shape matching.

**Heat Kernel Signature (HKS)**:
- **Method**: Descriptor based on heat diffusion on surface.
- **Benefit**: Intrinsic, multi-scale, isometry-invariant.
- **Use**: Feature matching, shape analysis.

**Wave Kernel Signature (WKS)**:
- **Method**: Descriptor based on wave equation on surface.
- **Benefit**: Similar to HKS, different properties.

**Functional Maps**:
- **Method**: Represent correspondence as matrix mapping functions.
- **Representation**: C matrix such that C·f₁ ≈ f₂ for corresponding functions.
- **Benefit**: Compact (k×k matrix), handles symmetry, efficient.
- **Use**: Non-rigid shape matching, partial matching.

**Applications**

**Shape Matching**:
- **Use**: Determine similarity between shapes.
- **Process**: Establish correspondence → measure geometric difference.
- **Benefit**: Shape retrieval, classification.

**Deformation Transfer**:
- **Use**: Transfer animation from source to target character.
- **Process**: Establish correspondence → transfer deformations.
- **Benefit**: Reuse animations across characters.

**Statistical Shape Modeling**:
- **Use**: Build statistical models of shape variation.
- **Process**: Establish correspondence across dataset → PCA.
- **Benefit**: Compact shape representation, shape completion.

**Texture Transfer**:
- **Use**: Map texture from one shape to another.
- **Process**: Establish correspondence → transfer texture coordinates.
- **Benefit**: Rapid texturing, style transfer.

**Shape Morphing**:
- **Use**: Smooth interpolation between shapes.
- **Process**: Establish correspondence → interpolate positions.
- **Benefit**: Realistic shape transitions.

**Challenges**

**Ambiguity**:
- **Problem**: Multiple plausible correspondences (symmetry, repetition).
- **Solution**: Semantic constraints, global consistency.

**Topology Differences**:
- **Problem**: Shapes with different topology (holes, genus).
- **Solution**: Partial matching, topology-aware methods.

**Scale Variation**:
- **Problem**: Shapes at different scales or with different proportions.
- **Solution**: Scale-invariant features, normalization.

**Partial Data**:
- **Problem**: Incomplete shapes, occlusions.
- **Solution**: Partial matching algorithms, completion.

**Deformation**:
- **Problem**: Non-rigid deformations change geometry.
- **Solution**: Intrinsic methods (geodesics), isometry-invariant features.

**Correspondence Methods**

**Blended Intrinsic Maps (BIM)**:
- **Method**: Optimize functional maps with regularization.
- **Benefit**: Robust, handles symmetry.

**Deep Functional Maps**:
- **Method**: Neural networks learn functional map representation.
- **Benefit**: Learn from data, handle complex deformations.

**PointNet-Based Matching**:
- **Method**: Neural networks process point clouds, predict correspondences.
- **Benefit**: End-to-end learning, handles noise.

**Spectral Methods**:
- **Method**: Use eigenfunctions of Laplacian for matching.
- **Benefit**: Intrinsic, multi-scale.

**Optimal Transport**:
- **Method**: Find correspondence minimizing transport cost.
- **Benefit**: Principled, handles partial matching.

**Quality Metrics**

**Geodesic Error**:
- **Definition**: Geodesic distance between predicted and ground truth correspondences.
- **Use**: Evaluate correspondence accuracy.

**Correspondence Accuracy**:
- **Definition**: Percentage of correct correspondences (within threshold).

**Princeton Protocol**:
- **Benchmark**: Standard evaluation for shape correspondence.
- **Metrics**: Geodesic error at different thresholds.

**Semantic Accuracy**:
- **Definition**: Correctness of semantic part matching.

**Correspondence Datasets**

**FAUST**:
- **Data**: Human body scans with ground truth correspondences.
- **Use**: Non-rigid shape matching benchmark.

**SHREC**:
- **Data**: Various shape matching challenges.
- **Use**: Benchmark different correspondence methods.

**TOSCA**:
- **Data**: Non-rigid shapes with ground truth.
- **Use**: Isometric shape matching.

**SMAL**:
- **Data**: Animal shapes with correspondences.
- **Use**: Quadruped shape analysis.

**Correspondence Tools**

**Research Tools**:
- **Libigl**: Geometry processing with correspondence tools.
- **CGAL**: Computational geometry algorithms.
- **PyFM**: Functional maps in Python.

**Commercial**:
- **Wrap**: Automatic correspondence and wrapping.
- **Maya**: Manual correspondence tools.
- **Blender**: Shape keys, correspondence for animation.

**Deep Learning**:
- **PyTorch3D**: Differentiable correspondence operations.
- **PointNet**: Point cloud processing for matching.

**Functional Maps Framework**

**Representation**:
- **Idea**: Represent correspondence as linear operator on functions.
- **Matrix**: C such that C·f₁ ≈ f₂ for corresponding functions.
- **Basis**: Use Laplacian eigenfunctions as basis.

**Optimization**:
- **Objective**: Minimize ||C·F₁ - F₂||² + regularization.
- **Constraints**: Orthogonality, bijectivity, smoothness.

**Benefits**:
- **Compact**: k×k matrix (k ≈ 20-100) vs. n×n for point-to-point.
- **Symmetry**: Naturally handles symmetric shapes.
- **Partial**: Extends to partial matching.

**Future of Shape Correspondence**

- **Learning-Based**: Neural networks learn correspondences from data.
- **Semantic**: Understand semantic meaning for better matching.
- **Partial**: Robust partial shape matching.
- **Real-Time**: Interactive correspondence computation.
- **Topology-Aware**: Handle topology differences.
- **Multi-Modal**: Correspondence across different representations (mesh, point cloud, implicit).

Shape correspondence is **fundamental to shape analysis** — it enables comparing, matching, and transferring properties between shapes, supporting applications from animation to statistical modeling to shape understanding, making it possible to reason about relationships across diverse 3D geometry.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/shape-correspondence) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
