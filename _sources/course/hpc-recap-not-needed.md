## HPC Systems

### HPC0/HPC1 Recap

HPC (High Performance Computers) systems can be used for:

* **High Performance** - A few jobs at a time that run on many processes in parallel across multiple CPUs and nodes.
* **High Throughput** - running many different jobs that use few processes on one node each.
* ARC3 and ARC4 both use a mix of both.

HPC system consist of nodes, which are computers connected together as one big system.

Each node in ARC3/ARC4 has multiple processors, each of which have multiple cores, allowing many processes to run in parallel.

```{note}
Software and code usually have to be specifically configured to run across multiple processors and typically use MPI to run across multiple nodes at once.
```

Most HPC systems, including ARC3/ARC4, use Linux.
HPC systems do not have GUIs like Windows or Apple systems and they are shared systems with monitored and controlled resources (CPU, memory, time, storage)
All HPC systems use a command line user interface (shell); the most common shell used in Linux systems is Bash.

Login nodes are provided to setup and configure software, code and data.

The compute nodes are not directly acessible and must be accessed using jobs (submitted as job scripts) for batch processing (interactive sessions are avaiable).

All jobs join a queue which is managed by a job scheduler system. The scheduler prioritises and best fits jobs  to run across all the nodes/processors/cores to maximise resource usage and provide prioritised fairness.

HPC systems can provide a mixture of different specification nodes. ARC3/ARC4 have high memory nodes and GPU nodes available, in addition to the standard nodes.

The filesytem on Linux uses forward slashes and is structured starting with the root directory `/`.

The home directory is your personal directory, where you find yourself located when you login by default.

ARC3/ARC4 provide /nobackup as temporary high speed storage to use for your jobs.

Most HPC systems provides a useful set of standard Linux commands to manage and manipulate files and directories.

In addition most HPC systems provide module systems to load and manage software and libraries, and also tools to manage and submit jobs.

```{note}
HPC0 and HPC1 provide an introduction to commands such as cd (change directory) sort, cut, wc (word count), tail, for loops, qsub (submit job), qstat (job status).
```

### Further reading

* HPC0 slides - <https://bit.ly/hpc0linux>
* HPC1 slides - <https://bit.ly/hpc1intro>
* Arcdocs - <https://arcdocs.leeds.ac.uk/>
* Arc website - <https://arc.leeds.ac.uk/>
* High-performance computing - <https://en.wikipedia.org/wiki/High-performance_computing>