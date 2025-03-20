# Advanced Slurm Guide

Slurm is the job manager used by both of our HPC systems, Wendian and Mio. In this guide we will go over the basics and some of the advanced features of Slurm to best facitiliate one's work on our HPC systems. 

## Basics 

Before we go into the less commonly used commands in Slurm, let's go over the basics. 

### General Slurm Commands

Here's a quick overview of commands you will use commonly:

* `sbatch` – Submit a batch script to Slurm.
* `salloc` - Allocates compute resources for an interactive shell job
* `squeue` – View information about jobs located in the Slurm scheduling queue.
* `sinfo` – View Slurm management information about nodes and partitions.
* `scancel` – Cancel jobs or job steps that were scheduled using Slurm.
* `scontrol` – View Slurm configuration and state. 

Next, let's go through each of these commands in a bit more detail with some examples.

### `sbatch`

`sbatch` will be your most commonly used command for submitting jobs to the slurm scheduler. A typical slurm script will look like this, which we denote as `submit_slurm.sh`:

````
#!/bin/bash
#SBATCH --nodes=1 # number of nodes
#SBATCH --ntasks-per-node=12 # number of tasks per node
#SBATCH --ntasks=12 # redundant; total number of tasks: ntasks = nodes * ntasks-per-node
#SBATCH --time=00:00:01 # time in HH:MM:SS
#SBATCH -o output.%j # standard print output labeled with SLURM job id %j
#SBATCH -e error.%j  # standard print error  labeled with SLURM job id %j

# module loads here

echo "Job has started!"
srun name_of_your_executable.exe
echo "Job has finished!
````

As explained in the [New User Guide](new_user_guide.md), the `#SBATCH` directives are a way to pass flags to `sbatch` within the job script. If you wanted to submit this job without the preamble `#SBATCH` directives, your submission would have to look like

````
sbatch --nodes=1 --ntasks-per-node=12 --ntasks=12 --time=00:00:01 -o output.%j -e error.%j submit_slurm.sh
````

This cleans up the submission process and allows you to change things as needed.

Slurm provides a number of other options for `sbatch` on its [docs page](https://slurm.schedmd.com/sbatch.html), but we will highlight some more commonly used ones here:


* `-A`, `--account=<Account_ID>` - This directive will allocate your job under a specific project ID. These are used on Wendian only, but allow for users with more than project to track which hours to run jobs against.
* `-a`, `--array=<array_indices>` - This directive allows one to submit an array of jobs using the index specified by the `-array` argument. We will discuss more on job arrays below.
* `-C`, `--constraint=<list_of_constraints>` - This allows a job to be constrainted to a subset of nodes. On Mio, commonly used constraints include the CPU architecture: `broadwell`, `haswell`, `sandybridge`, `ivybridge`. If you want to view which constraints are given to each node, use the `scontrol` command to get more details on the node's constraints. 
* `-c`, `--cpus-per-task=<number_of_cpus>` - Slurm distinguishes between CPUs and tasks. By default, `--cpus-per-task=1` and hence every CPU is assigned to a task. This allows finer control to allow for a job to stay on a single node, or to setup a multi-threaded job with some software.
* `-d`, `--dependency=<list_of_dependencies>` - This directive allows one to delay a job until a certain condition is met. We will show some slurm job dependency examples below. 
* `--exclusive` - This directives forces the  job to not share the resources it has allocated. For example, if a job requests 1 task with `--exclusive` enabled, the entire node will be allocated to that job. This can be useful for wanting to make sure CPU cache isn't shared to make sure performance is consistent. 
* `--gres=gpu:<GPU_TYPE>:<number_of_GPUs>` - `gres` can be used more broadly than just for GPUs, but on Wendian and Mio, they are mostly used to specifiy GPU resources. On Wendian, `<GPU_TYPE>` can either be `v100` or `a100`, especially the NVIDIA V100 and A100, respectively. `<number_of_GPUs>` can be values of 1 to 4 on either GPU type.   
* `--mem=<number of MB or GB>` - This directive specifies amount of memory *per node*. If you're running jobs with more than one node, it your total pool of memory wiill be equal to the value of `--mem` times the number of nodes allocated. 


### HPC@Mines Specific Commands

Below are some Slurm-related commands created specifically for HPC@Mines. To access these utilities, change to the following directory:

````
$ cd /sw/utility/local
````

* `slurmnodes` - View information about available nodes
* `slurmjobs` - View information about queues and running jobs
* `expands` - View a full list of nodes used for a job 

All these commands have a help flag by adding `-h` or `-help` to the end of it.

### Wendian-specific Commands 

#### Using Charge Accounts
The high-performance computing group at Mines uses an accounting system to manage allocations and usage of HPC platforms. Once allocations are awarded, users will include corresponding project account numbers with each job submission to track and report group usage numbers.

To view project account numbers against which a given user can run jobs, run the following command, replacing `<username>` with the specific username (you’ll only be able to run this command for your own username):

````
[username@wendian001 ~]$ sacctmgr show assoc user=<username> format=user,account%25 .
````

To view the association between your project account number and your project title, run the following command, replacing `<account number>` with the appropriate output from the previous command.

````
[username@wendian001 ~]$ sacctmgr show account <account number> format=account%25,descr%50 .
````

To charge your compute usage to a certain project account, include the following entry in the preamble of your batch submission script:

````
#SBATCH --account=2203081813
````
or

````
#SBATCH -A 2203081813
````

replacing `2203081813` with the project account number of interest.

Note that the “none” account is not a valid account and cannot be used to run jobs.


### Mio-specific commands

On Mio, Slurm schedules jobs on partitions. If you are not particular about the nodes on which your job will run, there is no need to specify a partition. If you would like to run on your group’s nodes or on the Power or GPU nodes, it’s necessary to specify a partition.  To do so, the option `-p` followed by the partition name must be added to the command to submit.  For example, to run in the `ppc` Power partition, and thus on the `ppc` PowerPC nodes, the syntax would be:

````
$ sbatch -p ppc --gres ${NAME_OF_YOUR_SCRIPT} 
````


## Checking on jobs using `sacct`



## Configuring compute resources

There are variety of configuration options for choosing finer details on what compute resources one can use for a HPC job. In this section, we briefly cover those options.  

### Determining node configuration



## Job Arrays

Slurm job arrays are a way to submit multiple jobs to the Slurm scheduler all at once. It uses an indexing "array" that tracks each of the jobs. 

### A simple example

### Using Job Array Index to setup parameter search



## Dependencies




