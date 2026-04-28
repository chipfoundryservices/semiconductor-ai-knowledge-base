# Multi-view stereo (MVS)

**Keywords**: multi-view stereo (mvs),multi-view stereo,mvs,computer vision

---

**Multi-view stereo (MVS)** is a technique for **computing dense 3D reconstruction from multiple calibrated images** — estimating depth for every pixel by matching corresponding points across views, producing detailed 3D models with millions of points, forming the dense reconstruction stage after Structure from Motion in photogrammetry pipelines.

**What Is Multi-View Stereo?**

- **Definition**: Dense 3D reconstruction from multiple views.
- **Input**: Images + camera poses (from SfM).
- **Output**: Dense depth maps or 3D point cloud.
- **Goal**: Reconstruct complete, detailed 3D geometry.

**MVS vs. Stereo**

**Two-View Stereo**:
- **Input**: Two images (stereo pair).
- **Output**: Single depth map.
- **Limitation**: Occlusions, ambiguities.

**Multi-View Stereo**:
- **Input**: Many images (3 to hundreds).
- **Output**: Multiple depth maps, fused into 3D model.
- **Benefit**: More robust, handles occlusions, reduces ambiguities.

**Why Multi-View Stereo?**

- **Completeness**: Multiple views cover more of the scene.
- **Robustness**: Redundancy reduces errors from occlusions, textureless regions.
- **Accuracy**: More views improve depth accuracy.
- **Detail**: Dense reconstruction captures fine details.

**MVS Pipeline**

1. **Input**: Images + camera poses (from SfM).
2. **Depth Map Estimation**: Compute depth map for each image.
3. **Depth Map Filtering**: Remove outliers, enforce consistency.
4. **Depth Map Fusion**: Merge depth maps into single 3D model.
5. **Meshing**: Convert point cloud to mesh (optional).
6. **Texturing**: Project images onto mesh (optional).

**Depth Map Estimation**

**Plane Sweep**:
- **Method**: For each pixel, sweep depth hypotheses, find best match.
- **Matching Cost**: Photometric similarity across views.
- **Aggregation**: Smooth cost volume.
- **Optimization**: Select depth minimizing cost.

**Patch Match**:
- **Method**: Propagate good depth estimates to neighbors.
- **Random Search**: Try random depth hypotheses.
- **Benefit**: Fast, handles large depth ranges.
- **Example**: COLMAP PatchMatch MVS.

**Learning-Based**:
- **Method**: Neural networks estimate depth from multiple views.
- **Cost Volume**: Build 3D cost volume, process with 3D CNN.
- **Examples**: MVSNet, CasMVSNet, TransMVSNet.
- **Benefit**: Better handling of textureless regions, occlusions.

**Matching Cost**

**Photometric Similarity**:
- **NCC (Normalized Cross-Correlation)**: Robust to brightness changes.
- **SAD (Sum of Absolute Differences)**: Simple, fast.
- **Census Transform**: Robust to illumination changes.

**Multi-View Consistency**:
- **Aggregate**: Combine costs from multiple views.
- **Robust**: Median, truncated mean to handle outliers.

**Depth Map Filtering**

**Geometric Consistency**:
- **Forward-Backward Check**: Project depth to other views, check consistency.
- **Triangulation Angle**: Reject points with small triangulation angle.
- **Reprojection Error**: Reject points with large reprojection error.

**Photometric Consistency**:
- **Check**: Verify photometric similarity across views.
- **Threshold**: Reject points below similarity threshold.

**Depth Map Fusion**

**Point Cloud Generation**:
- **Unproject**: Convert depth maps to 3D points.
- **Merge**: Combine points from all depth maps.
- **Filtering**: Remove duplicates, outliers.

**Volumetric Fusion**:
- **TSDF (Truncated Signed Distance Function)**: Fuse depth maps into volume.
- **Marching Cubes**: Extract mesh from TSDF.
- **Benefit**: Smooth, complete surface.

