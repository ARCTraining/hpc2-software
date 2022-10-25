# Containers

## What are containers?

Containers are a bit like VMs, but less contained than that.  It probably helps
to look at a diagram to help understand the limits of containers, compared to
virtual machines:

```{image} ../assets/img/course/containers/Docker-containerized-and-vm-transparent-bg.png
:alt: What is a container, docker.com, CC BY-SA 4.0 <https://creativecommons.org/licenses/by-sa/4.0>, via Wikimedia Commons
:width: 600px
:align: center
```

_docker.com, CC BY-SA 4.0 <https://creativecommons.org/licenses/by-sa/4.0>, via Wikimedia Commons_

The important bit to understand here, is what a container doesn't contain.
Fundamentally, a container doesn't include the base of the operating system,
the kernel.  So if you use a container technology on Linux, and your host
operating system runs Linux with a 4.18 kernel, then that's what everything
inside your container sees as well.  This is quite different to running an
actual virtual machine, where what's seen by running code can be entirely
different to the hypervisor's operating system.  Additionally, with a virtual
machine, you have to choose how much RAM you allocate when you start it up,
unlike a container which can use all of the host system's memory, in the same
way a normal process can.  This lack of total containment also means you get
full access to the network and GPUs of the host.

## Why do I want a container?

One of the problems with running software on different systems is that it can
be hard work recreating the software environment across systems, where you may
be running a different distribution.  By using a container, you can spend the
effort once to create the software environment, and then share that image and
use it on quite different systems.  By also publishing the recipe, you make it
very clear how you created this software environment, making it much easier to
update this in future.

## Can I use Docker on HPC?

No.

That's the short answer.  The longer answer is, you generally shouldn't want
to.  Docker is excellent for running web services in containerised
environments, but doesn't really have a security model that suits HPC, due to
the way it works.  Singularity/Apptainer solve this by allowing you to use
Docker containers, but via a tool that uses an alternative security model,
where containers run with the permissions of the user who is running them.
That makes them perfect for HPC, where we want all your work to run as your
user.

## Singularity vs Apptainer

First there was Singularity, then there was commercialisation that kept an open
source version, then a while after a fork after a slight disagreement, and
Apptainer was born.  Apptainer has been developed from Singularity, and we'll
only be looking at Apptainer in this course, but much of what's listed will
equally apply to Singularity.  Whilst there remains a community edition of
Singularity, we'll only be looking at Apptainer, but have tended to use the
`singularity` command line, which is still provided for backwards
compatibility.  Anywhere we use the `singularity` command, you could instead
use `apptainer`.

## Example of running a container

It's always good to do something simple to start iwht, to prove to yourself
things are working:

```bash
# Load the legacy singularity module
$ module add singularity

# Download a simple container
$ singularity pull docker://godlovedc/lolcow

# Run it
$ singularity run lolcow_latest.sif
INFO:    Using cached SIF image
 _________________________________________
/ This night methinks is but the daylight \
| sick.                                   |
|                                         |
| -- William Shakespeare, "The Merchant   |
\ of Venice"                              /
 -----------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```

It might not look on the face of it that this is very impressive, but we've just:

- pulled down a container from docker hub
- converted it into a SIF image
- run a piece of software, within an Ubuntu container

### What do those steps mean?

#### Pulling a container image down from Docker Hub

Docker Hub is a repository of containers.  Pulling down a container involves
pulling down the pieces of this container image over to your machine.

#### Converting it into a SIF image

Docker uses a container format that involves a number of layers, that add up
together to form your final container image.  Singularity uses a different
format SIF, which is a single file that contains everything you need to run
your container.

#### Running software within a container

When you come to use a container, you can run software inside the container,
but also see file shares outside the container, and access resources like
network and GPU of the host machine.  This allows us to use all the power of an
HPC, but with a software environment we've brought with us from elsewhere.  In
the example we just used `run` which executes a predefined piece of software
within the container, but you are not limited to only running the command set
when the container was created.

Note that you normally have read only access to the container, and only have
write access to directories of the host mapped into the container.

## What else can I do with containers?

### Access host storage within a container

By default with Apptainer, your home directory and /tmp are mapped into the
container.  On ARC4 we also default to mapping /nobackup and /local inside the
container.  If you want other directories to be mapped, you have to ask for
them.  If you wanted /data to be visible inside the container, you can just do:

```bash
$ singularity run -B /data example.sif
```

Or if you wanted to make something visible somewhere else (so /data outside
is presented within the container as /scratch):

```bash
$ singularity run -B /data:scratch example.sif
```

### Run an alternative command within a container

```bash
$ singularity exec example.sif cat /etc/issue
Ubuntu 16.04.3 LTS \n \l
```

### Have an interactive shell inside the container

```bash
$ singularity shell example.sif
```

### Use GPUs within a container

Singularity has a lovely way of pulling drivers and libraries for the GPU from
the host system into the container.  In this way we can have a container that
supports GPUs without the pain of having to have all the right drivers included
within it.  At runtime you just do:

