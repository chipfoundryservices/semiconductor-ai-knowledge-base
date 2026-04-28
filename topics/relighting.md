# Relighting

**Keywords**: relighting,computer vision

---

**Relighting** is the process of **changing the lighting in images or 3D scenes** — modifying illumination conditions to simulate different times of day, weather, or artificial lighting, enabling realistic lighting edits for photography, film, AR, and virtual production without recapturing the scene.

**What Is Relighting?**

- **Definition**: Modify lighting in captured images or scenes.
- **Input**: Image/scene + desired lighting conditions.
- **Output**: Image/scene with new lighting.
- **Goal**: Realistic lighting changes without physical recapture.

**Why Relighting?**

- **Photography**: Change lighting after capture (golden hour, studio lighting).
- **Film/VFX**: Match lighting across shots, create dramatic effects.
- **AR/VR**: Realistic virtual objects matching real lighting.
- **Virtual Production**: Real-time lighting changes on LED stages.
- **E-Commerce**: Show products under different lighting conditions.

**Relighting Approaches**

**Image-Based Relighting**:
- **Method**: Modify image appearance to simulate new lighting.
- **Techniques**: Intrinsic decomposition, neural relighting.
- **Benefit**: Works with single image.
- **Limitation**: Limited to plausible lighting changes.

**Geometry-Based Relighting**:
- **Method**: Reconstruct 3D geometry, relight using rendering.
- **Pipeline**: 3D reconstruction → material estimation → rendering with new lights.
- **Benefit**: Physically accurate, flexible lighting.
- **Challenge**: Requires accurate geometry and materials.

**Light Stage Capture**:
- **Method**: Capture subject under many lighting conditions.
- **Relight**: Linearly combine captured images for any lighting.
- **Benefit**: Photorealistic, accurate.
- **Challenge**: Requires expensive light stage equipment.

**Neural Relighting**:
- **Method**: Neural networks learn to relight images.
- **Training**: Learn from multi-illumination datasets.
- **Benefit**: Fast, works with single image.
- **Examples**: Neural Relighting, Deep Relighting Networks.

**Relighting Techniques**

**Intrinsic Image Decomposition**:
- **Method**: Separate reflectance and shading.
- **Relight**: Modify shading component, keep reflectance.
- **Benefit**: Lighting-independent material editing.

**Spherical Harmonics**:
- **Method**: Represent lighting as spherical harmonic coefficients.
- **Relight**: Change coefficients to modify lighting.
- **Benefit**: Compact representation, efficient.

**Environment Map Relighting**:
- **Method**: Use environment maps (HDR images) for lighting.
- **Relight**: Replace environment map.
- **Benefit**: Realistic global illumination.

**Neural Rendering**:
- **Method**: Neural networks render scene under new lighting.
- **Training**: Learn light transport from data.
- **Benefit**: Fast, handles complex effects.

**Applications**

**Portrait Photography**:
- **Use**: Change lighting on portraits after capture.
- **Examples**: Studio lighting, golden hour, dramatic lighting.
- **Benefit**: Flexibility without reshoots.

**Product Photography**:
- **Use**: Show products under different lighting.
- **Benefit**: Consistent lighting across product catalog.

**Film and VFX**:
- **Use**: Match lighting across shots, create effects.
- **Examples**: Day-for-night, time of day changes.
- **Benefit**: Creative control in post-production.

**Augmented Reality**:
- **Use**: Match virtual object lighting to real scene.
- **Benefit**: Realistic AR integration.

**Virtual Production**:
- **Use**: Real-time relighting on LED stages.
- **Benefit**: In-camera final pixels, reduced post-production.

**Relighting Challenges**

**Shadows**:
- **Problem**: Changing lighting requires changing shadows.
- **Challenge**: Realistic shadow synthesis.
- **Solution**: Geometry-aware methods, learned shadow generation.

**Specularities**:
- **Problem**: Highlights change with lighting direction.
- **Challenge**: View-dependent effects.
- **Solution**: BRDF estimation, physics-based rendering.

**Inter-Reflections**:
- **Problem**: Light bounces between surfaces.
- **Challenge**: Global illumination effects.
- **Solution**: Path tracing, neural rendering.

**Occlusions**:
- **Problem**: New lighting may reveal occluded regions.
- **Challenge**: Inpainting hidden areas.
- **Solution**: Multi-view capture, learned priors.

**Relighting Pipeline**

**Image-Based**:
1. **Intrinsic Decomposition**: Separate reflectance and shading.
2. **Lighting Estimation**: Estimate current lighting.
3. **Shading Synthesis**: Generate new shading for target lighting.
4. **Recomposition**: Combine reflectance with new shading.

**Geometry-Based**:
1. **3D Reconstruction**: Recover scene geometry.
2. **Material Estimation**: Estimate surface materials (BRDF).
3. **Lighting Specification**: Define new lighting (environment map, point lights).
4. **Rendering**: Render scene with new lighting.

**Neural**:
1. **Input**: Image + target lighting parameters.
2. **Network**: Neural network predicts relit image.
3. **Output**: Relit image.

**Relighting Methods**

**One Light At a Time (OLAT)**:
- **Capture**: Photograph subject with one light at a time.
- **Relight**: Linearly combine images for any lighting.
- **Benefit**: Accurate, flexible.
- **Challenge**: Requires many captures (100+).

**Polynomial Texture Maps (PTM)**:
- **Method**: Fit polynomial to pixel intensity vs. light direction.
- **Relight**: Evaluate polynomial for new light direction.
- **Benefit**: Compact, efficient.

**Reflectance Transfer**:
- **Method**: Transfer lighting from one image to another.
- **Use**: Match lighting across images.

**Deep Learning Relighting**:
- **Method**: Train neural networks on multi-illumination data.
- **Examples**: Deep Relighting Networks, Neural Relighting.
- **Benefit**: Single image input, fast inference.

**Quality Metrics**

- **PSNR**: Peak signal-to-noise ratio.
- **SSIM**: Structural similarity.
- **LPIPS**: Learned perceptual similarity.
- **User Studies**: Subjective realism assessment.
- **Shadow Accuracy**: Correctness of shadow placement and softness.

**Relighting Datasets**

**Multi-Illumination**:
- **MIT Intrinsic Images**: Objects under multiple lighting.
- **Light Stage Data**: Faces captured in light stages.

**Synthetic**:
- **Rendered Scenes**: 3D scenes rendered with different lighting.
- **Benefit**: Perfect ground truth.

**Relighting Tools**

**Commercial**:
- **Adobe Photoshop**: Basic relighting tools.
- **Substance Painter**: Material-based relighting.
- **Unreal Engine**: Real-time relighting for virtual production.

**Research**:
- **Neural Relighting**: Deep learning-based methods.
- **Light Stage**: Professional capture systems.

**Future of Relighting**

- **Single-Image**: Accurate relighting from single image.
- **Real-Time**: Interactive relighting for live applications.
- **Video**: Temporally consistent relighting for video.
- **Semantic**: Understand scene semantics for better relighting.
- **Generalization**: Models that work on any scene.

Relighting is **essential for modern visual content creation** — it enables flexible lighting control after capture, supporting applications from photography to film to augmented reality, making lighting a creative tool rather than a constraint.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/relighting) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
