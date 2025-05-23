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
# Additional config for the build process
  config:
    mount proc: yes
```

## Create a Singularity definition file

This is single command to tell Spack to make a definition file from that yaml
file:

```bash
spack containerize > gromacs-openmpi.def
```

## Build a container image file

Since we have a definition file, we now tell Singularity to build a container.
This is a fairly large and complex container, and happens to also require that
/proc is bind mounted into the container to make this work, but that won't be
the case with simpler containers:

```bash
module add apptainer
apptainer build gromacs-openmpi.sif gromacs-openmpi.def
```

## Use that container

As a proof of life, let's run gromacs from within that container:

```bash
$ apptainer run gromacs-openmpi.sif mpirun -np 4 gmx_mpi
                    :-) GROMACS - gmx_mpi, 2024.4-spack (-:

Executable:   /opt/software/linux-zen4/gromacs-2024.4-af5srsrjr7w2alftldvyek2chqu4rtls/bin/gmx_mpi
Data prefix:  /opt/software/linux-zen4/gromacs-2024.4-af5srsrjr7w2alftldvyek2chqu4rtls
Working dir:  /users/example/hpc2
Command line:
  gmx_mpi

SYNOPSIS

gmx [-[no]h] [-[no]quiet] [-[no]version] [-[no]copyright] [-nice <int>]
    [-[no]backup]
...
```

## References
[Spack container docs](https://spack.readthedocs.io/en/latest/containers.html)
