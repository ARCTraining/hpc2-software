# Recipes

Spack helps you write your own recipes to build software that Spack doesn't
currently know about.  This just shows a basic example of how you might do
this, and isn't intended to be comprehensive.

The full list of buildsystems can be found in the refences section, and we're
just going to run through a few simple examples.  I think the important point
to take away here is that Spack already knows how to build using a number of
common build tools, saving you having to write out raw shell commands to do the
necessary steps.

## Makefile

Some builds are just a simple Makefile, but often aren't well designed to allow
you to install it where you want, or make changes without doing raw edits to
the Makefile.  Let's install a game that uses a simple Makefile, and see how
this can still be quite straightforward.

I've found a game, [nSnake](https://github.com/alexdantas/nSnake), and want to
install it.  I've found the source tar file online, so let's start with
building a config, and just go with the default of checksumming a single
version, by pressing return when prompted.  When this completes, quit the editor.

```bash
$ spack create 'https://github.com/alexdantas/nSnake/archive/refs/tags/v3.0.0.tar.gz'
==> This looks like a URL for nSnake
==> Found 3 versions of nsnake:

  3.0.0  https://github.com/alexdantas/nSnake/archive/refs/tags/v3.0.0.tar.gz
  2.0.0  https://github.com/alexdantas/nSnake/archive/refs/tags/v2.0.0.tar.gz
  1.7    https://github.com/alexdantas/nSnake/archive/refs/tags/v1.7.tar.gz

==> How many would you like to checksum? (default is 1, q to abort)
==> Fetching https://github.com/alexdantas/nSnake/archive/refs/tags/v3.0.0.tar.gz
==> This package looks like it uses the makefile build system
==> Created template for nsnake package
==> Created package file: /home/me/spack/var/spack/repos/builtin/packages/nsnake/package.py
```

Now let's try and build this, and see what happens.

```bash
spack install nsnake
```

This build errors out, with a missing dependency:

```
  >> 40    /usr/bin/ld: cannot find -lyaml-cpp
```

So, how hard is it to add a dependency into spack for this?

```bash
spack edit nsnake
```

You'll see that in the template config, it has:

```python
    # FIXME: Add dependencies if required.
    # depends_on("foo")
```

So let's fix that:

```python
    depends_on("yaml-cpp")
```

So it'll now make sure that yaml-cpp is available for us, but we've not yet
told it how to use it.  If you looked at the Makefile for this project, you'd
see they recommend setting the environment variable LDFLAGS\_PLATFORM for any
additional linker flags required.  So to tell Spack to do this, change the `def
edit` section to look like this:

```python
    def edit(self, spec, prefix):
        env['LDFLAGS_PLATFORM'] = spec['yaml-cpp'].libs.ld_flags
        pass
```

Let's try installing it again:

```bash
spack install nsnake
```

We fail again:

```
install: cannot change permissions of '/usr/bin': Operation not permitted
```

Right, so it's trying to install into /usr/bin, rather than into our Spack
install directory.  With well behaved Makefiles, this will usually just work,
but in this case, we need to fix it.  Reviewing the Makefile, we find that
PREFIX is hardcoded into the Makefile, and can't be adjusted with an
environment variable like we did before.  No problem, Spack lets you edit the
Makefile.  Let's adjust our edit function:

```python
    def edit(self, spec, prefix):
        env['LDFLAGS_PLATFORM'] = spec['yaml-cpp'].libs.ld_flags
        makefile = FileFilter('Makefile')
        makefile.filter('PREFIX\s*=.*', 'PREFIX = ' + prefix)
        pass
```

We've added two lines to modify the Makefile.  This creates a filter on the
Makefile, which replaces the PREFIX line that's currently in there, with our
adjusted one.  Let's build it again:

```bash
spack install nsnake
```

Hurrah.  The software built, and installed.  Shall we test:

```bash
$ module add nsnake
$ nsnake
                                       ┌──────────────────────────────────────────────────────────────────────────────┐
                                       │__    _  _______  __    _  _______  ___   _  _______  ┌Main Menu─────────────┐│
                                       │|  |  | ||       ||  |  | ||   _   ||   | | ||       |│Arcade Mode           ││
                                       │|   |_| ||  _____||   |_| ||  |_|  ||   |_| ||    ___|│Level Select          ││
                                       │|       || |_____ |       ||       ||      _||   |___ │Game Settings         ││
                                       │|  _    ||_____  ||  _    ||       ||     |_ |    ___|│GUI Options           ││
                                       │| | |   | _____| || | |   ||   _   ||    _  ||   |___ │Controls              ││
                                       │|_|  |__||_______||_|  |__||__| |__||___| |_||_______|│Help                  ││
                                       │                   o   o      o o           o         │Quit                  ││
                                       │                   o   o      o o           o         │                      ││
                                       │                   o   o      @ o           o  o      │                      ││
                                       │                  oo   o        o         o oo o      │                      ││
                                       │                  oo   o        o         o oo o      │                      ││
                                       │                  oo   o        @         o oo o      │                      ││
                                       │            o     oo   o                  o oo o      │                      ││
                                       │            o     oo   o                  oooo o      │                      ││
                                       │            o   o o@   o                  oooo o      │                      ││
                                       │            o   o o    @                  ooo@ o      │                      ││
                                       │            o   @ o                       ooo  @      │                      ││
                                       │            o     @                       ooo         │                      ││
                                       │            oo    @                       @o@         │                      ││
                                       │            oo                             @          │                      ││
                                       │   o        oo                                        └──────────────────────┘│
                                       └──────────────────────────────────────────────────────────────────────────────┘
```

I think we can declare success!

Now, when you think what we've done here, that's quite impressive.  We've found
a piece of software on the Internet, and created a build config for it that
includes downloading, building, and installing not only the software we wanted,
but also a dependency, and installed both that software and its depdendencies
with a single command.  We were also deliberately lazy, and didn't bother
reading the installation instructions, and just blundered through without
trying to get it right first time.  I'm not recommending that approach, but
sometimes the installation documenation is pretty much non existent, so it's
good to be able to work this way.

## CMake

Let's just pick on a simple example CMake project, to show how easy life could
be.

```bash
spack create -t cmake https://github.com/maks-it/CMake-Tutorial
```

That creates a template configuration, that just needs a couple of edits.  It
highlights sections it thinks you should  fix, with FIXME sections:

```python
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

```python
version("develop", branch="master")
```

This way it knows to use the master branch to build a "develop" release.  This
the leaves us with this minimal config:

```python
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

```bash
spack install cmake-tutorial
```

Then we can prove it worked afterwards, by loading the module and running it:

```bash
$ module add cmake-tutorial
$ Tutorial 10
Computing sqrt of 10 to be 3.16228 using log
The square root of 10 is 3.16228
```

Further details can be found under the
[CMake section](https://spack-tutorial.readthedocs.io/en/latest/tutorial_buildsystems.html#cmake)
on the Spack website.

## Autotools

Here's a similarly trivial Autotools based example.  We're picking the autoreconf type, as this includes other typical steps required when using git sourced Autotools build packages:

```bash
spack create -t autoreconf https://github.com/lloydroc/autotools-example
```

We make the same basic edits done in Cmake, to tell it we're using git, and which branch to use for the `develop` version.  This leaves us with:

```python
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

```bash
spack install autotools-example
```

Then we can prove it worked afterwards, by loading the module and running it:

```bash
$ module add autotools-example
$ jupiter
hello jupiter
```
