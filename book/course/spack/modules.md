## Adjusting how we name modules

In the last section we built a version of pigz, and loaded it:

```
[scsjh@worklaptop ~]$ module add pigz-2.7-gcc-8.5.0-qkx6pxv 
[scsjh@worklaptop ~]$ pigz --version
pigz 2.7
```

Now, it's very clear what you're loading, but it's a bit wordy.  This format of
modules isn't necessarily what you want, and something simpler is likely more
generally useful.

One option is to simplify this a great deal, and hide some of that, if you know
it's not important to you.  So if you're using one compiler, you don't really
care about the compiler, you don't need the hash at the end if you're only
dealing with a single variant (covered later), and you'd probably like the
modules names more in line with how they have traditionally named, with the
version separate from the package name.

There's a single file within the spack directory that defines how spack
generates module files, and we can edit it via:

```
spack config edit modules
```

A simple starting point might be the following:
```
modules:
  default:
    tcl:
      hash_length: 0
      projections:
        all:  '{name}/{version}'
```

Here we're saying that we want the default module files that use tcl (the
default) to not have a hash, and to be named according to this name/version
model we're used to on other systems.  Quit and save that file, and we've told
Spack how we'd like these modules to be built in future, but existing modules
would still use the old scheme.  We can tell it to rebuild the modulefiles, and
delete existing files:

```
spack module tcl refresh --delete-tree -y
```

Testing this out now we can see all is how we wanted it:
```
$ module avail
---------- /home/scsjh/spack/share/spack/modules/linux-rhel8-haswell -----------
pigz/2.7  zlib/1.2.12  
```

Tidy module files, name in a nice simple fashion, which is probably enough for
many straightforward purposes.
