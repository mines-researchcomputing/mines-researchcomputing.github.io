# Intro to Parallel Programs on HPC - Using Gromacs
Author: Nicholas A. Danes, PhD

For this lab, we will learn how to run [GROMACS](https://www.gromacs.org/), a popular molecular dynamics software package used on HPC systems. It supports both shared and distributed memory parallel processing, making it a great first test problem for new HPC users.

To begin, we first need to download a benchmark problem from GROMACS and the workshop files. 
Copy the workshop materials using the following command:
	
```
cp /sw/BUILD/src/workshop/Workshop_Fall2025_day2.tar.gz ~/scratch
```
    
And go to that directory and untar it:
    
```
cd ~/scratch && tar -xf Workshop_Fall2025_day2.tar.gz
cd Workshop_Fall2024/rk2_python && ls
```


This sample problem is provided by the Max Planck Institue in Göttingen[1.] To download it, let's go to our scratch directory, create a new folder and then download the zip file:

```bash

cd ~/scratch/Workshop_Spring2025/gromacs
wget https://www.mpinat.mpg.de/benchRIB.zip

# unzip
unzip benchRIB.zip
```

If unzipped correctly, you should see a file in the directory called `benchRIB.tpr`:

```
$ ls
benchRIB.tpr  benchRIB.zip
```

Next, let's create a job script in order to run this job. We will try a few different configurations:

1. A single 36-core node, using 36 MPI tasks
2. A single 36-core node, using 36 OpenMP threads on 1 task

## Single node, all MPI tasks

First the first job, let's try one node, with 36 MPI tasks to run for 1 hour. We will also time the run in order to compare it to other configurations. Here is what the job script will look like:

```bash
#!/bin/bash
#SBATCH --job-name=gromacs-single-node-MPI
#SBATCH --account="fall25_hpc_workshop"
#SBATCH --exclusive # USE FOR THIS LAB ONLY
#SBATCH --output=%j.out
#SBATCH --time=00:60:00 ## format is HH:MM:SS
#SBATCH --nodes=1
#SBATCH --ntasks=36

module load compilers/gcc
module load mpi/openmpi/gcc
module load libs/fftw/gcc-ompi
module load apps/gromacs/gcc-ompi

# set the number of OpenMP threads
NTOMP=1

mkdir -p ~/scratch/jobs/${SLURM_JOBID}
cd ~/scratch/jobs/${SLURM_JOBID}

start=`date +%s.%N`
srun gmx_mpi mdrun -ntomp ${NTOMP} -s ${SLURM_SUBMIT_DIR}/benchRIB.tpr -resethway
end=`date +%s.%N`

runtime=$( echo "$end - $start" | bc -l )

echo $runtime
```


We will call the Slurm script file `single_node_MPI.sh`. To submit the job, type

```
$ sbatch single_node_MPI.sh
```

For reference, here is the results achieved for this case:
```
               Core t (s)   Wall t (s)        (%)
       Time:    14489.672      402.492     3600.0
                 (ns/day)    (hour/ns)
Performance:        4.294        5.589

GROMACS reminds you: "Roses are read // Violets are blue // Unexpected '}' on line 32" (Anonymous)

845.404034006
````

## Single node, all OpenMP threads on 1 task

GROMACS also supports OpenMP threads. To leverage this, we will tell Slurm we only want one task, but want 36 cpus per task on the job:

```bash
#!/bin/bash
#SBATCH --job-name=gromacs-single-node-MPI
#SBATCH --account="fall25_hpc_workshop"
#SBATCH --exclusive # USE FOR THIS LAB ONLY
#SBATCH --output=%j.out
#SBATCH --time=00:60:00 ## format is HH:MM:SS
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=36

module load compilers/gcc
module load mpi/openmpi/gcc
module load libs/fftw/gcc-ompi
module load apps/gromacs/gcc-ompi

# set the number of OpenMP threads
NTOMP=${SLURM_CPUS_PER_TASK}

mkdir -p ~/scratch/jobs/${SLURM_JOBID}
cd ~/scratch/jobs/${SLURM_JOBID}

start=`date +%s.%N`
srun gmx_mpi mdrun -ntomp ${NTOMP} -s ${SLURM_SUBMIT_DIR}/benchRIB.tpr -resethway
end=`date +%s.%N`

runtime=$( echo "$end - $start" | bc -l )

echo $runtime
```

For reference, the final result with 36 OpenMP threads on 1 task should look like:

```
Writing final coordinates.

               Core t (s)   Wall t (s)        (%)
       Time:    15936.559      442.684     3600.0
                 (ns/day)    (hour/ns)
Performance:        3.904        6.147

GROMACS reminds you: "Why add prime numbers? Prime numbers are made to be multiplied." (Lev Landau)

911.438343690
```

So we see with this particular test problem, running all MPI tasks with our node is slightly faster than running 1 task with 36 OpenMP thread.

[1] Dept. of Theoretical and Computational Biophysics, Max Planck Institute for Multidisciplinary Sciences, Göttingen, https://www.mpinat.mpg.de/grubmueller/bench . The GROMACS input .tpr files are licensed under the Creative Commons Attribution 4.0 International license.



## Exercises

Modify the job script to run the following cases. Do you see any improvement in performance?
1. Run with 1 node, 18 MPI tasks, 2 OpenMP threads per task
2. Run with 2 nodes, 36 MPI tasks (18 MPI tasks per node), 1 OpenMP thread per task
3. Run with 2 nodes, 72 MPI tasks (36 MPI tasks per node), 1 OpenMP thread per task
4. Run with 2 nodes, 36 MPI tasks (18 MPI tasks per node), 2 OpenMP thread per task
5. Run with 1 **GPU** node with 1 GPU card, 1 task (with 6 CPUs per task) by running the code with the following script. You can just copy and paste the following to a new job script called `single_node_GPU.sh`:

```bash
#!/bin/bash
#SBATCH --job-name=gromacs-single-node-GPU
#SBATCH -p gpu
#SBATCH --gres=gpu:v100:1
#SBATCH --output=%j.out
#SBATCH --time=00:60:00 ## format is HH:MM:SS
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=6
#SBATCH --account="fall25_hpc_workshop"

module load compilers/gcc
module load libs/cuda
module load mpi/openmpi/gcc-cuda
module load libs/fftw/gcc-ompi
ml apps/gromacs/gcc-ompi-cuda

# set the number of OpenMP threads
NTOMP=1

mkdir -p ~/scratch/jobs/${SLURM_JOBID}
cd ~/scratch/jobs/${SLURM_JOBID}

start=`date +%s.%N`
srun gmx mdrun -ntomp ${NTOMP} -nb gpu -s ${SLURM_SUBMIT_DIR}/benchRIB.tpr -resethway
end=`date +%s.%N`

runtime=$( echo "$end - $start" | bc -l )

echo $runtime
```

There a number of other parameters you can adjust to run on CPU versus GPU with gromacs, based on the physical solver. For more details if you're interested, see: https://manual.gromacs.org/documentation/current/user-guide/mdrun-performance.html



```python

```
