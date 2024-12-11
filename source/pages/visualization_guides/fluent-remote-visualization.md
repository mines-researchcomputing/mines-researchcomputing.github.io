# Ansys Remote Visualization Client Startup Guide

## Using ANSYS Remote Visualization Client at Mines

This utilizes the GPU of your computer over CPU rendering on the HPC systems. 
For quick access to Ansys products use either [wendian-ondemand.mines.edu](https://wendian-ondemand.mines.edu) 
or [mio-ondemand.mines.edu](https://mio-ondemand.mines.edu) for CPU rendering.
The following application notes are adapted for use at Colorado School of Mines for our clusters, Wendian and Mio. 
For detailed information read the [Fluent Workspaces User’s Guide, Chapter 1: 
Remote Visualization and Accessing Fluent Remotely](https://ansyshelp.ansys.com/account/secured?returnurl=/Views/Secured/corp/v242/en/flu_ug_workspaces/flu_ug_chp_remote_client.html), 
see Ansyshelp.ansys.com. Note that the Major and minor version and service pack numbers must match. 

The purpose of using the ANSYS Fluent Remote Visualization Client is to access remotely
running simulations without directly owning the process in case of the network disconnection. 
Using this application you can connect to a total of 6 running simulations and display the mesh, 
contour and vector plots, residual and monitor plots, and modify settings and send text commands 
to the TUI on the running Fluent (meshing or solver) server.

### Step 1: Submit a Slurm Job

To get started create a SLURM submit script with the following in a directory with a Fluent case file.
$HOME/scratch/$USER/fluent-cases/flremote-svr-start.submit
```
#!/bin/bash
#SLURM submit script filename: flremote-svr-start.submit
#SBATCH -t 00:30:00
#SBATCH -n1
#Opening 2-12 ports depending on host_node operation more than one server (2 minimum)
export REMOTING_PORTS=2678/portspan=2
# Loading Wendian module for Ansys version 2024R2
module load apps/ansys/242

# 3ddp for 3-Dimension double precision; -t1 for 1 cpu; -g for enabling graphics; -driver null for no graphical interface
# -sifile= for enabling the fluent server and giving a filename to store IP and PASSWORD
# use -i journal_of_TUI_commands.jou
# Filename starts with the variable SLURM_JOBID for uniqueness 

fluent 3ddp -t1 -g -driver null -sifile=./"$SLURM_JOBID"_fluent_server_info.txt
```

Now, submit this to the SLURM scheduler
```
[joeuser@wendian001.mines.edu]$ sbatch flremote-svr-start.submit
Submitted batch job 6033061
```
Once the Fluent job starts the file with the SLURM_JOBID_fluent_server_info.txt is created with the connection information, 
the Fluent cleanup script (can be run by executing ‘sh FILENAME’ at the command prompt), a transcript files, and the 
slurm-SLURM_JOBID output file. The connection information file identifies the IP address and password needed to 
create the network port tunnel from the client machine to this node.

For example, this job now has the follow files in the working directory.
```
[joeuser@wendian001.mines.ed]$ ls 
6033061_fluent_server_info.txt  cleanup-fluent-node136-20180.sh  fluent-20210216-154942-19982.trn  slurm-6033061.out
catalytic_converter.cas         flremote-svr-start.submit
```
As an alternative to an HPC running server. You may test running this from a Fluent GUI (Linux or Windows) by 
selecting “File” -> “Start/Stop Server” or run the TUI “server/start-server”. You can connect to local running 
copies of the server on the same machine to practice setting up commands or running scripts. 

### Step 1: Alternative Startup using a Journal File

#### Submit a Fluent job using predefined commands
Fluent executes commands through the TUI (Text User Interface). The format is a command followed by white space, comma, 
or answer to the prompted question. For a detailed list see the Ansys Help documentation for Fluent -> Fluent Text 
Command List -> Solution Mode. General navigation from a the fluent console is a directory structure. 
Press ‘enter/return’ will list available entries that are either commands or directories. 
Pressing ‘q’ will take you back one, and entering a command in the shortest unique letters will begin the 
prompts for options regarding that command. For example, ‘define/models/viscous/ke-realizable?’ is a simple yes 
or no, but ‘/define/boundary-conditions/velocity-inlet’ requires further information such as which surface(s), 
velocity, turbulent parameters, temperature, species, etc.

The a simple journal file may contain a read case, initialize or read data, iterate, save case and data, and exit. 
However, for our purposes here starting a visualization client server and reading a case is sufficient. 

The file “read_case.jou” contains these two lines:
```
;Journal file to start the visualization client server and read a case
server/start-client
file/read-case myFluentCase.cas
```

Then the command in the batch script to start fluent changes to call this journal file. Using multiple 
processors and nodes the variables $cpus and $SLURM_JOB_ID.nodes are created in the script 
See [Fluent using Multiple Nodes](./fluentstartup.md#fluent-using-multiple-nodes). So the last line becomes:
```
fluent 3ddp -t$cpus -cnf=$SLURM_JOB_ID.nodes -gu -i read_case.jou
```

### Step 2: Get the connection infomation and start a tunnel

Output the contents of the fluent server file: SLURM_JOBID_fluent_server_info.txt or use the Ondemand "Files" tab to view the text file.

```
[joeuser@wendian001.mines.edu]$ cat 6033061_fluent_server_info.txt
172.18.10.78:2678
229zknbn
```

Edit or copy the content to a file on your local machine and change the IP address to "localhost", for example 
on my desktop this file looks like this now:
C:\Users\joeuser\Desktop\6033061_fluent_server_info.txt
```
localhost:2678
229zknbn
```

#### Starting a Tunnel to the compute node host

Opening a Terminal (Linux) or Windows PowerShell and run the ssh command with the flag -L with the connection information.

```
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

Install the latest PowerShell for new features and improvements! https://aka.ms/PSWindows

PS Z:\> ssh -L 8001:c078:2678 joeuser@wendian.mines.edu
```
The first number '8001' is your choice, the compute host name is c078, and the second number '2678' is the port number gave in the submit script.

### Step 3: Start your local client and connect

Start the "Remote Visualization Client 2024 R2" App from the windows start menu. Locate the "Server info Filename" box 
under "Properties - RemoteSession 1" and click in the box. Select the file on your Desktop and the client will connect through your localhost port 
through the 'ssh' encripted tunnel to the compute node.

From here you can view the mesh, watch the simulation run, send commands to the server, and disconnect knowing the job is still running.

