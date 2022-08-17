# Containers

Spack provides support for assisting you with building containers that have
Spack install software within.

## Define your container

To define your container, you create a YAML file that describes what you want.
Here's I'm saying I want spack to install gromacs with MPI enabled, and
OpenMPI.  I want it to create the container for Singularity/Apptainer, and I
want it to make sure the libgomp1 package is installed onto the OS.

spack.yaml:

```yaml
spack:
# What I want Spack to install
  specs:
  - gromacs+mpi
  - openmpi

# What I want it to create
  container:
    format: singularity
# Bonus OS packages I'd like installed
    os_packages:
      final:
      - libgomp1
```

## Create a Singularity definition file

This is single command to tell Spack to make a definition file from that yaml
file:

```bash
spack containerize > Singularity.def
```

## Build a container image file

Since we have a definition file, we now tell Singularity to build a container.
This is a fairly large and complex container, and happens to also require that
/proc is bind mounted into the container to make this work, but that won't be
the case with simpler containers:

```bash
singularity build -B /proc gromacs-mpi.sif Singularity.def
```

## Use that container

As a proof of life, let's run gromacs from within that container:

```bash
$ mpirun -np 4 singularity run gromacs-mpi.sif gmx_mpi
INFO:    underlay of /etc/localtime required more than 50 (65) bind mounts
INFO:    underlay of /etc/localtime required more than 50 (65) bind mounts
INFO:    underlay of /etc/localtime required more than 50 (65) bind mounts
INFO:    underlay of /etc/localtime required more than 50 (65) bind mounts
                    :-) GROMACS - gmx_mpi, 2022.2-spack (-:

Executable:   /opt/software/linux-ubuntu18.04-skylake_avx512/gcc-7.5.0/gromacs-2022.2-un4jbsm6hv5x6542ffwjs5h4lscftflw/bin/gmx_mpi
Data prefix:  /opt/software/linux-ubuntu18.04-skylake_avx512/gcc-7.5.0/gromacs-2022.2-un4jbsm6hv5x6542ffwjs5h4lscftflw
Working dir:  /home/me/gromacs-singularity
Command line:
  gmx_mpi

SYNOPSIS

gmx [-[no]h] [-[no]quiet] [-[no]version] [-[no]copyright] [-nice <int>]
    [-[no]backup]
...
```
