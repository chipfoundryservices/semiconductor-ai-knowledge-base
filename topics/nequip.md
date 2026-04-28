# NequIP (Neural Equivariant Interatomic Potentials)

**Keywords**: nequip, equivariant neural network, machine learning force field, molecular dynamics ml, interatomic potential

---

**NequIP (Neural Equivariant Interatomic Potentials)** is **a machine learning framework for constructing highly accurate interatomic potential energy surfaces by encoding the fundamental symmetries of physics directly into the neural network architecture**, using E(3)-equivariant representations — features that transform predictably under 3D rotations, reflections, and translations — to achieve chemical accuracy with 100-1000x fewer training examples than non-equivariant approaches. Developed by Simon Batzner, Albert Musaelian, and collaborators at Harvard and Berkeley National Laboratory, NequIP represents the state-of-the-art in ML-based molecular simulation relevant to semiconductor process modeling, catalyst design, and materials discovery.

**The Physics Problem NequIP Solves**

Accurate atomic simulation requires computing the potential energy surface (PES) — how the energy of a collection of atoms depends on their positions. Traditional approaches face a fundamental tradeoff:

- **Density Functional Theory (DFT)**: Highly accurate but scales as O(N³) in system size — a 500-atom simulation costs 100 million times more than a 5-atom one
- **Classical force fields (CHARMM, AMBER, ReaxFF)**: Fast but limited to pre-parameterized atom types, cannot describe bond breaking/forming well
- **Neural network potentials**: Can learn complex PES from DFT data, but naive implementations need millions of training configurations because they do not exploit physical symmetries

NequIP's solution: Build the symmetries of physics into the network so it never has to learn them from data.

**E(3) Equivariance: The Core Innovation**

Physical systems obey three fundamental symmetries:
1. **Translation invariance**: Energy is the same regardless of where the molecule is positioned in space
2. **Rotation equivariance**: Rotating the molecule rotates the forces by the same amount but does not change the energy
3. **Inversion/Reflection symmetry**: Energy is unchanged by mirror operations (for non-chiral systems)

A standard neural network (e.g., SchNet, ANI) achieves translation invariance by working with pairwise distances, but handles rotation by **invariance** — only using scalar (rotation-independent) features. This discards directional information and forces the network to learn rotational behavior from data.

NequIP uses **equivariant** features:
- Scalar features (l=0): Energy, bond lengths — rotation-invariant
- Vector features (l=1): Forces, dipoles — rotate like vectors under rotation  
- Tensor features (l=2+): Polarizability, stress — transform as higher-order tensors

These features are combined using **tensor products** with Clebsch-Gordan coefficients (the mathematical machinery of angular momentum addition from quantum mechanics), ensuring every layer of the network maintains equivariance. When you rotate the input atoms, the network's intermediate representations rotate accordingly, and the output forces rotate consistently.

**Architecture Details**

NequIP is built on the e3nn library (equivariant neural network operations):

1. **Node embedding**: Each atom is initialized with a learnable embedding based on its element type
2. **Edge features**: For each atom pair within a cutoff radius, compute equivariant edge features using spherical harmonics of the relative position vector
3. **Message passing**: Equivariant convolutions aggregate neighbor information, mixing angular momentum channels via Clebsch-Gordan tensor products
4. **Radial networks**: Learned radial basis functions (Bessel functions) provide distance-dependent weights
5. **Multiple interaction layers**: 3-6 equivariant interaction blocks update node features
6. **Energy readout**: Scalar (l=0) features from each atom sum to total energy; forces are computed as negative gradients

**Data Efficiency: The Headline Advantage**

Benchmark comparisons on the rMD17 dataset (revised molecular dynamics trajectory for small molecules like aspirin, ethanol, benzene):

| Model | Training Examples | MAE Energy (meV/atom) | MAE Forces (meV/Å) |
|-------|------------------|-----------------------|--------------------|
| SchNet (invariant) | 950 | ~0.9 | ~5.0 |
| PhysNet (invariant) | 950 | ~0.6 | ~4.0 |
| **NequIP (equivariant)** | **950** | **~0.05** | **~0.3** |
| NequIP | 50 | ~0.1 | ~0.8 |

NequIP with just 50 training configurations outperforms invariant models trained on 950 examples. This is the practical significance: DFT calculations for complex materials (surfaces, defects, interfaces) cost $100-$1,000 per configuration. 100x fewer training points = 100x lower data collection cost.

**MACE: NequIP Successor**

MACE (Multi-Atomic Cluster Expansion) extends NequIP's approach with many-body message passing, further improving accuracy and generalization:
- MACE-MP-0 (2023): Universal foundation model for materials, trained on 150,000 DFT structures
- Can simulate diverse materials including metals, oxides, and organic molecules zero-shot
- Used by materials simulation software platforms (DeepMind, Microsoft Research)

**Applications in Semiconductor and AI Industries**

**Semiconductor R&D**:
- Thermal conductivity modeling of materials at device scale (phonon transport)
- Ion implantation damage evolution MD simulations — predicting defect profiles in silicon
- Gate dielectric interface reactions (SiO2/Si, HfO2/Si) — modeling oxide growth and defect formation
- Interconnect electromigration — copper grain boundary diffusion at atomic scale
- Packaging materials thermomechanical stress simulation

**Process Chemistry**:
- Plasma-surface interaction modeling for etch and deposition processes
- CVD precursor decomposition and surface reaction mechanisms
- CMP slurry-surface chemistry — predicting polishing selectivity

**Battery and Energy Materials**:
- Li-ion diffusion in cathode materials for EV and data center UPS applications
- Electrolyte decomposition prediction

**Getting Started with NequIP**

```
pip install nequip
# Requires PyTorch + e3nn

# Training command
nequip-train configs/your_config.yaml

# Key config parameters:
# r_max: cutoff radius (typically 4-6 Angstroms)
# num_layers: interaction blocks (4-8)
# l_max: maximum angular momentum (1-3)
# num_features: channel count (16-64)
```

For most materials applications, the pre-trained MACE-MP-0 foundation model provides excellent zero-shot accuracy without any custom DFT training data — check the MACE repository before investing in expensive DFT calculations.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/nequip) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
