# Autotools

Autotools is the name for the set of tools that make up the GNU Build System.
When you download a piece of software, and you can see `configure`,
`configure.in` or `configure.ac` you're looking at something setup to use
Autotools.  The workflow for building this sort of software tends to be quite
consistent:

- Download a copy of the software
- Generate a configure script if missing
- Check out the configure script to see what options there are
- Run the configure script with your chosen options
- Build
- Install

Let's run through an example piece of software showing each of these steps.
I'm picking on something from GitHub that includes all these steps.

## Download your software

This could be downloading and extracting a tarball:

```
$ wget https://github.com/barricklab/breseq/releases/download/v0.37.1/breseq-0.37.1-Source.tar.gz
$ tar xf breseq-0.37.1-Source.tar.gz
$ cd breseq-0.37.1
$ ls configure
configure
```

Here we can see that there's already a configure script present.  But what if
we do that same again but rather than use a released version, get the latest
version from git?

```
$ git clone https://github.com/barricklab/breseq
$ cd breseq
$ ls configure
ls: cannot access 'configure': No such file or directory
```

## Generate a configure script if missing

With this git example, we see there is no configure script present, so we need
to generate it.  Sometimes you're provided with a bootstrap.sh or autogen.sh,
as is the case here.  So we just run it:

```bash
$ ./bootstrap.sh
```

````{admonition} View full output
:class: dropdown
```
autoreconf: Entering directory `.'
autoreconf: configure.ac: not using Gettext
autoreconf: running: aclocal --force -I aux_build/m4
autoreconf: configure.ac: tracing
autoreconf: running: libtoolize --copy --force
libtoolize: putting auxiliary files in AC_CONFIG_AUX_DIR, 'aux_build'.
libtoolize: copying file 'aux_build/ltmain.sh'
libtoolize: putting macros in AC_CONFIG_MACRO_DIRS, 'aux_build/m4'.
libtoolize: copying file 'aux_build/m4/libtool.m4'
libtoolize: copying file 'aux_build/m4/ltoptions.m4'
libtoolize: copying file 'aux_build/m4/ltsugar.m4'
libtoolize: copying file 'aux_build/m4/ltversion.m4'
libtoolize: copying file 'aux_build/m4/lt~obsolete.m4'
autoreconf: running: /usr/bin/autoconf --force
autoreconf: running: /usr/bin/autoheader --force
autoreconf: running: automake --add-missing --copy --force-missing
configure.ac:28: installing 'aux_build/compile'
configure.ac:28: installing 'aux_build/config.guess'
configure.ac:28: installing 'aux_build/config.sub'
configure.ac:29: installing 'aux_build/install-sh'
configure.ac:29: installing 'aux_build/missing'
Makefile.am: installing './INSTALL'
extern/samtools-1.3.1/Makefile.am: installing 'aux_build/depcomp'
autoreconf: Leaving directory `.'
```
````

This sorts everything out ready for us to configure it.  If neither of these
scripts are present, you can instead do:

```bash
$ autoreconf -fi
```

````{admonition} View full output
:class: dropdown
```
libtoolize: putting auxiliary files in AC_CONFIG_AUX_DIR, 'aux_build'.
libtoolize: copying file 'aux_build/ltmain.sh'
libtoolize: putting macros in AC_CONFIG_MACRO_DIRS, 'aux_build/m4'.
libtoolize: copying file 'aux_build/m4/libtool.m4'
libtoolize: copying file 'aux_build/m4/ltoptions.m4'
libtoolize: copying file 'aux_build/m4/ltsugar.m4'
libtoolize: copying file 'aux_build/m4/ltversion.m4'
libtoolize: copying file 'aux_build/m4/lt~obsolete.m4'
configure.ac:28: installing 'aux_build/compile'
configure.ac:28: installing 'aux_build/config.guess'
configure.ac:28: installing 'aux_build/config.sub'
configure.ac:29: installing 'aux_build/install-sh'
configure.ac:29: installing 'aux_build/missing'
Makefile.am: installing './INSTALL'
extern/samtools-1.3.1/Makefile.am: installing 'aux_build/depcomp'
```
````

```{note}
This often results in exactly the same outcome, but the authors may have added
other things into the bootstrap script, so if present it's wise to use it.
```

# Configure the software

Running the configure script first lets you find out what options you have
(trimmed down output show below):

```bash
$ ./configure --help
Installation directories:
  --prefix=PREFIX         install architecture-independent files in PREFIX
                          [/usr/local]
```

````{admonition} View full output
:class: dropdown
```
$ ./configure --help
`configure' configures breseq 0.37.1 to adapt to many kinds of systems.

Usage: ./configure [OPTION]... [VAR=VALUE]...

