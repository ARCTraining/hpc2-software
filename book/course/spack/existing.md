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
path and tell Spack to try to find it.  In this example I'm going to swap out
the default Intel compiler on ARC4 for a shiny GNU compiler:

```bash
$ module add gcc/14.2.0
$ spack compiler add
==> Added 1 new compiler to /users/example/.spack/linux/compilers.yaml
    gcc@14.2.0
==> Compilers are defined in the following files:
    /users/example/.spack/linux/compilers.yaml
```

Spack now knows about this compiler.  If this was a compiler in a module, you
can just load the module beforehand, and Spack will add that compiler to its
known list.

If you want to set that as your preferred compiler, you can do this with a
single command:

```bash
spack config add packages:all:compiler:[gcc@14.2.0]
```

You can confirm your config, or make other changes with:

```bash
spack config edit packages
```

```yaml
packages:
  all:
    compiler: [gcc@14.2.0]
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
 -   phylobayesmpi@1.9%gcc@14.2.0 build_system=makefile arch=linux-rocky9-zen4
 -       ^gcc-runtime@14.2.0%gcc@14.2.0 build_system=generic arch=linux-rocky9-zen4
[e]      ^glibc@2.34%gcc@14.2.0 build_system=autotools arch=linux-rocky9-zen4
[+]      ^gmake@4.4.1%gcc@11.4.1~guile build_system=generic arch=linux-rocky9-zen4
[+]          ^gcc-runtime@11.4.1%gcc@11.4.1 build_system=generic arch=linux-rocky9-zen4
[e]          ^glibc@2.34%gcc@11.4.1 build_system=autotools arch=linux-rocky9-zen4
 -       ^openmpi@5.0.5%gcc@14.2.0+atomics~cuda~debug~gpfs~internal-hwloc~internal-libevent~internal-pmix~java~lustre~memchecker~openshmem~romio+rsh~static~two_level_namespace+vt+wrapper-rpath build_system=autotools fabrics=none romio-filesystem=none schedulers=none arch=linux-rocky9-zen4
 -           ^autoconf@2.72%gcc@14.2.0 build_system=autotools arch=linux-rocky9-zen4
 -               ^m4@1.4.19%gcc@14.2.0+sigsegv build_system=autotools patches=9dc5fbd,bfdffa7 arch=linux-rocky9-zen4
 -                   ^diffutils@3.10%gcc@14.2.0 build_system=autotools arch=linux-rocky9-zen4
 -                   ^libsigsegv@2.14%gcc@14.2.0 build_system=autotools arch=linux-rocky9-zen4
 -           ^automake@1.16.5%gcc@14.2.0 build_system=autotools arch=linux-rocky9-zen4
 -           ^hwloc@2.11.1%gcc@14.2.0~cairo~cuda~gl~libudev+libxml2~nvml~oneapi-level-zero~opencl+pci~rocm build_system=autotools libs=shared,static arch=linux-rocky9-zen4
 -               ^libpciaccess@0.17%gcc@14.2.0 build_system=autotools arch=linux-rocky9-zen4
 -                   ^util-macros@1.20.1%gcc@14.2.0 build_system=autotools arch=linux-rocky9-zen4
 -               ^libxml2@2.13.4%gcc@14.2.0+pic~python+shared build_system=autotools arch=linux-rocky9-zen4
 -                   ^libiconv@1.17%gcc@14.2.0 build_system=autotools libs=shared,static arch=linux-rocky9-zen4
 -                   ^xz@5.4.6%gcc@14.2.0~pic build_system=autotools libs=shared,static arch=linux-rocky9-zen4
 -               ^ncurses@6.5%gcc@14.2.0~symlinks+termlib abi=none build_system=autotools patches=7a351bc arch=linux-rocky9-zen4
 -           ^libevent@2.1.12%gcc@14.2.0+openssl build_system=autotools arch=linux-rocky9-zen4
 -               ^openssl@3.4.0%gcc@14.2.0~docs+shared build_system=generic certs=mozilla arch=linux-rocky9-zen4
 -                   ^ca-certificates-mozilla@2023-05-30%gcc@14.2.0 build_system=generic arch=linux-rocky9-zen4
 -           ^libtool@2.4.7%gcc@14.2.0 build_system=autotools arch=linux-rocky9-zen4
 -               ^findutils@4.9.0%gcc@14.2.0 build_system=autotools patches=440b954 arch=linux-rocky9-zen4
 -           ^numactl@2.0.18%gcc@14.2.0 build_system=autotools arch=linux-rocky9-zen4
 -           ^openssh@9.9p1%gcc@14.2.0+gssapi build_system=autotools arch=linux-rocky9-zen4
 -               ^krb5@1.21.3%gcc@14.2.0+shared build_system=autotools arch=linux-rocky9-zen4
 -                   ^bison@3.8.2%gcc@14.2.0~color build_system=autotools arch=linux-rocky9-zen4
 -                   ^gettext@0.22.5%gcc@14.2.0+bzip2+curses+git~libunistring+libxml2+pic+shared+tar+xz build_system=autotools arch=linux-rocky9-zen4
 -                       ^tar@1.34%gcc@14.2.0 build_system=autotools zip=pigz arch=linux-rocky9-zen4
[+]                          ^pigz@2.8%gcc@11.4.1 build_system=makefile arch=linux-rocky9-zen4
 -                           ^zstd@1.5.6%gcc@14.2.0+programs build_system=makefile compression=none libs=shared,static arch=linux-rocky9-zen4
 -               ^libedit@3.1-20240808%gcc@14.2.0 build_system=autotools arch=linux-rocky9-zen4
 -               ^libxcrypt@4.4.35%gcc@14.2.0~obsolete_api build_system=autotools patches=4885da3 arch=linux-rocky9-zen4
 -           ^perl@5.40.0%gcc@14.2.0+cpanm+opcode+open+shared+threads build_system=generic arch=linux-rocky9-zen4
 -               ^berkeley-db@18.1.40%gcc@14.2.0+cxx~docs+stl build_system=autotools patches=26090f4,b231fcc arch=linux-rocky9-zen4
 -               ^bzip2@1.0.8%gcc@14.2.0~debug~pic+shared build_system=generic arch=linux-rocky9-zen4
 -               ^gdbm@1.23%gcc@14.2.0 build_system=autotools arch=linux-rocky9-zen4
 -                   ^readline@8.2%gcc@14.2.0 build_system=autotools patches=bbf97f1 arch=linux-rocky9-zen4
 -           ^pkgconf@2.2.0%gcc@14.2.0 build_system=autotools arch=linux-rocky9-zen4
 -           ^pmix@5.0.3%gcc@14.2.0~munge~python~restful build_system=autotools arch=linux-rocky9-zen4
[+]          ^zlib-ng@2.2.1%gcc@11.4.1+compat+new_strategies+opt+pic+shared build_system=autotools arch=linux-rocky9-zen4
```

