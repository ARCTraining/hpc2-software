# Using existing software

Spack by default will quite happily just build everything itself, as long as a
limited number of [system requirements](https://spack.readthedocs.io/en/latest/getting_started.html#system-prerequisites)
are satisfied.  But you might want to use a compiler and MPI library from a
host system.  This is often the case if packages take a long time to build, or
if you've got access to optimised versions of software that you don't want to
replicate yourself such as on ARC3 and ARC4.

In these circumstances, it's useful to be able to tell Spack about existing
software, so it can use those instead of rebuilding them.  How much you tell
Spack about your host system, and the installed software is up to you.  On one
hand you can massively reduce how much software you need to build yourself, but
on the other hand, you become much more dependent on the host system.  There's
not one right answer.

## Compilers

Compilers are a bit different to other pieces of software.  Typically the
easiest way to get Spack to use a system compiler is to make sure it's in your
path and tell Spack to try to find it.  In this example I'm going to swap out
the default Intel compiler on ARC4 for a shiny GNU compiler:

```bash
$ module swap intel gnu/12.3.0
$ spack compiler add
==> Added 1 new compiler to /home/home02/me/.spack/linux/compilers.yaml
    gcc@12.3.0
==> Compilers are defined in the following files:
    /home/home02/me/.spack/linux/compilers.yaml
```

Spack now knows about this compiler.  If this was a compiler in a module, you
can just load the module beforehand, and Spack will add that compiler to its
known list.

If you want to set that as your preferred compiler, you can do this with a
single command:

```bash
spack config add packages:all:compiler:[gcc@12.3.0]
```

You can confirm your config, or make other changes with:

```bash
spack config add packages:all:compiler:[gcc@12.3.0]
```

```yaml
packages:
  all:
    compiler: [gcc@12.3.0]
```

## Other software

Take an example of building the software phylobayesmpi.  It only needs mpi
as a dependency, but mpi requires a lot of dependencies to be built.
You can see what Spack would like to build:

```bash
spack spec phylobayesmpi
```

<details>
<summary>Full spec</summary>

```
Input spec
--------------------------------
phylobayesmpi

Concretized
--------------------------------
phylobayesmpi@1.8b%gcc@12.3.0 build_system=makefile arch=linux-centos7-skylake_avx512
    ^openmpi@4.1.5%gcc@12.3.0~atomics~cuda~cxx~cxx_exceptions~gpfs~internal-hwloc~java~legacylaunchers~lustre~memchecker~orterunprefix+romio+rsh~singularity+static+vt+wrapper-rpath build_system=autotools fabrics=none schedulers=none arch=linux-centos7-skylake_avx512
        ^hwloc@2.9.1%gcc@12.3.0~cairo~cuda~gl~libudev+libxml2~netloc~nvml~oneapi-level-zero~opencl+pci~rocm build_system=autotools libs=shared,static arch=linux-centos7-skylake_avx512
            ^libpciaccess@0.17%gcc@12.3.0 build_system=autotools arch=linux-centos7-skylake_avx512
                ^util-macros@1.19.3%gcc@12.3.0 build_system=autotools arch=linux-centos7-skylake_avx512
            ^libxml2@2.10.3%gcc@12.3.0~python build_system=autotools arch=linux-centos7-skylake_avx512
                ^libiconv@1.17%gcc@12.3.0 build_system=autotools libs=shared,static arch=linux-centos7-skylake_avx512
                ^xz@5.4.1%gcc@12.3.0~pic build_system=autotools libs=shared,static arch=linux-centos7-skylake_avx512
            ^ncurses@6.4%gcc@12.3.0~symlinks+termlib abi=none build_system=autotools arch=linux-centos7-skylake_avx512
        ^numactl@2.0.14%gcc@12.3.0 build_system=autotools patches=4e1d78c,62fc8a8,ff37630 arch=linux-centos7-skylake_avx512
            ^autoconf@2.69%gcc@12.3.0 build_system=autotools patches=35c4492,7793209,a49dd5b arch=linux-centos7-skylake_avx512
            ^automake@1.16.5%gcc@12.3.0 build_system=autotools arch=linux-centos7-skylake_avx512
            ^libtool@2.4.7%gcc@12.3.0 build_system=autotools arch=linux-centos7-skylake_avx512
            ^m4@1.4.19%gcc@12.3.0+sigsegv build_system=autotools patches=9dc5fbd,bfdffa7 arch=linux-centos7-skylake_avx512
                ^diffutils@3.9%gcc@12.3.0 build_system=autotools arch=linux-centos7-skylake_avx512
                ^libsigsegv@2.14%gcc@12.3.0 build_system=autotools arch=linux-centos7-skylake_avx512
        ^openssh@9.3p1%gcc@12.3.0+gssapi build_system=autotools arch=linux-centos7-skylake_avx512
            ^krb5@1.20.1%gcc@12.3.0+shared build_system=autotools arch=linux-centos7-skylake_avx512
                ^bison@3.8.2%gcc@12.3.0 build_system=autotools arch=linux-centos7-skylake_avx512
                ^gettext@0.21.1%gcc@12.3.0+bzip2+curses+git~libunistring+libxml2+tar+xz build_system=autotools arch=linux-centos7-skylake_avx512
                    ^tar@1.34%gcc@12.3.0 build_system=autotools zip=pigz arch=linux-centos7-skylake_avx512
                        ^pigz@2.7%gcc@12.3.0 build_system=makefile arch=linux-centos7-skylake_avx512
                        ^zstd@1.5.5%gcc@12.3.0+programs build_system=makefile compression=none libs=shared,static arch=linux-centos7-skylake_avx512
            ^libedit@3.1-20210216%gcc@12.3.0 build_system=autotools arch=linux-centos7-skylake_avx512
            ^libxcrypt@4.4.33%gcc@12.3.0~obsolete_api build_system=autotools arch=linux-centos7-skylake_avx512
            ^openssl@1.1.1t%gcc@12.3.0~docs~shared build_system=generic certs=mozilla arch=linux-centos7-skylake_avx512
                ^ca-certificates-mozilla@2023-01-10%gcc@12.3.0 build_system=generic arch=linux-centos7-skylake_avx512
        ^perl@5.36.0%gcc@12.3.0+cpanm+open+shared+threads build_system=generic arch=linux-centos7-skylake_avx512
            ^berkeley-db@18.1.40%gcc@12.3.0+cxx~docs+stl build_system=autotools patches=26090f4,b231fcc arch=linux-centos7-skylake_avx512
            ^bzip2@1.0.8%gcc@12.3.0~debug~pic+shared build_system=generic arch=linux-centos7-skylake_avx512
            ^gdbm@1.23%gcc@12.3.0 build_system=autotools arch=linux-centos7-skylake_avx512
                ^readline@8.2%gcc@12.3.0 build_system=autotools patches=bbf97f1 arch=linux-centos7-skylake_avx512
        ^pkgconf@1.9.5%gcc@12.3.0 build_system=autotools arch=linux-centos7-skylake_avx512
        ^pmix@4.2.3%gcc@12.3.0~docs+pmi_backwards_compatibility~python~restful build_system=autotools arch=linux-centos7-skylake_avx512
            ^libevent@2.1.12%gcc@12.3.0+openssl build_system=autotools arch=linux-centos7-skylake_avx512
        ^zlib@1.2.13%gcc@12.3.0+optimize+pic+shared build_system=makefile arch=linux-centos7-skylake_avx512
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
==> The following specs have been detected on this system and added to /home/home02/me/.spack/packages.yaml
-- no arch / gcc@12.3.0 -----------------------------------------
openmpi@3.1.4
```

Spack is now aware of that software, so you can build against it without having
to rebuild openmpi, as long as you're happy with that version.  As you can see
from the output, this is written into your configuration, so will be remembered
for future runs.

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
    - spec: openmpi@3.1.4%gcc@=12.3.0~cuda+cxx~cxx_exceptions~java~memchecker+pmi~static~wrapper-rpath
        schedulers=slurm
      prefix: /apps/developers/libraries/openmpi/3.1.4/1/gnu-12.3.0
  all:
    compiler: [gcc@12.3.0]
```

```{admonition} OpenMPI on Red Hat oddity
There's a whole lot of interesting stuff in there that I've not had to think
about, as Spack has added all the details about the openmpi variant it
discovered.  We'll cover some of this in the advanced building section.
Annoyingly, it got one detail wrong because of how Red Hat split the
installation up.  Change `/usr/lib64/openmpi` to `/usr` for openmpi.  This
workaround for Red Hat's odd packaging is adequate for this install, but sadly
not for all packages, so you may find you need Spack to build its own OpenMPI
rather than use Red Hat's version in future.
```

Having told Spack about the software we want it to use from the system, let's
look again now at building mpiwrapper, and see what that looks like:

```bash
$ spack spec phylobayesmpi
Input spec
--------------------------------
phylobayesmpi

Concretized
--------------------------------
phylobayesmpi@1.8b%gcc@12.3.0 build_system=makefile arch=linux-centos7-skylake_avx512
    ^openmpi@3.1.4%gcc@12.3.0~atomics~cuda+cxx~cxx_exceptions~gpfs~internal-hwloc~java~legacylaunchers~lustre~memchecker~orterunprefix+pmi+romio+rsh~singularity~static+vt~wrapper-rpath build_system=autotools fabrics=none schedulers=slurm arch=linux-centos7-skylake_avx512
```

So now Spack believes that it can reuse my openmpi from the system and just
build the piece of software I've asked for.

```bash
$ spack install phylobayesmpi
[+] /apps/developers/libraries/openmpi/3.1.4/1/gnu-12.3.0 (external openmpi-3.1.4-3b2jktw2wa3tcn7j6axgi4osq3jnajnm)
==> Installing phylobayesmpi-1.8b-c7eirq7oq7nd56dorwlnylmc463f5whm
==> No binary for phylobayesmpi-1.8b-c7eirq7oq7nd56dorwlnylmc463f5whm found: installing from source
==> Fetching https://mirror.spack.io/_source-cache/archive/7f/7ff017bf492c1d8b42bfff3ee8e998ba1c50f4e4b3d9d6125647b91738017324.tar.gz
==> No patches needed for phylobayesmpi
==> phylobayesmpi: Executing phase: 'edit'
==> phylobayesmpi: Executing phase: 'build'
==> phylobayesmpi: Executing phase: 'install'
==> phylobayesmpi: Successfully installed phylobayesmpi-1.8b-c7eirq7oq7nd56dorwlnylmc463f5whm
  Stage: 0.13s.  Edit: 0.00s.  Build: 19.47s.  Install: 0.02s.  Post-install: 0.03s.  Total: 19.76s
[+] /tmp/me/spack/opt/spack/linux-centos7-skylake_avx512/gcc-12.3.0/phylobayesmpi-1.8b-c7eirq7oq7nd56dorwlnylmc463f5whm
```

So we've just install phylobayesmpi with Spack, using dependencies already
installed on the system, in seconds.  Lovely.

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
==> The following specs have been detected on this system and added to /home/home02/me/.spack/packages.yaml
openssl@1.0.2k-fips  openssl@3.1.0
```

Here we find it's detected both the system OpenSSL, along with a version
lurking within Anaconda.  If you found it picked up a package you didn't want
it to use, you can use `spack config edit packages` to adjust this after it's
detected packages.

In this case, you may choose to remove these two lines, to stop it from using the anaconda version:
```yaml
    - spec: openssl@3.1.0 
      prefix: /apps/developers/compilers/anaconda/2023.03/1/default
```
</details>
