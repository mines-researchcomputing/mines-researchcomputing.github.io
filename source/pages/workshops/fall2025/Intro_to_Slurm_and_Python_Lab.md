# Introduction to Slurm Lab



## Running your first job

Access to compute resources on Mines's HPC platforms in managed through a job scheduler. The job scheduler manages HPC resources by having users send jobs using scripts, asking for resources, what commands to run, and how to run them. The scheduler will launch the script on compute resources when they are available. The script consists of two parts, instructions for the scheduler and the commands that the user wants to run.

A sample script is shown below:

	#!/bin/bash
	#SBATCH --job-name="sample"
	#SBATCH --nodes=2
    #SBATCH --account="fall25_hpc_workshop" 
	#SBATCH --ntasks-per-node=4
	#SBATCH --ntasks=8
	#SBATCH --time=01:00:00

	cd $SCRATCH
	ls > myfiles
	srun hostname
	
The lines that begin with `#SBATCH` are instructions to the scheduler. In order:

1. `#SBATCH --job-name="sample"` - You are naming the job
2. `#SBATCH --nodes=2` - You are telling the scheduler that you want to run on two nodes
3. `#SBATCH --ntasks-per-node=4` - You want to run four tasks per node for a total of 8 tasks. (The â€”ntasks-per-node line is redundant in this case.)
6. `#SBATCH --time=01:00:00` - You will run no longer than 1 hr.

The last three lines are normal shell commands:

7. `cd $SCRATCH` - You will be put in your scratch directory.
8. `ls > myfiles` - A directory listing will be put in the file myfiles.
9. `srun hostname` - The srun command will launch the program hostname in parallel, in this case 8 copies will be started simultaneously. Note that the ls command is not run in parallel; only a single instance will be launched.

The script is launched using the `sbatch` command. By default the standard output and standard error will be put in files with the names `slurm-######.out` and `slurm-######.err` respectively, where `######` is a job number. For example running this script on Wendian produces:

	[username@wendian001 ~]$ sbatch dohost
	Submitted batch job 363541
	After some time:

	[username@wendian001 ~]$ ls -lt | head
	total 88120
	-rw-rw-r--  1 username username        64 Sep 24 16:28 slurm-363541.out
	-rw-rw-r--  1 username username      2321 Sep 24 16:28 myfiles
	...

	[username@wendian001 ~]$ cat slurm-363541.out
	c001
	c002
	c001
	c001
	c001
	c002
	c002
	c002

## Running your first compiled job - Hello World! 
Now that you can login, navigate the filesystem and understand the basics of the SLURM job scheduler, we are ready to send our first job to the scheduler. For the first example, we are going to show how to send a job using the simple C++ program for `Hello World`:


```c++
	// Source: https://devblogs.microsoft.com/cppblog/cpp-tutorial-hello-world/
	#include <iostream>
	int main()
	{
	  std::cout << "Hello, World!" << std::endl;
	  return 0;
	}
```

For this program hello_world.cpp, we can compile it using the system's C++ compiler `g++`:

	$ g++ hello_world.cpp -o hello_world.exe

Now, to send this job using a job script, we need to write one using the SLURM preamble definitions. A sample job script `run.slurm` for our Hello Program is below:

	#!/bin/bash
	#SBATCH --nodes=1 # number of nodes
	#SBATCH --ntasks-per-node=1 # number of tasks per node
	#SBATCH --ntasks=1 # redundant; total number of tasks: ntasks = nodes * ntasks-per-node
	#SBATCH --account="fall25_hpc_workshop" 
	#SBATCH --time=00:00:01 # time in HH:MM:SS
	#SBATCH --output output.%j # standard print output labeled with SLURM job id %j
	#SBATCH --error error.%j  # standard print error  labeled with SLURM job id %j

	echo "Job has started!"
	srun hello_world.exe
	echo "Job has finished!"

We will highlight some aspects of this SLURM script. First, the lines starting with `#SBATCH` are part of a SLURM script's preamble. There are a multitude of options for the preamble, where you read more about on the SLURM sbatch documentation page here.

The command srun tells SLURM what executable to run (in this case hello_world) and how to run it using all the SLURM preamble options above.

Now to send the job to SLURM we use `sbatch`:

	$ sbatch run.slurm

You should see the following printed to the screen:

	Submitted batch job $JOB_ID
where `$JOB_ID` will look something like `5711043`.

