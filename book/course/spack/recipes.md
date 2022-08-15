## Recipe

Spack helps you write your own recipes to build software that Spack doesn't
currently know about.  This just shows a basic example of how you might do
this, and isn't intended to be comprehensive.

The full list of buildsystems can be found in the refences section, and we're
just going to run through a few simple examples.  I think the important point
to take away here is that Spack already knows how to build using a number of
common build tools, saving you having to write out raw shell commands to do the
necessary steps.

### CMake

Let's just pick on a simple example CMake project, to show how easy life could
be.

```
$ spack create -t cmake https://github.com/maks-it/CMake-Tutorial
```

That creates a template configuration, that just needs a couple of edits.  It highlights sections it thinks you should  fix, with FIXME sections:

```
    # FIXME: Add a proper url for your package's homepage here.
    homepage = "https://www.example.com"
    url = "https://github.com/maks-it/CMake-Tutorial"

    # FIXME: Add a list of GitHub accounts to
    # notify when the package is updated.
    # maintainers = ["github_user1", "github_user2"]

    # FIXME: Add proper versions and checksums here.
    # version("1.2.3", "0123456789abcdef0123456789abcdef")

    # FIXME: Add dependencies if required.
    # depends_on("foo")

    def cmake_args(self):
        # FIXME: Add arguments other than
        # FIXME: CMAKE_INSTALL_PREFIX and CMAKE_BUILD_TYPE
        # FIXME: If not needed delete this function
```

I'm not going to fix all of these, but just do the bare minimum for now.
Change `url =` to `git =` since we're just using git and haven't got a tarball
containing this release.  Also define a version:

```
version("develop", branch="master")
```
This way it knows to use the master branch to build a "develop" release.  This the leaves us with this minimal config:

```
# Copyright 2013-2022 Lawrence Livermore National Security, LLC and other
# Spack Project Developers. See the top-level COPYRIGHT file for details.
#
# SPDX-License-Identifier: (Apache-2.0 OR MIT)

from spack.package import *

class CmakeTutorial(CMakePackage):
    """Demo package to show how to use Cmake"""

    homepage = "https://github.com/maks-it/CMake-Tutorial"
    git = "https://github.com/maks-it/CMake-Tutorial"

    version("develop", branch="master")
```

That really is it, for a minimal CMake package.  You can install this with:

```
$ spack install cmake-tutorial
```

Then we can prove it worked afterwards, by loading the module and running it:

```
$ module add cmake-tutorial
$ Tutorial 10
Computing sqrt of 10 to be 3.16228 using log
The square root of 10 is 3.16228
```

Further details can be found under the (CMake section)[https://spack-tutorial.readthedocs.io/en/latest/tutorial_buildsystems.html#cmake] on the Spack website.

### Autotools example

Here's a similarly trivial Autotools based example.  We're picking the autoreconf type, as this includes other typical steps required when using git sourced Autotools build packages:

```
$ spack create -t autoreconf https://github.com/lloydroc/autotools-example
```

We make the same basic edits done in Cmake, to tell it we're using git, and which branch to use for the `develop` version.  This leaves us with:

```
# Copyright 2013-2022 Lawrence Livermore National Security, LLC and other
# Spack Project Developers. See the top-level COPYRIGHT file for details.
#
# SPDX-License-Identifier: (Apache-2.0 OR MIT)

from spack.package import *


class AutotoolsExample(AutotoolsPackage):
    """Demo package to show how to use Autotools"""

    homepage = "https://github.com/lloydroc/autotools-example"
    git = "https://github.com/lloydroc/autotools-example"

    version("develop", branch="master")

    depends_on("autoconf", type="build")
    depends_on("automake", type="build")
    depends_on("libtool", type="build")
    depends_on("m4", type="build")

    def autoreconf(self, spec, prefix):
        autoreconf("--install", "--verbose", "--force")
```

You can install this with:
```
$ spack install --overwrite autotools-example
```

Then we can prove it worked afterwards, by loading the module and running it:

```
$ module add autotools-example
$ jupiter
hello jupiter
```
