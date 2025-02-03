# Policies

## HPC & Storage Rates and Best Practices

### HPC Rates (Wendian)

| **Node Type** | **Rate per hour [USD]** | **CPU core** | **Memory per CPU core [GB]** | **GPU** |
|---------------|-------------------------|--------------|------------------------------|---------|
| CPU           | $0.005                   | 1            | 5 or 10*                     | NA      |
| GPU enabled   | $0.03**                 | 6            | 48                           | 1 x NVIDIA V100  |

*Last updated: 8/14/2024*

*There are two types of CPU nodes on Wendian: (1) a "low" memory node of 192 GB, and (2) a "high" memory node of 384 GB node. Jobs will be routed to each of these nodes depending on requested resources.

**For GPU jobs, the V100 node has 4 GPU cards. For each GPU card you request, you automatically must pay for 6 CPU cores and 48 GB memory, since this is ¼ of the available compute resources on the GPU node.

### Checking HPC usage

If you would like to check your usage for a given month, we have provided some convenient commands on Wendian.

To check usage as a user, use the command `getUtilizationByUser`:

	$janedoe@wendian002:[~]: getUtilizationByUser 
	janedoe -- Cluster/Account/User Utilization 2023-04-01T00:00:00 - 2023-04-12T11:59:59 (993600 secs)
	"Account","User","Amount","Used"
	"hpcgroup","janedoe - Jane Doe",$1.23,0

To check usage as a PI for all your users, use the command `getUtilizationByPI`:

	pi@wendian002:[~]: getUtilizationByPI
	pi -- Cluster/Account/User Utilization 2023-04-01T00:00:00 - 2023-04-12T11:59:59 (993600 secs)
	"Account"|"User"|"Amount"
	"hpcgroup","janedoe - Jane Doe",$1.23,0
	"hpcgroup","johnsmith - John Smith",$1000.00,0

### Checking job efficiency

After a given job is complete, we have a tool installed called `reportseff` that allows one to quickly check the percent utilization of CPU and memory requested. This tool can let you check jobs for a given jobID, as well as check in a given job directory that has slurm output files.

