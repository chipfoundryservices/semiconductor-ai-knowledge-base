# Mask Cost

**Keywords**: mask cost, business

---

**Mask Cost** represents **the expense of photomask sets required for chip fabrication** — reaching millions of dollars at advanced nodes due to complex multi-patterning, EUV masks, and stringent specifications, becoming a major consideration in product economics, technology node decisions, and driving shared mask programs and maskless lithography research.

**What Is Mask Cost?**

- **Definition**: Total expense for complete photomask set needed to fabricate a chip.
- **Magnitude**: $150K per mask at 7nm, full mask set $10M+ for complex chips.
- **Trend**: Exponentially increasing with node advancement.
- **Impact**: Major NRE (non-recurring engineering) cost component.

**Why Mask Cost Matters**

- **Economic Barrier**: High NRE discourages small-volume products.
- **Design Decisions**: Influences architecture choices, reuse strategies.
- **Time-to-Market**: Mask fabrication on critical path (weeks).
- **Risk**: Expensive to fix errors, requires new mask set.
- **Business Model**: Drives MPW (multi-project wafer) and shuttle services.

**Mask Cost Components**

**Blank Substrate**:
- **Material**: Ultra-flat quartz with precise specifications.
- **Specifications**: Flatness <50nm, defect-free.
- **Cost**: $1K-5K per blank.
- **EUV**: More expensive due to multilayer reflective coating.

**E-Beam Writing**:
- **Process**: Electron beam writes pattern on mask.
- **Time**: Hours to days per mask for complex patterns.
- **Cost Driver**: Writing time proportional to pattern complexity.
- **Advanced Nodes**: More shots, tighter specs = longer write time.
- **Typical**: $50K-100K for writing at advanced nodes.

**Inspection**:
- **Defect Inspection**: Detect pattern defects, particles.
- **Actinic Inspection**: EUV masks require EUV-wavelength inspection.
- **Multiple Passes**: Initial, post-repair, final inspection.
- **Cost**: $20K-50K per mask.

**Repair**:
- **Defect Repair**: Fix detected defects using FIB (focused ion beam) or laser.
- **Yield**: Not all defects repairable, some masks scrapped.
- **Iterations**: May require multiple repair-inspect cycles.
- **Cost**: $10K-30K per mask.

**Pellicle**:
- **Protection**: Transparent membrane protects mask from particles.
- **EUV Challenge**: No pellicle for EUV yet (under development).
- **Cost**: $5K-20K per pellicle.

**Qualification**:
- **Wafer Printing**: Test mask on wafer to verify performance.
- **Metrology**: CD, overlay, defect printing characterization.
- **Iterations**: May require mask rework if fails qualification.
- **Cost**: Wafer costs + metrology + engineering time.

**Cost Drivers at Advanced Nodes**

**Multi-Patterning**:
- **LELE (Litho-Etch-Litho-Etch)**: 2× masks per layer.
- **SAQP (Self-Aligned Quadruple Patterning)**: Multiple mask layers.
- **Impact**: 2-4× more masks than single patterning.
- **Example**: 40-layer process becomes 80-160 masks with multi-patterning.

**EUV Masks**:
- **Reflective**: Multilayer Mo/Si mirror instead of transmissive.
- **Actinic Inspection**: Requires EUV-wavelength inspection tools (expensive).
- **No Pellicle**: Requires ultra-clean environment, more frequent cleaning.
- **Cost**: 2-3× more expensive than DUV masks.

**Tighter Specifications**:
- **CD Uniformity**: <1nm CD variation across mask.
- **Placement Accuracy**: <1nm pattern placement error.
- **Defect Density**: Near-zero defects.
- **Impact**: Lower mask yield, more scrapped masks, higher cost.

**Complexity**:
- **OPC (Optical Proximity Correction)**: Complex sub-resolution features.
- **ILT (Inverse Lithography Technology)**: Curvilinear patterns.
- **Shot Count**: More e-beam shots = longer write time.
- **Impact**: Exponentially longer write times.

**Mask Set Cost by Node**

