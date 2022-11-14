# Theory

## Introduction
### Why 

HPC systems are used to provide lots of fast computing resource that is available 24/7 and managed.

This lesson provides background on HPC systems and the software and tools that are commonly used on the Universities HPC systems.

### Overview of topics
* HPC Systems
* Build Tools
* Package Managers
* Environments

## HPC Systems
### HPC0/HPC1 Recap

HPC (High Performance Computers) systems can be used for:
* **High Performance** - A few jobs at a time that run on many processes in parallel across CPU's and nodes.
* **High Throughput** - running many different jobs that use few processes on one node each.
* ARC3 and ARC4 both use a mix of both.

HPC system cosists of nodes, which are computers connected together as one big system.

Each node in ARC3/ARC4 has multple processors which have multiple cores, allowing many processes to run in parallel.

Note: Software and code usually has to be specifically configured to run multiple processors and use MPI to run across multiple nodes at once.

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

Linux provides a full set of standard commands are used to manage and manipulate files and directories.
In addition HPC systems provide module systems to load and manage software and libraries and also tools to manage and submit jobs.

Note: HPC0 HPC1 provide introduction to commands such as cd (change directory) sort, cut, wc (word count), tail, for loops, qsub (submitted jobs), qstat (job status).

### Further reading
* HPC0 slides - https://bit.ly/hpc0linux
* HPC1 slides - https://bit.ly/hpc1intro
* Arcdocs - https://arcdocs.leeds.ac.uk/
* Arc website - https://arc.leeds.ac.uk/
* https://en.wikipedia.org/wiki/High-performance_computing

### Types of files (WHERE DOES THIS FIT ???????????????)
#### Intro
#### Example
* Libraries
* executables
* configuration files
* scripts
* include
* headers

## 	Build tools
### Intro
Build tools is a generic term for any programs and tools used to build software. Building software can include steps such as configuration, compiling, testing, packaging.

### Examples
autotools (*nix build system), cmake(build file generator for different build environments))
### Compilers and interpreters

A compiler is a program which translates source code written in a programming language into target code. This is typically into machine code to create an executable program.

An interpreter compiles and executes source code directly. They generall runs code slower than a compiled executable as it has to compile code every time the code is run, but there are optimisations such as bytecode compilation and caching that can vastly improve performance.

Some languages are capable of being compiled or run through an interpreter.
#### Examples
* gnu
* intel
* mpi
* cpython

* Configuration -scripts,  mangers ( (configuration options, build location)
* Packagers
* Testing ?
* Installing ?

## Package Managers
### Examples
* conda
* pip
* spack
### Usage
* searching
* downloads
* installs/removes
* upgrades/downgrades
* dependancies
* versions
* varients

## Environments
###	Different levels of abstraction
* Machines (physical and virtual)
* Containers
* Virtual Environments

### Virtual Machines
#### What are Virtual machines
* HyperV
* KVM
#### Examples
* HyperV
* Quemu
* Virtual Box
* WSL2

### Containers
#### What are containers
#### Examples
* Docker
* Apptainer/Singularity

### Virtual Environments
#### What are Virtual Enviroments
#### Examples
* conda
* pyenv
* spack ??
