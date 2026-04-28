# Physics-based rendering (PBR)

**Keywords**: physics-based rendering,computer vision

---

**Physics-based rendering (PBR)** is a rendering approach that **simulates light transport using physically accurate models** — following the laws of physics to produce realistic images by accurately modeling how light interacts with materials and surfaces, becoming the industry standard for film, games, and visualization.

**What Is Physics-Based Rendering?**

- **Definition**: Rendering using physically accurate light transport simulation.
- **Principle**: Follow laws of physics (energy conservation, reciprocity).
- **Goal**: Photorealistic images that behave correctly under any lighting.
- **Benefit**: Predictable, consistent results across lighting conditions.

**Why Physics-Based Rendering?**

- **Realism**: Produces photorealistic images.
- **Consistency**: Materials look correct under any lighting.
- **Predictability**: Physical correctness ensures plausible results.
- **Workflow**: Artist-friendly parameters (roughness, metalness).
- **Interoperability**: Standard material models work across tools.

**PBR Principles**

**Energy Conservation**:
- **Principle**: Reflected light ≤ incident light.
- **Implication**: Materials can't reflect more light than they receive.
- **Enforcement**: BRDF normalization, proper material models.

**Reciprocity**:
- **Principle**: f_r(ω_i, ω_o) = f_r(ω_o, ω_i)
- **Meaning**: Light path reversibility.
- **Implication**: Reflection same in both directions.

**Fresnel Reflection**:
- **Principle**: Reflection increases at grazing angles.
- **Effect**: Objects more reflective at edges.
- **Implementation**: Schlick approximation, full Fresnel equations.

**Microfacet Theory**:
- **Principle**: Surfaces composed of microscopic facets.
- **Effect**: Roughness from facet distribution.
- **Models**: GGX, Beckmann, Cook-Torrance.

**PBR Material Model**

**Metallic-Roughness Workflow**:
- **Base Color**: Albedo for dielectrics, reflectance for metals.
- **Metallic**: 0 (non-metal) to 1 (metal).
- **Roughness**: 0 (smooth) to 1 (rough).
- **Normal Map**: Surface detail.
- **Ambient Occlusion**: Cavity darkening.

**Specular-Glossiness Workflow**:
- **Diffuse Color**: Diffuse albedo.
- **Specular Color**: Specular reflectance.
- **Glossiness**: Inverse of roughness.
- **Less Common**: Metallic-roughness is now standard.

**PBR Rendering Equation**

**Rendering Equation**:
```
L_o(p, ω_o) = L_e(p, ω_o) + ∫ f_r(p, ω_i, ω_o) · L_i(p, ω_i) · (n · ω_i) dω_i
                              Ω

Where:
- L_o: Outgoing radiance
- L_e: Emitted radiance
- f_r: BRDF
- L_i: Incident radiance
- n: Surface normal
- Ω: Hemisphere
```

**Solving the Rendering Equation**:
- **Path Tracing**: Monte Carlo integration.
- **Rasterization + IBL**: Real-time approximation.
- **Radiosity**: Diffuse global illumination.

**PBR Techniques**

**Path Tracing**:
- **Method**: Trace light paths from camera through scene.
- **Benefit**: Accurate global illumination, all light transport effects.
- **Challenge**: Noisy, requires many samples.
- **Use**: Offline rendering (film, architecture).

**Image-Based Lighting (IBL)**:
- **Method**: Use environment maps for lighting.
- **Process**: Pre-filter environment map for different roughness levels.
- **Benefit**: Realistic lighting from HDR images.
- **Use**: Real-time rendering (games, AR).

**Physically-Based BRDF**:
- **Models**: Cook-Torrance, GGX microfacet.
- **Components**: Diffuse (Lambertian) + Specular (microfacet).
- **Benefit**: Energy conserving, physically plausible.

**Applications**

