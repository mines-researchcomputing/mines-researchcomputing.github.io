# Research Computing Consultation

The RC team offers a variety of consultation options for researchers at Mines. This includes:

- Parallel Computing
- Hardware
- Scientific Visualization
- General HPC
- Project Planning


If you'd like to set up a call or meeting with us, you can book [here](https://outlook.office365.com/book/RCTeamServices@mines0.onmicrosoft.com/).

## Parallel Computing Consultation Guidance

Are you working on a project where your code seems to be running too slow? Does your software support parallelism but aren't sure how to use it on multicore machines or multiple nodes on a HPC? Are you writing your own software that only works on 1 CPU core and want to learn how to scale up to multiple processing cores? We're here to help!

For Parallel Computing guidance, we recommend the researcher do some preliminary work before meeting with us. This includes:

- Read our [Parallel Scaling Guide](./user_guides/Parallel_Scaling_Guide.ipynb) for a brief introduction on how parallel computing is theorized and how performance is calculated.
- A brief description of the computational model and what scientific/engineering problem you're trying to solve
- Name of Software Package 
- Programing language of software (Most common: C/C++, Fortran, Python)
- Does the software support common parallel computing libraries?
	+ Shared Memory Parallelism (e.g. OpenMP)
	+ Distributed Memory Parallellism (e.g. MPI) 
- Does the software support GPUs (CUDA, ROCm, etc)?
- What hardware configurations has your problem been tested on?
- Other relevant compute parameters (e.g. 2D/3D domain, mesh type/resolution)
- Wall times for test problem using at least 1 CPU core


