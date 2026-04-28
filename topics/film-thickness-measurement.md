# Film Thickness Measurement

**Keywords**: film thickness measurement,ellipsometry spectroscopic,x-ray reflectometry xrr,interferometry optical,thin film metrology

---

**Film Thickness Measurement** is **the precision metrology that quantifies the thickness of deposited thin films from sub-nanometer to several microns — using optical ellipsometry, X-ray reflectometry, and interferometry to achieve <0.1nm measurement uncertainty for critical films, enabling process control of gate oxides, high-k dielectrics, metal barriers, and interconnect layers that must meet atomic-layer thickness specifications for proper device operation**.

**Spectroscopic Ellipsometry:**
- **Measurement Principle**: measures change in polarization state of reflected light as function of wavelength; incident linearly polarized light becomes elliptically polarized upon reflection; ellipsometric parameters Ψ (amplitude ratio) and Δ (phase difference) encode film thickness and optical properties
- **Data Analysis**: compares measured Ψ(λ) and Δ(λ) spectra to calculated spectra from optical models; Fresnel equations describe reflection from multilayer stacks; non-linear regression fits thickness and optical constants (n, k) to minimize error between measured and calculated spectra
- **Sensitivity**: achieves <0.1nm repeatability for films 1-1000nm thick; single-layer films measured with <0.5% accuracy; multilayer stacks (5-10 layers) measured with <1% accuracy per layer; KLA SpectraShape and J.A. Woollam systems provide 190-1700nm wavelength range
- **Applications**: gate oxide (1-5nm), high-k dielectrics (2-10nm), metal barriers (2-5nm), copper seed (10-50nm), dielectric films (50-500nm); measures thickness, refractive index, and extinction coefficient simultaneously

**X-Ray Reflectometry (XRR):**
- **Measurement Principle**: measures X-ray reflectivity vs incident angle (0.1-5 degrees); interference between reflections from film interfaces creates oscillations (Kiessig fringes); fringe period inversely proportional to film thickness; critical angle relates to film density
- **Multilayer Analysis**: resolves individual layer thicknesses in stacks of 10+ layers; measures thickness, density, and interface roughness for each layer; Rigaku and Bruker systems achieve 0.1nm thickness resolution and 0.01 g/cm³ density resolution
- **Advantages**: works on any material (metals, dielectrics, semiconductors); no optical model required; measures buried layers under opaque films; provides density information unavailable from optical methods
- **Limitations**: slow measurement (5-15 minutes per site); requires flat, uniform films; small spot size (1-10mm) may not represent wafer-level uniformity; used for reference metrology rather than inline monitoring

**Optical Interferometry:**
- **White Light Interferometry**: broadband light source creates interference fringes; fringe contrast maximum when optical path difference is zero; scanning vertical position locates surface; measures step heights and film thickness with <1nm vertical resolution
- **Spectral Reflectometry**: measures reflected intensity vs wavelength; interference between reflections from top and bottom film surfaces creates oscillations; fringe period inversely proportional to optical thickness (n·t); simple and fast but less accurate than ellipsometry
- **Thin Film Interference**: visible color fringes on films 100-1000nm thick; qualitative thickness assessment; used for quick visual inspection; quantitative measurement requires spectrophotometry
- **Applications**: CMP step height measurement, film thickness uniformity mapping, surface roughness characterization; Zygo and Bruker systems provide 3D surface topography with sub-nanometer vertical resolution

**Electrical Thickness Measurement:**
- **Capacitance-Voltage (CV)**: measures capacitance of MOS structure; C = ε₀·εᵣ·A/t where t is oxide thickness; achieves <0.1nm accuracy for gate oxides; measures electrical thickness (equivalent oxide thickness, EOT) rather than physical thickness
- **Equivalent Oxide Thickness (EOT)**: electrical thickness of high-k dielectric stack expressed as equivalent SiO₂ thickness; EOT = (εSiO₂/εhigh-k)·tphysical; critical parameter for transistor performance; target EOT <1nm for advanced nodes
- **Quantum Mechanical Correction**: ultra-thin oxides (<2nm) require quantum mechanical corrections; electron wavefunction penetration into electrodes reduces measured capacitance; corrected EOT differs from physical thickness by 0.3-0.5nm
- **Advantages**: measures electrical property directly relevant to device performance; non-destructive; requires test structures (capacitors) rather than product wafers

**Film Thickness Uniformity:**
- **Within-Wafer Uniformity**: measures thickness at 50-200 sites across wafer; calculates mean, range, and standard deviation; target <1% (1σ) for critical films; contour maps reveal deposition non-uniformity patterns
- **Edge Exclusion**: film thickness typically non-uniform within 3-5mm of wafer edge; edge exclusion zone not used for die placement; edge thickness monitored to detect process issues
- **Wafer-to-Wafer Uniformity**: thickness variation between wafers in a lot; target <0.5% (1σ); indicates process stability; run-to-run control compensates for systematic shifts
- **Lot-to-Lot Uniformity**: thickness variation over time; target <1% (1σ); monitors equipment drift and consumable aging; statistical process control tracks long-term trends

**Advanced Metrology Techniques:**
- **Grazing Incidence X-Ray Fluorescence (GIXRF)**: measures film thickness and composition simultaneously; combines XRF (composition) with angle-dependent intensity (thickness); measures ultra-thin films (0.5-50nm) with 0.1nm resolution
- **Transmission Electron Microscopy (TEM)**: cross-sectional TEM provides direct thickness measurement with <0.5nm resolution; destructive and slow (hours per sample); used for reference metrology and process development
- **Rutherford Backscattering Spectrometry (RBS)**: measures film thickness and composition by analyzing backscattered high-energy ions (1-3 MeV He⁺); absolute measurement without standards; slow and expensive; used for reference metrology
- **Acoustic Metrology**: picosecond ultrasonics measures film thickness from acoustic echo time; works on opaque films; emerging technology for advanced nodes

**Metrology Challenges:**
- **Ultra-Thin Films**: gate oxides <2nm approach single-digit atomic layers; measurement uncertainty becomes significant fraction of thickness; requires sub-angstrom precision
- **Multilayer Stacks**: high-k metal gate stacks contain 5-10 layers with total thickness <10nm; optical methods struggle to resolve individual layers; X-ray methods required
- **Patterned Wafers**: film thickness varies with pattern density (loading effects); metrology on unpatterned test areas may not represent device areas; on-device metrology emerging
- **High-Aspect-Ratio**: 3D NAND and DRAM structures with aspect ratios >50:1; film thickness at top, middle, and bottom differ; cross-sectional analysis required

**Process Control Integration:**
- **Inline Monitoring**: ellipsometry and spectral reflectometry provide fast (1-2 minutes per wafer) inline measurements; 100% wafer measurement for critical films; sampling for non-critical films
- **Advanced Process Control (APC)**: run-to-run controller adjusts deposition time or power based on thickness feedback; maintains target thickness despite tool drift and consumable aging
- **Feedforward Control**: uses incoming film thickness to adjust subsequent process steps; breaks error propagation chains; critical for multilayer stacks where each layer affects the next
- **Virtual Metrology**: predicts film thickness from deposition tool sensors (power, pressure, temperature, time) using machine learning; provides 100% coverage without physical measurement

Film thickness measurement is **the dimensional control in the vertical direction — ensuring that atomic-layer films meet their sub-nanometer specifications, that gate oxides provide the precise capacitance required for transistor operation, and that metal barriers prevent copper diffusion, making the invisible measurable and the unmeasurable controllable at the atomic scale**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/film-thickness-measurement) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
