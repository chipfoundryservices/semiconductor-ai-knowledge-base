# 3D Gaussian Splatting

**Keywords**: 3d gaussian splatting,computer vision

---

**3D Gaussian Splatting** is a **novel 3D scene representation using anisotropic 3D Gaussians** — representing scenes as collections of oriented ellipsoids that can be rendered extremely fast through rasterization, achieving real-time rendering speeds (100+ FPS) while maintaining quality comparable to NeRF, revolutionizing real-time photorealistic rendering.

**What Is 3D Gaussian Splatting?**

- **Definition**: Represent 3D scenes as sets of 3D Gaussian primitives.
- **Primitive**: Each Gaussian is an oriented ellipsoid with position, covariance, color, opacity.
- **Rendering**: Fast rasterization-based rendering (not ray marching).
- **Speed**: 100-200 FPS real-time rendering on consumer GPUs.
- **Quality**: Comparable to NeRF, often better for fine details.

**Why Gaussian Splatting?**

**Speed**:
- **Real-Time**: 100+ FPS rendering (vs. 1-30 FPS for NeRF variants).
- **Rasterization**: Leverages GPU rasterization pipeline.
- **No Ray Marching**: Avoids expensive volumetric integration.

**Quality**:
- **High Fidelity**: Photorealistic rendering quality.
- **Fine Details**: Captures thin structures better than NeRF.
- **View-Dependent**: Supports view-dependent effects.

**Flexibility**:
- **Explicit**: Gaussians can be edited, moved, deleted.
- **Interpretable**: Each Gaussian has clear geometric meaning.

**3D Gaussian Representation**

**Gaussian Primitive**:
- **Position**: μ = (x, y, z) — center of Gaussian.
- **Covariance**: Σ — 3x3 matrix defining shape and orientation.
- **Color**: c = (r, g, b) or spherical harmonics for view-dependence.
- **Opacity**: α — transparency.

**Gaussian Function**:
```
G(x) = exp(-1/2 (x - μ)^T Σ^-1 (x - μ))

Where:
- x: 3D point
- μ: Gaussian center
- Σ: Covariance matrix (defines ellipsoid shape)
```

**Anisotropic**:
- Gaussians are ellipsoids, not spheres.
- Can be stretched and oriented to match scene geometry.
- More efficient representation than isotropic Gaussians.

**How Gaussian Splatting Works**

**Training**:
1. **Initialization**: Start with sparse point cloud (from SfM).
2. **Optimization**: Optimize Gaussian parameters to match training images.
   - Position, covariance, color, opacity.
3. **Adaptive Density Control**: Add/remove Gaussians as needed.
   - Split large Gaussians in high-detail areas.
   - Remove low-opacity Gaussians.
4. **Convergence**: Train for 7k-30k iterations (minutes).

**Rendering**:
1. **Projection**: Project 3D Gaussians to 2D screen space.
2. **Sorting**: Sort Gaussians by depth (front to back).
3. **Rasterization**: Rasterize each Gaussian as 2D splat.
4. **Alpha Blending**: Blend Gaussians using alpha compositing.
5. **Output**: Final rendered image.

**Rendering Equation**:
```
C = Σ c_i α_i Π (1 - α_j)
    i        j<i

Where:
- C: Final pixel color
- c_i: Color of Gaussian i
- α_i: Opacity of Gaussian i
- Product: Accumulated transparency from front Gaussians
```

**Advantages Over NeRF**

**Speed**:
- **100x Faster Rendering**: 100+ FPS vs. 1-30 FPS.
- **Real-Time**: Suitable for interactive applications.
- **Rasterization**: Leverages optimized GPU pipelines.

**Training**:
- **Faster**: Minutes vs. seconds/hours for NeRF variants.
- **Simpler**: No complex sampling strategies needed.

**Editability**:
- **Explicit**: Can directly manipulate Gaussians.
- **Intuitive**: Move, scale, rotate, delete Gaussians.
- **Compositing**: Easy to combine multiple scenes.

**Quality**:
- **Fine Details**: Better at thin structures (wires, branches).
- **Sharp Edges**: Cleaner edges than volumetric methods.

**Applications**

**Real-Time VR/AR**:
- Interactive exploration of photorealistic scenes.
- Low-latency rendering for VR headsets.

**Gaming**:
- Photorealistic game environments from photos.
- Real-time rendering at high frame rates.

