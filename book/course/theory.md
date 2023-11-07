# Theory

## Introduction

This introductory lesson is a small set of computing theory and background information tailored to introduce you to the terminology and technologies that will be covered by further lessons in this course.

It provides supporting information to the tools used to run, build and maintain software on the University's HPC systems.

### Objectives

* Explain different types of files, environment variables and where to run your code.
* Explain about different methods to install and manage software.
* Explain the different steps and supporting tools in building software.
* Cover the different types of platforms you can utilise to build and run software on.

## Files

Files are structures designed to contain information, such as text, executable code, images, video.

Operating systems include tools to read, copy, modify and delete files.

### Types of files

There are numerous file types you need to be aware of when using and developing code for HPC.
Below are the key types of files:

* **executables** - these types of files contain compiled binary code. When opened, the operating system will execute the instructions of the code to run the program.
* **libraries** - In programming, a library contains common functions that can be reused in programs.

```{Note}
The term library is also used in computing for the name of a collection of related files, such as in /usr/share or as a package, eg tensorflow.
```

* **configuration** - these files contain strutured data that programs use to configure parameters and settings to use during execution.
* **scripts and code** - these files contain code (instructions) that form (a part of) a program. Generally scripts can be run directly (Bash, Python), code files need to be compiled to run (C, Java).
* **headers** - These are a specific type of include file that that specify declarations and interfaces, but do not provide any implementations . They allow programmers to create standardised, compatible data structures and reusable code.

### Path environment variables

These variables list common locations of files and are used by the system to lookup requested executables and libraries, without the need to provide the full path, which reduces the amount of configuration that would be required to run the same software on different systems.

