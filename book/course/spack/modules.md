# Module files

In the [earlier section](testinstall) we built a version of pigz, and loaded it:

```bash
$ spack load pigz
$ pigz --version
pigz 2.8
```

Now this is different to how we load modules on Aire.  You can make Spack
behave in the same way, should you wish, although generally we would recommend
you use the `spack` command to load software instead.

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
------ /users/example/spack/share/spack/modules/linux-rocky9-zen4 ------
pigz/2.8-gcc-11.4.1-gqwhh2i

# Make sure Spack has no packages enabled
$ spack unload -a

# On Aire, pigz is installed, but on your system you may find it's not.
$ pigz --version
pigz 2.5
$ module add pigz/2.8-gcc-11.4.1-gqwhh2i
Loading pigz/2.8-gcc-11.4.1-gqwhh2i
  Loading requirement: glibc/2.34-gcc-11.4.1-nuyxhw7 gcc-runtime/11.4.1-gcc-11.4.1-7hex6dy zlib-ng/2.2.1-gcc-11.4.1-5rrpd7
$ pigz --version
pigz 2.8
```

Now, it's very definitive what you're loading (pigz version 2.8 built with gcc
11.4.1 with a hash to pin it down further), but it's a bit wordy.  This format
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
name/version model we're used to on our general Linux systems.  Quit and save
that file, and we've told Spack how we'd like these modules to be built in
future, but existing modules would still use the old scheme.  We can tell it
to rebuild the modulefiles, and delete existing files:

```bash
spack module tcl refresh --delete-tree -y
```

Testing this out now we can see all is how we wanted it:

```bash
$ module avail pigz
------ /users/example/spack/share/spack/modules/linux-rocky9-zen4 ------
pigz/2.8-gqwh
```

We now have tidier module files, named in a simpler fashion, which is probably
enough for most straightforward purposes.  They can be loaded in the same style
you would on other systems:

```bash
$ module add pigz
$ module add pigz/2.8-gqwh
```

Now you may be wondering why we bothered including this hash, as it'd clearly
be tidier to omit it and just have `pigz/2.8` as the module name.

In this case, we didn't need to include a hash, as we've only got pigz built
in one way, but elsewhere we may have modules built with
different compilers, dependencies and variants.  So for example:

```bash
$ module avail zlib-ng
------ /users/example/spack/share/spack/modules/linux-rocky9-zen4 ------
zlib-ng/2.0.7-pjab  zlib-ng/2.2.1-5rrp  zlib-ng/2.2.1-xjlb
$ spack find zlib-ng
-- linux-rocky9-zen4 / gcc@11.4.1 -------------------------------
zlib-ng@2.0.7  zlib-ng@2.2.1

-- linux-rocky9-zen4 / gcc@14.2.0 -------------------------------
zlib-ng@2.2.1
==> 3 installed packages
```

You can see here we have two versions of zlib-ng built with two different
compilers, which would clearly class with a simpler scheme.

If you want more complicated structures, supporting multiple compilers, MPI
implementations, numerical libraries, this is all possible, and covered in the
link below in the references.  It also supports LMod as well as the more legacy
Tcl system.  There's also options for hiding a lot of the dependencies to slim
down how many modules are exposed to you as a user by default, but that's
aesthetic, and you can already take advantage of most of the power of Spack
without digging into those details.

## References

- [Module files tutorial](https://spack-tutorial.readthedocs.io/en/latest/tutorial_modules.html)