**Digital Twins**:
- Interactive 3D models of real spaces.
- Industrial inspection, facility management.

**Content Creation**:
- Rapid 3D asset creation for film, advertising.
- Photorealistic backgrounds and environments.

**Robotics**:
- Real-time 3D scene representation.
- Fast rendering for simulation and planning.

**3D Gaussian Splatting Pipeline**

1. **Image Capture**: Collect images with camera poses (COLMAP).
2. **Point Cloud Initialization**: Generate sparse point cloud.
3. **Gaussian Initialization**: Create Gaussian at each point.
4. **Optimization Loop**:
   - Render training views.
   - Compute loss (L1 + SSIM).
   - Backpropagate gradients.
   - Update Gaussian parameters.
   - Adaptive density control (split/clone/prune).
5. **Convergence**: Stop when loss plateaus.
6. **Export**: Save Gaussians for real-time rendering.

**Adaptive Density Control**

**Densification**:
- **Clone**: Duplicate Gaussians in under-reconstructed areas.
- **Split**: Split large Gaussians into smaller ones.
- **Trigger**: Based on gradient magnitude.

**Pruning**:
- **Remove**: Delete low-opacity Gaussians.
- **Remove**: Delete very large Gaussians (likely artifacts).
- **Benefit**: Keeps representation compact and efficient.

**Challenges**

**Memory**:
- Millions of Gaussians for complex scenes.
- Each Gaussian stores position, covariance, color, opacity.
- Typically 100-500 MB per scene.

**Artifacts**:
- **Floaters**: Spurious Gaussians in empty space.
- **Holes**: Missing geometry in some areas.
- **Mitigation**: Careful hyperparameter tuning, regularization.

**View-Dependent Effects**:
- Basic version uses RGB color (view-independent).
- Spherical harmonics add view-dependence (more memory).

**Quality Metrics**

- **PSNR**: 30-35 dB (comparable to NeRF).
- **SSIM**: 0.95-0.98 (high structural similarity).
- **LPIPS**: 0.02-0.05 (low perceptual difference).
- **Rendering FPS**: 100-200 FPS (real-time).
- **Training Time**: 5-30 minutes.

**Comparison with Other Methods**

**vs. NeRF**:
- **Speed**: 100x faster rendering.
- **Quality**: Comparable, better for fine details.
- **Editability**: Much easier to edit.

**vs. Instant NGP**:
- **Speed**: 3-10x faster rendering.
- **Quality**: Similar.
- **Memory**: More memory (explicit representation).

**vs. Traditional Meshes**:
- **Quality**: More photorealistic (view-dependent effects).
- **Flexibility**: Easier to create from images.
- **Rendering**: Slightly slower than textured meshes.

**Implementation**

**Official Implementation**:
- **GitHub**: graphdeco-inria/gaussian-splatting.
- **Language**: Python/CUDA.
- **Requirements**: NVIDIA GPU with CUDA.

**Viewers**:
- **WebGL Viewer**: View Gaussians in browser.
- **Real-Time Viewer**: C++/CUDA viewer for maximum performance.

**Usage**:
```bash
# Train on images
python train.py -s data/scene

# Real-time viewer
./SIBR_gaussianViewer_app -m output/scene
```

**Extensions and Variants**

**Dynamic Gaussian Splatting**:
- Extend to dynamic scenes with moving objects.
- Time-dependent Gaussian parameters.

**4D Gaussian Splatting**:
- Represent space-time (3D + time).
- Capture dynamic scenes over time.

**Semantic Gaussian Splatting**:
- Add semantic labels to Gaussians.
- Enable semantic scene understanding.

**Compressed Gaussian Splatting**:
- Reduce memory footprint.
- Quantization, compression techniques.

**Future Directions**

- **Compression**: Reduce memory for mobile deployment.
- **Editing Tools**: Intuitive interfaces for scene editing.
- **Generalization**: Learn priors for faster reconstruction.
- **Semantic Integration**: Combine with semantic understanding.
- **Large-Scale**: Efficient representation for city-scale scenes.

3D Gaussian Splatting is a **breakthrough in real-time rendering** — it achieves photorealistic quality at interactive frame rates, making it ideal for VR, AR, gaming, and any application requiring fast, high-quality 3D rendering, representing a major step toward practical photorealistic 3D graphics.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/3d-gaussian-splatting) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