* PATH - When you execute a program by giving its name, the shell looks it up in the directories listed in the `PATH` variable e.g. `/home/username/bin:/usr/local/bin:/usr/bin:/bin`
* LD_LIBRARY_PATH - when a program requests a library the dynamic loader will look in the directories listed in the `LD_LIBRARY_PATH`` variable e.g. `/usr/local/lib:/usr/lib:/lib`
* CPATH - when compiling, specifies the search path for header files, particularly when working with C and C++ programming languages.
* PKG_CONFIG_PATH - Where pkgconfig (manages the configuration of compilers and linking libraries) searches for pkg-config files

Installation of, or enabling software may add additional locations to PATH or LD_LIBRARY_PATH variables.
Users can modify these variables for their own user (not system wide), this may need to be done if you are building software yourself.

### Locations for building and running your own code

The best locations for building and running your code are your home directory (or sub directory) where your user usually has full permissions to run and manage files eg  `/home/username/` or other user writable/executable directories such as `/nobackup/username` on ARC3/ARC4.

## Software

### What is software?

* Software is the name given to executable computer programs.
* Software can include one or more executable, library, configuration, data, asset  files.
* Software can be run by the user or automatically by the operating system.
* Multiple pieces of software can be packaged together such as the Anaconda installer or Microsoft Office.

### Installing software

Software usually requires installation before it can be run. This is usually by one of the following methods:

* executable installer
* package file install
* archive with instructions of how to extract and install and run.
* source archives with build instructions and scripts.

```{Note}
It's possible to have an executable program or script that can be run directly without installation.
```

Executable installers, may not include dependencies (other files required to run); these could be provided by default by the operating system or may require installing separately.

### Package Managers

Package managers are used to manage applications, tools and libraries as packages.

Packages are archives (often compressed files like zip) and can contain any combination of software, libraries and their metadata.

Packages are collected and maintained together as distributions (eg conda-forge, pypi).

Package managers keep track of packages and all of their versions/variants, both installed and available to be installed, along with their dependencies.

They usually allow the following functionality:

* Searching (for both online repositories and installed)
* Downloading from package repositories
* Installation and removal of packages
* Upgrading/downgrading package versions
* Installation and removal of dependancies

The most commonly used package managers on ARC3/ARC4 are conda (distributed through Anaconda) and pip.

* [Anaconda](https://arcdocs.leeds.ac.uk/software/compilers/anaconda.html) is an environment and package manager, primarily for Python and R.
* [pip](https://en.wikipedia.org/wiki/Pip_(package_manager)) is a package manager for Python.

## Build tools

Build tools is a generic term for any programs and tools used to build software. Building software can include steps such as configuration, compiling, testing, packaging.

Common buildtools:

* Autotools - is a set of build tools to help make portable Unix-like software.
* CMake - is a cross platform build automation tool, to support the creation of configurations for build tools.
* Make - the traditional Unix/Linux build tool.

### Compilers

A compiler is a program which translates source code written in a programming language into target code. This is typically into machine code to create an executable program.

ARC3 and ARC4 have the [Intel](https://arcdocs.leeds.ac.uk/software/compilers/intel.html) compiler set as default, with the [GNU](https://arcdocs.leeds.ac.uk/software/compilers/gnu.html) and [LLVM](https://arcdocs.leeds.ac.uk/software/compilers/llvm.html) compilers being popular alternatives available on the module system.

#### Interpreters

An interpreter reads and executes source code directly. They generally run code slower than a compiled executable as it has to compile code every time the code is run, but there are optimisations such as bytecode compilation and caching that can vastly improve performance.

Python is a popular interpreted language and the most common interpreter is the default, [CPython](https://en.wikipedia.org/wiki/CPython).

Some languages are capable of being compiled or run through an interpreter.

### Build Configuration

Tools like CMake can be used to create different platform specific for different build tools configurations.

Configuration files usually contain locations of the source code, output locations, of existing components (libraries, assets such as help documentation, data).

They can contain options for conditional builds such as for debug and release builds and for additional features to take advantage of optional components such as input/output format/interfaces or hardware - such as GPUs.

### Packaging

```{Note}
Users do not have sufficient permissions to install system packages on our HPC systems.
Containers provide a mechanism for users to install system packages in a user managed environment.
```

Linux and package distributions usually provide tools to automate many steps of the process of creating software packages from source; this will include steps to compile source code, package the files required and test the package is installable/removable.

### Distribution

There is usually little need to distribute built academic code.

If you want to distribute your source code, you can use online repositories such as [GitHub](https://github.com/). These can be linked into open research repositories like [Zenodo](https://zenodo.org/).

Containers (and their definitions) are a popular way of distributing ready to run environments.

## Platforms

The platforms mentioned below provide different levels of abstraction to run your software and code in.  These include:

* Machines (physical and virtual)
* Containers
* Virtual Environments

They are extremely useful for helping make your projects reproducible, and also maintain compatibility and consistency for your projects.

### Physical Machines

This refers to your physical devices such as laptops, PCs, phones, tablets.

They utilise an **Operating System** (eg Windows, OSX, Android Linux) to control the hardware resources (CPU, storage, memory) to access data and run software and your code.

### Virtual Machines

```{Note}
ARC3/ARC4 cannot run virtual machines.
```

Virtual machines refer to applications (and system level hypervisors) that provide a software representation of hardware to run guest machines.

It is possible to create and run multiple guest machines on a single host.

The guest machines can use different virtual hardware to the host phyiscal machine and any operating system that supports the virtual hardware.

### Containers

Containers are an operating system level virtualisation that provides isolated instances.
They utilise the host operating system's kernel and provide access to (usually a limited set of) hardware resources (memory, CPU, storage, input devices).

Containers have less overhead (often no noticeable performance loss) due to not needing to emulate any hardware, are more flexible and require less storage and configuration.

ARC3 and ARC4 support [Singularity](https://en.wikipedia.org/wiki/Singularity_(software)) and [Apptainer](https://apptainer.org/) (the new successor to Singularity) container tools. They support the import and running of [Docker](https://www.docker.com/) containers.

### Virtual Environments

Virtual Environments are environments for managing isolated sets of user software (tools and libraries).
These are usually managed by a tool such as `conda` or `virtualenv`.

Each environment created can have its own set of software and when combined with package management can specify specific (or minimum or maximum) versions of some (or all) applications and libraries.

ARC3 and ARC4 provides the Anaconda module (that contains `conda` as the primary tool for managing environments, although others (such as virtualenv for Python) are available or installable.

## Summary

```{important}

* Common types of software files are executables, libraries, configuration, scripts and code.
* Path environment variables list common locations of executables and libraries so they can located in a standard method across different systems.
* Users should usually build and run code from user writable/executable directories (eg home directory).
* Software usually requires installation before it can be run.
* Package managers are used to manage and maintain software.
* Build tools can be used to compile, configure and package software.
* Machines (physical and virtual), Containers and Virtual Environments provide different levels of abstraction to run your code in.

```

## Further reading

* Computer files - <https://en.wikipedia.org/wiki/Computer_file>*
* Paths - <https://unix.stackexchange.com/a/45106>
* Home directory - <https://tldp.org/LDP/Linux-Filesystem-Hierarchy/html/home.html>
* Software - <https://en.wikipedia.org/wiki/Software>
* Package managers - <https://en.wikipedia.org/wiki/Package_manager>
* Building Software - <https://en.wikipedia.org/wiki/Software_build>
* Compilers - <https://en.wikipedia.org/wiki/Compiler>
* Virtual Machines - <https://en.wikipedia.org/wiki/Virtual_machine>
* Containers - <https://en.wikipedia.org/wiki/OS-level_virtualization>
* Singularity - <https://arcdocs.leeds.ac.uk/software/infrastructure/singularity.html>
* Anaconda - <https://arcdocs.leeds.ac.uk/software/compilers/anaconda.html>
* Virtual Environments - <https://carpentries-incubator.github.io/python-intermediate-development/12-virtual-environments/>
