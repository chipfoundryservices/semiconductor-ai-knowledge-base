# Depth completion

**Keywords**: depth completion,computer vision

---

**Depth completion** is the task of **generating dense depth maps from sparse depth measurements** — filling in missing depth values to create complete, high-resolution depth maps, typically combining sparse lidar points with dense RGB images to leverage the strengths of both sensors for autonomous vehicles, robotics, and 3D reconstruction.

**What Is Depth Completion?**

- **Definition**: Densify sparse depth measurements into complete depth maps.
- **Input**: Sparse depth (lidar, ToF) + RGB image (optional).
- **Output**: Dense depth map with depth for every pixel.
- **Goal**: Combine sparse accurate depth with dense image guidance.

**Why Depth Completion?**

**Sensor Limitations**:
- **Lidar**: Accurate but sparse (64-128 beams typical).
- **Stereo/Monocular**: Dense but less accurate, scale ambiguous.
- **Depth Sensors**: Limited range, indoor only.

**Complementary Strengths**:
- **Lidar**: Accurate metric depth, works in any lighting.
- **Camera**: Dense, high-resolution, captures appearance.
- **Combination**: Dense, accurate depth maps.

**Applications**:
- **Autonomous Vehicles**: Dense depth for obstacle detection, planning.
- **Robotics**: Detailed environment understanding.
- **3D Reconstruction**: Complete 3D models from sparse scans.

**Depth Completion Approaches**

**Interpolation-Based**:
- **Method**: Interpolate sparse depth using image guidance.
- **Techniques**: Bilateral filtering, guided filtering, inpainting.
- **Benefit**: Simple, fast.
- **Limitation**: Limited to smooth interpolation, no complex reasoning.

**Optimization-Based**:
- **Method**: Formulate as energy minimization problem.
- **Energy**: Data term (match sparse depth) + smoothness term (smooth depth).
- **Image Guidance**: Depth discontinuities align with image edges.
- **Benefit**: Principled, interpretable.
- **Limitation**: Slow, requires parameter tuning.

**Learning-Based**:
- **Method**: Neural networks learn to complete depth.
- **Training**: Supervised on dense ground truth depth.
- **Benefit**: Handles complex patterns, state-of-the-art accuracy.
- **Examples**: SparseToDense, DeepLidar, CSPN, PENet.

**Depth Completion Pipeline**

1. **Input**: Sparse lidar depth + RGB image.
2. **Feature Extraction**: Extract features from RGB and sparse depth.
3. **Fusion**: Combine RGB and depth features.
4. **Depth Prediction**: Predict dense depth map.
5. **Refinement**: Refine depth using confidence, multi-scale processing.
6. **Output**: Dense depth map.

**Depth Completion Networks**

**Early Fusion**:
- **Method**: Concatenate RGB and sparse depth, process jointly.
- **Benefit**: Simple, learns joint representation.

**Late Fusion**:
- **Method**: Process RGB and depth separately, fuse at end.
- **Benefit**: Specialized processing for each modality.

**Multi-Stage**:
- **Method**: Coarse-to-fine depth prediction.
- **Stages**: Coarse depth → refinement → final depth.
- **Benefit**: Capture both global structure and local details.

**Depth Completion Techniques**

**Convolutional Spatial Propagation Network (CSPN)**:
- **Innovation**: Learn affinity matrix for spatial propagation.
- **Benefit**: Propagate depth from sparse to dense guided by image.

**Confidence-Guided**:
- **Method**: Predict confidence for each depth value.
- **Use**: Weight predictions by confidence during fusion.
- **Benefit**: Handle uncertainty, improve robustness.

**Multi-Modal Fusion**:
- **Method**: Fuse RGB, sparse depth, and other modalities (normals, semantics).
- **Benefit**: Leverage complementary information.

**Self-Supervised**:
- **Method**: Train without dense ground truth.
- **Supervision**: Photometric consistency, sparse depth supervision.
- **Benefit**: Reduce annotation requirements.

**Applications**

**Autonomous Vehicles**:
- **Perception**: Dense depth for obstacle detection.
- **Planning**: Detailed environment understanding for path planning.
- **Safety**: Redundant depth estimation (lidar + camera).

**Robotics**:
- **Navigation**: Dense depth for obstacle avoidance.
- **Manipulation**: Detailed object geometry for grasping.
- **Mapping**: Complete 3D maps from sparse scans.

**3D Reconstruction**:
- **Complete Models**: Fill holes in sparse reconstructions.
- **High-Resolution**: Combine sparse accurate depth with dense image detail.

**AR/VR**:
- **Scene Understanding**: Dense depth for realistic AR/VR.
- **Occlusion**: Accurate depth for correct occlusion handling.

**Challenges**

**Sparsity**:
- **Problem**: Very sparse input (0.5-5% of pixels have depth).
- **Solution**: Strong image guidance, learned priors.

**Accuracy vs. Density Trade-off**:
- **Problem**: Interpolation may introduce errors.
- **Solution**: Confidence estimation, careful fusion.

**Edge Preservation**:
- **Problem**: Depth discontinuities at object boundaries.
- **Solution**: Image-guided filtering, edge-aware processing.

**Generalization**:
- **Problem**: Models trained on specific sensors/scenes may not generalize.
- **Solution**: Train on diverse data, domain adaptation.

**Quality Metrics**

**Error Metrics**:
- **RMSE**: Root mean squared error.
- **MAE**: Mean absolute error.
- **iRMSE**: Inverse RMSE (emphasizes close depths).
- **iMAE**: Inverse MAE.

**Accuracy Metrics**:
- **δ < 1.25**: Percentage within 25% relative error.
- **δ < 1.25²**: Within 56% relative error.
- **δ < 1.25³**: Within 95% relative error.

**Depth Completion Datasets**

**KITTI Depth Completion**:
- **Data**: Sparse lidar + RGB images from autonomous driving.
- **Ground Truth**: Dense depth from accumulated lidar scans.
- **Benchmark**: Standard benchmark for depth completion.

**NYU Depth V2**:
- **Data**: Indoor scenes with Kinect depth.
- **Use**: Indoor depth completion.

**Depth Completion Models**

**SparseToDense**:
- **Architecture**: Encoder-decoder with RGB and sparse depth input.
- **Training**: Supervised on KITTI.

**DeepLidar**:
- **Innovation**: Surface normals as intermediate representation.
- **Benefit**: Better edge preservation.

**CSPN (Convolutional Spatial Propagation Network)**:
- **Innovation**: Learned spatial propagation.
- **Benefit**: Efficient, accurate propagation.

**PENet (Pyramid Encoding Network)**:
- **Innovation**: Multi-scale pyramid encoding.
- **Benefit**: Capture both global and local context.

**Future of Depth Completion**

- **Real-Time**: Fast depth completion for real-time applications.
- **Self-Supervised**: Reduce reliance on dense ground truth.
- **Multi-Modal**: Integrate more sensors (radar, event cameras).
- **Semantic**: Leverage semantic understanding for better completion.
- **Uncertainty**: Quantify uncertainty in completed depth.
- **Generalization**: Models that work across sensors and scenes.

Depth completion is **essential for practical 3D perception** — it combines the accuracy of sparse depth sensors with the density of cameras, enabling detailed, accurate depth maps for autonomous vehicles, robotics, and 3D reconstruction applications.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/depth-completion) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
