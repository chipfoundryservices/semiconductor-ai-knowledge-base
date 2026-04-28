# Metrology Equipment

**Keywords**: metrology equipment semiconductor,optical critical dimension ocd,scatterometry measurement,x-ray metrology xrf,ellipsometry film thickness

---

**Metrology Equipment** is **the precision measurement instrumentation that characterizes critical dimensions, film thicknesses, overlay alignment, and material properties at nanometer-scale resolution — providing the quantitative feedback data that enables process control, yield learning, and technology development across all semiconductor manufacturing operations, with measurement uncertainties <1nm for advanced node requirements**.

**Optical Critical Dimension (OCD) Metrology:**
- **Scatterometry Principle**: illuminates periodic structures (gratings) with polarized light at multiple wavelengths and angles; measures reflected spectrum or angle-resolved intensity; compares to library of simulated spectra from rigorous coupled-wave analysis (RCWA) to extract CD, sidewall angle, and height
- **Spectroscopic Ellipsometry**: measures change in polarization state (Ψ and Δ) as function of wavelength; sensitive to film thickness, refractive index, and composition; KLA SpectraShape and Nova Prism systems achieve <0.3nm thickness repeatability for films 1-1000nm thick
- **Angle-Resolved Scatterometry**: measures reflected intensity vs angle at fixed wavelength; faster than spectroscopic methods; used for high-throughput inline monitoring; Applied Materials Viper and Nanometrics Atlas systems provide <1 second measurement time
- **Model-Based Analysis**: uses Maxwell's equations to simulate light interaction with 3D structures; fits measured spectra to simulated library by varying structure parameters; accuracy depends on model fidelity — requires accurate material optical constants and structure geometry

**X-Ray Metrology:**
- **X-Ray Fluorescence (XRF)**: excites atoms with X-rays, measures characteristic fluorescence energies to identify elements and quantify composition; measures film thickness and composition for metal films (Cu, W, Co, Ru); Bruker and Rigaku systems achieve 0.1nm thickness sensitivity for 1-100nm films
- **X-Ray Reflectometry (XRR)**: measures X-ray reflectivity vs incident angle; interference fringes encode film thickness and density information; non-destructive depth profiling of multilayer stacks; resolves individual layer thicknesses in 10-layer stacks with <0.2nm uncertainty
- **Small-Angle X-Ray Scattering (SAXS)**: characterizes nanoscale structures (pores, voids, grain size) in low-k dielectrics and metal films; measures size distributions and volume fractions; critical for advanced interconnect development
- **X-Ray Diffraction (XRD)**: measures crystal structure, strain, and texture; identifies phases and crystallographic orientation; used for high-k dielectrics, metal gates, and strain engineering characterization

**Scanning Probe Metrology:**
- **Atomic Force Microscopy (AFM)**: scans sharp tip (<10nm radius) across surface; measures topography with sub-nanometer vertical resolution; Bruker Dimension and Park Systems NX series provide 3D surface maps for roughness, step height, and pattern fidelity analysis
- **Scanning Tunneling Microscopy (STM)**: measures quantum tunneling current between conductive tip and sample; achieves atomic resolution on conductive surfaces; used for fundamental research and defect analysis rather than production metrology
- **Critical Dimension AFM (CD-AFM)**: uses flared tip to measure sidewall profiles of high-aspect-ratio structures; provides true 3D CD measurements that optical methods cannot; slow throughput (5-10 minutes per site) limits to reference metrology
- **Scanned Probe Microscopy (SPM)**: generic term encompassing AFM, STM, and variants (magnetic force microscopy, electrostatic force microscopy); provides nanoscale characterization beyond optical diffraction limits

**Overlay Metrology:**
- **Image-Based Overlay (IBO)**: captures images of overlay targets (box-in-box, frame-in-frame) from current and previous layers; measures relative displacement using image correlation; KLA Archer and ASML YieldStar systems achieve <0.3nm measurement precision
- **Diffraction-Based Overlay (DBO)**: uses scatterometry on specially designed grating targets; measures asymmetry in diffraction pattern to extract overlay; faster than IBO and works on smaller targets; enables high-density sampling across the wafer
- **On-Device Overlay**: measures overlay directly on product structures rather than dedicated targets; eliminates target-to-device offset errors; uses machine learning to extract overlay from complex product patterns
- **Overlay Control**: feeds measurements to lithography scanner for wafer-to-wafer correction; advanced process control adjusts alignment based on previous layer overlay; maintains overlay <2nm for critical layers at 5nm node

**Electrical Metrology:**
- **Four-Point Probe**: measures sheet resistance of doped silicon and metal films; four collinear probes eliminate contact resistance errors; KLA RS100 and Napson systems provide <0.5% measurement repeatability
- **Capacitance-Voltage (CV)**: measures capacitance vs applied voltage to extract doping profiles, oxide thickness, and interface properties; used for gate oxide and junction characterization
- **Hall Effect Measurement**: determines carrier concentration and mobility in doped semiconductors; applies magnetic field and measures transverse voltage; critical for transistor performance prediction
- **Kelvin Probe Force Microscopy (KPFM)**: maps work function and surface potential at nanoscale resolution; characterizes gate metals, doping variations, and contact barriers

**Metrology Challenges:**
- **Shrinking Targets**: as features shrink, dedicated metrology targets consume increasing die area; on-device metrology and smaller targets required; optical methods approach fundamental diffraction limits
- **3D Structures**: FinFETs, nanosheets, and 3D NAND require measurement of buried features and complex 3D geometries; X-ray and electron beam methods supplement optical techniques
- **Measurement Uncertainty**: advanced nodes require <1nm measurement uncertainty; achieving this requires sub-angstrom repeatability, accurate calibration standards, and sophisticated error analysis
- **Throughput vs Accuracy**: inline control requires high throughput (>100 wafers/hour); reference metrology prioritizes accuracy over speed; hybrid strategies use fast inline methods calibrated to slow reference methods

Metrology equipment is **the measurement foundation of semiconductor manufacturing — providing the nanometer-scale dimensional and compositional data that validates process performance, enables feedback control, and ensures that billions of transistors meet their atomic-scale specifications, making the invisible visible and the unmeasurable measurable**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/metrology-equipment-semiconductor) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
