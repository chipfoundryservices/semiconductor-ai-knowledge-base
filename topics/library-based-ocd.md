# Library-Based OCD (Optical Critical Dimension)

**Keywords**: library-based ocd, metrology

---

**Library-Based OCD (Optical Critical Dimension)** metrology is a technique that **matches measured optical spectra to pre-calculated theoretical spectra libraries** — enabling fast, accurate measurement of multiple structure parameters simultaneously by comparing experimental diffraction patterns against simulated reference database, the standard approach for inline semiconductor process control.

**What Is Library-Based OCD?**

- **Definition**: Optical metrology using pre-computed spectral libraries for parameter extraction.
- **Method**: Match measured spectrum to best-fit library entry.
- **Output**: Multiple parameters (CD, height, sidewall angle) from single measurement.
- **Speed**: Fast measurement via library lookup vs. real-time fitting.

**Why Library-Based OCD Matters**

- **Inline Capability**: Fast enough for production monitoring (seconds per site).
- **Multi-Parameter**: Measures CD, height, sidewall angle simultaneously.
- **Non-Destructive**: Optical measurement preserves wafer.
- **High Throughput**: Enables 100% wafer sampling if needed.
- **Cost Effective**: Lower cost per measurement than electron microscopy.

**How It Works**

**Step 1: Build Parametric Model**:
- **Structure Definition**: Define geometry (trapezoid, rectangle, complex shapes).
- **Parameters**: CD (critical dimension), height, sidewall angle, material properties.
- **Parameter Ranges**: Define min/max values for each parameter.
- **Material Stack**: Specify all layers and optical properties.

**Step 2: Generate Spectral Library**:
- **Simulation**: Use RCWA (Rigorous Coupled-Wave Analysis) to compute spectra.
- **Parameter Space**: Calculate spectra for combinations of parameter values.
- **Grid Sampling**: Typically 5-10 points per parameter dimension.
- **Computation Time**: Hours to days depending on complexity.
- **One-Time Cost**: Library generated once per structure type.

**Step 3: Measure Sample Spectrum**:
- **Illumination**: Broadband light at specific angle(s).
- **Detection**: Measure reflected/diffracted spectrum.
- **Wavelength Range**: Typically 200-1000nm.
- **Polarization**: Multiple polarizations for more information.
- **Measurement Time**: 1-5 seconds per site.

**Step 4: Library Matching**:
- **Search**: Find library entry with best spectral match.
- **Metric**: Minimize χ² or other goodness-of-fit measure.
- **Interpolation**: Interpolate between library points for precision.
- **Output**: Best-fit parameter values.
- **Speed**: Milliseconds for library lookup.

**Advantages**

**Speed**:
- **Library Lookup**: Much faster than real-time regression.
- **Throughput**: Enables high-sampling density.
- **Inline Use**: Fast enough for production monitoring.

**Multi-Parameter Measurement**:
- **Simultaneous**: All parameters from single measurement.
- **Correlation**: Captures parameter correlations.
- **Efficiency**: No need for multiple metrology tools.

**Robustness**:
- **Pre-Validated**: Library entries are pre-computed and validated.
- **Convergence**: No optimization convergence issues.
- **Repeatability**: Consistent results, no fitting variability.

**Limitations**

**Model Accuracy**:
- **Assumption**: Model must accurately represent real structure.
- **Simplifications**: Real structures more complex than models.
- **Impact**: Model errors propagate to measurements.
- **Mitigation**: Validate with reference metrology (SEM, TEM, AFM).

**Library Coverage**:
- **Parameter Space**: Library must cover actual parameter range.
- **Out-of-Range**: Extrapolation unreliable if parameters outside library.
- **Grid Density**: Trade-off between accuracy and library size.
- **Solution**: Adaptive libraries, expand as needed.

**Interpolation Accuracy**:
- **Between Points**: Must interpolate between library grid points.
- **Nonlinearity**: Spectral response may be nonlinear.
- **Error**: Interpolation introduces uncertainty.
- **Mitigation**: Denser grids in sensitive regions.

**Computational Cost**:
- **Library Generation**: Days of computation for complex structures.
- **Storage**: Large libraries require significant storage.
- **Updates**: New library needed for process changes.
- **Solution**: Efficient simulation, library compression.

**Alternative: Real-Time Regression**

**Method**:
- **On-the-Fly**: Optimize parameters to fit measured spectrum in real-time.
- **No Library**: No pre-computation required.
- **Flexibility**: Handles any parameter combination.

**Trade-Offs**:
- **Slower**: Minutes per measurement vs. seconds for library.
- **Convergence**: May fail to converge or find local minima.
- **Flexibility**: Better for R&D, process development.
- **Use Case**: When library impractical or parameters unknown.

**Applications**

**Lithography Process Control**:
- **After Develop**: Measure resist CD, height, profile.
- **Feedback**: Adjust exposure, focus based on measurements.
- **Sampling**: Multiple sites per wafer, every wafer.

**Etch Process Control**:
- **After Etch**: Measure final feature dimensions.
- **Endpoint**: Verify etch depth, profile.
- **Uniformity**: Map CD and height across wafer.

**CMP Monitoring**:
- **Remaining Thickness**: Measure film thickness after polish.
- **Uniformity**: Ensure uniform removal across wafer.
- **Endpoint**: Verify target thickness achieved.

**Advanced Patterning**:
- **Multi-Patterning**: Measure each patterning step.
- **Overlay**: Combined with overlay metrology.
- **3D Structures**: FinFETs, GAA, complex 3D geometries.

**Library Optimization**

**Adaptive Sampling**:
- **Dense Sampling**: More points in sensitive parameter regions.
- **Sparse Sampling**: Fewer points where response is smooth.
- **Benefit**: Smaller library with maintained accuracy.

**Dimensionality Reduction**:
- **PCA**: Principal component analysis of parameter space.
- **Sensitivity**: Focus on parameters with high spectral sensitivity.
- **Benefit**: Reduce library size, faster generation.

**Incremental Updates**:
- **Add Points**: Expand library as new parameter ranges encountered.
- **Refinement**: Add points where interpolation error high.
- **Benefit**: Start with coarse library, refine over time.

**Validation & Calibration**

**Reference Metrology**:
- **CD-SEM**: Validate CD measurements.
- **AFM**: Validate height and sidewall angle.
- **TEM**: Cross-section for complex 3D structures.
- **Correlation**: Establish correlation between OCD and reference.

**Model Validation**:
- **Goodness of Fit**: Check χ² values for library matches.
- **Residuals**: Analyze spectral residuals for systematic errors.
- **Outliers**: Identify measurements with poor fits.

**Periodic Recalibration**:
- **Drift**: Optical properties may drift over time.
- **Process Changes**: Update library for process modifications.
- **Frequency**: Quarterly or after significant process changes.

**Tools & Vendors**

- **KLA-Tencor**: SpectraShape, SpectraCD OCD systems.
- **Nova Measuring Instruments**: Integrated metrology solutions.
- **Nanometrics (Onto Innovation)**: Atlas OCD systems.
- **ASML**: Integrated metrology in lithography scanners.

Library-Based OCD is **the workhorse of semiconductor metrology** — by pre-computing spectral libraries, it enables fast, accurate, multi-parameter measurements that make inline process control practical, providing the measurement speed and throughput required for high-volume manufacturing at advanced nodes.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/library-based-ocd) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
