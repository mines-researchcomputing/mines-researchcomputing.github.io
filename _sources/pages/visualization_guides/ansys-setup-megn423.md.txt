# Ansys Fluent Startup Guide for Class MEGN 423 Fall 2025

This is a walk through for class today to get the software and start up Open Ondemand on Wendian compute nodes.

## Copy of Powerpoint presentation with walkthrough instuctions

```
scp $MINES_USERNAME@wendiandtn.mines.edu:/sw/examples/fluent/MEGN423-HPC-intro-2025.pptx .
```

NOTE: the "space" followed by "." are important here.

## Get the software for Windows
To start a download of the Ansys FLUIDS install for Windows from a PowerShell Terminal

```
scp $MINES_USERNAME@wendiandtn.mines.edu:/sw/BUILD/src/apps/ansys/Windows-2025R2/FLUIDS_2025R2_WINX64.zip .

```
And get the latest updates
```
scp $MINES_USERNAME@wendiandtn.mines.edu:/sw/BUILD/src/apps/ansys/Windows-2025R2/ANSYS_2025R2.02_WINX64.zip .
```

## A recorded training session for a reseach is available

Copy this training video to your machine with this powershell command.

```
scp $MINES_USERNAME@wendiandtn.mines.edu:/sw/examples/fluent/OOD-Fluent-overview-khood-April-5-2022_1280x720.mp4 .
```

## Before using Open Ondemand
Login into Wendian using PowerShell
Type password after running this command
```
ssh $MINES_USERNAME@wendian.mines.edu
```
NOTE: no text is show while typing in your password.

Access the fingerprint by typing "yes". The linux command prompt should look like this: [rgilmore@wendian001 ~]$

Make sure you are connected to the VPN on campus or from home. If you get a connection denied, move to another building and try again. If this still doesn't work submit a ticket here: https://helpcenter.mines.edu/TDClient/1946/Portal/Requests/TicketRequests/NewForm?ID=4GCQlvW5OYk_&RequestorType=Service

## Access Wendian through Open Ondemand Web Portal
Goto https://wendian-ondemand.mines.edu

## Starting Ansys Fluent from the Interactive Apps drop down
Select "ANSYS Workbench" and fill out the form as follows:
Account: megn423_fall2025_onilsen
Number of hours: 1-144
Number of nodes: 1-2
Number of cores: 1-32
Memory: 16 per core or max 502
Don't use "exclusive" unless you really mean it!
Node type: student or student-gpu (gpu has 64 cores)
CUDA version isn't used unless you selected a gpu node.
Number of gpus on a node: 1-4, but ignored on non-gpu Node Types
ANSYS Workbench version: 2025R2

Click Launch.

### My Interactive Sessions tab

The job will now be queued and once it's running you will see the total number of nodes and cores of the job.

Select the "Jobs" tab and "Active Jobs". Expand the running job with the drop down arrow to examine the nodes names of the machines this job is running on.

Back to the "My Interactive Sessions" tab change the compression between 3-5, and quality depending on the screen you're using. Select connect to start interacting with Ansys Workbench. Note: it maybe starting up still.

### Starting Fluent with multiple cores, or multiple nodes

Edit the properties of the setup node of Fluent, by enabling the value under "General" -> "Run Parallel Version".

#### For single node jobs

Under the property "Parallel Run Settings" change the "Number of Processors" to the total from the My Interactive Sessions for number of cores available to the job.

#### For multi-node jobs

Under the property "Parallel Run Settings" change the "Number of Processors" to the total from the My Interactive Sessions
 for number of cores available to the job.

And Disable the property "Use Shared Memory". This expands the properties for the machine list. Edit the field "Machine List" with a comma seperated list of of machine names found under "Jobs" -> "Active Jobs" for this running job "Node List" value. For example, "c079,c080"

## Starting Ansys Fluent from a Interactive Apps "Desktop"

Fill out the for similar to the "Ansys Workbench" above. Once connected you will have a full Linux Desktop session connected. Start a "Terminal" from the Applications menu in the upper left.

Load the software for Ansys:
```
module load apps/ansys
```
Note: "module load" can be shorted to "ml"

Change directories to your scratch directory and location of the files you're working with:

```
cd scratch/ansys-slurm-example
```

Start fluent launch
```
fluent
```

This will open the familiar window for launching fluent. Edit the number of processors (solver or meshing). Change to the "Parallel Settings" tab. If you're using a single node the "run types" is "Shared Memory On Local Machine". If you're uisng multiple nodes select "Distributed Memory on a Cluster" and fill in the field for Machine Names from the "Jobs" -> "Active Jobs" and the running job "Node List" value. For example, "c079,c080"

Click Start.

## Starting a SLURM job from the Terminal

You may use powershell or the "Clusters" drop down "Wendian Shell Access"

A complete example of running Ansys Fluent from a batch job using Fluent TUI commands is located in /sw/examlpes/fluent/ansys-slurm-example

This contains the necessary files to submit a job and have fluent start, read a case, initialize, solve, and save. Three files are needed: myFluent.cas.h5, submit-ansys.sbatch, and TUI_commands.jou

### Copy Fluent example to your scratch

To copy this example to your scratch directory:
```
cp -r /sw/examlpes/fluent/ansys-slurm-example $SCRATCH
```
This copies the directory "-r" recursively to the location /scratch/$USER which is stored in the variable $SCRATCH

### Change directory to this location

```
cd /scratch/$USER
```
The variable $USER contains your user name.

### Submit the SLURM job

```
sbatch submit-ansys.sbatch
```



