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
* HPC0 slides - https://bit.ly/hpc0linux
* HPC1 slides - https://bit.ly/hpc1intro
* Arcdocs - https://arcdocs.leeds.ac.uk/
* Arc website - https://arc.leeds.ac.uk/
* https://en.wikipedia.org/wiki/High-performance_computing

### Types of files (WHERE DOES THIS FIT ???????????????)

There are numerous file types you need to be aware of when using and developing code for HPC.
Below are the key types of files:

* executables - these types of files contain compiled binary code. When opened, the operating system will execute the instructions of the code to run the program.
* Libraries - these files usually contain compiled code, but cannot be directly run (they can also contain data, templates, documentation). They provide interfaces to be accessed from an existing running program. Libraries are useful for sharing common functionality between different programs.
* configuration - these files contain strutured data that programs use to  configure parameters and settings to use during execution.
* scripts and code - these files contain code (instructions) that form (a part of) a program. Generally scripts can be run directly (BASH, python), code files need to be pre compiled to run (C, Java)
* headers - These are a specific type of include file that that contains declarations and interfaces, but do not provide any implementations. They are programmers to create standardised, compatible data structures and code.

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

### Configuration managers
(configuration options, build location)

### Packagers
   
* Testing ?
* Installing ?

## Package Managers

Package managers are used to manage applications, tools and libraries as packages. Packages  are archives (often compressed files like zip) and can contain any combination of software, libraries and their metadata.

Packages managers keep track of the packages (and their versions/varients) available, installed and their dependencies.

  they usually allow the following functionality:

* Searching (for both online repositories and installed)
* Downloading from package repositories.
* installation and removal of packages.
* Upgrading/downgrading package versions.
* Installation and removal of dependancies.

The most commonly used package managers on ARC3/ARC4 are conda and pip.

## Environments

Environments provide different levels of abstraction to run your software and code in, these include:
* Machines (physical and virtual)
* Containers
* Virtual Environments

They are extremely useful for helping make your projects reproducible, maintain compatibility and concistancy for your projects.

### Physical Machines
This refers to your physical devices such as laptops, PCs, phones, tablets.

They utilise an **Operating System** (eg Windows, Mac, Android Linux) to control the hardware resources (CPU, storage, memory) to access data and run software and your code.

### Virtual Machines

```{Note}
ARC3/ARC4 cannot run virtual machines.
```

Virtual machines reffer to applications (and system level hypervisors) that provide a software representation of hardware to run guest machines.

It is possible to create and run multiple guest machines on a single host.

The guest machines can use different virtual hardware to the host phyiscal machine and any operating system that supports the virtual harware.

### Containers

Containers are an Operating System level virtualisation that provides isolated instances.
They utilise the host operating sytem's kernel and access to (usually a limited set of) hardware resources (memory, CPU, storage, input devices).

Containers have less overheard (often no noticable performance loss) due to not needing to emulate any hardware, more flexible and require less storage and configuration.

ARC3 and ARC4 support Singularity and Apptainer (the new sucessor to Singularity) container tools. They support the import and running of Docker containers.

### Virtual Environments

Virtual Environments are environments for managing isolated sets of user software (tools and libraries).
These are usually managed by a tool such as Anaconda or virtualenv.

Each environemt created can have its own set of software and when combined with package management can specify specific (or minimum or maximum) versions of some (or all) applications and libraries.

ARC3 and ARC4 provides Anaconda as the primary tool for managing environments, others (such as virtualenv for python) as available or installable.

### Further reading

* https://en.wikipedia.org/wiki/Virtual_machine
* https://en.wikipedia.org/wiki/OS-level_virtualization
* 
* https://arcdocs.leeds.ac.uk/software/infrastructure/singularity.html
* https://arcdocs.leeds.ac.uk/software/compilers/anaconda.html
* https://carpentries-incubator.github.io/python-intermediate-development/12-virtual-environments/
* [SWD2 Containers](containers)
* [SWD2 Conda Environments](conda.html#conda-environments)