</details>

You don't need to read the full spec above, but on Aire currently, this adds up to
43 packages, which I'm sure would take a fair old amount of time to build.  So
let's see if we can cut that down by using software already installed on my
system.

It can find certain software in your environment without you having to
configure anything by hand.  So if I load an openmpi module, and run:

```bash
$ module add openmpi/5.0.6/gcc-13.2.0_cuda-12.6.2
$ spack external find openmpi
==> The following specs have been detected on this system and added to /home/home02/me/.spack/packages.yaml
-- no arch / gcc@13.2.0 -----------------------------------------
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
    - spec: openmpi@5.0.6%gcc@=13.2.0+cuda~java~memchecker~static~wrapper-rpath fabrics=ofi,psm2,ucx
        schedulers=none
      prefix: /opt/apps/pkg/libraries/openmpi/5.0.6/gcc-13.2.0+cuda-12.6.2
  all:
    compiler: [gcc@14.2.0]
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
 -   phylobayesmpi@1.9%gcc@13.2.0 build_system=makefile arch=linux-rocky9-zen4
 -       ^gcc-runtime@13.2.0%gcc@13.2.0 build_system=generic arch=linux-rocky9-zen4
[e]      ^glibc@2.34%gcc@13.2.0 build_system=autotools arch=linux-rocky9-zen4
[+]      ^gmake@4.4.1%gcc@11.4.1~guile build_system=generic arch=linux-rocky9-zen4
[+]          ^gcc-runtime@11.4.1%gcc@11.4.1 build_system=generic arch=linux-rocky9-zen4
[e]          ^glibc@2.34%gcc@11.4.1 build_system=autotools arch=linux-rocky9-zen4
[e]      ^openmpi@5.0.6%gcc@13.2.0+atomics+cuda~debug~gpfs~internal-hwloc~internal-libevent~internal-pmix~java~lustre~memchecker~openshmem~romio+rsh~static~two_level_namespace+vt~wrapper-rpath build_system=autotools cuda_arch=none fabrics=ofi,psm2,ucx romio-filesystem=none schedulers=none arch=linux-rocky9-zen4
```

