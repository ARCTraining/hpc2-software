## Installing Spack

Installing Spack, at least in a basic form that is at least usable, is very
straightforward.  It's literally a single command:

```
$ git clone https://github.com/spack/spack
```

That's it.  Now, there's more you can to do to configure it, which we'll talk
about later, but as far as the initial installation, that's it.  But let's
deliberately do a little bit more in this installation section, just so we
convince ourselves that we've really done it.  We'll go over it in more detail
in the next section.

## Enabling Spack

So to use it, you need to load the profile script that enables Spack.  Having
just downloaded it as above, you can do:

```
$ . spack/share/spack/setup-env.sh
```

Spack is now alive in your terminal, and you can query available packages, or
look at one to see what options you have for building it.  So we can have a
look at what packages are available to install:

```
$ spack list
3dtk
3proxy
7zip
abacus
abduco
abi-compliance-checker
abi-dumper
abinit
abseil-cpp
abyss
...
```

Now that's quite a list (6498 different pieces of software at the time of
writing).

So let's move on to look at the next section, where we install one to show how
a simple package install works.
