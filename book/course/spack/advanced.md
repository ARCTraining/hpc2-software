# Advanced software install

Spack takes a number of decisions itself about how to build software, and what
dependencies to use.  You're able to adjust some of this via variants within an
individual package, but also you can tweak the dependency decisions, to use
different packages if you know better than the defaults.

(advanced:spack:variants)=
## Variants

Variants are where the package has options how to build it.  Let's pick on
`libtiff` as a nice example.  You can get information on a package in Spack,
which will tell you about how it can be built:

```bash
$ spack info libtiff
CMakePackage:   libtiff

Description:
    LibTIFF - Tag Image File Format (TIFF) Library and Utilities.

Homepage: http://www.simplesystems.org/libtiff/

Preferred version:
    4.5.0     https://download.osgeo.org/libtiff/tiff-4.5.0.tar.gz

Safe versions:
    4.5.0     https://download.osgeo.org/libtiff/tiff-4.5.0.tar.gz
    4.4.0     https://download.osgeo.org/libtiff/tiff-4.4.0.tar.gz
    4.3.0     https://download.osgeo.org/libtiff/tiff-4.3.0.tar.gz
    4.2.0     https://download.osgeo.org/libtiff/tiff-4.2.0.tar.gz
    4.1.0     https://download.osgeo.org/libtiff/tiff-4.1.0.tar.gz
    4.0.10    https://download.osgeo.org/libtiff/tiff-4.0.10.tar.gz
    4.0.9     https://download.osgeo.org/libtiff/tiff-4.0.9.tar.gz
    4.0.8     https://download.osgeo.org/libtiff/tiff-4.0.8.tar.gz
    4.0.7     https://download.osgeo.org/libtiff/tiff-4.0.7.tar.gz
    4.0.6     https://download.osgeo.org/libtiff/tiff-4.0.6.tar.gz
    4.0.5     https://download.osgeo.org/libtiff/tiff-4.0.5.tar.gz
    4.0.4     https://download.osgeo.org/libtiff/tiff-4.0.4.tar.gz
    3.9.7     https://download.osgeo.org/libtiff/tiff-3.9.7.tar.gz

Deprecated versions:
    None

Variants:
    Name [Default]          When                              Allowed values          Description
    ====================    ==============================    ====================    ===============================================

    build_system [cmake]    --                                cmake, autotools        Build systems supported by the package
    build_type [Release]    [build_system=cmake]              Debug, Release,         CMake build type
                                                              RelWithDebInfo,
                                                              MinSizeRel
    ccitt [on]              --                                on, off                 support for CCITT Group 3 & 4 algorithms
    generator [make]        [build_system=cmake]              ninja,make              the build system generator to use
    ipo [off]               [build_system=cmake               on, off                 CMake interprocedural optimization
                            ^cmake@3.9:]
    jbig [off]              --                                on, off                 use ISO JBIG compression
    jpeg [on]               --                                on, off                 use libjpeg
    jpeg12 [off]            [@4:]                             on, off                 enable libjpeg 8/12-bit dual mode
    lerc [off]              [@4.3:]                           on, off                 use libLerc
    libdeflate [off]        [@4.2:]                           on, off                 use libdeflate
    logluv [on]             --                                on, off                 support for LogLuv high dynamic range algorithm
    lzma [off]              [@4:]                             on, off                 use liblzma
    lzw [on]                --                                on, off                 support for LZW algorithm
    next [on]               --                                on, off                 support for NeXT 2-bit RLE algorithm
    old-jpeg [off]          --                                on, off                 support for Old JPEG compression
    packbits [on]           --                                on, off                 support for Macintosh PackBits algorithm
    pixarlog [off]          --                                on, off                 support for Pixar log-format algorithm
    thunder [on]            --                                on, off                 support for ThunderScan 4-bit RLE algorithm
    webp [off]              [@4.0.10:]                        on, off                 use libwebp
    zlib [on]               --                                on, off                 use zlib
    zstd [off]              [@4.0.10:]                        on, off                 use libzstd

Build Dependencies:
    cmake  gmake  gnuconfig  jbigkit  jpeg  lerc  libwebp  ninja  xz  zlib  zstd

Link Dependencies:
    jbigkit  jpeg  lerc  libwebp  xz  zlib  zstd

Run Dependencies:
    None
```