### Using the Module System with Slurm Jobs
Software required for high performance computing is complex due to its web of dependencies of libraries and other scientifc software programs. Furthermore, each piece of scientific software may require dependencies that may conflict with existing libraries, or require a different version of a library completely. Hence, a modular system allows one to load and unload dependencies as needed, depending on what is required from the software.

On Linux systems, paths for software and their libraries are determined by setting environmental variables. You can see the settings of all variables by running the command printenv. In most cases, the most important environment variables are `PATH` and `LD_LIBRARY_PATH`. `PATH` is an environment variable that sets a list of directories that can be searched for finding applications. Similarly, `LD_LIBRARY_PATH` defines a list of directories that will be searched to find libraries used by applications. If you enter a command and see `command not found` then it is possible the directory containing the application is not in `PATH`; a similar error occurs for `LD_LIBRARY_PATH` when it cannot find a required library. The module system is designed to easily set and unset collections of environment variables. On Mines HPC systems, this is done using the lmod module system. We will briefly describe some common important module commands and also how to use it in context of setting up and submitting a job.

### Important Module commands
* `module spider` - Lists all available modules in a cascading tree format
* `module -r spider mpi` - Lists all modules related to `mpi` in a cascading tree format
* `module keyword gromacs` - List modules related to the gromacs program
* `module load apps/gromacs/gcc-ompi-plumed` - Loads the Gromacs 2021.1 module
* `module unload apps/gromacs/gcc-ompi-plumed` - Unloads the Gromacs 2021.1 module
* `module purge` - Unloads all currenty loaded modules
* `module list` - Lists all currently loaded modules
Not all applications are accessible by all HPC users. Some codes are commercial and require licensing, and hence PI approval. Some require PI approval for other reasons. If you are unable to load a module, or see permission errors when executing a job, and would like to know how you might obtain access, please submit a help request.

Some modules have dependencies that need to be manually entered. For example, the gromacs module requires that modules for the compiler and MPI be loaded first. If there is an unsatisfied dependency you will be notified.

## Running jobs with modules

### Hello World! Using MPI

```c++
	// Reference:  https://mpi.deino.net/mpi_functions/MPI_Init.html
	#include "mpi.h"
	#include "stdio.h"
	using namespace std;
	int main(int argc, char* argv[])
	{
	int rank, np;
	MPI_Init(&argc,&argv); // initialize MPI
	MPI_Comm_size(MPI_COMM_WORLD, &np); // get total number of processors
	MPI_Comm_rank(MPI_COMM_WORLD, &rank); // get current rank  
	cout << "Hello World from rank " << rank << " out of " << np << " processors!" << endl;
	MPI_Finalize(); // finalize MPI
	return 0;
	}
```
For the second program, we wanted to demonstrate how to use the system's module system. This program initializes MPI, defines integer variables to store the number of processors np and the current processor/rank, rank, and then prints Hello World from rank `rank` out of `np` processors, followed by finalizing MPI.

To compile this code, we need to load modules from the module system. First, let's print out the PATH variable before loading the modules:

```
echo $PATH
```

On Wendian:

	$ module load compilers/gcc mpi/openmpi/gcc
This will load a newer GCC compiler that is required for our OpenMPI library we're going to use with the code. After loading the modules, print out the PATH variable to see what has changed:

```
echo $PATH
```
What has changed?


To compile the code, we will use the MPI-wrapped C++ compiler mpicxx:

	$ mpicxx  hello_world.cpp -o hello_world.exe
A sample job script (which we will call run.slurm) to use the executable will look like:

	#!/bin/bash
	#SBATCH --nodes=1 # number of nodes
	#SBATCH --ntasks-per-node=12 # number of tasks per node
	#SBATCH --ntasks=12 # redundant; total number of tasks: ntasks = nodes * ntasks-per-node
	#SBATCH --account="fall25_hpc_workshop" 
	#SBATCH --time=00:00:01 # time in HH:MM:SS
	#SBATCH --output output.%j # standard print output labeled with SLURM job id %j
	#SBATCH --error error.%j  # standard print error  labeled with SLURM job id %j

	# Load the modules used to compile the code in the job script
	module load compilers/gcc mpi/openmpi/gcc

	echo "Job has started!"
	srun hello_world.exe
	echo "Job has finished!"
As with the last example, we can send the job to the scheduler using the command:

	$ sbatch run.slurm
