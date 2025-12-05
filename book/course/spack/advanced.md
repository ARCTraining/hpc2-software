# Advanced software install

Spack takes a number of decisions itself about how to build software, and what
dependencies to use.  You're able to adjust some of this via variants within an
individual package, but also you can tweak the dependency decisions, to use
different packages if you know better than the defaults.

(advanced:spack:variants)=
## Variants

Variants are where the package has options how to build it.  Let's pick on
`clingo` as an example.  You can get information on a package in Spack, which
will tell you about how it can be built:

```bash
$ spack info clingo
CMakePackage:   clingo

Description:
    Clingo: A grounder and solver for logic programs Clingo is part of the
    Potassco project for Answer Set Programming (ASP). ASP offers a simple
    and powerful modeling language to describe combinatorial problems as
    logic programs. The clingo system then takes such a logic program and
    computes answer sets representing solutions to the given problem.

Homepage: https://potassco.org/clingo/

Preferred version:
    5.7.1      https://github.com/potassco/clingo/archive/v5.7.1.tar.gz

Safe versions:
    develop    [git] https://github.com/potassco/clingo.git on branch wip-20
    master     [git] https://github.com/potassco/clingo.git on branch master
    5.8.0      https://github.com/potassco/clingo/archive/v5.8.0.tar.gz
    5.7.1      https://github.com/potassco/clingo/archive/v5.7.1.tar.gz
    5.6.2      https://github.com/potassco/clingo/archive/v5.6.2.tar.gz
    5.5.2      https://github.com/potassco/clingo/archive/v5.5.2.tar.gz
    5.4.1      https://github.com/potassco/clingo/archive/v5.4.1.tar.gz
    spack      [git] https://github.com/potassco/clingo.git at commit 2a025667090d71b2c9dce60fe924feb6bde8f667

Deprecated versions:
    5.7.0      https://github.com/potassco/clingo/archive/v5.7.0.tar.gz
    5.5.1      https://github.com/potassco/clingo/archive/v5.5.1.tar.gz
    5.5.0      https://github.com/potassco/clingo/archive/v5.5.0.tar.gz
    5.4.0      https://github.com/potassco/clingo/archive/v5.4.0.tar.gz
    5.3.0      https://github.com/potassco/clingo/archive/v5.3.0.tar.gz
    5.2.2      https://github.com/potassco/clingo/archive/v5.2.2.tar.gz

Variants:
    apps [true]                 false, true
        build command line applications

    build_system [cmake]        cmake
        Build systems supported by the package

    build_type [Release]        Debug, MinSizeRel, RelWithDebInfo, Release
      when  build_system=cmake
        CMake build type

    docs [false]                false, true
        build documentation with Doxygen

    generator [make]            none
      when  build_system=cmake
        the build system generator to use

    ipo [false]                 false, true
      when  build_system=cmake
        CMake interprocedural optimization

    python [true]               false, true
        build with python bindings


Dependencies:
    bison@2.5:              build
      when @5.6:5.8,master platform=linux

    bison@2.5:              build
      when @5.6:5.8,master platform=darwin

    bison@2.5:              build
      when @5.6:5.8,master platform=freebsd

    bison@2.5:              build
      when @spack platform=linux

    bison@2.5:              build
      when @spack platform=darwin

    bison@2.5:              build
      when @spack platform=freebsd

    c                       build

    cmake@:3                build
      when @:5.7

    cmake@3.1:              build

    cmake@3.18:             build
      when @5.5:

    cmake@3.22.1:           build
      when @develop

    cxx                     build

    doxygen                 build
      when +docs

    gmake                   build
      when  build_system=cmake generator=make

    ninja                   build
      when  build_system=cmake generator=ninja

    py-cffi@1.14:           build, run
      when @5.5:+python platform=linux

    py-cffi@1.14:           build, run
      when @5.5:+python platform=darwin

    py-cffi@1.14:           build, run
      when @5.5:+python platform=freebsd

    python@3.6:             build, link, run
      when +python

    python-venv             build, run
      when +python

    re2c@0.13:              build

    re2c@0.13:              build
      when @spack

    re2c@0.13:              build
      when  platform=windows

    re2c@1.1.1:             build
      when @5.6:5.8,master

    re2c@2:                 build
      when @develop

    winbison@2.4.12:        build, link
      when  platform=windows


Licenses:
    MIT
```

From this you can see that there's lots of versions available, and there's a
number of variants listed, along with dependencies.  Let's say I wanted to
build version 5.7.0, and we want to build the documentation, not build
python bindings, and with a build\_type of Debug.  Basically, be as awkward as
possible and change everything:

