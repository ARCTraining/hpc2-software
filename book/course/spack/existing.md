# Using existing software

Spack by default will quite happily just build everything itself, as long as a
limited number of [system requirements](https://spack.readthedocs.io/en/latest/getting_started.html#system-prerequisites)
are satisfied.  But you might want to use a compiler and MPI library from a
host system.  This is often the case if packages take a long time to build, or
if you've got access to optimised versions of software that you don't want to
replicate yourself such as on Aire.

In these circumstances, it's useful to be able to tell Spack about existing
software, so it can use those instead of rebuilding them.  How much you tell
Spack about your host system, and the installed software is up to you.  On one
hand you can massively reduce how much software you need to build yourself, but
on the other hand, you become much more dependent on the host system.  There's
not one right answer.

## Compilers

Compilers are a bit different to other pieces of software.  Typically the
easiest way to get Spack to use a system compiler is to make sure it's in your
path and tell Spack to try to find it.  In this example I'm going to load a
newer GCC compiler than is available by default:

```bash
$ module add gcc/14.2.0
$ spack compiler add
==> Added 1 new compiler to /users/example/spack/etc/spack/packages.yaml
    gcc@14.2.0
==> Compilers are defined in the following files:
    /users/example/spack/etc/spack/packages.yaml
```

Spack now knows about this compiler.

You can confirm your config, or make other changes with:

```bash
spack config edit packages
```

```yaml
packages:
  gcc:
    externals:
    - spec: gcc@14.2.0 languages:='c,c++,fortran'
      prefix: /opt/apps/pkg/compilers/gcc/14.2.0
      extra_attributes:
        compilers:
          c: /opt/apps/pkg/compilers/gcc/14.2.0/bin/gcc
          cxx: /opt/apps/pkg/compilers/gcc/14.2.0/bin/g++
          fortran: /opt/apps/pkg/compilers/gcc/14.2.0/bin/gfortran
    - spec: gcc@11.4.1 languages:='c,c++'
      prefix: /usr
      extra_attributes:
        compilers:
          c: /usr/bin/gcc
          cxx: /usr/bin/g++
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
 -   phylobayesmpi@1.9 build_system=makefile arch=linux-rocky9-zen4 %cxx=gcc@14.2.0
[+]      ^compiler-wrapper@1.0 build_system=generic arch=linux-rocky9-zen4 
[e]      ^gcc@14.2.0~binutils+bootstrap~graphite~mold~nvptx~piclibs~profiled~strip build_system=autotools build_type=RelWithDebInfo languages:='c,c++,fortran' arch=linux-rocky9-zen4 
 -       ^gcc-runtime@14.2.0 build_system=generic arch=linux-rocky9-zen4 
[e]      ^glibc@2.34 build_system=autotools arch=linux-rocky9-zen4 
[+]      ^gmake@4.4.1~guile build_system=generic arch=linux-rocky9-zen4 %c=gcc@11.4.1
[e]          ^gcc@11.4.1~binutils+bootstrap~graphite~nvptx~piclibs~profiled~strip build_system=autotools build_type=RelWithDebInfo languages:='c,c++' arch=linux-rocky9-zen4 
[+]          ^gcc-runtime@11.4.1 build_system=generic arch=linux-rocky9-zen4 
 -       ^openmpi@5.0.8+atomics~cuda~debug+fortran~gpfs~internal-hwloc~internal-libevent~internal-pmix~ipv6~java~lustre~memchecker~openshmem~rocm~romio+rsh~static~two_level_namespace+vt+wrapper-rpath build_system=autotools fabrics:=none romio-filesystem:=none schedulers:=none arch=linux-rocky9-zen4 %c,cxx,fortran=gcc@14.2.0
 -           ^autoconf@2.72 build_system=autotools arch=linux-rocky9-zen4 
 -               ^m4@1.4.20+sigsegv build_system=autotools arch=linux-rocky9-zen4 %c,cxx=gcc@14.2.0
 -                   ^diffutils@3.10 build_system=autotools arch=linux-rocky9-zen4 %c=gcc@14.2.0
 -                   ^libsigsegv@2.14 build_system=autotools arch=linux-rocky9-zen4 %c=gcc@14.2.0
 -           ^automake@1.16.5 build_system=autotools arch=linux-rocky9-zen4 %c=gcc@14.2.0
 -           ^hwloc@2.11.1~cairo~cuda~gl~level_zero~libudev+libxml2~nvml~opencl+pci~rocm build_system=autotools libs:=shared,static arch=linux-rocky9-zen4 %c,cxx=gcc@14.2.0
 -               ^libpciaccess@0.17 build_system=autotools arch=linux-rocky9-zen4 %c=gcc@14.2.0
 -                   ^util-macros@1.20.1 build_system=autotools arch=linux-rocky9-zen4 
 -               ^libxml2@2.13.5~http+pic~python+shared build_system=autotools arch=linux-rocky9-zen4 %c=gcc@14.2.0
 -                   ^libiconv@1.18 build_system=autotools libs:=shared,static arch=linux-rocky9-zen4 %c=gcc@14.2.0
 -                   ^xz@5.6.3~pic build_system=autotools libs:=shared,static arch=linux-rocky9-zen4 %c=gcc@14.2.0
 -               ^ncurses@6.5~symlinks+termlib abi=none build_system=autotools patches:=7a351bc arch=linux-rocky9-zen4 %c,cxx=gcc@14.2.0
 -           ^libevent@2.1.12+openssl build_system=autotools arch=linux-rocky9-zen4 %c=gcc@14.2.0
 -               ^openssl@3.4.1~docs+shared build_system=generic certs=mozilla arch=linux-rocky9-zen4 %c,cxx=gcc@14.2.0
 -                   ^ca-certificates-mozilla@2025-05-20 build_system=generic arch=linux-rocky9-zen4 
 -           ^libtool@2.4.7 build_system=autotools arch=linux-rocky9-zen4 %c=gcc@14.2.0
 -               ^findutils@4.10.0 build_system=autotools patches:=440b954 arch=linux-rocky9-zen4 %c=gcc@14.2.0
 -                   ^gettext@0.23.1+bzip2+curses+git~libunistring+libxml2+pic+shared+tar+xz build_system=autotools arch=linux-rocky9-zen4 %c,cxx=gcc@14.2.0
 -                       ^tar@1.35 build_system=autotools zip=pigz arch=linux-rocky9-zen4 %c=gcc@14.2.0
[+]                          ^pigz@2.8 build_system=makefile arch=linux-rocky9-zen4 %c=gcc@11.4.1
 -                           ^zstd@1.5.7+programs build_system=makefile compression:=none libs:=shared,static arch=linux-rocky9-zen4 %c,cxx=gcc@14.2.0
 -           ^numactl@2.0.18 build_system=autotools arch=linux-rocky9-zen4 %c=gcc@14.2.0
 -           ^openssh@9.9p1+gssapi build_system=autotools arch=linux-rocky9-zen4 %c,cxx=gcc@14.2.0
 -               ^krb5@1.21.3+shared build_system=autotools arch=linux-rocky9-zen4 %c,cxx=gcc@14.2.0
 -                   ^bison@3.8.2~color build_system=autotools arch=linux-rocky9-zen4 %c,cxx=gcc@14.2.0
 -               ^libedit@3.1-20240808 build_system=autotools arch=linux-rocky9-zen4 %c=gcc@14.2.0
 -               ^libxcrypt@4.4.38~obsolete_api build_system=autotools arch=linux-rocky9-zen4 %c=gcc@14.2.0
 -           ^perl@5.40.0+cpanm+opcode+open+shared+threads build_system=generic arch=linux-rocky9-zen4 %c=gcc@14.2.0
 -               ^berkeley-db@18.1.40+cxx~docs+stl build_system=autotools patches:=26090f4,b231fcc arch=linux-rocky9-zen4 %c,cxx=gcc@14.2.0
 -               ^bzip2@1.0.8~debug~pic+shared build_system=generic arch=linux-rocky9-zen4 %c=gcc@14.2.0
 -               ^gdbm@1.23 build_system=autotools arch=linux-rocky9-zen4 %c=gcc@14.2.0
 -                   ^readline@8.2 build_system=autotools patches:=1ea4349,24f587b,3d9885e,5911a5b,622ba38,6c8adf8,758e2ec,79572ee,a177edc,bbf97f1,c7b45ff,e0013d9,e065038 arch=linux-rocky9-zen4 %c=gcc@14.2.0
 -           ^pkgconf@2.3.0 build_system=autotools arch=linux-rocky9-zen4 %c=gcc@14.2.0
 -           ^pmix@5.0.5~munge~python build_system=autotools arch=linux-rocky9-zen4 %c=gcc@14.2.0
[+]          ^zlib-ng@2.2.4+compat+new_strategies+opt+pic+shared build_system=autotools arch=linux-rocky9-zen4 %c,cxx=gcc@11.4.1
```

</details>

You don't need to read the full spec above, but on Aire currently, this adds
up to 37 missing packages, which I'm sure would take a fair old amount of time
to build.  So let's see if we can cut that down by using software already
installed on my system.

It can find certain software in your environment without you having to
configure anything by hand.  So if I load an openmpi module, and run:

```bash
$ module add openmpi/5.0.6/gcc-14.2.0
$ spack external find openmpi
==> The following specs have been detected on this system and added to /users/example/spack/etc/spack/packages.yaml
-- no arch / no compilers ---------------------------------------
openmpi@5.0.6
```

Spack is now aware of that software, so you can build against it without having
to rebuild openmpi, as long as you're happy with that version.  As you can see
from the output, this is written into your configuration, so will be remembered
for future runs.

If Spack wasn't able to auto detect the software, you can add it to the
packages.yaml by hand.  For example, if I wanted to add a system installed
libjpeg-turbo to spack, I could add a section for that into my packages.yaml:

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
    - spec: openmpi@5.0.6~cuda~java~memchecker~rocm~static~wrapper-rpath fabrics=ofi,psm2,ucx schedulers=none
      prefix: /opt/apps/pkg/libraries/openmpi/5.0.6/gcc-14.2.0
  gcc:
    externals:
    - spec: gcc@14.2.0 languages:='c,c++,fortran'
      prefix: /opt/apps/pkg/compilers/gcc/14.2.0
      extra_attributes:
        compilers:
          c: /opt/apps/pkg/compilers/gcc/14.2.0/bin/gcc
          cxx: /opt/apps/pkg/compilers/gcc/14.2.0/bin/g++
          fortran: /opt/apps/pkg/compilers/gcc/14.2.0/bin/gfortran
    - spec: gcc@11.4.1 languages:='c,c++'
      prefix: /usr
      extra_attributes:
        compilers:
          c: /usr/bin/gcc
          cxx: /usr/bin/g++
```

```{admonition} OpenMPI on Red Hat oddity
There's a whole lot of interesting stuff in there that I've not had to think
about, as Spack has added all the details about the openmpi variant it
discovered.  We'll cover some of this in the advanced building section.

If you're using Red Hat's version of OpenMPI, it can get one detail wrong
because of how Red Hat split the installation up.  Change `/usr/lib64/openmpi`
to `/usr` for openmpi.  This workaround for Red Hat's odd packaging is
adequate for this install, but sadly not for all packages, so you may find you
need Spack to build its own OpenMPI rather than use Red Hat's version in
future.
```

Having told Spack about the software we want it to use from the system, let's
look again now at building mpiwrapper, and see what that looks like:

```bash
$ spack spec phylobayesmpi
$ spack spec phylobayesmpi
 -   phylobayesmpi@1.9 build_system=makefile arch=linux-rocky9-zen4 %cxx=gcc@11.4.1
[+]      ^compiler-wrapper@1.0 build_system=generic arch=linux-rocky9-zen4
[e]      ^gcc@11.4.1~binutils+bootstrap~graphite~nvptx~piclibs~profiled~strip build_system=autotools build_type=RelWithDebInfo languages:='c,c++' arch=linux-rocky9-zen4
[+]      ^gcc-runtime@11.4.1 build_system=generic arch=linux-rocky9-zen4
[e]      ^glibc@2.34 build_system=autotools arch=linux-rocky9-zen4
[+]      ^gmake@4.4.1~guile build_system=generic arch=linux-rocky9-zen4 %c=gcc@11.4.1
[e]      ^openmpi@5.0.6+atomics~cuda~debug+fortran~gpfs~internal-hwloc~internal-libevent~internal-pmix~ipv6~java~lustre~memchecker~openshmem~rocm~romio+rsh~static~two_level_namespace+vt~wrapper-rpath build_system=autotools fabrics:=ofi,psm2,ucx romio-filesystem:=none schedulers:=none arch=linux-rocky9-zen4
```

So now Spack believes that it can reuse my openmpi from the system and it's
only actually going to build the piece of software I've asked for, rather than
the huge list of dependencies we had before.

```bash
$ spack install phylobayesmpi
[+] /usr (external glibc-2.34-riltp4wkahjqsanksbtayyu5sz2r2xzk)
[+] /usr (external gcc-11.4.1-jrckedbivv6kljrzsqljlem6z2dm3rge)
[+] /users/example/spack/opt/spack/linux-zen4/compiler-wrapper-1.0-fbl56cgz34aegvfgcruvmp3cpwtbrvat
[+] /opt/apps/pkg/libraries/openmpi/5.0.6/gcc-14.2.0 (external openmpi-5.0.6-rx6rdrwrfbirnxit5jpgoj3w3lxdni4n)
[+] /users/example/spack/opt/spack/linux-zen4/gcc-runtime-11.4.1-nvwittl62oq7v24ksdjoitzmfcoqdzzv
[+] /users/example/spack/opt/spack/linux-zen4/gmake-4.4.1-oqhxiah3frx4j7oy7urrlhttcg6couqx
==> No binary for phylobayesmpi-1.9-5xt25eudmtoskw5ayr47y3n5g3cvaq2h found: installing from source
==> Installing phylobayesmpi-1.9-5xt25eudmtoskw5ayr47y3n5g3cvaq2h [7/7]
==> Fetching https://mirror.spack.io/_source-cache/archive/56/567d8db995f23b2b0109c1e6088a7e5621e38fec91d6b2f27abd886b90ea31ce.tar.gz
    [100%]  701.19 KB @    1.4 MB/s
==> No patches needed for phylobayesmpi
==> phylobayesmpi: Executing phase: 'edit'
==> phylobayesmpi: Executing phase: 'build'
==> phylobayesmpi: Executing phase: 'install'
==> phylobayesmpi: Successfully installed phylobayesmpi-1.9-5xt25eudmtoskw5ayr47y3n5g3cvaq2h
  Stage: 1.08s.  Edit: 0.00s.  Build: 6.29s.  Install: 0.02s.  Post-install: 0.02s.  Total: 7.47s
[+] /users/example/spack/opt/spack/linux-zen4/phylobayesmpi-1.9-5xt25eudmtoskw5ayr47y3n5g3cvaq2h
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

This is actually quite straightforward, since Spack can successfully detect
and add this to your `packages.yaml` file.  I'm going to deliberately
complicate matters by having the miniforge module loaded when we run this, to
show you how you can tidy it up afterwards if it detects something you don't
want it to use.

```bash
$ spack external find openssl
==> The following specs have been detected on this system and added to /users/example/spack/etc/spack/packages.yaml
-- no arch / no compilers ---------------------------------------
openssl@3.0.7  openssl@3.3.2
```

Here we find it's detected both the system OpenSSL, along with a version
lurking within miniforge.  If you found it picked up a package you didn't want
it to use, you can use `spack config edit packages` to adjust this after it's
detected packages.

In this case, you may choose to remove these two lines, to stop it from using the miniforge version:
```yaml
    - spec: openssl@3.3.2
      prefix: /opt/apps/pkg/interpreters/miniforge/24.7.1
```
</details>
