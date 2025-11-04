# Building a test piece of software

So we've installed Spack, and activated it from the last section.

Let's now try installing a simple piece of software.  Pigz seems like a
reasonable place to start (parallel gzip), as it doesn't have too many
dependencies, so won't take long to try out:

```bash
$ spack install pigz
==> Compilers have been configured automatically from PATH inspection
==> Fetching https://ghcr.io/v2/spack/bootstrap-buildcache-v2.2/blobs/sha256:2010a2a50b9620c2bda7c5fa4e9ce137a115dbba35094857fecc819d9a00a789
==> Fetching https://ghcr.io/v2/spack/bootstrap-buildcache-v2.2/blobs/sha256:31f1649728e2d58902eb62d1c2e37b1cfc73e007089322a17463b3cb5777cb98
==> Installing "clingo-bootstrap@=spack~apps~docs+ipo+optimized+python+static_libstdcpp build_system=cmake build_type=Release commit=2a025667090d71b2c9dce60fe924feb6bde8f667 generator=make patches:=bebb819,ec99431 platform=linux os=centos7 target=x86_64" from a buildcache
[+] /usr (external gcc-11.4.1-fhj3ubjk6iqnexhdlnbcmlzjuo3iofwa)
[+] /usr (external glibc-2.34-z4aqp7yhzcmlwfcydu32wbqxounooohb)
==> No binary for compiler-wrapper-1.0-et3ejxhc3t5yoqbw74pqjv4izptvuj3q found: installing from source
==> Installing compiler-wrapper-1.0-et3ejxhc3t5yoqbw74pqjv4izptvuj3q [3/7]
==> Fetching https://mirror.spack.io/_source-cache/archive/a5/a5ff4fcdbeda284a7993b87f294b6338434cffc84ced31e4d04008ed5ea389bf
    [100%]   30.08 KB @   14.2 MB/s
==> No patches needed for compiler-wrapper
==> compiler-wrapper: Executing phase: 'install'
==> compiler-wrapper: Successfully installed compiler-wrapper-1.0-et3ejxhc3t5yoqbw74pqjv4izptvuj3q
  Stage: 0.10s.  Install: 0.00s.  Post-install: 0.01s.  Total: 0.13s
[+] /users/example/spack/opt/spack/linux-zen4/compiler-wrapper-1.0-et3ejxhc3t5yoqbw74pqjv4izptvuj3q
==> No binary for gcc-runtime-11.4.1-dqguk7uvu3bh5ehxvi7sv4qaartrartc found: installing from source
==> Installing gcc-runtime-11.4.1-dqguk7uvu3bh5ehxvi7sv4qaartrartc [4/7]
==> No patches needed for gcc-runtime
==> gcc-runtime: Executing phase: 'install'
==> gcc-runtime: Successfully installed gcc-runtime-11.4.1-dqguk7uvu3bh5ehxvi7sv4qaartrartc
  Stage: 0.00s.  Install: 0.03s.  Post-install: 0.01s.  Total: 0.06s
[+] /users/example/spack/opt/spack/linux-zen4/gcc-runtime-11.4.1-dqguk7uvu3bh5ehxvi7sv4qaartrartc
==> No binary for gmake-4.4.1-2fg6qftd7dmzej4hzzydnwqwnbl65zac found: installing from source
==> Installing gmake-4.4.1-2fg6qftd7dmzej4hzzydnwqwnbl65zac [5/7]
==> Fetching https://mirror.spack.io/_source-cache/archive/dd/dd16fb1d67bfab79a72f5e8390735c49e3e8e70b4945a15ab1f81ddb78658fb3.tar.gz
    [100%]    2.35 MB @   21.5 MB/s
==> No patches needed for gmake
==> gmake: Executing phase: 'install'
==> gmake: Successfully installed gmake-4.4.1-2fg6qftd7dmzej4hzzydnwqwnbl65zac
  Stage: 0.23s.  Install: 9.98s.  Post-install: 0.00s.  Total: 10.24s
[+] /users/example/spack/opt/spack/linux-zen4/gmake-4.4.1-2fg6qftd7dmzej4hzzydnwqwnbl65zac
==> No binary for zlib-ng-2.2.4-ro6otklzkonqr26ittllhxc72qpr37le found: installing from source
==> Installing zlib-ng-2.2.4-ro6otklzkonqr26ittllhxc72qpr37le [6/7]
==> Fetching https://mirror.spack.io/_source-cache/archive/a7/a73343c3093e5cdc50d9377997c3815b878fd110bf6511c2c7759f2afb90f5a3.tar.gz
    [100%]    2.42 MB @   23.8 MB/s
==> No patches needed for zlib-ng
==> zlib-ng: Executing phase: 'autoreconf'
==> zlib-ng: Executing phase: 'configure'
==> zlib-ng: Executing phase: 'build'
==> zlib-ng: Executing phase: 'install'
==> zlib-ng: Successfully installed zlib-ng-2.2.4-ro6otklzkonqr26ittllhxc72qpr37le
  Stage: 0.22s.  Autoreconf: 0.00s.  Configure: 2.89s.  Build: 0.83s.  Install: 0.37s.  Post-install: 0.01s.  Total: 4.39s
[+] /users/example/spack/opt/spack/linux-zen4/zlib-ng-2.2.4-ro6otklzkonqr26ittllhxc72qpr37le
==> No binary for pigz-2.8-rpmloasg4nsdwdo4lvyskke7xex5nvdd found: installing from source
==> Installing pigz-2.8-rpmloasg4nsdwdo4lvyskke7xex5nvdd [7/7]
==> Fetching https://mirror.spack.io/_source-cache/archive/2f/2f7f6a6986996d21cb8658535fff95f1c7107ddce22b5324f4b41890e2904706.tar.gz
    [100%]  128.77 KB @    3.1 MB/s
==> No patches needed for pigz
==> pigz: Executing phase: 'edit'
==> pigz: Executing phase: 'build'
==> pigz: Executing phase: 'install'
==> pigz: Successfully installed pigz-2.8-rpmloasg4nsdwdo4lvyskke7xex5nvdd
  Stage: 0.12s.  Edit: 0.00s.  Build: 0.98s.  Install: 0.00s.  Post-install: 0.00s.  Total: 1.15s
[+] /users/example/spack/opt/spack/linux-zen4/pigz-2.8-rpmloasg4nsdwdo4lvyskke7xex5nvdd
```