```bash
spack install --deprecated clingo@5.7.0+docs~python build_type=Debug
```

There we're just turned on a feature (docs), turned off a feature (python), and
told it we want the Debug build type.  Have a look through the references to
get further information, but this is actually the basics of choosing variants
of packages.

Even worse here, we're choosing to install a version that Spack really
recommends against.  Deprecated software potentially has been marked as such
because of security issues, so you likely want to avoid these, but if you know
best `--deprecated` allows you to install them anyway.  I'll revert to a more
sensible recommended version in the later examples!

## Compilers

You may want to build a package with a different compiler, that you already
have configured.

```bash
spack install clingo@5.7.1+docs~python %gcc@14.2.0
```

Now Spack will happily rebuild that specific variant you've asked for, with the
compiler you've asked for.

## Dependencies

You can also specify particular versions and variants of dependencies.  We can
take our previous build even further:

```bash
spack install clingo@5.7.1+docs~python build_type=Release ^bison@3.8.1 %gcc@14.2.0
```

This would build clingo with documentation, without Python bindings, choosing the
release build of it, using the GCC 14.2.0 compiler to build the bison
dependency.  When selecting dependencies, there's a subtle difference between
`%` and `^`.  `%` means a direct dependency (build perl with the gcc@14.2.0
compiler), whereas `^` means a transitive depedency, so when building libtiff,
and you're satisfying dependencies, use this package.

Take a look at the references for further reading on how this all works, along
with even more ways of defining these.

## Previewing a software install

Before installing a piece of software, you can review what Spack is planning on
doing, and which dependencies it's going to rely on.

```bash
$ spack spec atop %gcc@14.2.0
 -   atop@2.5.0 build_system=generic platform=linux os=rocky9 target=zen4 %c=gcc@14.2.0
[+]      ^compiler-wrapper@1.0 build_system=generic platform=linux os=rocky9 target=zen4
[e]      ^gcc@14.2.0~binutils+bootstrap~graphite~mold~nvptx~piclibs~profiled~strip build_system=autotools build_type=RelWithDebInfo languages:='c,c++,fortran' platform=linux os=rocky9 target=x86_64
[+]      ^gcc-runtime@14.2.0 build_system=generic platform=linux os=rocky9 target=zen4
[e]      ^glibc@2.34 build_system=autotools platform=linux os=rocky9 target=x86_64
[+]      ^gmake@4.4.1~guile build_system=generic platform=linux os=rocky9 target=zen4 %c=gcc@11.4.1
[e]          ^gcc@11.4.1~binutils+bootstrap~graphite~nvptx~piclibs~profiled~strip build_system=autotools build_type=RelWithDebInfo languages:='c,c++' platform=linux os=rocky9 target=x86_64
[+]          ^gcc-runtime@11.4.1 build_system=generic platform=linux os=rocky9 target=zen4
[+]      ^ncurses@6.5-20250705~symlinks+termlib abi=none build_system=autotools patches:=7a351bc platform=linux os=rocky9 target=zen4 %c,cxx=gcc@11.4.1
[+]          ^pkgconf@2.5.1 build_system=autotools platform=linux os=rocky9 target=zen4 %c=gcc@11.4.1
[+]      ^zlib-ng@2.2.4+compat+new_strategies+opt+pic+shared build_system=autotools platform=linux os=rocky9 target=zen4 %c,cxx=gcc@11.4.1
```

This way you can verify which versions it's expecting to use, and can adjust
your installation parameters if anything needs changing.

## Notes

In some instances you may find that Spack's desire to reuse existing modules
doesn't fit with what you want, and you want it to rebuild exactly as you've
asked.  In that instance, you can simply add the argument `--fresh` which tells
Spack to work out dependencies ignoring what's already been built, and select
what it thinks is optimal.

## Exercise

Install tmux version 3.2a with UTF-8 support.

<details>
<summary>Click here to reveal solution</summary>

### Solution

First you need to look at the info for tmux, to find out what variants are
available.  Some of the output below has been truncated for brevity:

