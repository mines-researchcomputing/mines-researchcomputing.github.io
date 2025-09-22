# Introduction to Slurm Lab



## Running your first job

Access to compute resources on Mines's HPC platforms in managed through a job scheduler. The job scheduler manages HPC resources by having users send jobs using scripts, asking for resources, what commands to run, and how to run them. The scheduler will launch the script on compute resources when they are available. The script consists of two parts, instructions for the scheduler and the commands that the user wants to run.

A sample script is shown below:

	#!/bin/bash
	#SBATCH --job-name="sample"
	#SBATCH --nodes=2
        #SBATCH --account="spr25_hpc_workshop" 
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
	#SBATCH --account="spr25_hpc_workshop" 
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
* `module load apps/gromacs/gcc-ompi-plumed/2021.1` - Loads the Gromacs 2021.1 module
* `module unload apps/gromacs/gcc-ompi-plumed/2021.1` - Unloads the Gromacs 2021.1 module
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
	#SBATCH --account="spr25_hpc_workshop" 
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

	$ conda create --name my_env python=3.7
You should see something similiar to the following print to the screen:

	Collecting package metadata (current_repodata.json): done
	Solving environment: done

	## Package Plan ##

	environment location: /u/pa/sh/username/.conda/envs/my_env

	added / updated specs:
		- python=3.7


	The following packages will be downloaded:

		package                    |            build
		---------------------------|-----------------
		ca-certificates-2021.1.19  |       h06a4308_0         121 KB
		certifi-2020.12.5          |   py37h06a4308_0         141 KB
		libedit-3.1.20191231       |       h14c3975_1         116 KB
		libffi-3.3                 |       he6710b0_2          50 KB
		ncurses-6.2                |       he6710b0_1         817 KB
		openssl-1.1.1i             |       h27cfd23_0         2.5 MB
		pip-20.3.3                 |   py37h06a4308_0         1.8 MB
		python-3.7.9               |       h7579374_0        45.3 MB
		readline-8.1               |       h27cfd23_0         362 KB
		setuptools-52.0.0          |   py37h06a4308_0         710 KB
		sqlite-3.33.0              |       h62c20be_0         1.1 MB
		tk-8.6.10                  |       hbc83047_0         3.0 MB
		wheel-0.36.2               |     pyhd3eb1b0_0          33 KB
		------------------------------------------------------------
											Total:        56.0 MB

	The following NEW packages will be INSTALLED:

	_libgcc_mutex      pkgs/main/linux-64::_libgcc_mutex-0.1-main
	ca-certificates    pkgs/main/linux-64::ca-certificates-2021.1.19-h06a4308_0
	certifi            pkgs/main/linux-64::certifi-2020.12.5-py37h06a4308_0
	ld_impl_linux-64   pkgs/main/linux-64::ld_impl_linux-64-2.33.1-h53a641e_7
	libedit            pkgs/main/linux-64::libedit-3.1.20191231-h14c3975_1
	libffi             pkgs/main/linux-64::libffi-3.3-he6710b0_2
	libgcc-ng          pkgs/main/linux-64::libgcc-ng-9.1.0-hdf63c60_0
	libstdcxx-ng       pkgs/main/linux-64::libstdcxx-ng-9.1.0-hdf63c60_0
	ncurses            pkgs/main/linux-64::ncurses-6.2-he6710b0_1
	openssl            pkgs/main/linux-64::openssl-1.1.1i-h27cfd23_0
	pip                pkgs/main/linux-64::pip-20.3.3-py37h06a4308_0
	python             pkgs/main/linux-64::python-3.7.9-h7579374_0
	readline           pkgs/main/linux-64::readline-8.1-h27cfd23_0
	setuptools         pkgs/main/linux-64::setuptools-52.0.0-py37h06a4308_0
	sqlite             pkgs/main/linux-64::sqlite-3.33.0-h62c20be_0
	tk                 pkgs/main/linux-64::tk-8.6.10-hbc83047_0
	wheel              pkgs/main/noarch::wheel-0.36.2-pyhd3eb1b0_0
	xz                 pkgs/main/linux-64::xz-5.2.5-h7b6447c_0
	zlib               pkgs/main/linux-64::zlib-1.2.11-h7b6447c_3


	Proceed ([y]/n)? 
	Proceed by typing 'y', followed by enter. You can then activate your new environment by typing
    
Once you confirm and the installation completes, you activate the conda environment with the following command: 

	$ conda activate my_env

You should now see (my_env) to the left on your command line like follows:

	(my_env) [username@wendian001 ~]$

