# Frequently Asked Questions

## What is HPC?
From the first reference given below: "High Performance Computing most generally refers to the practice of aggregating computing power in a way that delivers much higher performance than one could get out of a typical desktop computer or workstation in order to solve large problems in science, engineering, or business."

A high performance computer, sometimes called a supercomputer or a parallel computer is one that has access to some number of computing resources, in particular processors. The computing resources are used together to solve a problem. The resources are connected together by some network or bus and they work together by passing messages over the network.

The individual resource could be a simple collection of processors similar to what you might have in a laptop computer. Another type of resource that is becoming popular are GPUs or graphic processor units. Thes are similar to the chips that drive a lot of video games.

The basic idea behind HPC is if you have a problem that takes 64 hours on a single computer than why not use 64 computers and run it in 1 hour. Or you may have a problem that does not fit on a single computer so you could split it across several.

You may hear of a supercomputer being called a parallel computer. They are parallel because the the processors work together in parallel. Parallel computing can be thought of computing by committee, with all the same advantages and disadvantages. Each processor (committee member) works on a section of the problem. If the processors all have about the same amount of work to do the committee approach may work well. If there is too much communication (too many committee meetings) and/or if, say, one processor is lagging behind the others the calculation will be slowed.

An important point: the individual resources in a supercomputer might not be any more powerful than your laptop. What makes them fast is using many such resources together. So if you have software that runs on your laptop moving it to a supercomputer it might not run any faster unless it is rewritten to take advantage of multiple resources.

REFERENCES:
[What is high performance computing - insideHPC](http://insidehpc.com/hpc-basic-training/what-is-hpc/)

[Overview of High Performance Computing](https://www.mines.edu/ciarc/wp-content/uploads/sites/310/2019/03/HPCOverviewTK.pdf)


## How do I acknowledge usage of HPC@Mines resources?
To acknowledge your use of Mines HPC resources, we request that you use the following wording:
>> The authors acknowledge Colorado School of Mines supercomputing resources from the Research Computing Group (http://rc.mines.edu) made available for conducting the research reported in this paper.

## I'd like to install a Python library. How do I do that?

Please follow our [Python Enivronments](./user_guides/python_environments.md) guide to learn how to use Anaconda environments to install Python libraries you need for your research.
