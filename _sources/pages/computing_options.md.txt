# Computing Options at Mines

Mines offers a variety of high performance computing options. We have on-site HPC clusters, but also can provide user support for alternative high performance computing options if our on-site clusters do not fit your needs. We provide an overview of our computing options at Mines below.

If you have a specific use case, or have more questions, feel free to email us at [rc@mines.edu](mailto:rc@mines.edu).

## Pay-per-use HPC

### Wendian

Our primary HPC computing platform is Wendian, which started service at Mines for the Spring 2019 semester. We have extensive documentation on this system on this website, which you can read the details on our [Systems](./systems.md) page.

Wendian uses a "core-hour" model to charge for computing usage. We charge $0.005 per core-hour, which details can be found on our [Guidelines](./guidelines.md) page. 
  

### AWS Cloud Computing

Mines [Infrastructure Solutions (IS)](https://it.mines.edu/organization/infrastructure-services/) team in IT is currently working toward supporting researchers on AWS. If you are interested in using AWS for research, but need support, please reach out to us by submitting to a [ticket via Mines Help Desk](https://helpcenter.mines.edu/TDClient/1946/Portal/Requests/ServiceDet?ID=51556).


## Free or Credit-based HPC Options

### NSF ACCESS Program

The National Science Foundation provides compute credits for various levels of research through its [ACCESS](https://access-ci.org/) program. The requesting compute resources requires the basic following steps:

1. Create an [ACCESS account](https://identity.access-ci.org/new-user-direct) by clicking this [link](https://identity.access-ci.org/new-user-direct) and login using your ACCESS credentials.
    - NOTE: The ACCESS login portal will show Mines as as a login option, but will not work properly until you link from your logged in ACCESS-CI account to your Mines CI-LOGIN account (see [Identity linking page](https://operations.access-ci.org/identity/id-linking)). 
2. Request an [ACCESS allocation](https://allocations.access-ci.org/opportunities). There are several tiers of allocation options, each with their own requirements of submission. For details on the type of allocations and their requirements, see the [Prepare requests: Overview](https://allocations.access-ci.org/prepare-requests-overview) page. For the lowest tier allocation, the "Explore" tier, no prior NSF funding is required to request an allocation. Classrooms are also eligible to request allocations. *Make sure to add RC staff to your allocation as a member during the allocation request so we can directly support you on the project.* For your convenience, we've summarized the ACCESS allocation options in the table below:

| Allocation Type | Ideal For  |  Maximum ACCESS Credits Award | Duration |  Requires NSF Grant | Basic Requirements |
|--------------|-----------|------------|------------|------------|------------|
| Explore  | Benchmarking new projects/resources, Graduate students, exploratory research, research code development, small classrooms |  400,000 | Supported grant duration OR 12 months | No | Abstract of project only for proposal, half-way progress report (to receive second half of credits), and end of project follow-up report | 
| Discover  | Grant funded projects with moderate resource requirements, larger classes, NSF graduate fellowships, benchmarking/testing at smaller scale and scientific gateway development |  1,500,000 | Supported grant duration OR 12 months | No  | 1-page proposal, half-way progress report (to receive second half of credits) and end of project follow-up report | 
| Accelerate | Multi-grant and/or collaborative projects, mid-scale resoure requirements and growing scientific gateways | 3,000,000 |  Supported grant duration OR 12 months | Yes  | 3-page max length proposal, half-way progress report (to receive second half of credits) and end of project follow-up report | 
| Maximize | Large-scale projects  | Not Applicable | 12 months |  Yes  | 10-page max length proposal, half-way progress report (to receive second half of credits) and end of project follow-up report | 

For more details on comparisons of the allocations please see the primary reference: [https://allocations.access-ci.org/prepare-requests-overview#comparison-table](https://allocations.access-ci.org/prepare-requests-overview#comparison-table). 

3. Once your allocation is approved, review the ACCESS HPC computing partners on the [Available Resources](https://allocations.access-ci.org/resources) page. Note that each institutions will have different service units (SUs) conversions, so make sure to check before committing service units (SUs) to a given cluster partner.

4. Fill out the following ACCESS questionaire to determine which ACCESS computer partner to use based on your compute requirements:
https://access-ara.ccs.uky.edu:8080

If you have issues filling this out, feel free to submit an [HPC support ticket](https://helpcenter.mines.edu/TDClient/1946/Portal/Requests/ServiceDet?ID=52356). We summarize the ACCESS partners below:

5. Exchange your SUs to the desired HPC cluster by going to "Manage allocations" -> "Manage my projects" -> "New action" on the desired project -> "Exchange". Then choose the allocation resource partner you'd like to use by inputting how many SUs you would like to be credited to that partner. 


 

6. Go to that partner's HPC cluster page for assistance with logging in the first time.
7. Create a ticket at the [RC TDX Request page](https://helpcenter.mines.edu/TDClient/1946/Portal/Requests/ServiceDet?ID=52356) for assistance on building your software or other support on the chosen cluster. ACCESS also provides [support](https://support.access-ci.org/help-ticket) on their website, if that is preferred.

For more details on the ACCESS program, please visit the [FAQs](https://allocations.access-ci.org/ramps-policies-faqs) page on their website.

### RMACC Computing options at CU Boulder's Alpine System

Mines is a member of the [Rocky Mountain Advanced Compuing Consortium (RMACC)](https://rmacc.org/), which gives researchers access to CU Boulder's Alpine HPC cluster system. For more details on Alpine, please see their [overview](https://curc.readthedocs.io/en/latest/clusters/alpine/index.html) page. Furthermore, please see the [RMACC users](https://curc.readthedocs.io/en/latest/access/rmacc.html) page for specifics on how to gain access. 

#### Alpine Features
- Large AMD CPU nodes
- Large memory capacity nodes
- NVIDIA GPU node support
- User Support from either RC or CU Boulder staff
- Interactive/graphical user interface support through Open OnDemand

Although Alpine is free to use for RMACC use, there are some limitations. First, all jobs (including non-RMACC users) have shorter job time limits and other resource limits, which may be too restrictive for some workflows. For more details, see the [Alpine Hardware](https://curc.readthedocs.io/en/latest/clusters/alpine/alpine-hardware.html) page. Second, although all RC support staff have accounts on Alpine, our support will be more limited compared to our on-site offers. That said, we are still able to help support where we can if you submit a ticket to us for an Alpine-related issue. Finally, Alpine for RMACC users is currently only accessible through their web portal powered by [Open OnDemand](https://ondemand-rmacc.rc.colorado.edu/). In practice, this means that file transfer methods such as `scp` are currently not supported for RMACC users and file transfers will have to be done using the Open OnDemand file manager tool. 

### Mio

Mio is a *legacy* HPC system that is no longer open to *new* principle investigators (PIs) for research. Existing PI's still have access to their nodes and are able to request new users in their research group access to Mio. PI's whose nodes were retired due to age, are grandfathered onto Mio and still able to submit jobs to non-owned nodes. It is expected to be retired before the end of 2025.


