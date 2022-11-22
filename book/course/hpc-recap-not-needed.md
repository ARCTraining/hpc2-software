## HPC Systems (split into HPC and Linux ???)

### HPC0/HPC1 Recap (TOO WORDY !!)

HPC (High Performance Computers) systems can be used for:

* **High Performance** - A few jobs at a time that run on many processes in parallel across CPU's and nodes.
* **High Throughput** - running many different jobs that use few processes on one node each.
* ARC3 and ARC4 both use a mix of both.

HPC system cosists of nodes, which are computers connected together as one big system.

Each node in ARC3/ARC4 has multple processors which have multiple cores, allowing many processes to run in parallel.

```{note}
Software and code usually has to be specifically configured to run multiple processors and use MPI to run across multiple nodes at once.
```

Most HPC systems, including ARC3/ARC4, use Linux.
HPC systems do not have GUIs like Windows or a Mac and they are shared systems with monitored and controlled resources (CPU, memory, time, storage)
All HPC systems use a commandline user interfaces (shells), the most common shell used in Linux systems is bash.

Login nodes are provided to setup and configure software, code and data.

The compute nodes are not directly acessible and must be accessed using jobs (submitted as job scripts) for batch processing (interactive sessions are avaiable).

All jobs join a queue which is managed by a job scheduler system. The scheduler prioritises and best fits jobs  to run across all the nodes/processors/cores to maximise resource usage and provide prioritised fairness.

HPC systems can provide a mixture of diferent specification nodes. ARC3/ARC4 have high memory nodes and GPU nodes available.

The filesytem on Linux uses forward slashes and is structured starting with root directory.

The home directory is your personal directory that you login in to by default.

ARC3/ARC4 provide /nobackup as temporary high speed storage to use for your jobs.

Most HPC systems provides a useful set of standard Linux commands to manage and manipulate files and directories.

In addition most HPC systems provide module systems to load and manage software and libraries, and also tools to manage and submit jobs.

```{note}
HPC0 HPC1 provide introduction to commands such as cd (change directory) sort, cut, wc (word count), tail, for loops, qsub (submitted jobs), qstat (job status).
```

### Further reading

* HPC0 slides - <https://bit.ly/hpc0linux>
* HPC1 slides - <https://bit.ly/hpc1intro>
* Arcdocs - <https://arcdocs.leeds.ac.uk/>
* Arc website - <https://arc.leeds.ac.uk/>
* <https://en.wikipedia.org/wiki/High-performance_computing>