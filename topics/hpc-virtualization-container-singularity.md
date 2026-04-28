# HPC Virtualization and Containers: Singularity/Apptainer for HPC Portability — lightweight containers designed for HPC enabling reproducible workflows and cloud-burst capability

**Keywords**: hpc virtualization container singularity,container hpc kubernetes,singularity apptainer hpc,hpc cloud burst,containerized hpc workflow

---

**HPC Virtualization and Containers: Singularity/Apptainer for HPC Portability — lightweight containers designed for HPC enabling reproducible workflows and cloud-burst capability**

**Singularity (Now Apptainer) HPC Containers**
- **HPC-Native Design**: runs as user (not root), avoids security model mismatch with HPC resource management
- **Bind Mounts**: seamlessly mount shared file systems (Lustre, NFS) into container, transparent data access
- **MPI Support**: container MPI libraries (OpenMPI, MPICH) interoperate with host MPI (avoids version conflicts)
- **Reproducibility**: frozen environment (OS, libraries, versions), identical execution across clusters (portability)
- **Image Format**: Singularity Image Format (SIF) — single file (compressed), vs Docker multi-layer (complex distribution)

**Docker Limitations for HPC**
- **Root Daemon**: Docker runs as root (security risk in multi-tenant HPC), container escapes grant access to host
- **Namespace Isolation**: Docker containers appear as different users/GIDs in container (uid 0 = root), conflicts with HPC user model
- **Network Namespace**: container network isolation incompatible with tight MPI coupling (needs direct host network)
- **Storage Binding**: Docker volumes less flexible than Singularity bind mounts (mounted read-only default, performance issues)
- **Adoption**: Docker dominates cloud (AWS, Azure), but HPC community largely skipped Docker

**Podman Rootless Containers**
- **Root-Free Execution**: Podman runs without root daemon (compatible with HPC), secures container runtime
- **Docker Compatibility**: Podman CLI matches Docker (``podman run' same as ``docker run'), easier adoption
- **Performance**: negligible overhead vs Docker (similar cgroup mechanism)
- **Adoption**: emerging in HPC (RedHat sponsor), adoption slower than Singularity (HPC-specific advantage)

**Kubernetes for HPC**
- **Job Scheduler Integration**: Kubernetes (container orchestration) with HPC job scheduler (SLURM) — hybrid approach
- **Resource Requests**: pod CPU/memory requests mapped to SLURM node allocation
- **Batch Job Support**: kube-batch plugin (batch job scheduling), replaces default service-oriented scheduling
- **Challenges**: Kubernetes designed for cloud (long-running services), HPC prefers batch (short-lived jobs), mismatch in scheduling philosophy
- **Adoption**: niche HPC clusters (cloud-HPC hybrid), full replacement of SLURM unlikely

**Cloud-Burst for HPC**
- **On-Premises HPC**: primary cluster (fast, high-priority jobs), local storage, dedicated network
- **Cloud Overflow**: excess jobs overflow to cloud (AWS, Azure, Google Cloud), elasticity for variable load
- **Data Challenges**: moving data to cloud expensive (bandwidth cost, latency), data residency restrictions (HIPAA, proprietary models)
- **Workflow**: on-prem job manager submits excess to cloud (transparent to user), results fetched back
- **Cost**: cloud computing expensive ($0.10-1 per core-hour), justified only for sporadic overload (not continuous)

**Containerized HPC Workflow**
- **Application Container**: researcher packages code + libraries + data preprocessing in Singularity container
- **Reproducibility**: container frozen at publication, enables reproducible science (exact same compute, reproducible results)
- **Portability**: container runs on any HPC cluster (no module system hunting), simplifies collaboration
- **Version Control**: container images versioned (v1.0 with GROMACS 2020, v2.0 with GROMACS 2021), isolates dependency updates

**Container Performance in HPC**
- **Minimal Overhead**: container runtime ~1-2% overhead (vs native), negligible for scientific computing
- **I/O Performance**: container I/O (through mount point) same as native (direct file system access)
- **Memory**: container memory isolation (cgroup memory limit), enforced fairly across jobs
- **Network**: container network (veth pair) adds latency (1-3 µs MPI ping-pong), slight but measurable
- **GPU Containers**: nvidia-docker / docker GPU support routes GPU through container (seamless CUDA access)

**Module System vs Containers**
- **Traditional (Lmod/Environment Modules)**: text files modify PATH/LD_LIBRARY_PATH, many variants conflict
- **Container Approach**: frozen environment, no conflicts, but less flexible (hard to mix-and-match)
- **Hybrid**: modules inside container (flexibility + reproducibility), double complexity
- **Adoption**: both coexist (modules for quick prototyping, containers for production/publication)

**Container Registry and Distribution**
- **DockerHub**: public registry (millions of images), but HPC-specific images sparse
- **Singularity Hub**: deprecated (access restrictions), moved to Singularity Cloud
- **GitHub Container Registry (GHCR)**: free, public container distribution (linked to GitHub repos)
- **Local Registry**: HPC facilities maintain local registry (cached images, private Singularity images), reduces download time

**Container Orchestration in HPC**
- **Shifter (NERSC)**: container abstraction layer integrated with SLURM, allocates containers to nodes
- **Charliecloud**: minimal container solution (Singularity-like), alternative with smaller footprint
- **Enroot**: NVIDIA container solution (for GPU HPC), maps container to host device/library tree
- **Design**: all attempt to bridge container + HPC scheduling (not straightforward)

**Singularity Definition File (SDF)**
- **Build Recipe**: specifies base image (Ubuntu, CentOS), installation steps (apt, yum commands), environment setup
- **Bootstrap**: base OS image fetched from remote (Docker registry, Singularity library), reproducible builds
- **Example**: build from CentOS 7, install OpenMPI 3.1.0, compile GROMACS, set entrypoint to gmx binary
- **Versioning**: SDF committed to Git, enables build history + dependency tracking

**Reproducibility via Containers**
- **Publication**: researchers submit container + data + SDF alongside paper, reviewers can reproduce exactly
- **Fidelity**: same hardware architecture (x86-64), same OS/libraries, expected bit-for-bit reproducibility (with caveats)
- **Limitations**: floating-point arithmetic non-deterministic (see parallel computing reproducibility), compiler optimizations vary
- **Best Practice**: include input data + reference output in container, validation script checks results

**Cloud-HPC Hybrid Workflow Example**
- **Step 1**: on-premises simulation (MPI GROMACS, 100 nodes, 24 hours)
- **Step 2**: if queue full, burst 100 nodes to AWS (container deployed in parallel)
- **Step 3**: results aggregated, post-processing on-premises (central storage)
- **Cost-Benefit**: burst cost ~$10K (vs 2-day wait), worth for time-sensitive research

**Future Directions**: container image standardization (OCI: Open Container Initiative), wider HPC adoption expected (2023-2025), unikernel containers (even smaller footprint) emerging, container-native job schedulers (vs retrofit to SLURM).

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/hpc-virtualization-container-singularity) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
