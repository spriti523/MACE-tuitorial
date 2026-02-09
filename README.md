```markdown
# MACE Tutorial with LAMMPS on CPU

This repository provides a quick tutorial on how to compile LAMMPS with the MACE machine-learning potential package and run a simple simulation.

## 1. Overview

You will:

1. Obtain a MACE potential model.
2. Compile LAMMPS with the MACE package enabled (or use a provided executable).
3. Prepare input and structure files.
4. Run LAMMPS either interactively or via a job script.

---

## 2. Prerequisites

You should have access to:

- A Linux environment (local machine or HPC cluster)
- Git  
- CMake  
- MPI (e.g., OpenMPI)  
- A C++ compiler (e.g., GCC)  
- BLAS/LAPACK (e.g., Intel MKL)  
- [libtorch](https://pytorch.org/cppdocs/) (PyTorch C++ library)  

If you are on a cluster that uses environment modules (e.g., `module load`), adjust module names to match your system.

---

## 3. Obtain a MACE Potential

Download a MACE potential from the official repository:

- GitHub: <https://github.com/ACEsuit/mace>

For this tutorial, use:

- `mace-omat-0-medium.model-lammps.pt`

Place this file in a directory that will be accessible when running LAMMPS (e.g., the same directory as your input file).

---

## 4. Compile LAMMPS with MACE

You can either:

- Compile LAMMPS yourself with the MACE package enabled, or  
- Use a pre-compiled LAMMPS executable that already includes MACE support (if provided by your system).

### 4.1 Clone LAMMPS

```bash
git clone -b release https://github.com/lammps/lammps.git
cd lammps
```

### 4.2 Create a Build Directory

```bash
mkdir build
cd build
```

### 4.3 Load Required Modules (example)

Adapt these to your system:

```bash
module purge
module load gcc/15.1.0
module load openmpi/5.0.7
module load cmake
module load mkl
```

Ensure `libtorch` is downloaded and available, and note its location (e.g., `../../libtorch` relative to the `build` directory).

### 4.4 Configure with CMake (CPU build with MACE)

```bash
cmake -DCMAKE_INSTALL_PREFIX=$(pwd) \
      -DCMAKE_CXX_STANDARD=17 \
      -DCMAKE_CXX_STANDARD_REQUIRED=ON \
      -DBUILD_MPI=ON \
      -DBUILD_OMP=ON \
      -DPKG_OPENMP=ON \
      -DPKG_ML-MACE=ON \
      -DCMAKE_PREFIX_PATH=$(pwd)/../../libtorch \
      -DCMAKE_POLICY_VERSION_MINIMUM=3.5 \
      ../cmake
```

### 4.5 Build and Install

```bash
make -j
make install ( or simply run sh build.sh) as it already contains the above. Note: add appropriate path to your own libtorch folder in -DCMAKE_PREFIX_PATH=$(pwd)/../../libtorch \
```

For more detailed information on building LAMMPS with CMake, see the official documentation:

- <https://docs.lammps.org/Build_cmake.html>

---

## 5. LAMMPS Input and Structure Files

To run a simulation, you need:

1. **LAMMPS input file** (e.g., `input_script`)  
   - Contains simulation settings, MACE potential commands, run controls, etc.
2. **Structure file** (e.g., `structure.data` or `structure.xyz`)  
   - Contains atomic coordinates and system topology.
3. **MACE model file** (e.g., `mace-omat-0-medium.model-lammps.pt`)  
   - Called from within the LAMMPS input script when you define the potential.

For details on LAMMPS commands and input syntax, see:

- <https://docs.lammps.org/Commands.html>

---

## 6. Running LAMMPS

Assume your compiled executable is called `lmp` (or something like `lmp_mpi`), and you are in the directory containing your input file and structure file.

### 6.1 Serial Run

```bash
./lmp -in in.mace
```

Replace `lmp` with the actual name of your executable if different.

### 6.2 Parallel Run (Example with MPI)

```bash
mpirun -np NPROCS ./lmp -in in.mace
```

Replace `NPROCS` with the number of MPI ranks you want to use.

### 6.3 Running via Job Script on a Cluster

If you are on an HPC system with a scheduler (e.g., SLURM, PBS), you can use a job submission script. Dedicated job script is provided.

You can perhaps now use MACE as a machine-learning potential within LAMMPS for your simulations.
