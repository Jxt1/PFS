# PFS: Core Subgraph Matching Engine (Binary-only Minimal Release)

PFS is a high-performance enumeration-based subgraph matching engine. This minimal release focuses on the runtime binary and essential usage docs only. Source files, test scripts, logs, and research artifacts are not included in the distribution.

## Project Context (ICDE'26)
This release targets the paper:
- "Parallelizing Failing-Set Pruning for Efficient Subgraph Matching Algorithm" (ICDE 2026).

Open-source policy:
- The full source code will be open-sourced after the paper is accepted.
- Until acceptance, we provide a binary-only distribution for evaluation and usage.

If you use or reference this project, please cite the ICDE'26 work above. For related prior work, you may also cite:
- "IVE: Accelerating Enumeration-based Subgraph Matching via Exploring Isolated Vertices." ICDE 2024.

## Table of Contents
- [Project Context (ICDE'26)](#project-context-icde26)
 - [Run](#run)
- [Environment](#environment)
- [Input Format](#input-format)
- [Datasets](#datasets)

> Note: This is a binary-only minimal release for evaluation. Source files and auxiliary artifacts are intentionally not included and will be open-sourced after paper acceptance (ICDE'26).

## Input Format
Both data and query graphs are vertex-labeled undirected graphs. A file starts with `t N M`, where `N` is the number of vertices and `M` is the number of edges. Vertex and edge lines are formatted as:
- Vertex: `v VertexID LabelId Degree`
- Edge: `e VertexId VertexId`

Vertex IDs start from 0 and range in `[0, N-1]`. Example:
```bash
t 5 6
v 0 0 2
v 1 1 3
v 2 2 3
v 3 1 2
v 4 2 2
e 0 1
e 0 2
e 1 2
e 1 3
e 2 4
e 3 4
```

## Datasets
Official dataset for PFS (ICDE'26) is publicly available:
- https://drive.google.com/file/d/1HSCIqxGGtbC7LpoCifeFTPd8hMR444vA/view?usp=sharing

Notes:
- Provided for evaluation and research use.
- Please cite the ICDE'26 paper when using the dataset.

## Docker Quick Start
The easiest way to run PFS is using our pre-built Docker image:
```bash
# Pull the Docker image
docker pull woolee081/pfs:latest

# Run the container
docker run -it woolee081/pfs:latest

# Run PFS
mpirun -np 4 ./bin/pfs -d ./examples/data.txt -q ./examples/query.txt -num 100000
```
## Run

```bash
# Check MPI version
mpirun --version

# Make executable (if needed)
chmod +x ./bin/pfs

# Single-node run with 4 processes (minimal reproduction)
mpirun -np 4 ./bin/pfs -d ./examples/data.txt -q ./examples/query.txt -num 100000

# Optional: log output and timing
time mpirun -np 4 ./bin/pfs -d ./examples/data.txt -q ./examples/query.txt -num 100000 | tee pfs_run.log
```

Note: If using MPICH, use `mpiexec` instead of `mpirun`.


## Environment
```bash
cat /etc/os-release
uname -a
mpirun --version
gcc --version
/usr/bin/ldd --version
ldd ./bin/pfs
printenv LD_LIBRARY_PATH
```

pfs Environment snapshot:
- OS: Ubuntu 18.04.6 LTS (Bionic Beaver).
- Kernel: Linux mnode22 4.15.0-180-generic x86_64.
- MPI: Open MPI 5.0.5 (mpirun 5.0.5).
- Compiler: GCC 9.4.0; glibc: 2.31.
- MPI library location: refer to the directory of `libmpi.so.*` as shown by `ldd ./bin/pfs`; ensure that directory is on `LD_LIBRARY_PATH`.
- Runtime dependencies summary: `libmpi.so.40`, `libopen-pal.so.80`, `libpmix.so.2`, `libevent_*`, `libhwloc.so.15`, and system `libstdc++`, `libc`, `libgcc_s`.

pfs2 Environment snapshot:
- CPU: AMD Ryzen 7 5800H with Radeon Graphics (8 cores, 16 threads)
- OS: Ubuntu Ubuntu 22.04.3 LTS (Jammy Jellyfish).
- Kernel:  Linux 6.6.87.2-microsoft-standard-WSL2 x86_64.
- MPI: Open MPI 4.1.2 (mpirun 4.1.2).
- Compiler: GCC 12.3.0; glibc: 2.35.
