# Systems 
HPC@Mines has two main HPC systems available for researchers and students: Wendian and Mio. 
 
## Wendian
Wendian is the newest high performance computing platform at Mines. Wendian came online in the fall of 2018. It contains 87 compute nodes; Skylake Intel processors comprise the bulk of the system, with nine Nvidia GPU nodes and two OpenPower nodes. Max performance rating is over 350 teraflops. Additionally, Wendian has three administration nodes and six file system nodes holding up to 1152 terabytes of raw storage with over 10 gigabytes/second transfer speeds. Wendian currently runs  [CentOS](https://centos.org/)  7, a community-driven, functionality-compatible computing platform to  [Red Hat Enterprise Linux](https://www.redhat.com/en/technologies/linux-platforms/enterprise-linux) . Simulations are managed as jobs using the  [SLURM](https://slurm.schedmd.com/overview.html)  scheduler.
 
### Wendian Compute Configuration
 
| **Processor**                       | **Cores** | **Memory (GB)** | **Nodes / Cards** | **Cores Total** | **Memory Total (GB)** |
|-------------------------------------|-----------|-----------------|-------------------|-----------------|-----------------------|
| Skylake 6154                        | 36        | 192             | 39                | 1,404           | 7,488                 |
| Skylake 6154                        | 36        | 384             | 39                | 1,404           | 14,976                |
| Skylake 5118                        | 24        | 192             | 5                 | 120             | 960                   |
| Skylake 5218                        | 32        | 192             | 4                 | 128             | 768                   |
| GPU cards for 5118's \(Volta V100\) | 32        | 20              | 640               |
| GPU cards for 5218's \(Tesla A100\) | 40        | 16              | 640               |
| Totals                              |           |                 |                   | 3,056           | 24,192                |
 
#### Compute Node Details
- 78 Relion XO1132G Server – Skylake Nodes
	* Dual Intel Xeon 6154 (18 cores, 36 threads, 3.0GHz)
		* 39 nodes with 192GB RAM, DDR4-2666MHz ECC (12 x 16GB)
		* 39 nodes with 384GB RAM, DDR4-2666MHz ECC (12 x 32GB)
	* 256 GB SSD
* 5 Relion XO1114GTS Server GPU Nodes 
	* Dual Intel Xeon Gold 5118 CPU (12 cores, 24 threads, 2.30GHz)
	* 192GB RAM, DDR4-2666MHz REG, ECC, 2R (12 x 16GB)
	* 256GB SSD, 2.5″, SATA, 6Gbps, 0.2 DWPD, 3D TLC (Micron 1100)
	* 4 x NVIDIA Tesla V100-SXM2, 32GB HBM2, 5120 CUDA, 640 Tensor, 300W
* 4 Relion XO1114GT GPU Server Nodes
	* Dual Intel Xeon Gold 5218 (16C, 2.30GHz, 125W)
	* 192GB RAM, DDR4-2933MHz ECC (12 x 16GB)
	* 256GB SSD
	* 4 x NVIDIA A100-PCIe, 40GB HBM2
 
#### Filesystem
* 960TB Usable Capacity @ 10GB/s
* Relion 1900 Server – running the BeeGFS MDS
* 2 x 150GB SSD, 2.5″, SATA, 6Gbps, 1 DWPD, 3D MLC
* 4 x 400GB SSD, 2.5″, SATA, 6Gbps, 3 DWPD, MLC
* IceBreaker 4936 Server – running BeeGFS OSS
* 2 x 150GB SSD, 2.5″, SATA, 6Gbps, 1 DWPD, 3D MLC
* 4 x 36 x 8TB HDD, 3.5″, SAS, 12Gbps – 1,152TB raw
* Ability to create parallel file systems from local disk on the fly
 
### Cooling
The XO1132g and X01114GTS servers have on-board water cooling for the CPUs. These are all fed water from a cooling distribution unit, a CDU. This removes about 60% of the total heat generated. The water to the compute resources is in a closed loop. The CDU has a heat exchanger with the heat emitted by the closed loop warming chilled water from central facilities. Remaining heat from the servers and heat generated by the other nodes is removed via two in-row coolers. The equipment list includes (2) APC ACRC301S In-Row Coolers and a MOTIVAIR Coolant Distribution Unit MCDU25.

## Mio 

Mio is currently the oldest running HPC system @ Mines. It is a 53+ Tflop HPC cluster for Mines student and faculty research use.
The name “Mio” is a play on words. It is a Spanish translation of the word “mine,” as in “belongs to me.” The phrase “The computer is mine” can be translated as “El ordenador es mío.”


### Students

Students have already purchased some access to Mio with Tech Fee funds—usable for general research, class projects, and learning HPC techniques. Students may also at times use Mio nodes purchased by their academic advisor or other professors. The HPC Group offers assistance to students (and faculty) to get up and running on Mio. Individual consultations and workshops are available.

### Faculty

Mio holds many advantages for professors:  
* There’s no need to manage their own HPC resources  
* Professors can access other professors’ resources when allowed  
* Mines supplies high-quality Infiniband network infrastructure, which greatly improves the scalability of multinode applications  
	
	
### Mio Compute Configuration

As of Feb. 20 2024, the available compute options are as follows:
````
$ /sw/utility/local/slurmnodes -fAvailableFeatures -fRealMemory | /sw/utility/local/jlines 3
 compute000    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute001    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute002    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute003    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute004    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute005    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute006    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute007    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute008    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute009    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute010    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute011    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute012    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute013    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute014    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute015    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute016    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute017    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute018    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute019    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute020    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute021    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute022    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute023    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute024    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute025    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute026    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute027    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute178    RealMemory 254000    AvailableFeatures core24,haswell,mlx4,fdr
 compute179    RealMemory 254000    AvailableFeatures core24,haswell,mlx4,fdr
 compute180    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute181    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute182    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute183    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute184    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute186    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute187    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute188    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute189    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute190    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute191    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute193    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute194    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute195    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute196    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute197    RealMemory 64000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute198    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute199    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute200    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute201    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute202    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute203    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute204    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute205    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute206    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute207    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute208    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute209    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute210    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute211    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute212    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute213    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute214    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute215    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute216    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute217    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute218    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 compute219    RealMemory 254000    AvailableFeatures core28,broadwell,mlx4,fdr
 gpu004    RealMemory 190903    AvailableFeatures core16,skylake,mlx4,qdr
````
