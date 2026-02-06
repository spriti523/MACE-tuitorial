# MACE-tuitorial
MACE with lammps

# To compile LAMMPS 
Step 1: Get mace potential. You can download from https://github.com/ACEsuit/mace. For tuitorial purpose use we will be using mace-omat-0-medium.model-lammps.pt

Step 2: Compile lammps with mace or use the provided executable

          Compiling lammps with mace using cpu. 
          --> git clone LAMMPS using "git clone -b release https://github.com/lammps/lammps.git"
          --> build it from scratch https://docs.lammps.org/Build_cmake.html Follow through
          --> If not use the following to make it using following in a build folder
          module purge
          module load gcc/15.1.0
          module load openmpi/5.0.7
          module load cmake
          module load mkl
          cmake -DCMAKE_INSTALL_PREFIX=$(pwd) \
                -D CMAKE_CXX_STANDARD=17 \
                -D CMAKE_CXX_STANDARD_REQUIRED=ON \
                -D BUILD_MPI=ON \
                -D BUILD_OMP=ON \
                -D PKG_OPENMP=ON \
                -D PKG_ML-MACE=ON \
                -D CMAKE_PREFIX_PATH=$(pwd)/../../libtorch \
                -DCMAKE_POLICY_VERSION_MINIMUM=3.5 \
                ../cmake
 
 Step 3: To learn and run LAMMPS--> Manual https://docs.lammps.org/Commands.html.

 # To run lammps
 Step 4: Run lammps using the job script provided. You need input and a structure file. 
         --> Input file contains the control for running the commands.
         --> Structure file contains the coordinates.
         --> run serially by using. lmp -in input. 
         --> run on cores using the attached job submission script.
 



