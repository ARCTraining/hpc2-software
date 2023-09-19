# Building a test piece of software

So we've installed Spack, and activated it from the last section.

Let's now try installing a simple piece of software.  Pigz seems like a
reasonable place to start (parallel gzip), as it doesn't have too many
dependencies, so won't take long to try out:

```bash
$ spack install pigz
==> Fetching https://mirror.spack.io/bootstrap/github-actions/v0.4/build_cache/linux-centos7-x86_64-gcc-10.2.1-clingo-bootstrap-spack-idkenmhnscjlu5gjqhpcqa4h7o2a7aow.spec.json
==> Fetching https://mirror.spack.io/bootstrap/github-actions/v0.4/build_cache/linux-centos7-x86_64/gcc-10.2.1/clingo-bootstrap-spack/linux-centos7-x86_64-gcc-10.2.1-clingo-bootstrap-spack-idkenmhnscjlu5gjqhpcqa4h7o2a7aow.spack
==> Installing "clingo-bootstrap@=spack%gcc@=10.2.1~docs~ipo+python+static_libstdcpp build_type=Release arch=linux-centos7-x86_64" from a buildcache
==> Installing zlib-1.2.13-qnajvk7ynalyguulhazkz2wlewqe4ddd
==> No binary for zlib-1.2.13-qnajvk7ynalyguulhazkz2wlewqe4ddd found: installing from source
==> Fetching https://mirror.spack.io/_source-cache/archive/b3/b3a24de97a8fdbc835b9833169501030b8977031bcb54b3b3ac13740f846ab30.tar.gz
==> No patches needed for zlib
==> zlib: Executing phase: 'edit'
==> zlib: Executing phase: 'build'
==> zlib: Executing phase: 'install'
==> zlib: Successfully installed zlib-1.2.13-qnajvk7ynalyguulhazkz2wlewqe4ddd
  Stage: 0.16s.  Edit: 1.79s.  Build: 1.01s.  Install: 0.27s.  Post-install: 0.05s.  Total: 3.45s
[+] /tmp/me/spack/opt/spack/linux-centos7-skylake_avx512/gcc-12.3.0/zlib-1.2.13-qnajvk7ynalyguulhazkz2wlewqe4ddd
==> Installing pigz-2.7-4a2l6sel73vtugtk7d7hmpqfyg6pvzyi
==> No binary for pigz-2.7-4a2l6sel73vtugtk7d7hmpqfyg6pvzyi found: installing from source
==> Fetching https://mirror.spack.io/_source-cache/archive/d2/d2045087dae5e9482158f1f1c0f21c7d3de6f7cdc7cc5848bdabda544e69aa58.tar.gz
==> No patches needed for pigz
==> pigz: Executing phase: 'edit'
==> pigz: Executing phase: 'build'
==> pigz: Executing phase: 'install'
==> pigz: Successfully installed pigz-2.7-4a2l6sel73vtugtk7d7hmpqfyg6pvzyi
  Stage: 0.11s.  Edit: 0.00s.  Build: 2.13s.  Install: 0.01s.  Post-install: 0.03s.  Total: 2.46s
[+] /tmp/me/spack/opt/spack/linux-centos7-skylake_avx512/gcc-12.3.0/pigz-2.7-4a2l6sel73vtugtk7d7hmpqfyg6pvzyi
```

That is quite wordy, but to note, it's installed pigz, and also installed a
dependency of it, zlib.  Just asking for it to install that one package has led
to it downloading and installing another, and it's quite happy doing that
without needing any guidance.

```bash
$ spack find pigz
-- linux-centos7-skylake_avx512 / gcc@12.3.0 --------------------
pigz@2.7
==> 1 installed package
```

At this point we can see there's a version available for pigz.  I can now load
this, and test it to confirm it's worked:

```bash
$ spack load pigz
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
   $ spack find k8
   -- linux-centos7-skylake_avx512 / gcc@12.3.0 --------------------
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