**Poisson Reconstruction**:
- **Input**: Oriented point cloud (points + normals).
- **Output**: Watertight mesh.
- **Benefit**: Fills holes, smooth surface.

**Applications**

**Cultural Heritage**:
- **Digitization**: Create detailed 3D models of artifacts, buildings.
- **Preservation**: Digital archives of historical sites.
- **Virtual Tours**: Explore heritage sites remotely.

**Film and VFX**:
- **Set Reconstruction**: Digitize film sets for VFX.
- **Actor Capture**: Create digital doubles.
- **Environment Capture**: Photorealistic backgrounds.

**Architecture**:
- **As-Built Documentation**: Capture existing buildings.
- **BIM**: Create Building Information Models.
- **Renovation Planning**: Accurate measurements for renovation.

**E-Commerce**:
- **Product Modeling**: 3D models for online shopping.
- **Virtual Try-On**: Visualize products in customer space.

**Robotics**:
- **Mapping**: Build detailed 3D maps for navigation.
- **Manipulation**: Understand object geometry for grasping.

**Challenges**

**Textureless Regions**:
- **Problem**: Smooth surfaces lack features for matching.
- **Solution**: Regularization, learning-based methods.

**Occlusions**:
- **Problem**: Objects hidden in some views.
- **Solution**: Multi-view consistency checks, outlier filtering.

**Reflections and Transparency**:
- **Problem**: Violate Lambertian assumption.
- **Solution**: Robust matching costs, outlier rejection.

**Computational Cost**:
- **Problem**: Dense matching is expensive.
- **Solution**: GPU acceleration, efficient algorithms.

**MVS Methods**

**Traditional MVS**:
- **PMVS**: Patch-based Multi-View Stereo.
- **CMVS**: Clustering for large-scale MVS.
- **COLMAP**: State-of-the-art traditional MVS.

**Learning-Based MVS**:
- **MVSNet**: Deep learning for MVS depth estimation.
- **CasMVSNet**: Cascade cost volume for efficiency.
- **TransMVSNet**: Transformer-based MVS.
- **PatchmatchNet**: Learned PatchMatch for MVS.

**Hybrid**:
- **ACMM**: Adaptive Checkerboard Multi-View Matching.
- **ACMP**: Adaptive Checkerboard Multi-View Propagation.

**Quality Metrics**

- **Completeness**: Percentage of surface reconstructed.
- **Accuracy**: Distance to ground truth geometry.
- **Precision**: Percentage of reconstructed points within threshold.
- **Recall**: Percentage of ground truth points reconstructed.
- **F-Score**: Harmonic mean of precision and recall.

**MVS Benchmarks**

**DTU**: Indoor objects with ground truth.
**Tanks and Temples**: Outdoor and indoor scenes.
**ETH3D**: High-resolution multi-view stereo benchmark.
**BlendedMVS**: Large-scale MVS dataset.

**MVS Tools**

**Open Source**:
- **COLMAP**: State-of-the-art SfM and MVS.
- **OpenMVS**: Open-source MVS library.
- **MVE**: Multi-View Environment.

**Commercial**:
- **RealityCapture**: Fast commercial photogrammetry.
- **Agisoft Metashape**: Professional photogrammetry.
- **Pix4D**: Drone mapping and photogrammetry.

**Learning-Based**:
- **MVSNet**: Neural MVS depth estimation.
- **CasMVSNet**: Cascade MVS network.

**Future of MVS**

- **Real-Time**: Instant dense reconstruction from video.
- **Learning-Based**: Neural networks as standard.
- **Semantic**: 3D models with semantic labels.
- **Dynamic**: Reconstruct moving objects and scenes.
- **Large-Scale**: Efficient MVS for city-scale environments.
- **Robustness**: Handle challenging conditions (reflections, transparency).

Multi-view stereo is **essential for detailed 3D reconstruction** — it produces dense, accurate 3D models from images, enabling applications from cultural heritage preservation to virtual reality to robotics, forming the dense reconstruction stage that follows Structure from Motion in modern photogrammetry pipelines.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/multi-view-stereo-mvs) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
