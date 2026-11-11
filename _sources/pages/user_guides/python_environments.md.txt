# Using Anaconda for python environments
Python is a commonly used programming language for scientific workloads. Among the ways to use Python libraries, the [Anaconda](https://anaconda.org) binary distribution is a very popular means for managing python packages and environments. On Mines’ HPC system users, we recommend they use our centrally managed anaconda installation for setting up Python enivronments. 
 
We will go through how to setup several common anaconda environments.
 
## Setting up base environment
 
For all environments listed below, we will need to setup a base environment. There are a few different ways to do this, with several advanced options that can be viewed [here](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html).
 
### Loading the Python module
 
#### Wendian and Mio
```
module load apps/python3      
``` 
 
### Creating  and activating a conda environment under your home directory
 
To create a conda environment using a environment name of your choice requires the following command:
 
```
conda create --name name_of_your_environment
```
You then will be asked to confirm by typing ‘y’ and press enter.
 
This will create your new environment and will be saved under  under `$HOME/.conda/envs/name_of_your_environment`
 
To activate the environment, type the command
```
conda activate name_of_your_environment
```
 
In your terminal, you should now see the following to the left of your username
```
(name_of_your_environment) username@hpc$
```
 
### Creating conda environment in a custom location
 
To create a conda environment in a custom location, use the `—prefix` flag when using `conda create`:
 
```
conda create --prefix=/your/custom/path/
```
 
You will be asked to confirm by typing ‘y’ like above. To activate the environment, type the command
 
```
conda activate /your/custom/path
```
 
 
## Appliciation-specific conda environment
 
All instructions here we be assumed to be created under `$HOME/.conda
 
### NumPy + SciPy + Matplotlib
 
For this environment, you can set it up in one command when initially creating the environment:
 
```
conda create --name scientific_python numpy scipy matplotlib
```
 
### Tensorflow (GPU)
 
For Tensorflow with NVIDIA GPU support, GPU support is included with Tensorflow:
```
create create --name tf_env tensorflow 
```
 
### Tensorflow (CPU-only)
 
If you want tensorflow with CPU support only, use the following command when creating your environment:
```
conda create --name tf_cpu_only_env tensorflow-cpu
```
 
## Cleaning up conda packages
 
After installing packages into a conda environment, there will many packages left as cache under `$HOME/.conda/pkgs` by default. These cached packages can be reused when creating new environments that depend on packages that were previously installed. However, this cache can become quite large and often cause you to reach the 20 GB limit of `$HOME`. There are few different options for cleaning up the package cache.
 
 
### Remove all cached packages
 
```
conda clean --all
```
 
This will remove all cached packages under `$HOME/.conda/pkgs`

## Proper mpi4py setup to use system MPI on Wendian/Mio

Load the required modules

```bash
module load apps/python3
module load compilers/gcc
module load mpi/openmpi/gcc-cuda
```

Set the environment variable `CC` to `mpicc`:
```bash
export CC=$(which mpicc)
```

You can double check it was set correctly by typing:

```bash
echo $CC
```

And you should see something like:
```bash
/sw/mpi/openmpi/4.1.4/gnu-cuda/bin/mpicc
```

Next, create your conda environment:
```bash
conda create -n mpi4py_test python=3.11 -y
```

and activate it:
```bash
conda activate mpi4py_test
```

Now install `mpi4py` using *pip* not *conda*:
```bash
pip install --no-cache-dir mpi4py numpy
```

You can double check it works by launching python and importing the module:
```bash
$ python
>>> from mpi4py import MPI
>>> MPI.Comm
<class 'mpi4py.MPI.Comm'>
```
## Conda to $SCRATCH directory guide

By default, Anaconda puts your conda environments under `$HOME/.conda`. Although this is convenient, restrictions to home directory quotas can limit what type of conda environments can be stored there.

Conda provides the ability to move your conda environment to any directory. In this guide, we will show you how to move your home directory using a file called `.condarc`.


### Setting up your conda environments to store in `$SCRATCH`


First we need to make the folders that will hold our conda environments and their packages:

```
mkdir -p ~/scratch/conda_envs
mkdir -p ~/scratch/conda_pkgs
```

Next, make a file in your home directory called .condarc:

```
touch $HOME/.condarc
```

Open the file, then copy and paste the following:
```
channels:
  - conda-forge
  - defaults
envs_dirs:
  - ~/scratch/conda_envs
  - ~/.conda/envs
pkgs_dirs:
  - ~/scratch/conda_pkgs
  - ~/.conda/pkgs
```


Relogin to {Wendian, Mio} and load the python module and try to make your first new conda environment.

```
module load apps/python3
conda create -n test_env  -y
```

Verify that your conda environment is in `$SCRATCH/conda_envs`:

```
ls $SCRATCH/conda_envs
```