You can now add packages you need for your conda environment either using pip or the conda commands. For example, we can install the petsc4py package through conda-forge:

	(my_env) [username@wendian ~]$ conda install -c conda-forge petsc4py
	You should see something similiar print to the screen:

	Collecting package metadata (current_repodata.json): done
	Solving environment: done

	## Package Plan ##

	environment location: /u/pa/sh/username/.conda/envs/my_env

	added / updated specs:
		- petsc4py


	The following packages will be downloaded:

		package                    |            build
		---------------------------|-----------------
		ca-certificates-2020.12.5  |       ha878542_0         137 KB  conda-forge
		certifi-2020.12.5          |   py37h89c1867_1         143 KB  conda-forge
		hdf5-1.10.6                |nompi_h7c3c948_1111         3.1 MB  conda-forge
		hypre-2.18.2               |       hc98498a_1         1.7 MB  conda-forge
		krb5-1.17.2                |       h926e7f8_0         1.4 MB  conda-forge
		libblas-3.9.0              |       8_openblas          11 KB  conda-forge
		libcblas-3.9.0             |       8_openblas          11 KB  conda-forge
		libcurl-7.71.1             |       hcdd3856_3         302 KB  conda-forge
		libgfortran-ng-7.5.0       |      h14aa051_18          22 KB  conda-forge
		libgfortran4-7.5.0         |      h14aa051_18         1.3 MB  conda-forge
		liblapack-3.9.0            |       8_openblas          11 KB  conda-forge
		libopenblas-0.3.12         |pthreads_hb3c22a3_1         8.2 MB  conda-forge
		metis-5.1.0                |    h58526e2_1006         4.1 MB  conda-forge
		mpi-1.0                    |            mpich           4 KB  conda-forge
		mpich-3.3.2                |       h846660c_5         6.4 MB  conda-forge
		mumps-include-5.2.1        |      ha770c72_10          23 KB  conda-forge
		mumps-mpi-5.2.1            |      h12930e3_10         3.5 MB  conda-forge
		numpy-1.18.1               |   py37h8960a57_1         5.2 MB  conda-forge
		parmetis-4.0.3             |    h9f7b9cf_1005         290 KB  conda-forge
		petsc-3.13.6               |       h9a2a0d4_1         9.8 MB  conda-forge
		petsc4py-3.13.0            |   py37h66998c9_5         1.3 MB  conda-forge
		ptscotch-6.0.9             |       h294ddb0_1         1.6 MB  conda-forge
		python_abi-3.7             |          1_cp37m           4 KB  conda-forge
		scalapack-2.0.2            |    hfacbc1e_1009         2.2 MB  conda-forge
		suitesparse-5.6.0          |       h717dc36_0         2.4 MB  conda-forge
		superlu-5.2.2              |       hfe2efc7_0         218 KB  conda-forge
		superlu_dist-6.2.0         |       h5e15a89_2         900 KB  conda-forge
		------------------------------------------------------------
											Total:        54.1 MB

	The following NEW packages will be INSTALLED:

	hdf5               conda-forge/linux-64::hdf5-1.10.6-nompi_h7c3c948_1111
	hypre              conda-forge/linux-64::hypre-2.18.2-hc98498a_1
	krb5               conda-forge/linux-64::krb5-1.17.2-h926e7f8_0
	libblas            conda-forge/linux-64::libblas-3.9.0-8_openblas
	libcblas           conda-forge/linux-64::libcblas-3.9.0-8_openblas
	libcurl            conda-forge/linux-64::libcurl-7.71.1-hcdd3856_3
	libgfortran-ng     conda-forge/linux-64::libgfortran-ng-7.5.0-h14aa051_18
	libgfortran4       conda-forge/linux-64::libgfortran4-7.5.0-h14aa051_18
	liblapack          conda-forge/linux-64::liblapack-3.9.0-8_openblas
	libopenblas        conda-forge/linux-64::libopenblas-0.3.12-pthreads_hb3c22a3_1
	libssh2            conda-forge/linux-64::libssh2-1.9.0-hab1572f_5
	metis              conda-forge/linux-64::metis-5.1.0-h58526e2_1006
	mpi                conda-forge/linux-64::mpi-1.0-mpich
	mpich              conda-forge/linux-64::mpich-3.3.2-h846660c_5
	mumps-include      conda-forge/linux-64::mumps-include-5.2.1-ha770c72_10
	mumps-mpi          conda-forge/linux-64::mumps-mpi-5.2.1-h12930e3_10
	numpy              conda-forge/linux-64::numpy-1.18.1-py37h8960a57_1
	parmetis           conda-forge/linux-64::parmetis-4.0.3-h9f7b9cf_1005
	petsc              conda-forge/linux-64::petsc-3.13.6-h9a2a0d4_1
	petsc4py           conda-forge/linux-64::petsc4py-3.13.0-py37h66998c9_5
	ptscotch           conda-forge/linux-64::ptscotch-6.0.9-h294ddb0_1
	python_abi         conda-forge/linux-64::python_abi-3.7-1_cp37m
	scalapack          conda-forge/linux-64::scalapack-2.0.2-hfacbc1e_1009
	scotch             conda-forge/linux-64::scotch-6.0.9-h0eec0ba_1
	suitesparse        conda-forge/linux-64::suitesparse-5.6.0-h717dc36_0
	superlu            conda-forge/linux-64::superlu-5.2.2-hfe2efc7_0
	superlu_dist       conda-forge/linux-64::superlu_dist-6.2.0-h5e15a89_2
	tbb                conda-forge/linux-64::tbb-2020.2-hc9558a2_0

	The following packages will be UPDATED:

	certifi            pkgs/main::certifi-2020.12.5-py37h06a~ --> conda-forge::certifi-2020.12.5-py37h89c1867_1

	The following packages will be SUPERSEDED by a higher-priority channel:

	ca-certificates    pkgs/main::ca-certificates-2021.1.19-~ --> conda-forge::ca-certificates-2020.12.5-ha878542_0


	Proceed ([y]/n)?

Confirm by typing 'y' and pressing enter. You now should have your own Python environment with petsc4py!

#### Deactivating your conda environment
Once you are done with your environment, you can disable it by issuing the command:

	$ conda deactivate
    
   