Please refer to the GitHub page for more information: [https://github.com/troycomi/reportseff](https://github.com/troycomi/reportseff)

If you find that you are poorly utilizing CPU or memory resources, feel free to reach out to the [Mines Help Center](https://helpcenter.mines.edu) for an [HPC Consultation](https://helpcenter.mines.edu/TDClient/1946/Portal/Requests/ServiceDet?ID=30287). Job efficiency likely needs to be solved by a case by case basis, and we can serve you best this way.

### Storage Rates

The current storage rate policy is below:

| Storage | Rate [USD/Terabyte/Month] |
|-----------|------------|
| Orebits, backed-up | $1.75 |
| Orebits, no back-up | $1.00 |

*Last updated: 08/14/2024*

## Data Policy

There are a few data solutions we offer at Mines for research data:

- Orebits - High capacity research storage *not* connected to HPC

The following are only on Wendian & Mio:

- `/scratch` - Active research data, subject to 180 day data purge
- `/projects` - Scratch research data used within a research project that shared with multiple users, also subject to 180 day data purge. The PI must request a projects directory with the list of authorized users.
- `/sets` - Long term data storage available on HPC. The PI must request a sets directory with a list of authorized users.

Note that all data on these directories have no redudancy, so please keep up with your own backups of active research data.

Your account privileges may be suspended if we detect any attempt to evade the data purge policies (i.e. scripting the touching of files to keep them current)

The table below breaks down the purge policy and associated costs of each the data solutions:

| Type | Purge Policy | Cost | Redunancy |
|-----------|------------|---------|-------|
| Scratch (`/scratch`) | >180 days | Free | No
| Projects (`/projects`) | >180 days | Free | No
| Wendian Long-Term Storage (`/sets`) | None | Free | No |
| Orebits | None | $2/TB/month | Yes |

> Your account privileges may be suspended if we detect any attempt to evade the data purge policies (i.e. scripting the touching of files to keep them current)

*Last updated: 11/09/2023*

## HPC Etiquette

### Login/Management Node

When you login one of our HPC systems, you login what is known as the "management" node. This node allows one to login to HPC systems and interface with the job scheduler SLURM. Additionally, the management node can also be used to edit files, create environment and compile codes. However as a general rule, **running simulation software on the management node is prohibited.** On both HPC systems, a software called `arbiter` monitors system resources used on the login node. If you are using too many CPU resources, an automated email will be sent to you Mines E-Mail, warning you and throttling your CPU usage. Once a cooldown period ends, your CPU allotment will return to normal.

### Scratch vs Home Directory

### Home Directory Policy

Every user has 20GB of data allocated to their `$HOME` directory. A common issue with filling this storage is conda environments. You can clean your conda packages by following [this](https://wpfiles.mines.edu/wp-content/uploads/ciarc/docs/pages/user_guides/python_environments.html#cleaning-up-conda-packages) page.

### Scratch Policy

Files on `/scratch` (e.g. `$SCRATCH`)  is a short-term shared filesystem for storing data currently necessary for active research projects. Subject to purge on a six-month (180 day) cycle. There are no limits (within reason) to amount of data.  

### Slurm

#### Walltime Policy

The standard maximum walltime is six days (144 hours):

``
#SBATCH –time=144:00:00.
``

This policy is strictly enforced by HPC@Mines.  In the event that the computational problem you are tasked with solving seems to require a walltime that exceeds 144 hours, we strongly encourage that you find alternative approaches to simply extending walltime.  Below are two possible approaches.

##### Increase the amount of parallelism

By increasing the number of cores/nodes used in your job, you can often decrease the total wall time needed. If your code is only a single-core workload, feel free to reach out to us for a [HPC technical consultation](https://helpcenter.mines.edu/TDClient/1946/Portal/Requests/ServiceDet?ID=33487) for other workflow options.

#### Incorporate checkpointing

Checkpointing is the process of periodically saving the state of a code's program execution so that it can be resumed at a later time.  This is extremely helpful in mitigating the effects on your calculation in the event of an unexpected crash or error.  By saving output periodically, or at a certain recurring point, and being able to restart the calculation using the saved output, a catastrophic loss of an entire days-long compute effort could be avoided.  Using checkpointing to intentionally restart a calculation at a reasonably estimated point is a recommended approach to remain within the six-day maximum walltime.

For more focused computational assistance, with the above situations and other compute aspects of your research, the HPC@Mines team is available and willing to provide personal, one-on-one assistance.  Please [submit a help request](https://helpcenter.mines.edu/TDClient/1946/Portal/Requests/ServiceDet?ID=33487) to start the process.  We also suggest consulting with members of your group or other peers currently using similar codes or applications; they may provide expedited answers to your questions, based on their experience.

## High-Performance Computing (HPC) Node Life Cycle

### Policy Statement

This policy outlines the support and decommissioning process for High-Performance Computing (HPC) nodes within our organization. It establishes the duration of support under a service contract, outlines the repair procedures after the contract ends, and sets the retirement timeframe of seven years for HPC nodes.

### Policy Details

1. Support under Service Contract:
    1. HPC nodes will be covered under a service contract for a specified duration, which will be determined during the procurement process. The service contract will include technical support, maintenance, and repair services.
    2. The service contract duration will be communicated to the relevant stakeholders and documented by the HPC administration team.
2. Post-Service Contract Repairs:
    1. After the service contract ends, the HPC administration team will continue to support HPC nodes for repairs if they meet the following criteria:
        1. The issue is identified as an easy fix that can be resolved without significant time, effort, or expense.
        2. The necessary parts for repair are readily available and can be obtained within a reasonable timeframe and cost.
    2. Repairs falling outside the criteria mentioned above may be considered on a case-by-case basis, subject to approval by the designated authority based on factors such as the node's importance, overall HPC system requirements, and financial considerations.
3. Retirement at Seven Years:
    1. At the seven-year mark from the date of initial deployment or purchase, HPC nodes will be decommissioned as part of the standard retirement process.
    2. The HPC administration team will coordinate the retirement process with the affected users.
    3. Upon retirement, the node will be securely removed from the HPC cluster, and any remaining components will be properly disposed of or repurposed as per applicable guidelines.
4. Exceptional Cases:
    1. In exceptional cases where repairing a node beyond the service contract period may be necessary due to critical dependencies or unique circumstances, a deviation from this policy may be considered.
    2. Exceptions to the retirement timeframe or repair criteria must be approved by the research computing manager, following a thorough evaluation and justification process.

### Review and Revision

This policy shall be reviewed periodically to ensure its effectiveness and relevance. Revisions to the policy may be made as necessary, with approval from the HPC steering committee.

## Wendian Policies

### Student Partition of Wendian HPC Cluster

#### Purpose

The Student partition of Wendian is dedicated to supporting student education. Access to this partition is available for use by students under the guidance of professors or sponsors. The primary goals are to promote hands-on learning of HPC (High Performance Computing) practices; including the responsible use of resources, efficient task management, and checkpointing.

### Departmental Contributions and Priority Access

- A special thank you to the Applied Mathematics & Statistics (AMS) department for contributing to the funding of the Student partition. In recognition of their contribution, AMS will receive priority access to resources, proportional to their investment.  
- Other departments or groups interested in contributing to the future expansion of the HPC cluster should reach out to the Mines Research Computing team. In return for contributions, departments will be provided similar priority access proportional to their contribution.

#### System Specifications

- Number of Nodes: 7  
- Cores per Node: 32 Intel Xeon Platinum cores (INTEL XEON PLATINUM 8562Y+)  
- Total Cores Available: 224 cores  
- Memory per node:  502 GB (usable)

#### Access

- Professors or sponsors must submit a TDX ticket to request a Wendian account.  
- In the TDX ticket, the following details must be provided:  
  - **Purpose**: Provide a general description of the project and its goals.  
  - **Start and End Dates**: Clearly specify the expected duration of the course.  
  - **Max CPU Cores Requested**: The maximum number of CPU cores the user is expected to use per job.  
  - **Software Requirements:** List all software necessary for the project.  
  - **Interactive Sessions:** Provide a tentative schedule for when interactive sessions are needed.

TDX ticket requests will be reviewed to ensure equitable and appropriate resource allocation.

#### Resource Allocation and Limits

To encourage resource sharing and good HPC habits, the following limits apply:

- Maximum Wall Time: 4 hours per job. Users are encouraged to break large workflows into smaller jobs to maximize throughput and promote fairness in scheduling.  
- Course-Based Allocations: Resource allocations will be based on the specific needs of each course, as provided in advance in the TDX ticket. This ensures resources are allocated appropriately to match the scope of the course.  *See above: Eligibility*  
- Checkpointing: Users are encouraged to checkpoint long-running jobs to avoid data loss or waste of computational resources.  
- Fair Usage: Slurm’s Fair-Share algorithm prioritizes jobs based on prior usage.  Users are expected to share resources and be considerate of other users. No user should monopolize the partition for extended periods.

#### Project Duration

- Project accounts will be granted for a specific time period as requested in the TDX ticket, with a clearly defined start and end date.  
- Projects that extend beyond the approved end date may request an extension through a follow-up TDX ticket, subject to availability of resources.

#### Job Scheduling and Queuing

- In-class interactive usage is provided via reservations on a first-come, first-served basis as requested in the TDX ticket.
- Jobs are scheduled on a first-come, first-served basis until the partition is full. Once the partition is full jobs will be given priority based largely on the QoS and [Fair-Share](https://slurm.schedmd.com/fair_tree.html) factors.  
- Users are encouraged to break large workflows into smaller jobs to maximize throughput and promote fairness in scheduling.

#### Support and Help

HPC support is available to assist users with technical questions, software installations, and job submission issues within reasonable limits. Please note that the support team will not complete assignments or perform coursework on behalf of students. For assistance, contact the Mines Research Computing team through the ticketing system: [Research Computing (RC) Services](https://helpcenter.mines.edu/TDClient/1946/Portal/Requests/TicketRequests/NewForm?ID=4GCQlvW5OYk_&RequestorType=Service).

#### Data Management and Security

- Users are responsible for managing their own data, including backup and transfer of results. Regular data transfers and cleanup are encouraged to free up storage.  
- [Data Management consultations](https://outlook.office365.com/book/ResearchComputingSupportTeamServices@mines0.onmicrosoft.com/?ae=true&login_hint) are available to learn how to use Globus & OnDemand

#### Termination of Access

- Access to the Student partition will automatically be terminated after the course’s end date.  
- Users are expected to remove any relevant data prior to the course’s end date.  
- Violations of this policy, including attempting to monopolize resources or use of the student partition for funded work, may result in earlier termination of access.

## Data Center Guidelines

The Research Computing (RC) at Colorado School of Mines provides condo & colocation services at its CTLM facility, offering fast connectivity for hosting research servers. This webpage outlines the guidelines and requirements for physically hosting data center equipment in this shared facility, benefiting the Colorado School of Mines research community.  

### Condo Additions

Principal Investigators (PIs) may integrate exclusive hardware partitions into the main cluster to support specific research needs while ensuring system compatibility and operational efficiency

#### Financial Justification and Funding

Hardware purchases require a clear financial justification and must be funded using unrestricted capital funds.

#### Support

The Research Computing team is responsible for maintaining all aspects of the computing environment, including the operating system, security features, and software. Hardware support and management are fully centralized within the Research Computing team. This model encourages a hands-off approach for Principal Investigators (PIs)."

#### Hardware

PIs must collaborate with the Research Computing team to ensure technical compatibility and obtain approval for vendor selection and hardware specifications. Integrated hardware must include InfiniBand cards, adhere to cluster interoperability standards, be under warranty, and be no more than five years old.

#### Warranty

All equipment must be under warranty or maintenance contract. Equipment is expected to have a maximum operational lifespan of five years, and customers should plan for regular equipment refresh cycles.

#### Start-Up Fees

Start-up fees will apply per port and per kW to cover the cost of cables and switches. Rates will be provided upon consultation with the Research Computing team.

#### Procurement Process

The Research Computing team will collaborate with the Procurement team to facilitate hardware purchases and ensure compliance with all institutional policies.

#### Inventory

Customers authorize Mines IT & Research Computing to manage the Annual Capital Asset Inventory process.
Hosted (Co-Located) Additions

### Colocation

This outlines the guidelines and requirements for faculty interested in  managing their own hardware within the centrally managed Mines data center. Due to limited space availability, these guidelines ensure equitable access and responsible resource utilization.  

#### Cybersecurity

Adherence to industry best practices and relevant security regulations is mandatory. While Mines IT provides a secure network environment, failure to maintain adequate security standards on the part of a hardware owner may result in disconnection from the network to mitigate potential security risks to the overall campus environment.

#### Rack Space

Customers may only deploy equipment that has been approved by Mines IT. Allocated rack space must be used exclusively, and customers are obligated to respect the space allotted to other users.

#### Hardware

Equipment must function reliably within a cooling temperature up to 85°F.   Servers must have redundant power supplies or run the risk of losing function during data center and network maintenance windows.  A baseboard management system is strongly suggested such as iLO, iDRAC, or other BMC for the purpose of remote OS upgrades, rebooting, etc.  Owners are responsible for physical delivery to the CTLM server room, unboxing, rack installation, and configuration (OS, sys admin configs, networking, user management) of the hosted server.  IT System Administration team will assist with network port, network cable setup, KVM connection, and power access within the CTLM data center.

#### Access

Systems are accessible via the network 24x7, except for regularly scheduled maintenance downtime related to the Mines network or server room maintenance.  Physical access to the data center is restricted to authorized faculty and staff 24/7 after registering with the building proctor and setting up credentials with Mines Lock Shop.

#### Inventory

System owners are expected to manage the Annual Capital Asset Inventory process.

#### Warranty

All equipment must be under warranty or maintenance contract. Equipment is expected to have a maximum operational lifespan of five years, and customers should plan for regular equipment refresh cycles. Customers are responsible for promptly updating equipment information with Mines IT upon replacement.

#### Support

Mines IT does NOT provide direct technical support for co-located equipment. Principal Investigators (PIs) are responsible for the maintenance and support of their hardware and software.

#### Cost

Start-up fees will be assessed based on port usage and power consumption (per kW). These fees cover the costs associated with cabling and network switches. Rates will be determined during consultation with the Research Computing team