To assign environment variables (e.g., CC, CFLAGS...), specify them as
VAR=VALUE.  See below for descriptions of some of the useful variables.

Defaults for the options are specified in brackets.

Configuration:
  -h, --help              display this help and exit
      --help=short        display options specific to this package
      --help=recursive    display the short help of all the included packages
  -V, --version           display version information and exit
  -q, --quiet, --silent   do not print `checking ...' messages
      --cache-file=FILE   cache test results in FILE [disabled]
  -C, --config-cache      alias for `--cache-file=config.cache'
  -n, --no-create         do not create output files
      --srcdir=DIR        find the sources in DIR [configure dir or `..']

Installation directories:
  --prefix=PREFIX         install architecture-independent files in PREFIX
                          [/usr/local]
  --exec-prefix=EPREFIX   install architecture-dependent files in EPREFIX
                          [PREFIX]

By default, `make install' will install all the files in
`/usr/local/bin', `/usr/local/lib' etc.  You can specify
an installation prefix other than `/usr/local' using `--prefix',
for instance `--prefix=$HOME'.

For better control, use the options below.

Fine tuning of the installation directories:
  --bindir=DIR            user executables [EPREFIX/bin]
  --sbindir=DIR           system admin executables [EPREFIX/sbin]
  --libexecdir=DIR        program executables [EPREFIX/libexec]
  --sysconfdir=DIR        read-only single-machine data [PREFIX/etc]
  --sharedstatedir=DIR    modifiable architecture-independent data [PREFIX/com]
  --localstatedir=DIR     modifiable single-machine data [PREFIX/var]
  --libdir=DIR            object code libraries [EPREFIX/lib]
  --includedir=DIR        C header files [PREFIX/include]
  --oldincludedir=DIR     C header files for non-gcc [/usr/include]
  --datarootdir=DIR       read-only arch.-independent data root [PREFIX/share]
  --datadir=DIR           read-only architecture-independent data [DATAROOTDIR]
  --infodir=DIR           info documentation [DATAROOTDIR/info]
  --localedir=DIR         locale-dependent data [DATAROOTDIR/locale]
  --mandir=DIR            man documentation [DATAROOTDIR/man]
  --docdir=DIR            documentation root [DATAROOTDIR/doc/breseq]
  --htmldir=DIR           html documentation [DOCDIR]
  --dvidir=DIR            dvi documentation [DOCDIR]
  --pdfdir=DIR            pdf documentation [DOCDIR]
  --psdir=DIR             ps documentation [DOCDIR]

Program names:
  --program-prefix=PREFIX            prepend PREFIX to installed program names
  --program-suffix=SUFFIX            append SUFFIX to installed program names
  --program-transform-name=PROGRAM   run sed PROGRAM on installed program names

System types:
  --build=BUILD     configure for building on BUILD [guessed]
  --host=HOST       cross-compile to build programs to run on HOST [BUILD]

Optional Features:
  --disable-option-checking  ignore unrecognized --enable/--with options
  --disable-FEATURE       do not include FEATURE (same as --enable-FEATURE=no)
  --enable-FEATURE[=ARG]  include FEATURE [ARG=yes]
  --enable-shared[=PKGS]  build shared libraries [default=yes]
  --enable-static[=PKGS]  build static libraries [default=yes]
  --enable-fast-install[=PKGS]
                          optimize for fast installation [default=yes]
  --disable-libtool-lock  avoid locking (might break parallel builds)
  --enable-dependency-tracking
                          do not reject slow dependency extractors
  --disable-dependency-tracking
                          speeds up one-time build
  --enable-silent-rules   less verbose build output (undo: "make V=1")
  --disable-silent-rules  verbose build output (undo: "make V=0")

Optional Packages:
  --with-PACKAGE[=ARG]    use PACKAGE [ARG=yes]
  --without-PACKAGE       do not use PACKAGE (same as --with-PACKAGE=no)
  --with-pic[=PKGS]       try to use only PIC/non-PIC objects [default=use
                          both]
  --with-aix-soname=aix|svr4|both
                          shared library versioning (aka "SONAME") variant to
                          provide on AIX, [default=aix].
  --with-gnu-ld           assume the C compiler uses GNU ld [default=no]
  --with-sysroot[=DIR]    Search for dependent libraries within DIR (or the
                          compiler's sysroot if not specified).
  --without-libunwind     Disable preferring to use libunwind for debug info

Some influential environment variables:
  CC          C compiler command
  CFLAGS      C compiler flags
  LDFLAGS     linker flags, e.g. -L<lib dir> if you have libraries in a
              nonstandard directory <lib dir>
  LIBS        libraries to pass to the linker, e.g. -l<library>
  CPPFLAGS    (Objective) C/C++ preprocessor flags, e.g. -I<include dir> if
              you have headers in a nonstandard directory <include dir>
  LT_SYS_LIBRARY_PATH
              User-defined run-time library search path.
  CPP         C preprocessor
  CXX         C++ compiler command
  CXXFLAGS    C++ compiler flags
  CXXCPP      C++ preprocessor

Use these variables to override the choices made by `configure' or to help
it to find libraries and programs with nonstandard names/locations.

Report bugs to <jeffrey.e.barrick@gmail.com>.
breseq home page: <http://barricklab.org/breseq>.
```
````

This may show you features you can turn on and off, or explain which variables
are used by the build script.  In this example, we're just going to tell it
where we'd like it installed to, and nothing else.  Typically I'd advise
installing software for a given project in one location, rather than each
program being installed separately, but it's up to you.

```bash
$ ./configure --prefix=/nobackup/example/project1
```

````{admonition} View full output
:class: dropdown
```
$ ./configure --prefix=/nobackup/example/project1
checking build system type... x86_64-pc-linux-gnu
checking host system type... x86_64-pc-linux-gnu
checking how to print strings... printf
checking for gcc... gcc
checking whether the C compiler works... yes
checking for C compiler default output file name... a.out
checking for suffix of executables... 
checking whether we are cross compiling... no
checking for suffix of object files... o
checking whether we are using the GNU C compiler... yes
checking whether gcc accepts -g... yes
checking for gcc option to accept ISO C89... none needed
checking whether gcc understands -c and -o together... yes
checking for a sed that does not truncate output... /usr/bin/sed
checking for grep that handles long lines and -e... /usr/bin/grep
checking for egrep... /usr/bin/grep -E
checking for fgrep... /usr/bin/grep -F
checking for ld used by gcc... /usr/bin/ld
checking if the linker (/usr/bin/ld) is GNU ld... yes
checking for BSD- or MS-compatible name lister (nm)... /usr/bin/nm -B
checking the name lister (/usr/bin/nm -B) interface... BSD nm
checking whether ln -s works... yes
checking the maximum length of command line arguments... 1572864
checking how to convert x86_64-pc-linux-gnu file names to x86_64-pc-linux-gnu format... func_convert_file_noop
checking how to convert x86_64-pc-linux-gnu file names to toolchain format... func_convert_file_noop
checking for /usr/bin/ld option to reload object files... -r
checking for objdump... objdump
checking how to recognize dependent libraries... pass_all
checking for dlltool... no
checking how to associate runtime and link libraries... printf %s\n
checking for ar... ar
checking for archiver @FILE support... @
checking for strip... strip
checking for ranlib... ranlib
checking for gawk... gawk
checking command to parse /usr/bin/nm -B output from gcc object... ok
checking for sysroot... no
checking for a working dd... /usr/bin/dd
checking how to truncate binary pipes... /usr/bin/dd bs=4096 count=1
checking for mt... no
checking if : is a manifest tool... no
checking how to run the C preprocessor... gcc -E
checking for ANSI C header files... yes
checking for sys/types.h... yes
checking for sys/stat.h... yes
checking for stdlib.h... yes
checking for string.h... yes
checking for memory.h... yes
checking for strings.h... yes
checking for inttypes.h... yes
checking for stdint.h... yes
checking for unistd.h... yes
checking for dlfcn.h... yes
checking for objdir... .libs
checking if gcc supports -fno-rtti -fno-exceptions... no
checking for gcc option to produce PIC... -fPIC -DPIC
checking if gcc PIC flag -fPIC -DPIC works... yes
checking if gcc static flag -static works... no
checking if gcc supports -c -o file.o... yes
checking if gcc supports -c -o file.o... (cached) yes
checking whether the gcc linker (/usr/bin/ld -m elf_x86_64) supports shared libraries... yes
checking whether -lc should be explicitly linked in... no
checking dynamic linker characteristics... GNU/Linux ld.so
checking how to hardcode library paths into programs... immediate
checking whether stripping libraries is possible... yes
checking if libtool supports shared libraries... yes
checking whether to build shared libraries... yes
checking whether to build static libraries... yes
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for a thread-safe mkdir -p... /usr/bin/mkdir -p
checking whether make sets $(MAKE)... yes
checking whether make supports the include directive... yes (GNU style)
checking whether make supports nested variables... yes
checking dependency style of gcc... gcc3
checking for g++... g++
checking whether we are using the GNU C++ compiler... yes
checking whether g++ accepts -g... yes
checking how to run the C++ preprocessor... g++ -E
checking for ld used by g++... /usr/bin/ld -m elf_x86_64
checking if the linker (/usr/bin/ld -m elf_x86_64) is GNU ld... yes
checking whether the g++ linker (/usr/bin/ld -m elf_x86_64) supports shared libraries... yes
checking for g++ option to produce PIC... -fPIC -DPIC
checking if g++ PIC flag -fPIC -DPIC works... yes
checking if g++ static flag -static works... no
checking if g++ supports -c -o file.o... yes
checking if g++ supports -c -o file.o... (cached) yes
checking whether the g++ linker (/usr/bin/ld -m elf_x86_64) supports shared libraries... yes
checking dynamic linker characteristics... (cached) GNU/Linux ld.so
checking how to hardcode library paths into programs... immediate
checking dependency style of g++... gcc3
checking for gawk... (cached) gawk
checking how to run the C preprocessor... gcc -E
checking whether ln -s works... yes
checking whether make sets $(MAKE)... (cached) yes
configure: Local path from bin to data...
configure: ../share
checking arpa/inet.h usability... yes
checking arpa/inet.h presence... yes
checking for arpa/inet.h... yes
checking fcntl.h usability... yes
checking fcntl.h presence... yes
checking for fcntl.h... yes
checking for inttypes.h... (cached) yes
checking netdb.h usability... yes
checking netdb.h presence... yes
checking for netdb.h... yes
checking stddef.h usability... yes
checking stddef.h presence... yes
checking for stddef.h... yes
checking for stdint.h... (cached) yes
checking for stdlib.h... (cached) yes
checking for string.h... (cached) yes
checking sys/socket.h usability... yes
checking sys/socket.h presence... yes
checking for sys/socket.h... yes
checking for unistd.h... (cached) yes
checking wchar.h usability... yes
checking wchar.h presence... yes
checking for wchar.h... yes
checking for stdbool.h that conforms to C99... yes
checking for _Bool... yes
checking for inline... inline
checking for int16_t... yes
checking for int32_t... yes
checking for int64_t... yes
checking for int8_t... yes
checking for off_t... yes
checking for size_t... yes
checking for uint16_t... yes
checking for uint32_t... yes
checking for uint64_t... yes
checking for uint8_t... yes
checking for _LARGEFILE_SOURCE value needed for large files... no
checking for stdlib.h... (cached) yes
checking for GNU libc compatible malloc... yes
checking for stdlib.h... (cached) yes
checking for GNU libc compatible realloc... yes
checking for bzero... yes
checking for gethostbyaddr... yes
checking for gethostbyname... yes
checking for memmove... yes
checking for memset... yes
checking for pow... no
checking for select... yes
checking for socket... yes
checking for sqrt... no
checking for strchr... yes
checking for strdup... yes
checking for strerror... yes
checking for strstr... yes
checking for strtol... yes
checking for ftello... yes
checking for library containing deflateSetHeader... -lz
checking for library containing acosf... -lm
checking for library containing pthread_create... -lpthread
checking libunwind.h usability... no
checking libunwind.h presence... no
checking for libunwind.h... no
checking for library containing unw_getcontext... no
Using execinfo.h for backtrace.
checking for .git/refs/heads/master... yes
checking for .git/refs/heads/master... (cached) yes
checking that generated files are newer than configure... done
configure: creating ./config.status
config.status: creating extern/samtools-1.3.1/htslib-1.3.1/Makefile
config.status: creating extern/samtools-1.3.1/Makefile
config.status: creating src/c/breseq/Makefile
config.status: creating Makefile
config.status: creating aux_build/config.h
config.status: executing libtool commands
config.status: executing depfiles commands
```
````

You'll then got a lot of output showing it testing your system and ensuring it
can find all the bits it needs.

If it doesn't find what it needs, you need to look at the options provided by
`--help` or look into setting standard environment variables like
CPATH/LIBRARY_PATH, as covered in the [Theory section](theory).

## Building

Assuming everything's gone well so far with configure, it will have now written out a top level Makefile, which contains the necessary steps to build and install your software.  Building the software is now just a case of:

```bash
$ make
```

If you're running on a multicore machine, you can ask it to run several build
jobs in parallel.  For example, to build using 8 cores:

```bash
$ make -j 8
```

```{note}
Poorly written makefiles will sometimes build incorrectly when built in
parallel, but you'd normally be fine just running `make` again afterwards which
would complete the build.
```

## Install

Once built, to install it at the location you requested:

```bash
$ make install
```

There you have it.  Your software is installed at the location you asked.  It's
still up to you at this point to either run the software directly from this
location, or update you environment variables to make this software generally
available to you.  Details on this is covered in the [Theory section](theory).

## Summary

```{important}
- Introduces Autotools for configuring build systems
- Showcased an example of deploying `breseq` using Autotools
    - Showed how to find what options are available when configuring
    - Highlighted using `--prefix` for appropriate installation of software locally
    - Showed steps to build and install using GNU Make after configuring with Autotools
```

# References

[Autotools](https://www.gnu.org/software/automake/manual/html_node/Autotools-Introduction.html)
