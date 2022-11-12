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

#### Examples
autotools (*nix build system), cmake(build file generator for different build environments))
* Compilers (gnu, intel, mpi)
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
