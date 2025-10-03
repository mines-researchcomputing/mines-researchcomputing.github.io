# Case Study: Researcher using Open Source Finite Element Code FEniCS for modeling reacting flows

## 1. Identify Software Usage

Researcher wants to use FEniCS 2019.1, an open source Finite Element software package. No restrictions on licensing/usage. Python/C++ code which requires compilation from source.

Website: [https://fenicsproject.org/download/archive/](FEniCS Project)

User also may write their own extensions/modules that work with the FEniCS 2019.1 base. This will require the Python tool Cython, an open source package available with [pypi](https://pypi.org/project/Cython).

Science Domain: Computational Fluid Dynamics; Mathematical Biology

## 2. Identify workload and workflows

FEniCS 2019.1 is primarily a code based on using the message passing interface (MPI) library, using domain decomposition to break up the computational domain into chunks that can be solved on separate MPI tasks. MPI communication is then used to share information between tasks for updates on the quantities of interest between domain chunks. Depending on the size of the computational domain, measured by the number of cells used to discretize the domain, the problem can scale up to many nodes and/or require high memory.   


## 3. Find published benchmark (little to no experience with software) or manual benchmarking (established codebase or simulation input deck)

FEniCS has many published papers regarding its construction and performance.

### Legacy FEniCS

M. S. Alnaes, J. Blechta, J. Hake, A. Johansson, B. Kehlet, A. Logg, C. Richardson, J. Ring, M. E. Rognes and G. N. Wells. The FEniCS Project Version 1.5, *Archive of Numerical Software* 3 (2015). [[doi.org/10.11588/ans.2015.100.20553](https://doi.org/10.11588/ans.2015.100.20553)]

A. Logg, K.-A. Mardal, G. N. Wells et al. Automated Solution of Differential Equations by the Finite Element Method, , Springer(2012). [[doi.org/10.1007/978-3-642-23099-8](https://doi.org/10.1007/978-3-642-23099-8)]

### DOLFIN

A. Logg and G. N. Wells. DOLFIN: Automated Finite Element Computing, *ACM Transactions on Mathematical Software* 37 (2010). [[arΧiv](http://arxiv.org/abs/1103.6248)] [[doi.org/10.1145/1731022.1731030](https://doi.org/10.1145/1731022.1731030)]

A. Logg, G. N. Wells and J. Hake. DOLFIN: a C++/Python Finite Element Library, in: A. Logg, K.-A. Mardal and G. N. Wells (eds) Automated Solution of Differential Equations by the Finite Element Method (chapter 10), volume 84 of *Lecture Notes in Computational Science and Engineering*, Springer (2012).

### FFC

R. C. Kirby and A. Logg. A Compiler for Variational Forms, *ACM Transactions on Mathematical Software* 32 (2006). [[arΧiv](http://arxiv.org/abs/1112.0402)] [[doi.org/10.1145/1163641.1163644](https://doi.org/10.1145/1163641.1163644)]

A. Logg, K. B. Ølgaard, M. E. Rognes and G. N. Wells. FFC: the FEniCS Form Compiler, in: A. Logg, K.-A. Mardal and G. N. Wells (eds) Automated Solution of Differential Equations by the Finite Element Method (chapter 11), volume 84 of *Lecture Notes in Computational Science and Engineering*, Springer (2012).

K. B. Ølgaard and G. N. Wells. Optimisations for Quadrature Representations of Finite Element Tensors Through Automated Code Generation, *ACM Transactions on Mathematical Software* 37 (2010). [[arΧiv](http://arxiv.org/abs/1104.0199)] [[doi.org/10.1145/1644001.1644009](https://doi.org/10.1145/1644001.1644009)]

### FIAT

R. C. Kirby. Algorithm 839: FIAT, a New Paradigm for Computing Finite Element Basis Functions, *ACM Transactions on Mathematical Software* 30 (2004) 502--516. [[doi.org/10.1145/1039813.1039820](https://doi.org/10.1145/1039813.1039820)]

R. C. Kirby. FIAT: Numerical Construction of Finite Element Basis Functions, in: A. Logg, K.-A. Mardal and G. N. Wells (eds) Automated Solution of Differential Equations by the Finite Element Method (chapter 13), volume 84 of *Lecture Notes in Computational Science and Engineering*, Springer (2012).

## 4. Run test problem across multiple hardware configurations

Initial testing of a 2D [Poisson equation](https://fenicsproject.org/pub/tutorial/sphinx1/._ftut1003.html) and [2D Heat Equation](https://fenicsproject.org/pub/tutorial/html/._ftut1006.html) conducted on 8-core Intel 4th-generation x86 based workstation. Problem shows initial scalablity up the 8 cores on the local machine for a sufficient problem size.


## 5. Identify Pre-Funded HPC options 

User has access to the legacy Mio legacy HPC, but would like to secure funding to scale problem to larger compute nodes. Initial work can be continued on Mio HPC. 

## 6. Perform baseline parallel and efficiency analysis on Pre-Funded HPC choice
Initial testing on Mio suggests that the user's problem size scales to about 1 node (36 cores), with a runtime of about 5 days. This translates to about 4320 core-hours per job. The user plans to run about 100 jobs a month related to this research. This translates to 43,200 jobs per month. On Wendian's $0.02 core-hr rate, this would be about $864 per month or $10,368 per year.

## 7. Secure Funding

User secures funding from [private entity] with line item for HPC usage at a $10,000 per fiscal year budget.

### 8. Secure allocation on paid/NSF cluster, or fund AWS HPC

User decides to work on Wendian HPC with budget, and plans to explore AWS HPC options for cloud bursting workflows in the future.