When the job completes, you should see an output file called `output.%j` where `%j` is the job ID. The output for this job should look similar to:

	Job has started!
	Hello, World! from rank 1 out of 12 processors!
	Hello, World! from rank 3 out of 12 processors!
	Hello, World! from rank 5 out of 12 processors!
	Hello, World! from rank 7 out of 12 processors!
	Hello, World! from rank 9 out of 12 processors!
	Hello, World! from rank 10 out of 12 processors!
	Hello, World! from rank 11 out of 12 processors!
	Hello, World! from rank 0 out of 12 processors!
	Hello, World! from rank 2 out of 12 processors!
	Hello, World! from rank 4 out of 12 processors!
	Hello, World! from rank 6 out of 12 processors!
	Hello, World! from rank 8 out of 12 processors!
	Job has finished!  


### Using Python
For many cases, Python is a popular scientific computing program language with a large library of modules to choose from. There are so many dependencies that managing python modules through the HPC module system alone would become incredibly cumbersome. As an alternative, we recommend HPC users take control of the python modules they need by creating their own environment using Anaconda. We will briefly go over how to do this below.


#### Creating your own environment using Anaconda
First step is to load the desired version Python version:

	$ module load apps/python3

Then create your own python environment using the following command:

	$ conda create --name my_env python=3.13
	
You should see something similiar to the following print to the screen:
```
Retrieving notices: done
Channels:
 - conda-forge
 - defaults
Platform: linux-64
Collecting package metadata (repodata.json): done
Solving environment: done


==> WARNING: A newer version of conda exists. <==
    current version: 24.11.3
    latest version: 25.7.0

Please update conda by running

    $ conda update -n base -c conda-forge conda



## Package Plan ##

  environment location: /u/pa/sh/ndanes/.conda/envs/my_env

  added / updated specs:
    - python=3.13


The following packages will be downloaded:

    package                    |            build
    ---------------------------|-----------------
    libgcc-15.1.0              |       h767d61c_5         805 KB  conda-forge
    libgomp-15.1.0             |       h767d61c_5         437 KB  conda-forge
    libuuid-2.41.2             |       he9a06e4_0          36 KB  conda-forge
    openssl-3.5.3              |       h26f9b46_1         3.0 MB  conda-forge
    python-3.13.7              |h2b335a9_100_cp313        32.0 MB  conda-forge
    ------------------------------------------------------------
                                           Total:        36.3 MB

The following NEW packages will be INSTALLED:

  _libgcc_mutex      conda-forge/linux-64::_libgcc_mutex-0.1-conda_forge 
  _openmp_mutex      conda-forge/linux-64::_openmp_mutex-4.5-2_gnu 
  bzip2              conda-forge/linux-64::bzip2-1.0.8-hda65f42_8 
  ca-certificates    conda-forge/noarch::ca-certificates-2025.8.3-hbd8a1cb_0 
  ld_impl_linux-64   conda-forge/linux-64::ld_impl_linux-64-2.44-h1423503_1 
  libexpat           conda-forge/linux-64::libexpat-2.7.1-hecca717_0 
  libffi             conda-forge/linux-64::libffi-3.4.6-h2dba641_1 
  libgcc             conda-forge/linux-64::libgcc-15.1.0-h767d61c_5 
  libgomp            conda-forge/linux-64::libgomp-15.1.0-h767d61c_5 
  liblzma            conda-forge/linux-64::liblzma-5.8.1-hb9d3cd8_2 
  libmpdec           conda-forge/linux-64::libmpdec-4.0.0-hb9d3cd8_0 
  libsqlite          conda-forge/linux-64::libsqlite-3.50.4-h0c1763c_0 
  libuuid            conda-forge/linux-64::libuuid-2.41.2-he9a06e4_0 
  libzlib            conda-forge/linux-64::libzlib-1.3.1-hb9d3cd8_2 
  ncurses            conda-forge/linux-64::ncurses-6.5-h2d0b736_3 
  openssl            conda-forge/linux-64::openssl-3.5.3-h26f9b46_1 
  pip                conda-forge/noarch::pip-25.2-pyh145f28c_0 
  python             conda-forge/linux-64::python-3.13.7-h2b335a9_100_cp313 
  python_abi         conda-forge/noarch::python_abi-3.13-8_cp313 
  readline           conda-forge/linux-64::readline-8.2-h8c095d6_2 
  tk                 conda-forge/linux-64::tk-8.6.13-noxft_hd72426e_102 
  tzdata             conda-forge/noarch::tzdata-2025b-h78e105d_0 


Proceed ([y]/n)?
```


    
Once you confirm and the installation completes, you activate the conda environment with the following command: 

	$ conda activate my_env

