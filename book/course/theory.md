# Theory

## Introduction

### Why

This lesson provides supporting information to the tools used to use, build and maintain software on the Universities HPC systems by introducing you to the terminology and technologies that will be covered by further lessons in this course.

### Overview of topics

* Linux ????
* Build Tools
* Package Managers
* Environments (Virtual Environments ???)

### Types of files (WHERE DOES THIS FIT ???????????????)

There are numerous file types you need to be aware of when using and developing code for HPC.
Below are the key types of files:

* executables - these types of files contain compiled binary code. When opened, the operating system will execute the instructions of the code to run the program.
* Libraries - these files usually contain compiled code, but cannot be directly run (they can also contain data, templates, documentation). They provide interfaces to be accessed from an existing running program. Libraries are useful for sharing common functionality between different programs.
* configuration - these files contain strutured data that programs use to  configure parameters and settings to use during execution.
* scripts and code - these files contain code (instructions) that form (a part of) a program. Generally scripts can be run directly (BASH, python), code files need to be pre compiled to run (C, Java)
* headers - These are a specific type of include file that that specify declarations and interfaces, but do not provide any implementations . They allow programmers to create standardised, compatible data structures and reusable code.

## Build tools

Build tools is a generic term for any programs and tools used to build software. Building software can include steps such as configuration, compiling, testing, packaging.

Common buildtools :

* autotools - is a set of build tools to help make portal Unix-like software.
* cmake - is a cross platform build automation tool, to support the creation of configurations for build tools.
* Make - the traditional unix/Linux build tool.

### Compilers

A compiler is a program which translates source code written in a programming language into target code. This is typically into machine code to create an executable program.

ARC3 and ARC3 have the [intel](https://arcdocs.leeds.ac.uk/software/compilers/intel.html) compiler set as default, with the [GNU](https://arcdocs.leeds.ac.uk/software/compilers/gnu.html) and [LLVM](https://arcdocs.leeds.ac.uk/software/compilers/llvm.html) compilers being popular alternatives available on the module system.

#### Interpreters

An interpreter compiles and executes source code directly. They generall runs code slower than a compiled executable as it has to compile code every time the code is run, but there are optimisations such as bytecode compilation and caching that can vastly improve performance.

Python is a popular interpretated language and the most common interpretor is the default, [CPython](https://en.wikipedia.org/wiki/CPython).

Some languages are capable of being compiled or run through an interpreter.

### Build Configuration

Tools like cmake can be used to create different platform specific for different build tools configurations.

Configuration files usually contain locations of the source code, output locations, of existing components (libraries, assets such as help documentation, data).

They can contain options for conditional builds such as for debug and release builds and for additional features to take advantage of optional components  such as input/output format/interfaces or hardware - such as GPUs.

* Packaging ?
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

The most commonly used package managers on ARC3/ARC4 are anaconda and pip.

* [Anaconda](https://arcdocs.leeds.ac.uk/software/compilers/anaconda.html) is a environment and package manager, primarily for python and r.
* [pip](https://en.wikipedia.org/wiki/Pip_(package_manager)) is the package manager for python.

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

* <https://en.wikipedia.org/wiki/Virtual_machine>
* <https://en.wikipedia.org/wiki/OS-level_virtualization>
*
* <https://arcdocs.leeds.ac.uk/software/infrastructure/singularity.html>
* <https://arcdocs.leeds.ac.uk/software/compilers/anaconda.html>
* <https://carpentries-incubator.github.io/python-intermediate-development/12-virtual-environments/>
* [SWD2 Containers](containers)
* [SWD2 Conda Environments](conda.html#conda-environments)
