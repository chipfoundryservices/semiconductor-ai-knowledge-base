# Depth estimation from single image

**Keywords**: depth estimation from single image,computer vision

---

**Depth estimation from single image** is the task of **predicting per-pixel depth from a single RGB image** — inferring 3D scene geometry from 2D appearance using learned priors about object sizes, perspective, occlusions, and scene layout, enabling 3D understanding without stereo cameras or depth sensors.

**What Is Single-Image Depth Estimation?**

- **Definition**: Predict depth map from single RGB image.
- **Input**: Single RGB image.
- **Output**: Depth map (distance to camera for each pixel).
- **Challenge**: Ill-posed problem — infinite 3D scenes project to same 2D image.
- **Solution**: Learn priors from data to resolve ambiguity.

**Why Single-Image Depth?**

- **Accessibility**: Works with any camera, no special hardware.
- **Convenience**: No stereo calibration, no multiple views needed.
- **Ubiquity**: Enable depth understanding on billions of existing images.
- **Applications**: AR, robotics, autonomous vehicles, photography.

**Depth Estimation Approaches**

**Geometric Cues**:
- **Perspective**: Parallel lines converge at vanishing points.
- **Occlusion**: Closer objects occlude farther objects.
- **Relative Size**: Known object sizes provide scale.
- **Texture Gradient**: Texture density increases with distance.

**Learning-Based**:
- **Supervised**: Train on images with ground truth depth.
- **Self-Supervised**: Train on stereo pairs or video sequences.
- **Transfer Learning**: Pre-train on large datasets, fine-tune.

**Depth Estimation Methods**

**Supervised Learning**:
- **Training Data**: RGB images + ground truth depth (from lidar, depth sensors).
- **Network**: CNN or Transformer encoder-decoder.
- **Loss**: L1, L2, or scale-invariant loss.
- **Examples**: MiDaS, DPT, AdaBins.

**Self-Supervised Learning**:
- **Training Data**: Stereo pairs or monocular video.
- **Supervision**: Photometric consistency.
- **Process**:
  1. Predict depth from left image.
  2. Warp right image using predicted depth.
  3. Minimize difference between left and warped right.
- **Examples**: Monodepth, Monodepth2, PackNet.

**Depth Estimation Architectures**

**Encoder-Decoder**:
- **Encoder**: Extract features (ResNet, EfficientNet, ViT).
- **Decoder**: Upsample to full resolution depth map.
- **Skip Connections**: Preserve fine details.

**Transformer-Based**:
- **DPT (Dense Prediction Transformer)**: Vision Transformer for depth.
- **Benefit**: Better global context, long-range dependencies.

**Multi-Scale**:
- **Predict**: Depth at multiple scales.
- **Benefit**: Capture both coarse structure and fine details.

**Applications**

**Augmented Reality**:
- **Occlusion**: Render AR objects behind real objects.
- **Placement**: Place virtual objects on real surfaces.
- **Interaction**: Enable realistic AR interactions.

**Autonomous Vehicles**:
- **Obstacle Detection**: Identify obstacles and their distances.
- **Path Planning**: Plan safe paths using depth information.
- **Backup**: Complement lidar with camera-based depth.

**Robotics**:
- **Navigation**: Avoid obstacles using depth.
- **Manipulation**: Understand object geometry for grasping.
- **Mapping**: Build 3D maps from monocular cameras.

**Photography**:
- **Bokeh**: Simulate depth-of-field effects.
- **Refocusing**: Change focus after capture.
- **3D Photos**: Create 3D effects from 2D images.

**Accessibility**:
- **Navigation Assistance**: Help visually impaired navigate.
- **Scene Description**: Describe spatial layout of scenes.

**Challenges**

**Scale Ambiguity**:
- **Problem**: Monocular depth has unknown scale.
- **Solution**: Predict relative depth, or use known object sizes.

**Textureless Regions**:
- **Problem**: Smooth surfaces lack features.
- **Solution**: Learn priors, use global context.

**Occlusions**:
- **Problem**: Can't see behind objects.
- **Solution**: Infer from context, learned priors.

**Generalization**:
- **Problem**: Models trained on specific data may not generalize.
- **Solution**: Train on diverse datasets, domain adaptation.

**Depth Estimation Datasets**

**Indoor**:
- **NYU Depth V2**: Indoor scenes with Kinect depth.
- **ScanNet**: RGB-D scans of indoor environments.

**Outdoor**:
- **KITTI**: Autonomous driving with lidar depth.
- **Cityscapes**: Urban street scenes.

**Mixed**:
- **MegaDepth**: Internet photos with SfM depth.
- **Taskonomy**: Diverse indoor scenes.

**Quality Metrics**

**Absolute Metrics**:
- **RMSE**: Root mean squared error.
- **MAE**: Mean absolute error.
- **Abs Rel**: Mean absolute relative error.

**Relative Metrics**:
- **δ < 1.25**: Percentage of pixels with relative error < 25%.
- **δ < 1.25²**: Within 56% relative error.
- **δ < 1.25³**: Within 95% relative error.

**Scale-Invariant**:
- **SILog**: Scale-invariant logarithmic error.
- **Benefit**: Robust to scale ambiguity.

**Depth Estimation Models**

**MiDaS**:
- **Training**: Mixed datasets (multiple sources).
- **Benefit**: Generalizes well to diverse scenes.
- **Output**: Relative depth (scale ambiguous).

**DPT (Dense Prediction Transformer)**:
- **Architecture**: Vision Transformer encoder + convolutional decoder.
- **Benefit**: State-of-the-art accuracy, good generalization.

**AdaBins**:
- **Innovation**: Adaptive bins for depth prediction.
- **Benefit**: Better handling of depth range.

**Monodepth2**:
- **Training**: Self-supervised on monocular video.
- **Benefit**: No ground truth depth needed.

**Depth Estimation Techniques**

**Multi-Task Learning**:
- **Method**: Train depth jointly with other tasks (segmentation, normals).
- **Benefit**: Shared representations improve all tasks.

**Domain Adaptation**:
- **Method**: Adapt model trained on synthetic data to real data.
- **Benefit**: Leverage large synthetic datasets.

**Test-Time Optimization**:
- **Method**: Fine-tune on test image using self-supervision.
- **Benefit**: Improve accuracy on specific image.

**Future of Single-Image Depth**

- **Zero-Shot**: Generalize to any scene without training.
- **Metric Depth**: Predict absolute depth, not just relative.
- **Real-Time**: Fast depth estimation for mobile devices.
- **Video**: Temporally consistent depth for video.
- **Semantic**: Integrate semantic understanding.
- **Foundation Models**: Large pre-trained models for depth.

Single-image depth estimation is a **fundamental capability in computer vision** — it enables 3D understanding from ordinary 2D images, making depth perception accessible without special hardware, supporting applications from augmented reality to robotics to photography.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/depth-estimation-from-single-image) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
