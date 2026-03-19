# Running MATLAB on HPC Systems
 
Before you can use MATLAB, you must get approval from your PI to load the matlab modules. This is to encourage HPC users to use software better optimized for paralel computing resources. Once you are approved by your PI and added as an approved user, you can follow the instructions below.

To get started with MATLAB on {Mio, Wendian}, you will need to load the Matlab module from the module system either:

1. An interactive job – requesting a node in order to run Matlab in the command line
2. Within a job script – submitting a Matlab script to Mio’s job queuing system (called Slurm)

## Running an interactive job
Type the following into the command line to initiate an interactive job:

```
salloc -N 1 --ntasks=1 --cpus-per-task=12
```

This will request a single node (-N 1) with 12 cpus (–cpus-per-task=12) to be run within the terminal.. Once you are allocated the job, you should see something like this on the screen:

```
salloc: job ####### has been allocated resources
```

where `#######` is the job number. Once you see this, you can load the module for Matlab:

On Mio:

```
module load Apps/Matlab/R2020a
```

On Wendian:

```
module load apps/matlab/R2020a
```

To start Matlab, type the following to open up the command prompt:

```
matlab -nodesktop -nosplash
```
You will now have access to Matlab’s command prompt, and can navigate your directories on {Mio, Wendian} through it as well.

## Submitting a Matlab Job to {Mio, Wendian}
However, if you already have a Matlab script ready to run, it is recommended to submit through a slurm job script. In the directory where your Matlab script is, create a new file (using the text editor of your choice, I’ll use vim):

```
vim my_run_script.sh
```

Once you’ve created the file, I have provided a basic template for the job script; you will modify the parameters to fit your needs:

```
#!/bin/bash
#SBATCH -n 1
#SBATCH -t 1:59:00
#SBATCH -J test_script
#SBATCH -o slurm.%j.out
#SBATCH -e slurm.%j.err
# load the Matlab module
# uncomment this for Mio
#module load Apps/Matlab/R2020a
# uncomment this for  Wendian:
module load apps/matlab/R2020a
# run your job
matlab -nodisplay -nodesktop -r "run ./name_of_script.m ; quit"
```

You then submit the job to the SLURM scheduler by typing:

```
$ sbatch my_run_script.sh
```

You can monitor the status of your job(s) by typing:

```
squeue -u username
```