From this you can see that there's lots of versions available, and there's a
number of variants listed, along with dependencies.  Let's say I wanted to
build version 4.4.0, and we want webp support to be on, jpeg support to be off,
and a build\_type of Release.  Basically, be as awkward as possible and change
everything:

```bash
spack install libtiff @4.4.0 +webp -jpeg build_type=Release
```

There we're just turned on a feature (webp), turned off a feature (jpeg), and
told it we want the Release build type.  Have a look through the references to
get further information, but this is actually the basics of choosing variants
of packages.

## Compilers

You may want to build a package with a different compiler, that you already
have configured.

```bash
spack install libtiff @4.4.0 +webp -jpeg build_type=Release %gcc@4.8.5
```

Now Spack will happily rebuild that specific variant you've asked for, with the
compiler you've asked for.

## Dependencies

You can also specify particular versions and variants of dependencies.  We can
take our previous build even further:

```bash
spack install libtiff @4.4.0 +webp -jpeg build_type=Release ^perl@5.30.0 %gcc@4.8.5
```

This would build using Perl 5.30.0, without JPEG support, and with WebP
support, using the GNU 4.8.5 compiler.  Note that we have used a ^ to say which
version of perl we want to use because it's a package that we depend on,
whereas we have to use % to say which compiler to use.

## Previewing a software install

Before installing a piece of software, you can review what Spack is planning on
doing, and which dependencies it's going to rely on.

```bash
$ spack spec atop
Input spec
--------------------------------
atop

Concretized
--------------------------------
atop@2.5.0%gcc@12.3.0 build_system=generic arch=linux-centos7-skylake_avx512
    ^ncurses@6.4%gcc@12.3.0~symlinks+termlib abi=none build_system=autotools arch=linux-centos7-skylake_avx512
        ^pkgconf@1.9.5%gcc@12.3.0 build_system=autotools arch=linux-centos7-skylake_avx512
    ^zlib@1.2.13%gcc@12.3.0+optimize+pic+shared build_system=makefile arch=linux-centos7-skylake_avx512
```

This way you can verify which versions it's expecting to use, and can adjust
your installation parameters if anything needs changing.

## Notes

In some instances you may find that Spack's desire to reuse existing modules
doesn't fit with what you want, and you want it to rebuild exactly as you've
asked.  In that instance, you can simply add the argument `--fresh` which tells
Spack to work out dependencies ignoring what's already been built, and select
what it thinks is optimal.

You'll note in our section on simplifying the modules system, we simplified it
to the point that the naming scheme used was name/version.  This loses all
details about variants, making it impossible to distinguish between variants.
If you only ever build one particular instance of a module, this isn't a
problem, but if you're wanting to switch between multiple variants, or with
different compilers, you'll either want to stick with the original scheme, or
look at alternative naming schemes that can capture the necessary detail.

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
    3.3a      https://github.com/tmux/tmux/releases/download/3.3a/tmux-3.3a.tar.gz

Safe versions:
    master    [git] https://github.com/tmux/tmux.git on branch master
    3.3a      https://github.com/tmux/tmux/releases/download/3.3a/tmux-3.3a.tar.gz
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
    Name [Default]              When    Allowed values    Description
    ========================    ====    ==============    ==============================================

    build_system [autotools]    --      autotools         Build systems supported by the package
    static [off]                --      on, off           Create a static build
    utf8proc [off]              --      on, off           Build with UTF-8 support from utf8proc library

Build Dependencies:
    autoconf  automake  gnuconfig  libevent  ncurses  pkgconfig  utf8proc

Link Dependencies:
    autoconf  automake  libevent  ncurses  utf8proc

Run Dependencies:
    None
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
tmux 3.2
$ spack find -v tmux
-- linux-centos7-skylake_avx512 / gcc@12.3.0 --------------------
tmux@3.2a~static+utf8proc build_system=autotools
==> 1 installed package
```

You can see from this that we're using the shiny new 3.2 version, and can
confirm that it was built with UTF-8 support included.

</details>

## References

- [Basic Usage](https://spack.readthedocs.io/en/latest/basic_usage.html)
- [Installing packages](https://spack-tutorial.readthedocs.io/en/latest/tutorial_basics.html#installing-packages)
- [Module tutorial](https://spack-tutorial.readthedocs.io/en/latest/tutorial_modules.html)
