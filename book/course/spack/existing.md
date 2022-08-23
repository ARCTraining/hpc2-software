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

<details>
<summary>Full spec</summary>

```
Input spec
--------------------------------
mpiwrapper

Concretized
--------------------------------
mpiwrapper@2.8.1%gcc@8.5.0~ipo build_type=RelWithDebInfo arch=linux-rhel8-haswell
    ^cmake@3.23.3%gcc@8.5.0~doc+ncurses+ownlibs~qt build_type=Release arch=linux-rhel8-haswell
        ^ncurses@6.3%gcc@8.5.0~symlinks+termlib abi=none arch=linux-rhel8-haswell
            ^pkgconf@1.8.0%gcc@8.5.0 arch=linux-rhel8-haswell
        ^openssl@1.1.1q%gcc@8.5.0~docs~shared certs=mozilla patches=3fdcf2d arch=linux-rhel8-haswell
            ^ca-certificates-mozilla@2022-07-19%gcc@8.5.0 arch=linux-rhel8-haswell
            ^perl@5.34.1%gcc@8.5.0+cpanm+shared+threads arch=linux-rhel8-haswell
                ^berkeley-db@18.1.40%gcc@8.5.0+cxx~docs+stl patches=b231fcc arch=linux-rhel8-haswell
                ^bzip2@1.0.8%gcc@8.5.0~debug~pic+shared arch=linux-rhel8-haswell
                    ^diffutils@3.8%gcc@8.5.0 arch=linux-rhel8-haswell
                        ^libiconv@1.16%gcc@8.5.0 libs=shared,static arch=linux-rhel8-haswell
                ^gdbm@1.19%gcc@8.5.0 arch=linux-rhel8-haswell
                    ^readline@8.1.2%gcc@8.5.0 arch=linux-rhel8-haswell
                ^zlib@1.2.12%gcc@8.5.0+optimize+pic+shared patches=0d38234 arch=linux-rhel8-haswell
    ^openmpi@4.1.4%gcc@8.5.0~atomics~cuda~cxx~cxx_exceptions~gpfs~internal-hwloc~java~legacylaunchers~lustre~memchecker+romio+rsh~singularity+static+vt+wrapper-rpath fabrics=none schedulers=none arch=linux-rhel8-haswell
        ^hwloc@2.8.0%gcc@8.5.0~cairo~cuda~gl~libudev+libxml2~netloc~nvml~oneapi-level-zero~opencl+pci~rocm+shared arch=linux-rhel8-haswell
            ^libpciaccess@0.16%gcc@8.5.0 arch=linux-rhel8-haswell
                ^libtool@2.4.7%gcc@8.5.0 arch=linux-rhel8-haswell
                    ^m4@1.4.19%gcc@8.5.0+sigsegv patches=9dc5fbd,bfdffa7 arch=linux-rhel8-haswell
                        ^libsigsegv@2.13%gcc@8.5.0 arch=linux-rhel8-haswell
                ^util-macros@1.19.3%gcc@8.5.0 arch=linux-rhel8-haswell
            ^libxml2@2.9.13%gcc@8.5.0~python arch=linux-rhel8-haswell
                ^xz@5.2.5%gcc@8.5.0~pic libs=shared,static arch=linux-rhel8-haswell
        ^numactl@2.0.14%gcc@8.5.0 patches=4e1d78c,62fc8a8,ff37630 arch=linux-rhel8-haswell
            ^autoconf@2.69%gcc@8.5.0 patches=35c4492,7793209,a49dd5b arch=linux-rhel8-haswell
            ^automake@1.16.5%gcc@8.5.0 arch=linux-rhel8-haswell
        ^openssh@9.0p1%gcc@8.5.0+gssapi arch=linux-rhel8-haswell
            ^krb5@1.19.3%gcc@8.5.0+shared arch=linux-rhel8-haswell
                ^bison@3.8.2%gcc@8.5.0 arch=linux-rhel8-haswell
                ^gettext@0.21%gcc@8.5.0+bzip2+curses+git~libunistring+libxml2+tar+xz arch=linux-rhel8-haswell
                    ^tar@1.34%gcc@8.5.0 zip=pigz arch=linux-rhel8-haswell
                        ^pigz@2.7%gcc@8.5.0 arch=linux-rhel8-haswell
                        ^zstd@1.5.2%gcc@8.5.0+programs compression=none libs=shared,static arch=linux-rhel8-haswell
            ^libedit@3.1-20210216%gcc@8.5.0 arch=linux-rhel8-haswell
        ^pmix@4.1.2%gcc@8.5.0~docs+pmi_backwards_compatibility~restful arch=linux-rhel8-haswell
            ^libevent@2.1.12%gcc@8.5.0+openssl arch=linux-rhel8-haswell
```

</details>

You don't need to read the full spec above, but on my system, this adds up to
36 packages, which I'm sure would take a fair old amount of time to build.  So
let's see if we can cut that down by using software already installed on my
system.

It can find certain software in your environment without you having to
configure anything by hand.  So if I load an openmpi module, and run:

```bash
$ spack external find openmpi
==> The following specs have been detected on this system and added to /home/me/.spack/packages.yaml
-- no arch / gcc@8.5.0 ------------------------------------------
openmpi@4.1.1
```

Spack is now aware of that software, so you can build against it without having
to rebuild openmpi, as long as you're happy with that version.  As you can see
from the output, this is written into your configuration, so will be remembered
for future runs.

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

My full packages.yaml now looks like this, if looked at with `spack config edit packages`:

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
installation up.  Change `/usr/lib64/openmpi` to `/usr` for openmpi.  This
workaround for Red Hat's odd packaging is adequate for this install, but sadly
not for all packages, so you may find you need Spack to build its own OpenMPI
rather than use Red Hat's version in future.

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

## Exercise

A common package to pull from the host system is OpenSSL, as it's almost always
available, involves a fair few dependencies, and is frequently updated for
security.  This last point makes it quite appealing to link against, as it
means operating security updates will keep Spack installed software secure as
far as OpenSSL libraries are concerned.

Add this as an externally provided piece of software to Spack.

<details>
<summary>Click here to reveal solution</summary>

### Solution

This is actually quite straightforward, since Spack can successfully detect and add this to your `packages.yaml` file:

```bash
$ spack external find openssl
==> The following specs have been detected on this system and added to /home/me/.spack/packages.yaml
openssl@1.1.1k
```

</details>