**28nm**:
- **Masks per Layer**: 1 (mostly single patterning).
- **Total Masks**: 30-40 masks.
- **Cost per Mask**: $50K-80K.
- **Total Set**: $2M-3M.

**7nm/5nm**:
- **Masks per Layer**: 2-4 (multi-patterning).
- **Total Masks**: 80-120 masks.
- **Cost per Mask**: $150K-200K.
- **Total Set**: $12M-24M.

**3nm (EUV)**:
- **EUV Masks**: 15-20 EUV masks.
- **DUV Masks**: 60-80 DUV masks.
- **Cost per EUV Mask**: $250K-300K.
- **Cost per DUV Mask**: $150K-200K.
- **Total Set**: $15M-30M.

**Impact on Product Economics**

**Break-Even Volume**:
- **High NRE**: Requires high production volume to amortize.
- **Example**: $20M mask set / $100 per chip = 200K chips to break even.
- **Impact**: Discourages low-volume specialty products.

**Design Reuse**:
- **Platform Approach**: Reuse masks across product variants.
- **Derivative Products**: Minimize new masks for derivatives.
- **IP Reuse**: Reuse validated IP blocks to avoid new masks.

**Technology Node Selection**:
- **Cost vs. Performance**: Balance performance gain vs. mask cost.
- **Node Skipping**: Some products skip nodes due to mask cost.
- **Long-Lived Nodes**: 28nm, 40nm remain popular due to lower mask cost.

**Mitigation Strategies**

**Multi-Project Wafer (MPW)**:
- **Shared Masks**: Multiple designs share same mask set.
- **Cost Sharing**: Mask cost split among participants.
- **Benefit**: Enables prototyping, low-volume production.
- **Services**: MOSIS, CMP, Europractice offer MPW.

**Shuttle Services**:
- **Scheduled Runs**: Regular fabrication runs with shared masks.
- **Small Die**: Allocate small area per design.
- **Cost**: $10K-100K vs. $10M+ for full mask set.

**Mask Reuse**:
- **Platform Masks**: Design products to share masks.
- **Programmable Logic**: Use FPGAs, avoid custom masks.
- **Software Differentiation**: Differentiate products in software, not hardware.

**Maskless Lithography**:
- **Direct Write**: E-beam or multi-beam direct write on wafer.
- **No Masks**: Eliminate mask cost entirely.
- **Challenge**: Throughput too low for high-volume production.
- **Use Case**: Prototyping, very low volume, rapid iteration.

**Design for Manufacturability**:
- **Simpler Patterns**: Reduce OPC complexity, shot count.
- **Restricted Design Rules**: Use regular patterns, reduce mask complexity.
- **Benefit**: Lower mask cost, faster turnaround.

**Future Trends**

**EUV Adoption**:
- **Fewer Masks**: EUV reduces multi-patterning, fewer total masks.
- **Higher Cost per Mask**: But total set cost may be lower.
- **Net Effect**: Potentially lower total mask cost at 3nm and below.

**High-NA EUV**:
- **Next Generation**: 0.55 NA EUV for 2nm and below.
- **Mask Cost**: Even more expensive masks.
- **Benefit**: Further reduce multi-patterning.

**Maskless Lithography Progress**:
- **Multi-Beam**: Thousands of parallel e-beams.
- **Throughput**: Approaching viability for some applications.
- **Timeline**: 5-10 years for production readiness.

**Tools & Vendors**

- **Mask Writers**: ASML (Twinscan), NuFlare, IMS.
- **Mask Inspection**: KLA-Tencor, ASML, Lasertec.
- **Mask Repair**: Carl Zeiss, Rave.
- **Mask Shops**: Photronics, Toppan, DNP, HOYA.

Mask Cost is **a critical factor in semiconductor economics** — as mask sets reach $20M-30M at advanced nodes, they fundamentally shape product decisions, business models, and technology choices, driving innovation in mask reuse, MPW services, and maskless lithography while creating economic barriers that concentrate advanced node production among high-volume products.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/mask-cost) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