That is quite wordy, but you can see that it has installed pigz, after first
installing its dependencies, compiler-wrapper, gcc-runtime, gmake, zlib-ng.
Just asking for it to install that one package has led to it downloading and
installing several, and it's quite happy doing that without needing any
guidance.

```bash
$ spack find pigz
-- linux-rocky9-zen4 / %c=gcc@11.4.1 ----------------------------
pigz@2.8
==> 1 installed package
```

At this point we can see there's a version available for pigz.  I can now load
this, and test it to confirm it's worked:

```bash
$ pigz --version
pigz 2.5
$ spack load pigz
$ pigz --version
pigz 2.8
$ spack unload pigz
```

Excellent.  To recap, by this stage we've:

- Downloaded and installed Spack
- Used Spack to list available packages
- Installed a piece of sofware with Spack
- Used that installed piece of software

That's quite an acheivement give it doesn't really feel like we've done too
much work yet.

## Exercise

Install and use the `cowsay` software, to produce an ascii picture of a cow
saying "Moooo!".  The command you can run to test your install is:

```
cowsay Moooo!
```

<details>
<summary>Click here to reveal solution</summary>

### Solution

- Install cowsay

   ```bash
   spack install cowsay
   ```

- Find the name of the module

   ```bash
   $ spack find cowsay
   -- linux-rocky9-zen4 / no compilers -----------------------------
   cowsay@3.04
   ==> 1 installed package
   ```

- Load the software

   ```bash
   spack load cowsay
   ```

- Test the software

   ```bash
   $ cowsay Moooo!
    ________ 
    < Moooo! >
    -------- 
           \   ^__^
            \  (oo)\_______
               (__)\       )\/\
                   ||----w |
                   ||     ||
   $ spack unload cowsay
   ```

</details>

## References

- [Installing Packages](https://spack-tutorial.readthedocs.io/en/latest/tutorial_basics.html#installing-packages)
