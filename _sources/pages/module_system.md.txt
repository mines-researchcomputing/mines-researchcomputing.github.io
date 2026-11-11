# The Module System
The module system is commonly used on many HPC systems to help users set up their environment to run particular programs. The behavior on Linux systems is controlled by setting environmental variables. You can see the settings of all variables by running the command `printenv`. Arguably, the most important variables are `PATH` and `LD_LIBRARY_PATH`. `PATH` is a list of directories that can be searched for finding applications. Likewise, `LD_LIBRARY_PATH` is a list of directories that will be searched to find libraries used by applications. If you enter a command and see command not found then it is possible the directory containing the application is not in `PATH`. If an application can not find a library it the system will display a similar message. The module system is designed to easily set collections of variables. You can set a number of variables by loading a module Mines uses the lmod module system. 

Full documentation on the module system can be found at: [https://lmod.readthedocs.io/en/latest](https://lmod.readthedocs.io/en/latest/)

## Notes on Lmod
Not all applications are accessible by all HPC users.  Some codes are commercial and require licensing, and hence PI approval.  Some require PI approval for other reasons.  If you are unable to load a module, or see permission errors when executing a job, and would like to know how you might obtain access, please submit a help request.
Some modules have dependencies that need to be manually entered. For example, the gromacs module requires that modules for the compiler and MPI be loaded first. If there is an unsatisfied dependency you will be notified.

## Common module commands

````
module spider
````
Lists all available modules

````
module -r spider mpi
````
List modules that are related to MPI

````
module keyword gromacs
````
List modules that are related to the gromacs program
````
module load apps/gromacs/gcc-ompi-plumed/2021.1
OR ON MIO
module load apps/gromacs/intel-impi/2021.2
````
Load the module for gromacs version 2021.1 with gcc compiler and open-mpi support with plumed on Wendian, or load gromacs version 2021.2 with the intel compiler and intel-mpi suport– this will enable you to run gromacs
````
module purge
````
Unload all modules
````
module list
````
List currently loaded modules



## Modules for Compilers


The primary compilers on Wendian and Intel are from the Intel and gnu (gcc) suites. For most parallel applications, an MPI compiler is also needed. MPI compilers actually require a backend compiler, typically Intel or gcc based. The gcc compilers are:

* `gcc` – C
* `g++` – C++
* `gfortran` – fortran


The Intel compilers are:
* `icc` - C
* `icpc` – C++
* `ifort` – fortran

The default version of the gcc compilers on both Wendian and Mio, version 4.8.5,  is rather old. Newer versions of the compilers are available via a module load. For example, on Mio:

````
[joeuser@wendian001 ~]$ gcc --version
gcc (GCC) 4.8.5 20150623 (Red Hat 4.8.5-44)
Copyright (C) 2015 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

[joeuser@wendian001 ~]$ module load compilers/gcc/9 

[joeuser@wendian001 ~]$ gcc --version
gcc (GCC) 9.3.1 20200408 (Red Hat 9.3.1-2)
Copyright (C) 2019 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
````

You will also see the same versions printed if querying `g++` or `gfortran`. 

Similarly, with Intel compilers:

````
[joesuer@wendian001 ~]$ module load compilers/intel/18.0 

[joesuer@wendian001 ~]$ icc --version
icc (ICC) 18.0.1 20171018
Copyright (C) 1985-2017 Intel Corporation.  All rights reserved.
````

### MPI Compiler Wrappers

The most popular implementations of the MPI standard available on the HPC platforms are OpenMPI, MPICH, and Intel MPI. MPI compilers are actually scripts that set environmental variables and then call backend compilers. Both versions of MPI can be used with either gnu or Intel backend compilers. Next, we present some examples below if using Wendian.


#### Build with GCC and OpenMPI

````
[joeuser@wendian001 ~]$ module purge 
[joeuser@wendian001 ~]$ module load compilers/gcc/9.3.1 
[joeuser@wendian001 ~]$ module load mpi/openmpi/gcc/3.1.3 

[joeuser@wendian001 ~]$ which mpicc
/sw/mpi/openmpi/3.1.3-gnu-7.3.1/bin/mpicc

[joeuser@wendian001 ~]$ which mpic++
/sw/mpi/openmpi/3.1.3-gnu-7.3.1/bin/mpicxx

[joeuser@wendian001 ~]$ which mpif90
/sw/mpi/openmpi/3.1.3-gnu-7.3.1/bin/mpif90

[joeuser@wendian001 ~]$ which mpif77
/sw/mpi/openmpi/3.1.3-gnu-7.3.1/bin/mpif77
````

You can also verify that the backend compiler is gcc:

````
joeuser@wendian002:[~]: mpicc --version
gcc (GCC) 9.3.1 20200408 (Red Hat 9.3.1-2)
Copyright (C) 2019 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
````


#### Build with Intel compilers and Openmpi
````
[joeuser@wendian001~]$ module purge
[joeuser@wendian001 ~]$ module load compilers/gcc/9.3.1 
[joeuser@wendian001 ~]$ module load compilers/intel/2021.1
[joeuser@wendian001 ~]$ module load mpi/openmpi/intel-cuda/4.1.2

[joeuser@wendian001 ~]$ which mpicc
/sw/mpi/openmpi/4.1.2/intel-cuda/bin/mpiicc

[joeuser@wendian001 ~]$ mpicc --version
icc (ICC) 2021.1 Beta 20201112
Copyright (C) 1985-2020 Intel Corporation.  All rights reserved.
````

#### Build with Intel and Intel MPI

````
[joeuser@wendian001 ~]$ module purge
[joeuser@wendian001 ~]$ module load compilers/intel/2021.1
[joeuser@wendian001 ~]$ module load mpi/impi/2021.1 
[joeuser@wendian001 ~]: which mpicc
/sw/compilers/intel/oneapi/mpi/2021.1.1/bin/mpicc
[joeuser@wendian001 ~]: mpiicc -v
mpiicc for the Intel(R) MPI Library 2021.1 for Linux*
Copyright 2003-2020, Intel Corporation.
icc version 2021.1 (gcc version 4.8.5 compatibility)
````

## More on Compilers


### Intel compilers
Both Wendian and Mio HPC systems contain various generations of Intel processors. Mio contains Sandybridge, Ivybridge, Haswell, Broadwell and Skylake (oldest to newest) processor, and Wendian uses Skylake processors. This is important because newer generation processors have some instructions that will not work on older processors. The newer instructions offer optimizations for the newer processors. Some of these optimizations allow for significantly faster speed for some operations, in particular when working on arrays or vectors. When a program is built with an Intel compiler, it detects what generation processor is being used to compile the code and will include the latest instructions for that processor. If the code is then run on an older processor it might return an illegal instruction error. Or if it is run on a newer processor it will not take advantage of the increased functionality. Applications built on Wendian may not run on Mio, because Wendian’s head nodes contains Skylake processors. It is possible to:

* Build an application with the lowest common instruction set so it will run on all processors, often referred to as a "universal" binary 
* Build an application so that it contains multiple sets of instructions so it will use advanced features on processors when available  
* Build an application so that it will only run on specific (or newer) processors  

There are two options that control the instruction set used for a compile: `-ax` and `-march`. These commands have the same sub-options, specifying which processor to target. The `-ax` sub-options are additive; you can specify multiple processors and the binary will contain instructions for each. This will create programs that can be larger but it will run well on all specified processors. The `-march` option will only take a single sub-option. This will create programs that can run well on the specified generation processor or newer but will most likely not run on older processors. The table below shows the various generations of processors on Mines’ platforms and the important extra instructions that are added in each generation. With the exception of fma these are advanced versions of vector instructions.



| –	| Core 2 | sandybridge | ivybridge | haswell | broadwell |	skylake |
| --- | --- | --- | --- | --- | --- | --- | 
| `sse` |	x |	x |	x |	x |	x |	x |
| `sse2` | x | x | x | x | x | x |
| `sse4_1` | | x | x | x | x | x |
| `sse4_2` | | x | x | x | x | x |
| `avx` |  | x  | x | x  |	x | x  |
| `fma` |	 |	|	| x  |	x |	x  |
| `avx2` |  |   |	| x  |	x |	x  |
| `avx512f` |  |    |	| 	| 	|   x  |
| `avx512cd` |  |   |	| 	| 	|   x  |


#### `-ax` Suboptions


| Suboption |	Adding instructions for Processor |
| --- | --- |
| `-ax=AVX` | Sandybridge or newer |
| `-ax=SANDYBRIDGE` | Sandybridge or newer |
| `-ax=IVYBRIDGE`	| Ivybridge or newer |
| `-ax=HASWELL` |	Haswell or newer |
| `-ax=BROADWELL`	| Broadwell or newer |
| `-ax=SKYLAKE` |	Skylake |
| `-ax=SKYLAKE-AVX512` | Skylake |
| `-ax=CORE-AVX-I` |	Sandybridge or newer |
| `-ax=CORE-AVX-2` |	Ivybridge or newer | 
| `-ax=CORE-AVX512` |	Skylake |

As noted above, `-ax` options are additive. You can specify `-ax=AVX,CORE-AVX512` and the code should run on any processor but it will also use the Skylake instruction set if run on a node that supports it. Since the `-ax` options are additive, if you specify `-ax=CORE-AVX512` the code will contain the default instruction set which will run anywhere but will also contain Skylake specific instructions that will be used if run on that processor. As you add more options the code will grow in size. The table below shows the options for march:



#### `-march` Suboptions

| Processor | Set |
| --- | --- |
| `-march` | anywhere |
| `-march=core-avx-i`	| sandybridge or newer |
|`-march=sandybridge` |	sandybridge or newer |
| `-march=ivybridge` |	ivybridge or newer |
|`-march=haswell`	| haswell or newer |
|`-march=core-avx2` |	haswell or newer |
|`-march=broadwell`| broadwell or newer |
|`-march=skylake`	| skylake |
|`-march=skylake-avx512` |	skylake |
|`-march=core-avx-i`	| sandybridge or newer |
|`-march=core-avx-2`	| ivybridge or newer |

The `-march` options are *not* additive. You can only specify one and you can not use both the `-ax` and `-march` options. If you build an application on Wendian and don’t specify either `-ax` or `-march` it will in default to effectively `-march=skylake-avx512` and the application will most likely not run on Mio. 

Here are some example compiles with notes on where the apps will run and the size of the application.
 
##### Build on Mio to run anywhere

````
[tkaiser@mio001 hybrid]$ icc phostone.c 
[tkaiser@mio001 hybrid]$ ls -lt a.out
-rwxr-x--x 1 tkaiser tkaiser 120095 Sep  7 13:18 a.out
````

##### Build on Mio to run anywhere but include skylake instructions
````
[tkaiser@mio001 hybrid]$ icc -ax=SKYLAKE-AVX512 phostone.c -o runs_well
[tkaiser@mio001 hybrid]$ ls -lt a.out
-rwxr-x--x 1 tkaiser tkaiser 140969 Sep  7 13:18 a.out
````

##### Build on Mio to run anywhere but include skylake instructions, same as above
````
[tkaiser@mio001 hybrid]$ icc -ax=SKYLAKE-AVX512,AVX phostone.c 
[tkaiser@mio001 hybrid]$ ls -lt a.out
-rwxr-x--x 1 tkaiser tkaiser 140969 Sep  7 13:19 a.out
````

##### Build on Mio but can only run on skylake nodes
````
[tkaiser@mio001 hybrid]$ icc -march=skylake-avx512  phostone.c -o skylake_only
[tkaiser@mio001 hybrid]$ ls -lt a.out
-rwxr-x--x 1 tkaiser tkaiser 125392 Sep  7 13:19 a.out
````

##### Trying to run a skylake code on Mio returns an error
````
[tkaiser@mio001 hybrid]$ srun -N 1 --tasks-per-node=8 ./a.out
srun: job 4393558 queued and waiting for resources
srun: job 4393558 has been allocated resources
srun: error: compute030: tasks 0-7: Illegal instruction (core dumped)
````

### GCC Compilers
The gcc compilers support the `-march` option as described under [Intel Compilers](module_system.md#intel-compilers) with the following sub-options:

| Processor | Compatibility |
| --- | --- |
| `-march` | anywhere |
|`-march=sandybridge` |	sandybridge or newer |
|`-march=ivybridge` |	ivybridge or newer |
|`-march=haswell` |	haswell or newer |
|`-march=broadwell` |	broadwell or newer |
|`-march=skylake` |	skylake or newer |


## Current Modules

### Wendian

*Last updated: June 10, 2022*


````
$ module avail 

------------------------------------------- /sw/mfiles -------------------------------------------
   StdEnv                                          apps/tcltk/8.6.9-gnu
   StdEnv1901                                      apps/test/1.2.3
   apps/abaqus/2019                                apps/trimmomatic/0.39
   apps/abinit/gcc-ompi/9.6.2                      apps/valgrind/gcc/3.14.0
   apps/adf/intelmpi/2019.305                      apps/vasp/intel-impi-vtst-1.8.4/5.4.4.pl2
   apps/adf/intelmpi/2020.101                      apps/vasp/intel-impi/5.4.4.pl2
   apps/adf/intelmpi/2022.103               (D)    apps/vasp/intel-impi/5.4.4
   apps/ansys/182                                  apps/vasp/intel-impi/6.1.2                (D)
   apps/ansys/202                                  compilers/gcc/7.3.1
   apps/ansys/211                                  compilers/gcc/9.3.1                       (D)
   apps/ansys/212                                  compilers/intel/2018
   apps/ansys/221                                  compilers/intel/2019
   apps/ansys/222                                  compilers/intel/2019.5
   apps/ansys/231                           (D)    compilers/intel/2021.1                    (D)
   apps/ansysem/202                                compilers/java/1.8.0_181
   apps/ansysem/211                                compilers/julia/1.5.1
   apps/ansysem/222                                libs/asdf-library/intel-impi/1.0.2
   apps/ansysem/231                         (D)    libs/asdf-library/intel-impi/02-27-2020   (D)
   apps/autodock_vina/1.1.2                        libs/boost/gcc-ompi/1.73.0
   apps/bowtie2/2.4.5                              libs/boost/intel-impi/1.68.0
   apps/charmm/gcc-mpich/sep-14                    libs/cuda/10.0
   apps/charmm/gcc-ompi-ljpme/nov-17-21            libs/cuda/10.1
   apps/charmm/gcc-ompi/nov-17-21                  libs/cuda/10.2
   apps/charmm/intel-impi/nov-17-21                libs/cuda/11.3                            (D)
   apps/charmm/intel-ompi-ljpme/nov-17-21          libs/curl/7.64.0-gnu
   apps/cp2k/gcc-ompi/8.2                          libs/eigen3/e3.3.7
   apps/cp2k/gcc-ompi/2022.2                (D)    libs/fftw/gcc-ompi/3.3.8
   apps/forge/22.0.2                               libs/fftw/gcc9-mpich/3.3.8
   apps/fvcom/intel-mpich/4.3.1                    libs/fftw/gcc9-ompi/3.3.8
   apps/gaussian09/g09                             libs/fftw/intel-impi/3.3.8
   apps/gaussian16/aun/c01                         libs/hdf5/gcc-ompi/1.10.7
   apps/gaussian16/c01                             libs/hdf5/intel-impi/1.8.10
   apps/gdal/gcc/3.0.2                             libs/hdf5/intel/1.10.2
   apps/gnu-parallel/20200322                      libs/hhsuite/3.3.0
   apps/gromacs/gcc-ompi-4/2021.1                  libs/lapack/gcc/3.6.1
   apps/gromacs/gcc-ompi-plumed/2021               libs/libint/2.7.0
   apps/gromacs/gcc-ompi-plumed/2021.1      (D)    libs/libint/2.7.2                         (D)
   apps/gromacs/gcc-ompi/2020.1                    libs/metis/5.1.0
   apps/gromacs/gcc-ompi/2021.1                    libs/mkl/2021.1
   apps/gromacs/gcc-ompi/2022               (D)    libs/mumps/5.1.2
   apps/gromacs/intel-impi/2019.3                  libs/netcdf-c/gcc-ompi/4.8.1
   apps/gromacs/intel-impi/2021.1           (D)    libs/netcdf-c/gcc-ompi/4.9.2              (D)
   apps/gurobi/10.0.0                              libs/netcdf-fortran/gcc-ompi/4.5.4
   apps/ibamr/gcc-ompi/0.11.0                      libs/nfft/intel/3.5.0
   apps/lammps/aun/29Oct20                         libs/nvhpc-byo-compiler/21.2
   apps/lammps/intel-impi-plumed/23Jun2022         libs/nvhpc-nompi/21.2
   apps/lammps/intel-impi/20Nov2020                libs/nvhpc/21.2
   apps/matlab/2018b                               libs/nvidia-optix/7.3
   apps/matlab/2020a                               libs/openblas/gcc/0.3.12
   apps/matlab/2021b                        (D)    libs/parmetis/gcc/4.0.3
   apps/mcnp/62                                    libs/pcre2/gcc/10.42
   apps/megahit/1.2.9                              libs/pfft/gcc-ompi/1.0.8a
   apps/metabat2/20220328                          libs/scotch/6.0.5a
   apps/mocassin/gcc/1.0.9                         libs/szip/gcc/2.1.1
   apps/modulus/singularity/22.09                  libs/wxwidgets/gcc/3.1.3
   apps/openfoam-esi/gcc-openmpi/2106              mpi/impi/2018
   apps/openfoam-esi/gcc-openmpi/2206       (D)    mpi/impi/2019.3
   apps/openfoam/gcc-openmpi/8                     mpi/impi/2019.5
   apps/openfoam/gcc-openmpi/9                     mpi/impi/2021.1                           (D)
   apps/openfoam/gcc-openmpi/10             (D)    mpi/mpich/gcc/3.0.4
   apps/openmm/gcc/7.7.0                           mpi/mpich/intel/3.4.2
   apps/paraview/gcc-gpu/5.9.0                     mpi/openmpi/gcc-cuda-10.2/4.1.2
   apps/paraview/gcc/5.8.1                         mpi/openmpi/gcc-cuda/4.1.2
   apps/paraview/gcc/5.9.0                  (D)    mpi/openmpi/gcc-cuda/4.1.4                (D)
   apps/poisfft/20200609                           mpi/openmpi/gcc/3.1.3
   apps/prms/gcc/5.2.1                             mpi/openmpi/intel-cuda/4.1.2
   apps/python2/2018.12                            mpi/openmpi/intel/3.1.3
   apps/python2/intel/2018.3                       utility/spack/0.16.2
   apps/python3/intel/2018.3                       utility/spack/0.18.1                      (D)
   apps/python3/2018.12                            utility/standard/autoconf/2.69
   apps/python3/2020.02                            utility/standard/bzip2/1.0.8
   apps/python3/2022.10                     (D)    utility/standard/cmake/3.11.4
   apps/qchem/5.2                                  utility/standard/cmake/3.16.2
   apps/quantum_espresso/gcc-ompi/7.1              utility/standard/cmake/3.18.2
   apps/quantum_espresso/intel-impi-aun/6.5        utility/standard/cmake/3.19.6             (D)
   apps/quantum_espresso/intel-impi/6.5            utility/standard/curl/7.64.0
   apps/quantum_espresso/intel-impi/7.1     (D)    utility/standard/git/2.40.1               
   apps/r/gcc/4.2.3                                utility/standard/gnu_parallel/20200322
   apps/saga/gcc/7.4.0                             utility/standard/tcltk/8.6.9
   apps/serpent/1.1.7                              utility/standard/turbovnc/2.2.4
   apps/serpent2/2.1.32                            utility/wgrib2/3.0.2i
````



### Mio

*Last updated: June 10, 2022*

````
$ module avail

------------------------------------------- /sw/mfiles -------------------------------------------
   apps/adf/intelmpi/2019.302                       apps/vasp/intel-impi-pl2/5.4.4
   apps/adf/intelmpi/2019.305                       apps/vasp/intel-impi/5.4.4
   apps/adf/intelmpi/2020.101                       apps/zeo++/0.3
   apps/adf/intelmpi/2022.103                (D)    compilers/gcc/9
   apps/ansys/202                                   compilers/intel/18.0
   apps/ansys/211                                   compilers/intel/19.0
   apps/ansys/212                                   compilers/intel/19.5
   apps/ansys/221                                   compilers/intel/2021.1
   apps/ansys/222                                   compilers/intel/2021.2                 (D)
   apps/ansys/231                            (D)    libs/bzip2/gcc/1.0.6
   apps/ansysem/211                                 libs/cuda/11.2
   apps/ansysem/231                          (D)    libs/fftw3/gcc-openmpi/3.3.7
   apps/bocs/intel/4.0.0                            libs/fftw3/intel-impi/3.3.8
   apps/cmg/2020.109.gu                             libs/hdf5/intel-impi/1.12.0
   apps/denise_black_edition/gcc-openmpi/1.4        libs/libtorch/1.12.1
   apps/denise_black_edition/intel-impi/1.4         libs/metis/5.1.0
   apps/forge/22.0.2                                libs/mkl/2021.1
   apps/gaussian16/avx/c01                          libs/mkl/2021.2                        (D)
   apps/gaussian16/avx2/c01                         libs/mumps/5.1.2
   apps/gaussian16/c01                              libs/nvhpc/21.3
   apps/gromacs/gcc-ompi/2019.6                     libs/pcre2/gcc/10.40
   apps/gromacs/gcc-ompi/2020.2              (D)    libs/petsc/gcc-ompi-dbg/3.13
   apps/gromacs/intel-impi/2019.6                   libs/petsc/intel-impi-dbg/3.15
   apps/gromacs/intel-impi/2020.1                   libs/petsc/intel-impi-universal/3.15
   apps/gromacs/intel-impi/2021.2            (D)    libs/petsc/intel-impi/3.15
   apps/ifos2d/gcc-openmpi/2.0.3                    libs/scotch/6.0.5a
   apps/lammps/29oct20                              libs/xz/gcc/5.2.5
   apps/lammps/gcc-ompi-nequip/29Sep21              mpi/impi/2018.1
   apps/lammps/gcc-ompi-plumed/23Jun2022            mpi/impi/2019.1
   apps/lammps/gcc-ompi/29Sep21                     mpi/impi/2019.5
   apps/lammps/gcc/29oct20                          mpi/impi/2021.1
   apps/lammps/intel-impi/29oct20                   mpi/impi/2021.2                        (D)
   apps/lammps/intel-ompi/29oct20                   mpi/openmpi/gcc/3.0.0
   apps/matlab/2020a                                mpi/openmpi/gcc/3.0.1
   apps/matlab/2021b                         (D)    mpi/openmpi/gcc/4.1.1                  (D)
   apps/openfoam-esi/gcc-openmpi/2106               mpi/openmpi/intel/3.0.0
   apps/openfoam/gcc-openmpi/9                      mpi/openmpi/intel/4.1.1                (D)
   apps/paraview/gcc/5.9.0                          utility/standard/cmake/3.18.2
   apps/python2/2.7.11                              utility/standard/cmake/3.26.3          (D)
   apps/python3/2020.02                             utility/standard/git/2.40.1
   apps/python3/2021.11                             utility/standard/gnu_parallel/20210922
   apps/python3/2022.10                      (D)    utility/standard/likwid/gcc/5.2.2
   apps/quantum_espresso/intel-impi/6.5             utility/standard/scons/3.1.2
   apps/r/3.6.3                                     utility/standard/spack/0.16.2
   apps/r/4.2.0                              (D)    utility/standard/turbovnc/2.1.2
   apps/seisspace/5000.10.0                         utility/standard/turbovnc/2.2.6        (D)
   apps/vasp/intel-impi-cneb/5.4.1                  utility/standard/valgrind/gcc/3.20.0
````