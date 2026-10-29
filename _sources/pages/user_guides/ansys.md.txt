# ANSYS Fluent Job Submission Guide

Ansys Fluent is a commercial application licensed for campus wide use for both 
research and academic use. The available and used license types and numbers are 
reported by using a Windows computer with Ansys installed and start the program: 
"Ansys Licensing Settings (Any Version)."

## Software module
To use the latest version on Mines HPC platforms:
- `module load apps/ansys`

To list all the available version on Mines HPC run" `module avail ansys`

## Example files
To copy the example files from a Mines HPC system directory:

- `cp -r /sw/examples/fluent/ansys-slurm-example $SCRATCH/.`

and then change to that directory:

- `cd $SCRATCH/ansys-slurm-example`

## Slurm sbatch script

The file `submit-ansys.sbatch` below contains lines that SLURM needs, line starting with "#SBATCH", 
then environment variables to setup fluent correct to use HPC, followed by loading the HPC software module, 
and then final the command to start fluent with startup flags, the parts starting with `-`.

```
#!/bin/sh
# SLURM submit script filename: submit-ansys.sbatch To run: sbatch submit-ansys.sbatch
#SBATCH --job-name fluent-simulation-test
#SBATCH --output fluent.%j.o
#SBATCH --error fluent.%j.e
#SBATCH --partition=compute
##SBATCH --account=<yourACCOUNTID>
#SBATCH --nodes 2
#SBATCH --ntasks 72
#SBATCH --mem=183G
#SBATCH --time=0-00:10:00

# environment variables used by Fluent
export FLUENT_AFFINITY=0
export SLURM_ENABLED=1
export SCHEDULER_TIGHT_COUPLING=1

# File name to store machine names assign by SLURM
FL_SCHEDULER_HOST_FILE=slurm.${SLURM_JOB_ID}.hosts
scontrol show hostnames > $FL_SCHEDULER_HOST_FILE

# Load Software - Leaving off a version number will load the latest
module load apps/ansys

# 3ddp for 3-Dimension double precision; -t1 for 1 cpu; -g for enabling graphics; 
# -driver null for no graphical interface
# use -i journal_of_TUI_commands.jou
# -command="/file/set-batch-options no yes yes" makes fluent exit if an error occurs in reading 
# the TUI_commands.jou file
# -mpi=intel selects Intel as program to handle the Message Passing Interface
# -pinfiniband.ofed sets the program network interface to use infiniband and OpenFabrics code base.
# Filename starts with the Variable SLURM_JOBID for uniqueness
echo "Starting Fluent.." `which fluent`
export DIM_AND_PRECISON=3ddp
# On Mio (this is a two line command with "\" continuing on next line):
#fluent $DIM_AND_PRECISON -t$SLURM_NTASKS -cnf=$FL_SCHEDULER_HOST_FILE \
#-command="/file/set-batch-options no yes yes" -mpi=intel -pinfinband.ofed -gu -i TUI_commands.jou

# On Wendian:
fluent $DIM_AND_PRECISON -t$SLURM_NTASKS -cnf=$FL_SCHEDULER_HOST_FILE \
-command="/file/set-batch-options no yes yes" -mpi=intel -pinfinband.ofed -gu -i TUI_commands.jou

```

## Commands that run in the Fluent console TUI (Text User Interface)

Simple form of Fluent Console TUI commands journal file
```
;A comment start with this character or to add blank lines
;Read case and data files upon startup
file/read-case myfluent.cas.h5
;file/read-data myFluent.dat.h5
solve/initialize/initialize-flow
;iteration 100 times
solve/iterate 100
file/write-case-data solved_myFluent.cas.h5
parallel/timer/usage
exit
```
To iterate transient solver the TUI command is 
```solve/dual-time-iterate <timesteps> <iteration_per_timestep>```
Refer to the Ansys Fluent TUI help documentation.

## Submitting the Ansys Fluent job
To submit this job to the SLURM scheduler run `sbatch submit-ansys.sbatch` in the terminal.

- `sbatch submit-ansys.sbatch`

This will output when successfully submited with a job number.
```
Submitted batch job 44321000
```

## Output of created file by fluent
You will now have in the directory the files used to start the job.

```
myFluent.cas.h5		submit-ansys.sbatch	TUI_commands.jou
```

And the create files when the job is running or completed. The .trn is the 
console output from fluent, while the .o file includes more information 
from SLURM at the top and bottom. Run `ls` to see the directory contents.

```
cleanup-fluent-c024-123445.sh	fluent.44321000.e	fluent.44321000.o
fluent-20250320-123055-123445.trn	slurm.44321000.hosts
```

## Monitoring a Fluent job
To monitor the running simulation you can download and open the .trn file or 
directly monitor updates as they happen with the following command, replacing the file 
name with your own:

- `tail -f fluent-20250320-123055-123445.trn`

Press Ctrl+C to exit the `tail` command.

## Slurm commands 
- To look at your SLURM job in the queue use: `squeue -u$USER`
- To cancel a job you no longer want running: `scancel <JOB ID FROM ABOVE COMMAND>`
- or run the script `sh cleanup-fluent-<node>-<PID>.sh` created when fluent started.

## See Mines guide to starting Fluent using Remote Visualization

[Ansys Remote Visualization Client Startup Guide](../visualization_guides/fluent-remote-visualization.md)
