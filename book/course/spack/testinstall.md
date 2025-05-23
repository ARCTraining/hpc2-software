# Building a test piece of software

So we've installed Spack, and activated it from the last section.

Let's now try installing a simple piece of software.  Pigz seems like a
reasonable place to start (parallel gzip), as it doesn't have too many
dependencies, so won't take long to try out:

```bash
$ spack install pigz
[+] /usr (external glibc-2.34-nuyxhw7kdup423xfoh3erg5yl7c3xrlh)
==> Installing gcc-runtime-11.4.1-7hex6dyh2ttbdeywfkq5vbsinmnhjoub [2/5]
==> No binary for gcc-runtime-11.4.1-7hex6dyh2ttbdeywfkq5vbsinmnhjoub found: installing from source
==> No patches needed for gcc-runtime
==> gcc-runtime: Executing phase: 'install'
==> gcc-runtime: Successfully installed gcc-runtime-11.4.1-7hex6dyh2ttbdeywfkq5vbsinmnhjoub
  Stage: 0.00s.  Install: 0.05s.  Post-install: 0.04s.  Total: 0.11s
[+] /users/example/spack/opt/spack/linux-rocky9-zen4/gcc-11.4.1/gcc-runtime-11.4.1-7hex6dyh2ttbdeywfkq5vbsinmnhjoub
==> Installing gmake-4.4.1-36fbslt63hhoisn7shlrkgd5fsb2awmz [3/5]
==> No binary for gmake-4.4.1-36fbslt63hhoisn7shlrkgd5fsb2awmz found: installing from source
==> Fetching https://mirror.spack.io/_source-cache/archive/dd/dd16fb1d67bfab79a72f5e8390735c49e3e8e70b4945a15ab1f81ddb78658fb3.tar.gz
==> No patches needed for gmake
==> gmake: Executing phase: 'install'
==> gmake: Successfully installed gmake-4.4.1-36fbslt63hhoisn7shlrkgd5fsb2awmz
  Stage: 0.21s.  Install: 10.08s.  Post-install: 0.02s.  Total: 10.36s
[+] /users/example/spack/opt/spack/linux-rocky9-zen4/gcc-11.4.1/gmake-4.4.1-36fbslt63hhoisn7shlrkgd5fsb2awmz
==> Installing zlib-ng-2.2.1-5rrpd7bzlpd7tgfp6nfw2z23dpzpcme7 [4/5]
==> No binary for zlib-ng-2.2.1-5rrpd7bzlpd7tgfp6nfw2z23dpzpcme7 found: installing from source
==> Fetching https://mirror.spack.io/_source-cache/archive/ec/ec6a76169d4214e2e8b737e0850ba4acb806c69eeace6240ed4481b9f5c57cdf.tar.gz
==> No patches needed for zlib-ng
==> zlib-ng: Executing phase: 'autoreconf'
==> zlib-ng: Executing phase: 'configure'
==> zlib-ng: Executing phase: 'build'
==> zlib-ng: Executing phase: 'install'
==> zlib-ng: Successfully installed zlib-ng-2.2.1-5rrpd7bzlpd7tgfp6nfw2z23dpzpcme7
  Stage: 0.18s.  Autoreconf: 0.00s.  Configure: 2.81s.  Build: 0.97s.  Install: 0.08s.  Post-install: 0.04s.  Total: 4.18s
[+] /users/example/spack/opt/spack/linux-rocky9-zen4/gcc-11.4.1/zlib-ng-2.2.1-5rrpd7bzlpd7tgfp6nfw2z23dpzpcme7
==> Installing pigz-2.8-gqwhh2itznlqmnlv7qfddsckiogws3jh [5/5]
==> No binary for pigz-2.8-gqwhh2itznlqmnlv7qfddsckiogws3jh found: installing from source
==> Fetching https://mirror.spack.io/_source-cache/archive/2f/2f7f6a6986996d21cb8658535fff95f1c7107ddce22b5324f4b41890e2904706.tar.gz
==> No patches needed for pigz
==> pigz: Executing phase: 'edit'
==> pigz: Executing phase: 'build'
==> pigz: Executing phase: 'install'
==> pigz: Successfully installed pigz-2.8-gqwhh2itznlqmnlv7qfddsckiogws3jh
  Stage: 0.09s.  Edit: 0.00s.  Build: 1.02s.  Install: 0.01s.  Post-install: 0.03s.  Total: 1.23s
[+] /users/example/spack/opt/spack/linux-rocky9-zen4/gcc-11.4.1/pigz-2.8-gqwhh2itznlqmnlv7qfddsckiogws3jh
```

That is quite wordy, but to note, it's installed pigz, and also installed
dependencies of it, gcc-runtime, gmake, zlib-ng and pigz.  Just asking for it
to install that one package has led to it downloading and installing several,
and it's quite happy doing that without needing any guidance.

```bash
$ spack find pigz
-- linux-rocky9-zen4 / gcc@11.4.1 -------------------------------
pigz@2.8
==> 1 installed package
```

At this point we can see there's a version available for pigz.  I can now load
this, and test it to confirm it's worked:

```bash
$ spack load pigz
$ pigz --version
pigz 2.8
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
   $ spack find k8
   -- linux-rocky9-zen4 / gcc@11.4.1 -------------------------------
   k8@0.2.4
   ==> 1 installed package
   ```

- Load the software

   ```
   $ spack load k8
   ```

- Test the software

   ```
   $ k8 -e 'print( "Square root of 25 is: " + Math.sqrt(25) );'
   Square root of 25 is: 5
   ```

</details>

## References

- [Installing Packages](https://spack-tutorial.readthedocs.io/en/latest/tutorial_basics.html#installing-packages)
