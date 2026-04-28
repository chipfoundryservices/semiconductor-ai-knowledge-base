# Visual navigation

**Keywords**: visual navigation,robotics

---

**Visual navigation** is the capability of **robots to navigate through environments using visual information from cameras** — enabling autonomous movement by interpreting visual scenes to localize, map, plan paths, avoid obstacles, and reach goals without relying on GPS or pre-built maps, making robots capable of operating in indoor and GPS-denied environments.

**What Is Visual Navigation?**

- **Definition**: Navigation using camera images as primary sensor.
- **Input**: RGB images, RGB-D (depth), or stereo camera pairs.
- **Output**: Robot motion commands (velocity, steering).
- **Goal**: Navigate from current location to goal location safely and efficiently.

**Why Visual Navigation?**

- **Rich Information**: Cameras provide dense, high-resolution information.
  - Recognize objects, read signs, understand scenes.

- **Passive Sensing**: Cameras don't emit signals (unlike lidar, radar).
  - Stealthy, low power, no interference.

- **Cost**: Cameras are cheap compared to lidar.
  - Enables low-cost robots.

- **Human-Like**: Humans navigate primarily using vision.
  - Intuitive, leverages human-designed environments (signs, markings).

**Visual Navigation Components**

**Localization**:
- **Problem**: Where am I?
- **Solution**: Estimate robot pose from visual observations.
- **Methods**: Visual odometry, place recognition, SLAM.

**Mapping**:
- **Problem**: What does environment look like?
- **Solution**: Build map from visual observations.
- **Methods**: SLAM, 3D reconstruction, semantic mapping.

**Path Planning**:
- **Problem**: How to reach goal?
- **Solution**: Compute path from current to goal location.
- **Methods**: A*, RRT, potential fields, learned policies.

**Obstacle Avoidance**:
- **Problem**: How to avoid collisions?
- **Solution**: Detect obstacles, adjust path.
- **Methods**: Depth estimation, optical flow, learned avoidance.

**Visual Navigation Approaches**

**Classical Methods**:
- **Visual SLAM**: Simultaneous localization and mapping.
  - ORB-SLAM, LSD-SLAM, DSO.
  - Build map, localize within it.

- **Visual Odometry**: Estimate motion from image sequences.
  - Track features, estimate camera motion.

- **Geometric Planning**: Plan paths on built maps.
  - A*, Dijkstra, RRT on occupancy grid.

**Learning-Based Methods**:
- **End-to-End Learning**: Direct image-to-action mapping.
  - Neural network: image → steering command.
  - Learn from demonstrations or reinforcement learning.

- **Learned Representations**: Learn visual features for navigation.
  - Self-supervised learning, contrastive learning.

- **Semantic Navigation**: Navigate using semantic understanding.
  - "Go to the kitchen" — recognize kitchen from images.

**Hybrid Methods**:
- **Learned Perception + Classical Planning**: Use learning for perception, classical methods for planning.
- **Example**: Neural network detects obstacles, A* plans path.

**Visual Navigation Tasks**

**Point-Goal Navigation**:
- **Task**: Navigate to specified coordinates.
- **Input**: Target position (x, y) or (x, y, z).
- **Challenge**: Localization, path planning.

**Object-Goal Navigation**:
- **Task**: Navigate to object (e.g., "find the chair").
- **Input**: Object category or description.
- **Challenge**: Object recognition, exploration.

**Image-Goal Navigation**:
- **Task**: Navigate to location shown in image.
- **Input**: Goal image.
- **Challenge**: Visual place recognition, viewpoint changes.

**Instruction Following**:
- **Task**: Follow natural language directions.
- **Input**: "Go down the hallway, turn left at the painting"
- **Challenge**: Language grounding, spatial reasoning.

**Challenges in Visual Navigation**

**Appearance Changes**:
- Lighting variations (day/night, shadows).
- Seasonal changes (leaves, snow).
- Dynamic objects (people, vehicles).

**Occlusions**:
- Objects block view of environment.
- Partial observability.

