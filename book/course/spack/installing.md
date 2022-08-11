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
writing).  So let's just install one to show how a simple package install
works:

```
$ spack install pigz
==> Installing zlib-1.2.12-liumxhopy2yde7xw7rt6flucgt4vt7fv
==> No binary for zlib-1.2.12-liumxhopy2yde7xw7rt6flucgt4vt7fv found: installing from source
==> Fetching https://mirror.spack.io/_source-cache/archive/91/91844808532e5ce316b3c010929493c0244f3d37593afd6de04f71821d5136d9.tar.gz
==> Applied patch /home/scsjh/spack/var/spack/repos/builtin/packages/zlib/configure-cc.patch
==> zlib: Executing phase: 'install'
==> zlib: Successfully installed zlib-1.2.12-liumxhopy2yde7xw7rt6flucgt4vt7fv
  Fetch: 0.24s.  Build: 5.79s.  Total: 6.02s.
[+] /home/scsjh/spack/opt/spack/linux-rhel8-haswell/gcc-8.5.0/zlib-1.2.12-liumxhopy2yde7xw7rt6flucgt4vt7fv
==> Installing pigz-2.7-qkx6pxv5qtwcjsmmfrqzbo32g5uwkshy
==> No binary for pigz-2.7-qkx6pxv5qtwcjsmmfrqzbo32g5uwkshy found: installing from source
==> Fetching https://mirror.spack.io/_source-cache/archive/d2/d2045087dae5e9482158f1f1c0f21c7d3de6f7cdc7cc5848bdabda544e69aa58.tar.gz
==> No patches needed for pigz
==> pigz: Executing phase: 'edit'
==> pigz: Executing phase: 'build'
==> pigz: Executing phase: 'install'
==> pigz: Successfully installed pigz-2.7-qkx6pxv5qtwcjsmmfrqzbo32g5uwkshy
  Fetch: 0.67s.  Build: 2.23s.  Total: 2.89s.
[+] /home/scsjh/spack/opt/spack/linux-rhel8-haswell/gcc-8.5.0/pigz-2.7-qkx6pxv5qtwcjsmmfrqzbo32g5uwkshy
```

That is quite wordy, but to note, it's installed pigz, and also installed a
dependency of it, zlib.  Just asking for it to install that one package has led
to it downloading and installing another, and it's quite happy done that
without needing any guidance.

Let's now see how you can load that module, and use it.  Now the first time you
build something with spack, you have to reload the setup script to make it find
these newly build packages.  In future this wouldn't be necessary:

```
$ . spack/share/spack/setup-env.sh
$ module avail pigz
------------------------------------------------- /tmp/spack/share/spack/modules/linux-rhel8-haswell --------------------------------------------------
pigz-2.7-gcc-8.5.0-qkx6pxv
```

At this point we can see there's a module available for pigz, with a bunch of
stuff after the version, which we'll cover later.  So I can now load this, and
test it to confirm it's worked:

```
[scsjh@worklaptop ~]$ module add pigz-2.7-gcc-8.5.0-qkx6pxv 
[scsjh@worklaptop ~]$ pigz --version
pigz 2.7
```

Excellent.  To recap, we've just:
  - Downloaded and installed Spack
  - Used Spack to list available packages
  - Installed a piece of sofware with Spack
  - Used that installed piece of software