So now Spack believes that it can reuse my openmpi from the system and other
than a little shim package (gcc-runtime), it's only actually going to build
the piece of software I've asked for, rather than the huge list of
dependencies we had before.

```bash
$ spack install phylobayesmpi
[+] /usr (external glibc-2.34-yzotqqevluwtq3jprsigxmtetjc3s3c7)
[+] /usr (external glibc-2.34-nuyxhw7kdup423xfoh3erg5yl7c3xrlh)
[+] /opt/apps/pkg/libraries/openmpi/5.0.6/gcc-13.2.0+cuda-12.6.2 (external openmpi-5.0.6-73sqmyovadjm2mdbuqrmpd5vm3ihygyo)
==> Installing gcc-runtime-13.2.0-r7anquf3s2qyszutz3dprsgfjg2baypt [4/7]
==> No binary for gcc-runtime-13.2.0-r7anquf3s2qyszutz3dprsgfjg2baypt found: installing from source
==> No patches needed for gcc-runtime
==> gcc-runtime: Executing phase: 'install'
==> gcc-runtime: Successfully installed gcc-runtime-13.2.0-r7anquf3s2qyszutz3dprsgfjg2baypt
  Stage: 0.00s.  Install: 0.22s.  Post-install: 0.12s.  Total: 0.37s
[+] /users/example/spack/opt/spack/linux-rocky9-zen4/gcc-13.2.0/gcc-runtime-13.2.0-r7anquf3s2qyszutz3dprsgfjg2baypt
[+] /users/example/spack/opt/spack/linux-rocky9-zen4/gcc-11.4.1/gcc-runtime-11.4.1-7hex6dyh2ttbdeywfkq5vbsinmnhjoub
[+] /users/example/spack/opt/spack/linux-rocky9-zen4/gcc-11.4.1/gmake-4.4.1-36fbslt63hhoisn7shlrkgd5fsb2awmz
==> Installing phylobayesmpi-1.9-6267nytqg6se4wqmg6p37ftescnz2jpz [7/7]
==> No binary for phylobayesmpi-1.9-6267nytqg6se4wqmg6p37ftescnz2jpz found: installing from source
==> Fetching https://mirror.spack.io/_source-cache/archive/56/567d8db995f23b2b0109c1e6088a7e5621e38fec91d6b2f27abd886b90ea31ce.tar.gz
==> No patches needed for phylobayesmpi
==> phylobayesmpi: Executing phase: 'edit'
==> phylobayesmpi: Executing phase: 'build'
==> phylobayesmpi: Executing phase: 'install'
==> phylobayesmpi: Successfully installed phylobayesmpi-1.9-6267nytqg6se4wqmg6p37ftescnz2jpz
  Stage: 1.18s.  Edit: 0.00s.  Build: 6.59s.  Install: 0.05s.  Post-install: 0.06s.  Total: 7.95s
[+] /users/example/spack/opt/spack/linux-rocky9-zen4/gcc-13.2.0/phylobayesmpi-1.9-6267nytqg6se4wqmg6p37ftescnz2jpz
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
==> The following specs have been detected on this system and added to /users/example/.spack/packages.yaml
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