You should now see (my_env) to the left on your command line like follows:

	(my_env) [username@wendian001 ~]$

You can now add packages you need for your conda environment either using pip or the conda commands. For example, we can install the petsc4py package through conda-forge:

	(my_env) [username@wendian ~]$ conda install -c conda-forge petsc4py
	# You should see something similiar print to the screen:
```
Channels:
 - conda-forge
 - defaults
Platform: linux-64
Collecting package metadata (repodata.json): done
Solving environment: done


==> WARNING: A newer version of conda exists. <==
    current version: 24.11.3
    latest version: 25.7.0

Please update conda by running

    $ conda update -n base -c conda-forge conda



## Package Plan ##

  environment location: /u/pa/sh/ndanes/.conda/envs/my_env

  added / updated specs:
    - petsc4py


The following packages will be downloaded:

    package                    |            build
    ---------------------------|-----------------
    _x86_64-microarch-level-4  | 2_skylake_avx512           8 KB  conda-forge
    attr-2.5.2                 |       h39aace5_0          66 KB  conda-forge
    fftw-3.3.10                |mpi_openmpi_h99e62ba_10         2.0 MB  conda-forge
    hdf5-1.14.6                |mpi_openmpi_h4fb29d0_3         3.8 MB  conda-forge
    hypre-2.32.0               |mpi_openmpi_h398ea61_1         1.9 MB  conda-forge
    keyutils-1.6.3             |       hb9d3cd8_0         131 KB  conda-forge
    libaec-1.1.4               |       h3f801dc_0          36 KB  conda-forge
    libamd-3.3.3               | haaf9dc3_7100102          49 KB  conda-forge
    libblas-3.9.0              | 36_h91f140b_blis          17 KB  conda-forge
    libbtf-2.3.2               | h32481e8_7100102          27 KB  conda-forge
    libcamd-3.3.3              | h32481e8_7100102          46 KB  conda-forge
    libcap-2.76                |       h0b2e76d_0         119 KB  conda-forge
    libcblas-3.9.0             | 36_h3c44731_blis          17 KB  conda-forge
    libccolamd-3.3.4           | h32481e8_7100102          42 KB  conda-forge
    libcholmod-5.3.1           | h59ddab4_7100102         1.1 MB  conda-forge
    libcolamd-3.3.4            | h32481e8_7100102          33 KB  conda-forge
    libcurl-8.14.1             |       h332b0f4_0         439 KB  conda-forge
    libfabric-2.2.0            |       ha770c72_2          14 KB  conda-forge
    libfabric1-2.2.0           |       h3ff6011_2         666 KB  conda-forge
    libgcc-ng-15.1.0           |       h69a702a_5          29 KB  conda-forge
    libgfortran-15.1.0         |       h69a702a_5          28 KB  conda-forge
    libgfortran-ng-15.1.0      |       h69a702a_5          29 KB  conda-forge
    libgfortran5-15.1.0        |       hcea5267_5         1.5 MB  conda-forge
    libhwloc-2.12.1            |default_h7f8ec31_1002         2.3 MB  conda-forge
    libiconv-1.18              |       h3b78370_2         772 KB  conda-forge
    libklu-2.3.5               | hf24d653_7100102         142 KB  conda-forge
    liblapack-3.9.0            |12_hd37a5e2_netlib         2.7 MB  conda-forge
    libnghttp2-1.67.0          |       had1ee68_0         651 KB  conda-forge
    libpmix-5.0.8              |       h4bd6b51_2         714 KB  conda-forge
    libptscotch-7.0.8          | int32_h0de9fd6_2         179 KB  conda-forge
    libscotch-7.0.8            | int32_h16dc488_2         347 KB  conda-forge
    libspqr-4.3.4              | h852d39f_7100102         213 KB  conda-forge
    libstdcxx-15.1.0           |       h8f9b012_5         3.7 MB  conda-forge
    libstdcxx-ng-15.1.0        |       h4852527_5          29 KB  conda-forge
    libsuitesparseconfig-7.10.1| h92d6892_7100102          42 KB  conda-forge
    libsystemd0-257.9          |       h996ca69_0         481 KB  conda-forge
    libudev1-257.9             |       h085a93f_0         141 KB  conda-forge
    libumfpack-6.3.5           | heb53515_7100102         424 KB  conda-forge
    libxml2-2.15.0             |       h26afc86_0          44 KB  conda-forge
    libxml2-16-2.15.0          |       ha9997c6_0         546 KB  conda-forge
    mpi-1.0.1                  |          openmpi           6 KB  conda-forge
    mumps-include-5.8.1        |       h158ef2a_3          19 KB  conda-forge
    mumps-mpi-5.8.1            |       hcd43f66_3         2.6 MB  conda-forge
    numpy-2.3.3                |  py313hf6604e3_0         8.5 MB  conda-forge
    openmpi-5.0.8              |     h2fe1745_107         3.7 MB  conda-forge
    parmetis-4.0.3             |    h02de7a9_1007         270 KB  conda-forge
    petsc-3.23.6               |  real_h5e9295b_0        20.8 MB  conda-forge
    petsc4py-3.23.6            |np2py313h72f38b6_1         1.8 MB  conda-forge
    rdma-core-59.0             |       hecca717_0         1.2 MB  conda-forge
    scalapack-2.2.0            |       h16fb9de_4         1.8 MB  conda-forge
    superlu-7.0.1              |       h8f6e6c4_0         274 KB  conda-forge
    superlu_dist-9.1.0         |       h3349319_0         1.0 MB  conda-forge
    ucc-1.5.1                  |       hb729f83_0         8.4 MB  conda-forge
    ucx-1.19.0                 |       hc93acc0_4         7.4 MB  conda-forge
    ------------------------------------------------------------
                                           Total:        83.0 MB

The following NEW packages will be INSTALLED:

  _x86_64-microarch~ conda-forge/noarch::_x86_64-microarch-level-4-2_skylake_avx512 
  attr               conda-forge/linux-64::attr-2.5.2-h39aace5_0 
  blis               conda-forge/linux-64::blis-0.9.0-h4ab18f5_2 
  c-ares             conda-forge/linux-64::c-ares-1.34.5-hb9d3cd8_0 
  fftw               conda-forge/linux-64::fftw-3.3.10-mpi_openmpi_h99e62ba_10 
  hdf5               conda-forge/linux-64::hdf5-1.14.6-mpi_openmpi_h4fb29d0_3 
  hypre              conda-forge/linux-64::hypre-2.32.0-mpi_openmpi_h398ea61_1 
  icu                conda-forge/linux-64::icu-75.1-he02047a_0 
  keyutils           conda-forge/linux-64::keyutils-1.6.3-hb9d3cd8_0 
  krb5               conda-forge/linux-64::krb5-1.21.3-h659f571_0 
  libaec             conda-forge/linux-64::libaec-1.1.4-h3f801dc_0 
  libamd             conda-forge/linux-64::libamd-3.3.3-haaf9dc3_7100102 
  libblas            conda-forge/linux-64::libblas-3.9.0-36_h91f140b_blis 
  libbtf             conda-forge/linux-64::libbtf-2.3.2-h32481e8_7100102 
  libcamd            conda-forge/linux-64::libcamd-3.3.3-h32481e8_7100102 
  libcap             conda-forge/linux-64::libcap-2.76-h0b2e76d_0 
  libcblas           conda-forge/linux-64::libcblas-3.9.0-36_h3c44731_blis 
  libccolamd         conda-forge/linux-64::libccolamd-3.3.4-h32481e8_7100102 
  libcholmod         conda-forge/linux-64::libcholmod-5.3.1-h59ddab4_7100102 
  libcolamd          conda-forge/linux-64::libcolamd-3.3.4-h32481e8_7100102 
  libcurl            conda-forge/linux-64::libcurl-8.14.1-h332b0f4_0 
  libedit            conda-forge/linux-64::libedit-3.1.20250104-pl5321h7949ede_0 
  libev              conda-forge/linux-64::libev-4.33-hd590300_2 
  libevent           conda-forge/linux-64::libevent-2.1.12-hf998b51_1 
  libfabric          conda-forge/linux-64::libfabric-2.2.0-ha770c72_2 
  libfabric1         conda-forge/linux-64::libfabric1-2.2.0-h3ff6011_2 
  libgcc-ng          conda-forge/linux-64::libgcc-ng-15.1.0-h69a702a_5 
  libgcrypt-lib      conda-forge/linux-64::libgcrypt-lib-1.11.1-hb9d3cd8_0 
  libgfortran        conda-forge/linux-64::libgfortran-15.1.0-h69a702a_5 
  libgfortran-ng     conda-forge/linux-64::libgfortran-ng-15.1.0-h69a702a_5 
  libgfortran5       conda-forge/linux-64::libgfortran5-15.1.0-hcea5267_5 
  libgpg-error       conda-forge/linux-64::libgpg-error-1.55-h3f2d84a_0 
  libhwloc           conda-forge/linux-64::libhwloc-2.12.1-default_h7f8ec31_1002 
  libiconv           conda-forge/linux-64::libiconv-1.18-h3b78370_2 
  libklu             conda-forge/linux-64::libklu-2.3.5-hf24d653_7100102 
  liblapack          conda-forge/linux-64::liblapack-3.9.0-12_hd37a5e2_netlib 
  libnghttp2         conda-forge/linux-64::libnghttp2-1.67.0-had1ee68_0 
  libnl              conda-forge/linux-64::libnl-3.11.0-hb9d3cd8_0 
  libpmix            conda-forge/linux-64::libpmix-5.0.8-h4bd6b51_2 
  libptscotch        conda-forge/linux-64::libptscotch-7.0.8-int32_h0de9fd6_2 
  libscotch          conda-forge/linux-64::libscotch-7.0.8-int32_h16dc488_2 
  libspqr            conda-forge/linux-64::libspqr-4.3.4-h852d39f_7100102 
  libssh2            conda-forge/linux-64::libssh2-1.11.1-hcf80075_0 
  libstdcxx          conda-forge/linux-64::libstdcxx-15.1.0-h8f9b012_5 
  libstdcxx-ng       conda-forge/linux-64::libstdcxx-ng-15.1.0-h4852527_5 
  libsuitesparsecon~ conda-forge/linux-64::libsuitesparseconfig-7.10.1-h92d6892_7100102 
  libsystemd0        conda-forge/linux-64::libsystemd0-257.9-h996ca69_0 
  libudev1           conda-forge/linux-64::libudev1-257.9-h085a93f_0 
  libumfpack         conda-forge/linux-64::libumfpack-6.3.5-heb53515_7100102 
  libxml2            conda-forge/linux-64::libxml2-2.15.0-h26afc86_0 
  libxml2-16         conda-forge/linux-64::libxml2-16-2.15.0-ha9997c6_0 
  lz4-c              conda-forge/linux-64::lz4-c-1.10.0-h5888daf_1 
  metis              conda-forge/linux-64::metis-5.1.0-hd0bcaf9_1007 
  mpi                conda-forge/noarch::mpi-1.0.1-openmpi 
  mumps-include      conda-forge/linux-64::mumps-include-5.8.1-h158ef2a_3 
  mumps-mpi          conda-forge/linux-64::mumps-mpi-5.8.1-hcd43f66_3 
  numpy              conda-forge/linux-64::numpy-2.3.3-py313hf6604e3_0 
  openmpi            conda-forge/linux-64::openmpi-5.0.8-h2fe1745_107 
  parmetis           conda-forge/linux-64::parmetis-4.0.3-h02de7a9_1007 
  petsc              conda-forge/linux-64::petsc-3.23.6-real_h5e9295b_0 
  petsc4py           conda-forge/4linux-64::petsc4py-3.23.6-np2py313h72f38b6_1 
  rdma-core          conda-forge/linux-64::rdma-core-59.0-hecca717_0 
  scalapack          conda-forge/linux-64::scalapack-2.2.0-h16fb9de_4 
  superlu            conda-forge/linux-64::superlu-7.0.1-h8f6e6c4_0 
  superlu_dist       conda-forge/linux-64::superlu_dist-9.1.0-h3349319_0 
  ucc                conda-forge/linux-64::ucc-1.5.1-hb729f83_0 
  ucx                conda-forge/linux-64::ucx-1.19.0-hc93acc0_4 
  yaml               conda-forge/linux-64::yaml-0.2.5-h280c20c_3 
  zstd               conda-forge/linux-64::zstd-1.5.7-hb8e6e7a_2 

```

Confirm by typing 'y' and pressing enter. You now should have your own Python environment with petsc4py!

#### Deactivating your conda environment
Once you are done with your environment, you can disable it by issuing the command:

	$ conda deactivate

## Organizing your jobs using environment variables between your home and scratch directory

The easiest way to organize SLURM jobs is to use the provided slurm job ID that is automatically assigned when your job is submitted. This job ID is conveniently stored in an environment variable called `SLURM_JOBID`. In many HPC environments, your scratch directory is faster for input/output, but it usually not backed up, whereas your home directory is. For the job example below, we will re-use our "Hello World using MPI!" example above, but automatically move the job workload to scratch, including its output.


   