```bash
$ spack info tmux
AutotoolsPackage:   tmux

Description:
    Tmux is a terminal multiplexer. What is a terminal multiplexer? It lets
    you switch easily between several programs in one terminal, detach them
    (they keep running in the background) and reattach them to a different
    terminal. And do a lot more.

Homepage: https://tmux.github.io

Preferred version:
    3.5a      https://github.com/tmux/tmux/releases/download/3.5a/tmux-3.5a.tar.gz

Safe versions:
    master    [git] https://github.com/tmux/tmux.git on branch master
    3.5a      https://github.com/tmux/tmux/releases/download/3.5a/tmux-3.5a.tar.gz
    3.5       https://github.com/tmux/tmux/releases/download/3.5/tmux-3.5.tar.gz
    3.4       https://github.com/tmux/tmux/releases/download/3.4/tmux-3.4.tar.gz
    3.3a      https://github.com/tmux/tmux/releases/download/3.3a/tmux-3.3a.tar.gz
    3.3       https://github.com/tmux/tmux/releases/download/3.3/tmux-3.3.tar.gz
    3.2a      https://github.com/tmux/tmux/releases/download/3.2a/tmux-3.2a.tar.gz
    3.2       https://github.com/tmux/tmux/releases/download/3.2/tmux-3.2.tar.gz
    3.1c      https://github.com/tmux/tmux/releases/download/3.1c/tmux-3.1c.tar.gz
    3.1b      https://github.com/tmux/tmux/releases/download/3.1b/tmux-3.1b.tar.gz
    3.1a      https://github.com/tmux/tmux/releases/download/3.1a/tmux-3.1a.tar.gz
    3.1       https://github.com/tmux/tmux/releases/download/3.1/tmux-3.1.tar.gz
    3.0a      https://github.com/tmux/tmux/releases/download/3.0a/tmux-3.0a.tar.gz
    3.0       https://github.com/tmux/tmux/releases/download/3.0/tmux-3.0.tar.gz
    2.9a      https://github.com/tmux/tmux/releases/download/2.9a/tmux-2.9a.tar.gz
    2.9       https://github.com/tmux/tmux/releases/download/2.9/tmux-2.9.tar.gz
    2.8       https://github.com/tmux/tmux/releases/download/2.8/tmux-2.8.tar.gz
    2.7       https://github.com/tmux/tmux/releases/download/2.7/tmux-2.7.tar.gz
    2.6       https://github.com/tmux/tmux/releases/download/2.6/tmux-2.6.tar.gz
    2.5       https://github.com/tmux/tmux/releases/download/2.5/tmux-2.5.tar.gz
    2.4       https://github.com/tmux/tmux/releases/download/2.4/tmux-2.4.tar.gz
    2.3       https://github.com/tmux/tmux/releases/download/2.3/tmux-2.3.tar.gz
    2.2       https://github.com/tmux/tmux/releases/download/2.2/tmux-2.2.tar.gz
    2.1       https://github.com/tmux/tmux/releases/download/2.1/tmux-2.1.tar.gz
    1.9a      https://github.com/tmux/tmux/releases/download/1.9a/tmux-1.9a.tar.gz

Deprecated versions:
    None

Variants:
    build_system [autotools]        autotools
        Build systems supported by the package

    jemalloc [false]                false, true
      when @3.5:
        Use jemalloc for memory allocation

    static [false]                  false, true
        Create a static build

    utf8proc [false]                false, true
        Build with UTF-8 support from utf8proc library


Dependencies:
    autoconf         build, link
      when @master

    automake         build, link
      when @master

    c                build

    gmake            build
      when  build_system=autotools

    gnuconfig        build
      when  build_system=autotools target=ppc64le:

    gnuconfig        build
      when  build_system=autotools target=aarch64:

    gnuconfig        build
      when  build_system=autotools target=riscv64:

    jemalloc         build, link
      when +jemalloc

    libevent         build, link

    ncurses          build, link

    pkgconfig        build

    utf8proc         build, link
      when +utf8proc

    yacc             build
      when @3:


Licenses:
    ISC
```

You can see from this that there is indeed a version 3.2a available, and to add
UTF-8 support, you need to enable the utf8proc feature.  That then gives an
install command of:

```bash
spack install tmux@3.2a+utf8proc
```

If you've installed this and want to verify it's worked:

```bash
$ spack load tmux
$ tmux -V
tmux 3.2a
$ spack find -v tmux
-- linux-rocky9-zen4 / %c=gcc@11.4.1 ----------------------------
tmux@3.2a~static+utf8proc build_system=autotools patches:=c1b61a1
==> 1 installed package
```

You can see from this that we're using the shiny new 3.2a version, and can
confirm that it was built with UTF-8 support included.

</details>

## References

- [Basic Usage](https://spack.readthedocs.io/en/latest/package_fundamentals.html)
- [Installing packages](https://spack-tutorial.readthedocs.io/en/latest/tutorial_basics.html#installing-packages)
- [Module tutorial](https://spack-tutorial.readthedocs.io/en/latest/tutorial_modules.html)
- [Spec Syntax](https://spack.readthedocs.io/en/latest/spec_syntax.html)
