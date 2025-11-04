# Building a test piece of software

So we've installed Spack, and activated it from the last section.

Let's now try installing a simple piece of software.  Pigz seems like a
reasonable place to start (parallel gzip), as it doesn't have too many
dependencies, so won't take long to try out:

```bash
$ spack install pigz
==> Fetching https://ghcr.io/v2/spack/bootstrap-buildcache-v1/blobs/sha256:82ec278bef26c42303a2c2c888612c0d37babef615bc9a0003530e0b8b4d3d2c
==> Fetching https://ghcr.io/v2/spack/bootstrap-buildcache-v1/blobs/sha256:0c5831932608e7b4084fc6ce60e2b67b77dab76e5515303a049d4d30cd772321
==> Installing "clingo-bootstrap@=spack~docs+ipo+optimized+python+static_libstdcpp build_system=cmake build_type=Release generator=make patches:=bebb819,ec99431 arch=linux-centos7-x86_64" from a buildcache
==> Warning: The default behavior of tarfile extraction has been changed to disallow common exploits (including CVE-2007-4559). By default, absolute/parent paths are disallowed and some mode bits are cleared. See https://access.redhat.com/articles/7004769 for more details.
==> Compilers have been configured automatically from PATH inspection
[+] /usr (external glibc-2.34-riltp4wkahjqsanksbtayyu5sz2r2xzk)
==> No binary for compiler-wrapper-1.0-fbl56cgz34aegvfgcruvmp3cpwtbrvat found: installing from source
==> Installing compiler-wrapper-1.0-fbl56cgz34aegvfgcruvmp3cpwtbrvat [2/7]
==> Fetching https://mirror.spack.io/_source-cache/archive/c6/c65a9d2b2d4eef67ab5cb0684d706bb9f005bb2be94f53d82683d7055bdb837c
    [100%]   30.23 KB @   25.3 MB/s
==> No patches needed for compiler-wrapper
==> compiler-wrapper: Executing phase: 'install'
==> compiler-wrapper: Successfully installed compiler-wrapper-1.0-fbl56cgz34aegvfgcruvmp3cpwtbrvat
  Stage: 0.10s.  Install: 0.00s.  Post-install: 0.01s.  Total: 0.16s
[+] /users/example/spack/linux-zen4/compiler-wrapper-1.0-fbl56cgz34aegvfgcruvmp3cpwtbrvat
[+] /usr (external gcc-11.4.1-jrckedbivv6kljrzsqljlem6z2dm3rge)
==> No binary for gcc-runtime-11.4.1-nvwittl62oq7v24ksdjoitzmfcoqdzzv found: installing from source
==> Installing gcc-runtime-11.4.1-nvwittl62oq7v24ksdjoitzmfcoqdzzv [4/7]
==> No patches needed for gcc-runtime
==> gcc-runtime: Executing phase: 'install'
==> gcc-runtime: Successfully installed gcc-runtime-11.4.1-nvwittl62oq7v24ksdjoitzmfcoqdzzv
  Stage: 0.00s.  Install: 0.05s.  Post-install: 0.01s.  Total: 0.11s
[+] /users/example/spack/linux-zen4/gcc-runtime-11.4.1-nvwittl62oq7v24ksdjoitzmfcoqdzzv
==> No binary for gmake-4.4.1-oqhxiah3frx4j7oy7urrlhttcg6couqx found: installing from source
==> Installing gmake-4.4.1-oqhxiah3frx4j7oy7urrlhttcg6couqx [5/7]
==> Fetching https://mirror.spack.io/_source-cache/archive/dd/dd16fb1d67bfab79a72f5e8390735c49e3e8e70b4945a15ab1f81ddb78658fb3.tar.gz
    [100%]    2.35 MB @   35.4 MB/s
==> No patches needed for gmake
==> gmake: Executing phase: 'install'
==> gmake: Successfully installed gmake-4.4.1-oqhxiah3frx4j7oy7urrlhttcg6couqx
  Stage: 0.18s.  Install: 11.30s.  Post-install: 0.00s.  Total: 11.54s
[+] /users/example/spack/linux-zen4/gmake-4.4.1-oqhxiah3frx4j7oy7urrlhttcg6couqx
==> No binary for zlib-ng-2.2.4-xlbwd4mxozwnrbxmnw5y2vfv6ja7jbmn found: installing from source
==> Installing zlib-ng-2.2.4-xlbwd4mxozwnrbxmnw5y2vfv6ja7jbmn [6/7]
==> Fetching https://mirror.spack.io/_source-cache/archive/a7/a73343c3093e5cdc50d9377997c3815b878fd110bf6511c2c7759f2afb90f5a3.tar.gz
    [100%]    2.42 MB @   36.5 MB/s
==> No patches needed for zlib-ng
==> zlib-ng: Executing phase: 'autoreconf'
==> zlib-ng: Executing phase: 'configure'
==> zlib-ng: Executing phase: 'build'
==> zlib-ng: Executing phase: 'install'
==> zlib-ng: Successfully installed zlib-ng-2.2.4-xlbwd4mxozwnrbxmnw5y2vfv6ja7jbmn
  Stage: 0.19s.  Autoreconf: 0.00s.  Configure: 3.43s.  Build: 1.07s.  Install: 0.09s.  Post-install: 0.01s.  Total: 4.94s
[+] /users/example/spack/linux-zen4/zlib-ng-2.2.4-xlbwd4mxozwnrbxmnw5y2vfv6ja7jbmn
==> No binary for pigz-2.8-q6uzabdufqoqobe4oialctrdf3sfkxdv found: installing from source
==> Installing pigz-2.8-q6uzabdufqoqobe4oialctrdf3sfkxdv [7/7]
==> Fetching https://mirror.spack.io/_source-cache/archive/2f/2f7f6a6986996d21cb8658535fff95f1c7107ddce22b5324f4b41890e2904706.tar.gz
    [100%]  128.77 KB @    5.6 MB/s
==> No patches needed for pigz
==> pigz: Executing phase: 'edit'
==> pigz: Executing phase: 'build'
==> pigz: Executing phase: 'install'
==> pigz: Successfully installed pigz-2.8-q6uzabdufqoqobe4oialctrdf3sfkxdv
  Stage: 0.10s.  Edit: 0.00s.  Build: 1.12s.  Install: 0.00s.  Post-install: 0.00s.  Total: 1.38s
[+] /users/example/spack/linux-zen4/pigz-2.8-q6uzabdufqoqobe4oialctrdf3sfkxdv
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

   ```
   spack install cowsay
   ```

- Find the name of the module

   ```
   $ spack find cowsay
   -- linux-rocky9-zen4 / no compilers -----------------------------
   cowsay@3.04
   ==> 1 installed package
   ```

- Load the software

   ```
   $ spack load cowsay
   ```

- Test the software

   ```
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
