# Module files

In the [earlier section](testinstall) we built a version of pigz, and loaded it:

```bash
$ spack load pigz
$ pigz --version
pigz 2.7
```

Now this is different to how we load modules on ARC.  You can make Spack behave
in the same way, should you wish, although generally we would recommend you use
the `spack` command to load software instead.

## Setup

You need to do one step to enable module file creation:

```bash
spack config add modules:default:enable:[tcl]
```

This alone is enough to tell Spack that in future you'd like it to generate tcl
format module files when installing software in future.  But we'll need to take
another step now to both create a module file for our pigz install, and to make
it visible in our current session.

```bash
spack module tcl refresh -y
```

This will then rebuild all the module files for previous installed software.
You need to source the spack environment again to make them visible to the
module command though:

```bash
$ . spack/share/spack/setup-env.sh
```

We can confirm now that we can see the module with the module command:

```bash
$ module avail pigz
$ module avail pigz

--------------------------------------------------------------------------- /tmp/me/spack/share/spack/modules/linux-centos7-haswell ----------------------------------------------------------------------------
pigz-2.7-gcc-4.8.5-ji42zlq

------------------------------------------------------------------------ /tmp/me/spack/share/spack/modules/linux-centos7-skylake_avx512 ------------------------------------------------------------------------
pigz-2.7-gcc-12.3.0-4a2l6se

$ pigz --version
-bash: pigz: command not found
$ module add pigz-2.7-gcc-12.3.0-4a2l6se
$ pigz --version
pigz 2.7
```

Now, it's very definitive what you're loading (pigz version 2.7 built with gcc
12.3.0 with a hash to pin it down further), but it's a bit wordy.  This format
of modules isn't necessarily what you want, and something cleaner is likely
more generally useful.  Don't be bothered by the mangle of characters at the
end (the hash).  This just precisely identifies a given build of software,
which can be useful if you need to distinguish between two slightly different
builds of a given piece of software.  We're going to mostly ignore this in this
course, as we're not looking at such complex installations.

One option is to simplify this a great deal, hiding a lot of that information,
which might be appealing if you know that information is not important to you.
So if you're using one compiler, you don't really care about the compiler used,
you don't need the hash at the end if you're only dealing with a single variant
(covered [later](advanced:spack:variants)), and you'd probably like the modules
names more in line with how they have traditionally been named, with the
version separate from the package name.

There's a single file within the spack directory that defines how spack
generates module files, and we can edit it via:

```bash
spack config edit modules
```

A basic starting point might be the following:

```yaml
modules:
  default:
    enable: [tcl]
    tcl:
      hash_length: 4
      projections:
        all:  '{name}/{version}'
```

Here we're saying that we want the default module files that use tcl (the
default) to have a four character hash, and to be named according to this
name/version model we're used to on other systems.  Quit and save that file,
and we've told Spack how we'd like these modules to be built in future, but
existing modules would still use the old scheme.  We can tell it to rebuild the
modulefiles, and delete existing files:

```bash
spack module tcl refresh --delete-tree -y
```

Testing this out now we can see all is how we wanted it:

```bash
$ module avail
$ module avail pigz
--------------------------------------------------------------------------- /tmp/me/spack/share/spack/modules/linux-centos7-haswell ----------------------------------------------------------------------------
pigz/2.7-ji42
------------------------------------------------------------------------ /tmp/me/spack/share/spack/modules/linux-centos7-skylake_avx512 ------------------------------------------------------------------------
pigz/2.7-4a2l
```

We now have tidier module files, named in a simpler fashion, which is probably
enough for most straightforward purposes.  They can be loaded in the same style
you would on ARC:

```bash
$ module add pigz
$ modules add pigz/2.7-ji42
```

In this case, we still need to include a hash, as we've got pigz built with two
different compilers, and elsewhere we have cmake built with two different sets
of dependencies, complicating matters further.

If you want more complicated structures, supporting multiple compilers, MPI
implementations, numerical libraries, this is all possible, and covered in the
link below in the references.  It also supports LMod as well as the more legacy
Tcl system.

## References

- [Module files tutorial](https://spack-tutorial.readthedocs.io/en/latest/tutorial_modules.html)
