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
building a config, and just go with the default of checksumming of the listed
versions, by pressing `c` when prompted.  When this completes, quit the editor.

```bash
$ spack create 'https://github.com/alexdantas/nSnake/archive/refs/tags/v3.0.0.tar.gz'
==> This looks like a URL for nSnake
==> Selected 3 versions
  3.0.0  https://github.com/alexdantas/nSnake/archive/refs/tags/v3.0.0.tar.gz
  2.0.0  https://github.com/alexdantas/nSnake/archive/refs/tags/v2.0.0.tar.gz
  1.7    https://github.com/alexdantas/nSnake/archive/refs/tags/v1.7.tar.gz

==> Enter number of versions to take, or use a command:
    [c]hecksum  [e]dit  [f]ilter  [a]sk each  [n]ew only  [r]estart  [q]uit
action> c
==> Fetching https://github.com/alexdantas/nSnake/archive/refs/tags/v3.0.0.tar.gz
==> Fetching https://github.com/alexdantas/nSnake/archive/refs/tags/v1.7.tar.gz
==> Fetching https://github.com/alexdantas/nSnake/archive/refs/tags/v2.0.0.tar.gz
==> This package looks like it uses the makefile build system
==> Created template for nsnake package
==> Created package file: /users/example/spack/var/spack/repos/builtin/packages/nsnake/package.py
```

Now let's try and build this, and see what happens.

```bash
spack install nsnake
```

This build errors out, with a missing dependency:

```
  >> 13    src/Interface/Ncurses.hpp:5:10: fatal error: ncurses.h: No such file or directory
```

So, how hard is it to add a dependency into spack for this, and adjust the
build process to use it?

```bash
spack edit nsnake
```

You'll see that in the template config, it has:

```python
    # FIXME: Add dependencies if required.
    # depends_on("foo")
```

So let's fix that and add in a dependency for ncurses:

```python
    depends_on("ncurses")
```

Let's try installing it again:

```bash
spack install nsnake
```

This build errors out, with a missing dependency:

```
  >> 47    /usr/bin/ld: cannot find -lyaml-cpp
```

Let's edit it again:

```bash
spack edit nsnake
```

We'll add in another line:
```python
    depends_on("yaml-cpp")
```

Another attempt to install it, surely it'll build now:

```bash
spack install nsnake
```

We fail again:

```
  >> 44    /usr/bin/ld: src/Interface/Ncurses.o: undefined reference to symbol 'cbreak@@@@NCURSES6_TINFO_5.0.19991023'
```

Blame me now for trying to build a game that's ten years old.  It doesn't use
ncurses properly and fails to link a required library.  But believe it or not,
there's an option within the Spack recipe for ncurses that let's us avoid
needing this library!  Let's tweak it again:

```python
    depends_on("ncurses~termlib")
```

Another attempt to install it:

```bash
spack install nsnake
```

We fail again!:

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
$ spack load nsnake
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

### Notes on setting other variables

You may find that software doesn't pick up the new libraries you've added, and
needs more guidance.  If you looked at the Makefile for this project, you'd see
they recommend setting the environment variable LDFLAGS\_PLATFORM for any
additional linker flags required.  So you could tell Spack to do this, change
the `def edit` section to look like this:

```python
    def edit(self, spec, prefix):
        env['LDFLAGS_PLATFORM'] = spec['yaml-cpp'].libs.ld_flags
        pass
```

For those unsure of what LDFLAGS, linkers, binpaths, or anything else here is
talking about, please refer to the [theory section](../theory).

This isn't needed for this particular software because Spack tries to
intervene and hardcode this links to libraries where it can, but it's included
here as a note.

### Summary

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
class CmakeTutorial(CMakePackage):
    """FIXME: Put a proper description of your package here."""

    # FIXME: Add a proper url for your package's homepage here.
    homepage = "https://www.example.com"
    url = "https://github.com/maks-it/CMake-Tutorial"

    # FIXME: Add a list of GitHub accounts to
    # notify when the package is updated.
    # maintainers("github_user1", "github_user2")

    # FIXME: Add the SPDX identifier of the project's license below.
    # See https://spdx.org/licenses/ for a list. Upon manually verifying
    # the license, set checked_by to your Github username.
    license("UNKNOWN", checked_by="github_user1")

    # FIXME: Add proper versions and checksums here.
    # version("1.2.3", md5="0123456789abcdef0123456789abcdef")

    # FIXME: Add dependencies if required.
    # depends_on("foo")

    def cmake_args(self):
        # FIXME: Add arguments other than
        # FIXME: CMAKE_INSTALL_PREFIX and CMAKE_BUILD_TYPE
        # FIXME: If not needed delete this function
        args = []
        return args
```

I'm not going to fix all of these, but just do pretty much the bare minimum
for now.  Change `url =` to `git =` since we're just using git and haven't got
a tarball containing this release, and fix the homepage.  Also define a
version:

```python
version("develop", branch="master")
```

This way it knows to use the master branch to build a "develop" release.  This
the leaves us with this minimal config:

```python
# Copyright 2013-2024 Lawrence Livermore National Security, LLC and other
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
$ spack load cmake-tutorial
$ Tutorial 10
Computing sqrt of 10 to be 3.16228 using log
The square root of 10 is 3.16228
```

Further details can be found under the
[CMake section](https://spack-tutorial.readthedocs.io/en/latest/tutorial_buildsystems.html#cmake)
on the Spack website.

## Autotools

Here's a similarly trivial Autotools based example.  We're picking the
autoreconf type, as this includes other typical steps required when using git
sourced Autotools build packages:

```bash
spack create -t autoreconf https://github.com/lloydroc/autotools-example
```

We make the same basic edits done in the Cmake section, to tell it we're using
git, and which branch to use for the `develop` version.  This leaves us with:

```python
# Copyright 2013-2024 Lawrence Livermore National Security, LLC and other
# Spack Project Developers. See the top-level COPYRIGHT file for details.
#
# SPDX-License-Identifier: (Apache-2.0 OR MIT)

from spack.package import *

class AutotoolsExample(AutotoolsPackage):
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
$ spack load autotools-example
$ jupiter
hello jupiter
```

## Summary

Here we're shown you three different ways of building recipes to build software
using common build tools.  The effort required is relatively small, and you now
have the ability to build each of these with a custom compiler, along with
specific versions of any other dependencies.
