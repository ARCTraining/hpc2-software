# Building a test piece of software

So we've installed Spack, and activated it from the last section.

Let's now try installing a simple piece of software.  Pigz seems like a
reasonable place to start (parallel gzip), as it doesn't have too many
dependencies, so won't take long to try out:

```bash
$ spack install pigz
==> Installing zlib-1.2.12-liumxhopy2yde7xw7rt6flucgt4vt7fv
==> No binary for zlib-1.2.12-liumxhopy2yde7xw7rt6flucgt4vt7fv found: installing from source
==> Fetching https://mirror.spack.io/_source-cache/archive/91/91844808532e5ce316b3c010929493c0244f3d37593afd6de04f71821d5136d9.tar.gz
==> Applied patch /home/me/spack/var/spack/repos/builtin/packages/zlib/configure-cc.patch
==> zlib: Executing phase: 'install'
==> zlib: Successfully installed zlib-1.2.12-liumxhopy2yde7xw7rt6flucgt4vt7fv
  Fetch: 0.24s.  Build: 5.79s.  Total: 6.02s.
[+] /home/me/spack/opt/spack/linux-rhel8-haswell/gcc-8.5.0/zlib-1.2.12-liumxhopy2yde7xw7rt6flucgt4vt7fv
==> Installing pigz-2.7-qkx6pxv5qtwcjsmmfrqzbo32g5uwkshy
==> No binary for pigz-2.7-qkx6pxv5qtwcjsmmfrqzbo32g5uwkshy found: installing from source
==> Fetching https://mirror.spack.io/_source-cache/archive/d2/d2045087dae5e9482158f1f1c0f21c7d3de6f7cdc7cc5848bdabda544e69aa58.tar.gz
==> No patches needed for pigz
==> pigz: Executing phase: 'edit'
==> pigz: Executing phase: 'build'
==> pigz: Executing phase: 'install'
==> pigz: Successfully installed pigz-2.7-qkx6pxv5qtwcjsmmfrqzbo32g5uwkshy
  Fetch: 0.67s.  Build: 2.23s.  Total: 2.89s.
[+] /home/me/spack/opt/spack/linux-rhel8-haswell/gcc-8.5.0/pigz-2.7-qkx6pxv5qtwcjsmmfrqzbo32g5uwkshy
```

That is quite wordy, but to note, it's installed pigz, and also installed a
dependency of it, zlib.  Just asking for it to install that one package has led
to it downloading and installing another, and it's quite happy doing that
without needing any guidance.

Let's now see how you can load that module, and use it.  The first time you
build something with Spack, you have to reload the setup script to make it find
these newly build packages.  In future this shouldn't be necessary:

```bash
$ . spack/share/spack/setup-env.sh
$ module avail pigz
------------------------------------------------- /tmp/spack/share/spack/modules/linux-rhel8-haswell --------------------------------------------------
pigz-2.7-gcc-8.5.0-qkx6pxv
```

At this point we can see there's a module available for pigz, with a bunch of
stuff after the version, which we'll cover later.  I can now load this, and
test it to confirm it's worked:

```bash
$ module add pigz-2.7-gcc-8.5.0-qkx6pxv
$ pigz --version
pigz 2.7
```

Excellent.  To recap, by this stage we've:

- Downloaded and installed Spack
- Used Spack to list available packages
- Installed a piece of sofware with Spack
- Used that installed piece of software

That's quite an acheivement give it doesn't really feel like we've done too
much work yet.

## Exercise

Install and use the `k8` software, so we can find out the square root of 25
using javascript.  The command you can run to test your install is:

```
k8 -e 'print( "Square root of 25 is: " + Math.sqrt(25) );'
```

<details>
<summary>Click here to reveal solution</summary>

### Solution

- Install k8

   ```
   spack install k8
   ```

- Find the name of the module

   ```
   $ module avail k8
   ---------- /home/me/spack/share/spack/modules/linux-rhel8-haswell -----------
   k8-0.2.4-gcc-8.5.0-vdryjow
   ```

- Load the module

   ```
   module add k8-0.2.4-gcc-8.5.0-vdryjow
   ```

- Test the software

   ```
   $ k8 -e 'print( "Square root of 25 is: " + Math.sqrt(25) );'
   Square root of 25 is: 5
   ```

</details>

## References

- [Installing Packages](https://spack-tutorial.readthedocs.io/en/latest/tutorial_basics.html#installing-packages)
