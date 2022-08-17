# Using existing software

Spack by default will quite happily just build everything itself, as long as a
limited number of [system requirements](https://spack.readthedocs.io/en/latest/getting_started.html#system-prerequisites)
are satisfied.  But you might want to use a compiler and MPI library from a
host system.  This is often the case if packages take a long time to build, or
if you've got access to optimised versions of software that you don't want to
replicate yourself.

In these circumstances, it's useful to be able to tell Spack about existing
software, so it can use those instead of rebuilding them.  How much you tell
Spack about your host system, and the installed software is up to you.  On one
hand you can massively reduce how much software you need to build yourself, but
on the other hand, you become much more dependent on the host system.  There's
not one right answer.

## Compilers

Compilers are a bit different to other pieces of software.  Typically the
easiest way to get Spack to use a system compiler is to make sure it's in your
path, and then tell Spack to find it:

```bash
$ spack compiler add
==> Added 1 new compiler to /home/me/.spack/linux/compilers.yaml
    gcc@8.5.0
==> Compilers are defined in the following files:
    /home/me/.spack/linux/compilers.yaml
```

Spack now knows about this compiler.  If this was a compiler in a module, you
can just load the module beforehand, and Spack will add that compiler to its
known list.

If you want to set that as your preferred compiler, run this to edit the
config:

```bash
spack config edit packages
```

And tell it to use this compiler

```yaml
packages:
  all:
    compiler:
    - 'gcc@8.3.0' # Your preferred compiler here
```

## Other software

Take an example of building the software mpiwrapper.  It only needs cmake and
openmpi as dependencies, but these themselves require lots of dependencies to
built.  You can see what Spack would like to build:

```bash
spack spec mpiwrapper
```

I'll save you all the output, but on my system, this adds up to 35 packages,
which I'm sure would take a fair old amount of time to build.  So let's see if
we can cut that down by using software already installed on my system.

It can find certain software in your environment without you having to
configure anything by hand.  So if I load an openmpi module, and run:

```bash
$ spack external find openmpi
==> The following specs have been detected on this system and added to /home/me/.spack/packages.yaml
-- no arch / gcc@8.5.0 ------------------------------------------
openmpi@4.1.1
```

Spack is now aware of that software, so you can build against it without having
to rebuild openmpi, as long as you're happy with that version.

I also want it to discover cmake:

```bash
$ spack external find cmake
==> The following specs have been detected on this system and added to /home/me/.spack/packages.yaml
cmake@3.20.2
```

If Spack wasn't able to auto detect the software, you can add it to the
packages.yaml by hand.  For example, if I wanted to add a system installed
libjpeg-turbo to spack, I can add a section for that into my packages.yaml:

```yaml
  libjpeg-turbo:
    externals:
    - spec: libjpeg-turbo@1.5.3
      prefix: /usr
```

My full packages.yaml now looks like this:

```yaml
packages:
  openmpi:
    externals:
    - spec: openmpi@4.1.1%gcc@8.5.0+atomics~cuda+cxx~cxx_exceptions+java~memchecker+pmi~static~wrapper-rpath
        fabrics=ofi,psm2,ucx schedulers=slurm
      prefix: /usr/lib64/openmpi
  cmake:
    externals:
    - spec: cmake@3.20.2
      prefix: /usr
  all:
    compiler:
    - gcc@8.3.0   # Your preferred compiler here
  libjpeg-turbo:
    externals:
    - spec: libjpeg-turbo@1.5.3
      prefix: /usr
```

There's a whole lot of interesting stuff in there that I've not had to think
about, as Spack has added all the details about the openmpi variant it
discovered.  We'll cover some of this in the advanced building section.
Annoyingly, it got one detail wrong because of how Red Hat split the
installation up.  Change `/usr/lib64/openmpi` to `/usr` for openmpi.

Having told Spack about the software we want it to use from the system, let's
look again now at building mpiwrapper, and see what that looks like:

```bash
$ spack spec mpiwrapper
Input spec
--------------------------------
mpiwrapper

Concretized
--------------------------------
mpiwrapper@2.8.1%gcc@8.5.0~ipo build_type=RelWithDebInfo arch=linux-rhel8-haswell
    ^cmake@3.20.2%gcc@8.5.0~doc+ncurses+ownlibs~qt build_type=Release arch=linux-rhel8-haswell
    ^openmpi@4.1.1%gcc@8.5.0+atomics~cuda+cxx~cxx_exceptions~gpfs~internal-hwloc+java~legacylaunchers~lustre~memchecker+pmi+romio+rsh~singularity~static+vt~wrapper-rpath fabrics=ofi,psm2,ucx schedulers=slurm arch=linux-rhel8-haswell
```

So now Spack believes that it can reuse my openmpi and cmake from the system,
and just build the piece of software I've asked for.

```bash
$ spack install mpiwrapper
[+] /usr (external cmake-3.20.2-b2vel5wyij5d4svvpdwhyio4dwuxly3a)
==> openmpi@4.1.1 : has external module in ['pi/openmpi-x86_64']
[+] /usr (external openmpi-4.1.1-motfvxmtmqhhzqizmifts2lz5hx5x3t6)
==> Installing mpiwrapper-2.8.1-pbtnwnb75qiwerrf265rod2v3echlogq
==> No binary for mpiwrapper-2.8.1-pbtnwnb75qiwerrf265rod2v3echlogq found: installing from source
==> Fetching https://mirror.spack.io/_source-cache/archive/e6/e6fc1c08ad778675e5b58b91b4658b12e3f985c6d4c5c2c3e9ed35986146780e.tar.gz
==> No patches needed for mpiwrapper
==> mpiwrapper: Executing phase: 'cmake'
==> mpiwrapper: Executing phase: 'build'
==> mpiwrapper: Executing phase: 'install'
==> mpiwrapper: Successfully installed mpiwrapper-2.8.1-pbtnwnb75qiwerrf265rod2v3echlogq
  Fetch: 0.77s.  Build: 9.74s.  Total: 10.52s.
[+] /home/me/spack/opt/spack/linux-rhel8-haswell/gcc-8.5.0/mpiwrapper-2.8.1-pbtnwnb75qiwerrf265rod2v3echlogq
```

So we've just install mpiwrapper with Spack, using dependencies already install
on the system, in seconds.  Lovely.