**Ambiguity**:
- Similar-looking places (symmetry, repetition).
- Perceptual aliasing.

**Scale**:
- Large environments require efficient exploration.
- Long-horizon navigation.

**Dynamics**:
- Moving obstacles (people, vehicles).
- Real-time replanning required.

**Visual Navigation Sensors**

**Monocular Camera**:
- Single RGB camera.
- Cheap, compact, but no direct depth.
- Depth from motion (structure from motion).

**Stereo Camera**:
- Two cameras for depth estimation.
- Passive depth sensing.
- Limited range, sensitive to calibration.

**RGB-D Camera**:
- RGB + depth sensor (structured light, ToF).
- Direct depth measurement.
- Limited range (typically < 10m).

**360° Camera**:
- Omnidirectional view.
- See all directions simultaneously.
- Useful for exploration, loop closure.

**Applications**

**Indoor Robots**:
- **Service Robots**: Navigate homes, offices, hospitals.
- **Delivery Robots**: Deliver items within buildings.
- **Cleaning Robots**: Vacuum, mop floors.

**Outdoor Robots**:
- **Delivery Robots**: Sidewalk delivery (Starship, Nuro).
- **Agricultural Robots**: Navigate fields, orchards.
- **Inspection Robots**: Inspect infrastructure, facilities.

**Drones**:
- **Indoor Drones**: Navigate GPS-denied environments.
- **Inspection**: Inspect buildings, bridges, power lines.
- **Search and Rescue**: Navigate disaster sites.

**Autonomous Vehicles**:
- **Self-Driving Cars**: Navigate roads using cameras.
- **Parking**: Visual navigation in parking lots.

**Visual SLAM**

**Monocular SLAM**:
- **ORB-SLAM**: Feature-based SLAM.
  - Track ORB features, build sparse map.

- **LSD-SLAM**: Direct method, uses image intensities.
  - Dense or semi-dense reconstruction.

**RGB-D SLAM**:
- **KinectFusion**: Dense 3D reconstruction.
- **ElasticFusion**: Real-time dense SLAM.

**Stereo SLAM**:
- **ORB-SLAM2/3**: Supports stereo cameras.
- **VINS**: Visual-inertial SLAM.

**Learning-Based Visual Navigation**

**End-to-End Learning**:
- **Input**: Camera image.
- **Output**: Steering command or velocity.
- **Training**: Imitation learning or reinforcement learning.
- **Example**: NVIDIA PilotNet for autonomous driving.

**Modular Learning**:
- **Learned Perception**: Depth estimation, obstacle detection.
- **Classical Planning**: Path planning on learned representations.

**Semantic Navigation**:
- **Semantic Mapping**: Build maps with object labels.
- **Goal Specification**: "Go to the refrigerator"
- **Planning**: Navigate using semantic understanding.

**Quality Metrics**

- **Success Rate**: Percentage of goals reached.
- **Path Length**: Distance traveled (shorter is better).
- **Time**: Duration to reach goal.
- **Collisions**: Number of collisions (fewer is better).
- **Robustness**: Performance under variations (lighting, clutter).

**Visual Navigation Benchmarks**

**Habitat**: Photorealistic indoor navigation simulator.
- Point-goal, object-goal, image-goal navigation.

**Gibson**: Real-world 3D scans for navigation.

**Matterport3D**: Indoor scenes for embodied AI.

**CARLA**: Autonomous driving simulator.

**Future of Visual Navigation**

- **Foundation Models**: Large pre-trained models for navigation.
- **Zero-Shot Generalization**: Navigate novel environments without training.
- **Semantic Understanding**: Navigate using high-level scene understanding.
- **Multi-Modal**: Combine vision with other sensors (lidar, audio).
- **Lifelong Learning**: Continuously improve from experience.

Visual navigation is **essential for autonomous robots** — it enables robots to move through environments using the rich information provided by cameras, making robots capable of operating in diverse indoor and outdoor settings where GPS is unavailable or insufficient.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/visual-navigation) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