```bash
$ singularity run --nv example.sif
```

All available Nvidia GPUs are then visible to the container, along with the
necessary drivers to use them.  If you need a CUDA SDK or similar, that would
need to be included within the container.

## Building a container

Now, what if an existing Docker container doesn't exist, and I want to build
myself a container?  

With Singularity, you needed root access, or a special tweak made to allow you
to build containers.  Neither of those were an option on the HPC, but are if
you have access to either a Linux desktop, or a virtual machine.

Apptainer makes things more interesting, as it supports a mode of operation
that removes the need for additional rights, and so we're looking to roll this
out universally in the near future.  If you have access to a university RHEL 8
system, we should be able to enable Apptainer on your machine now, if it's not
already available.

This is also now available on ARC4.

### Creating a recipe

A recipe lists all the steps needed to make a container, and also what happens
when we run a container.  I'm starting with a fairly silly little example.
Let's create a file called lolcow.def:

```
BootStrap: docker
From: ubuntu:16.04

%post
  apt-get -y update
  apt-get -y install fortune cowsay lolcat

%environment
  export LC_ALL=C
  export PATH=/usr/games:$PATH

%runscript
  fortune | cowsay | lolcat
```

This container starts with a Docker container (Ubuntu 16.04), and installs a few necessary packages.  The it defines what happens when we run it: `fortune | cowsay | lolcat`.

### Converting a Dockerfile to a Singularity recipe

There's a tool written in Python that allows you to convert from a Dockerfile into a Singularity format file.  Whilst not perfect, this often allows for simple creations of images, where no prebuilt Docker image exists:

```bash
# Load anaconda
$ module add anaconda

# Install spython (only need to do this once)
$ pip install spython --user

# Download an example Dockerfile
$ wget https://raw.githubusercontent.com/GodloveD/lolcow/master/Dockerfile

# Convert it into Singularity format
$ spython recipe Dockerfile lolcow.def
```

Then you could proceed to build it into a SIF file as show below.

### Generate a SIF image from a recipe

```bash
$ singularity build lolcow.sif lolcow.def
```

### Test the image we've created

```
$ singularity run lolcow.sif
/ Q: How did you get into artificial   \
| intelligence? A: Seemed logical -- I |
\ didn't have any real intelligence.   /
 --------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```

## Submitting jobs with containers

There's no great difference with submitting jobs to use containers than there
is using them interactively.  Here's an example job submission:

```
# Run with the current working directory
#$ -cwd
# Run for up to ten minutes
#$ -l h_rt=0:10:0
# Request 8 cores
#$ -pe smp 8

# Load the necessary module
module add apptainer

# Run the default command in the container
singularity run lolcow_latest.sif

# Run a custom command inside the container
singularity exec lolcow_latest.sif fortune
```

## Bonus section

There's more content we'd like to include than we've really got time for, but
some bonus content is available here.

````{admonition} Sandboxes
:class: dropdown

When writing a recipe you may find it hard to come up with a working recipe first time.  This is where sandboxes can come in handy.  If you create a sandbox, rather than creating an image file, it writes out the files into a directory, and you can then have a container run in this directory, but with write access, which you do not normally have.  So you write your basic recipe, and build the container like this:

```bash
$ singularity build --sandbox lolcow lolcow.def
```

Now we can have a session within it:

```bash
$ singularity shell --fakeroot --writable lolcow
```

You can now experiment inside this sandbox to work out what you need for your recipe.
````

## References

[Apptainer Documentation](https://apptainer.org/docs/user/main/)

## Summary

```{important}
- [What are containers](#what-are-containers)?
- [Why do I want a container](#why-do-i-want-a-container)?
- [Can I use Docker on HPC](#can-i-use-docker-on-hpc)?
- [Singularity vs Apptainer](#singularity-vs-apptainer)
- [Example of running a container](#example-of-running-a-container) and a look into the steps involved:
    - [Pulling a container image down from Docker Hub](#pulling-a-container-down-from-docker-hub)
    - [Converting it into a SIF image](#converting-it-into-a-sif-image)
    - [Running software within a container](#running-software-within-a-container)
- What else can I do with containers?
  - [Access host storage within a container](#access-host-storage-within-a-container)
  - [Run an alternative command within a container](#run-an-alternative-command-within-a-container)
  - [Have an interactive shell inside the container](#have-an-interactive-shell-inside-the-container)
  - [Use GPUs within a container](#use-gpus-within-a-container)
- Building a container
  - [Creating a recipe](#creating-a-recipe)
  - [Converting a Dockerfile to a Singularity recipe](#converting-a-dockerfile-to-a-singularity-recipe)
  - [Generate a SIF image from a recipe](#generate-a-sif-image-from-a-recipe)
  - [Test the image we've created](#test-the-image-we-ve-created)
- [Submitting jobs with containers](#submitting-jobs-with-containers)
- Bonus section
  - [Sandboxes](#sandboxes)
```