**Film and VFX**:
- **Use**: Photorealistic CGI for movies.
- **Benefit**: Seamless integration of CGI with live action.
- **Tools**: Arnold, RenderMan, V-Ray.

**Gaming**:
- **Use**: Realistic graphics in real-time.
- **Benefit**: Immersive, believable environments.
- **Engines**: Unreal Engine, Unity, Frostbite.

**Product Visualization**:
- **Use**: Accurate product rendering for marketing.
- **Benefit**: Photorealistic product images.

**Architecture**:
- **Use**: Realistic visualization of designs.
- **Benefit**: Accurate lighting and material representation.

**Virtual Production**:
- **Use**: Real-time rendering for LED stages.
- **Benefit**: In-camera final pixels.

**PBR Workflow**

1. **Modeling**: Create 3D geometry.
2. **Texturing**: Create PBR material maps (albedo, roughness, metallic, normal).
3. **Lighting**: Set up lights or environment maps.
4. **Rendering**: Render using PBR renderer.
5. **Post-Processing**: Color grading, compositing.

**PBR Material Authoring**

**Substance Painter**:
- **Use**: Paint PBR materials on 3D models.
- **Benefit**: Real-time PBR preview.

**Quixel Mixer**:
- **Use**: Create PBR materials from scans.
- **Benefit**: Photorealistic materials.

**Blender**:
- **Use**: Node-based PBR material creation.
- **Benefit**: Free, powerful.

**Challenges**

**Computational Cost**:
- **Problem**: Accurate light transport is expensive.
- **Solution**: Approximations (IBL), denoising, GPU acceleration.

**Material Complexity**:
- **Problem**: Real materials are complex (layered, anisotropic, subsurface).
- **Solution**: Advanced material models, multi-layer BRDFs.

**Artist Workflow**:
- **Problem**: Physical correctness can be unintuitive.
- **Solution**: Artist-friendly parameters, presets, validation tools.

**Real-Time Constraints**:
- **Problem**: Full path tracing too slow for real-time.
- **Solution**: Approximations (IBL, screen-space effects), hardware ray tracing.

**PBR in Real-Time**

**Deferred Shading**:
- **Method**: Separate geometry and lighting passes.
- **Benefit**: Efficient for many lights.

**Image-Based Lighting**:
- **Method**: Pre-filtered environment maps.
- **Benefit**: Realistic lighting, efficient.

**Screen-Space Reflections**:
- **Method**: Reflect visible geometry.
- **Benefit**: Plausible reflections, fast.
- **Limitation**: Only reflects visible objects.

**Hardware Ray Tracing**:
- **Method**: GPU-accelerated ray tracing (RTX, DXR).
- **Benefit**: Accurate reflections, shadows, global illumination.
- **Use**: Modern games, real-time applications.

**Quality Metrics**

- **Physical Correctness**: Energy conservation, reciprocity.
- **Visual Realism**: Photorealism, believability.
- **Consistency**: Materials look correct under different lighting.
- **Performance**: Frame rate, rendering time.

**PBR Standards**

**glTF**:
- **Standard**: 3D asset format with PBR materials.
- **Workflow**: Metallic-roughness.
- **Use**: Web, AR, VR.

**USD (Universal Scene Description)**:
- **Standard**: Pixar's scene description format.
- **Materials**: Supports PBR materials.
- **Use**: Film, VFX pipelines.

**MaterialX**:
- **Standard**: Material definition language.
- **Benefit**: Interoperability across tools.

**Future of PBR**

- **Real-Time Path Tracing**: Full path tracing at interactive rates.
- **Neural Rendering**: AI-accelerated PBR rendering.
- **Advanced Materials**: Better models for complex materials.
- **Spectral Rendering**: Full spectral light transport.
- **Accessibility**: Easier PBR for all creators.

Physics-based rendering is the **foundation of modern computer graphics** — it produces photorealistic images by accurately simulating light transport, making it the standard for film, games, visualization, and any application requiring realistic visual quality.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/physics-based-rendering) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
