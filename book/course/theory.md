# Theory

## Introduction
### Why 
Open research, reproducibility, easier to support, productivity reinventing the wheel etc.
### Overview of topics
* Fundamentals
* Build Tools
* Package Managers
* Environments

## Fundamentals
### HPC0/HPC1 Recap

HPC systems can be used for:
# High Performance - A few jobs at a time that run on many processes in parallel across CPU's and nodes.
# High Throughput - running many different jobs that use few processes on one node each.
# ARC3 and ARC4 both use a mix of both.

HPC system cosists of nodes, which are computers connected together as one big system.

Most HPC systems, including ARC3/ARC4 use Linux.
All HPC systems use commandline user interfaces (shells), the most common shell used in Linux systems is bash.

They do not have GUIs like Windows or a Mac
The compute nodes are not directly acessible and must be accessed using batch jobs (submitted as job scripts).

Login nodes are provided to setup and configure software, code and data.


The filesytem uses forward slashes and is structured starting with root directory. 

The home directory is your personal directory that you login in to by default.
* General HPC Architecture (Login Nodes, Nodes, GPU)
* Throughtput
* Paralalisation MPI
* Modules
* Jobs (queue, interactive)
* File systems (nobackup / home)
* Common Commands
### Further reading
* HPC0 slides
* HPC1 slides
* Arcdocs (links from previous training)

### Types of files
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
* What are build tools

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
