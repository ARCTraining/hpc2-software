# Advanced software install

Spack takes a number of decisions itself about how to build software, and what
dependencies to use.  You're able to adjust some of this via variants within an
individual package, but also you can tweak the dependency decisions, to use
different packages if you know better than the defaults.

## Variants

Variants are where the package has options how to build it.  Let's pick on
`piranha` as a nice example.  You can get information on a package in Spack, which
will tell you about how it can be built:

```bash
$ spack info piranha
CMakePackage:   piranha

Description:
    Piranha is a computer-algebra library for the symbolic manipulation of
    sparse multivariate polynomials and other closely-related symbolic
    objects (such as Poisson series).

Homepage: https://bluescarni.github.io/piranha/sphinx/

Preferred version:
    0.5        https://github.com/bluescarni/piranha/archive/v0.5.tar.gz

Safe versions:
    develop    [git] https://github.com/bluescarni/piranha.git on branch master
    0.5        https://github.com/bluescarni/piranha/archive/v0.5.tar.gz

Deprecated versions:
    None

Variants:
    Name [Default]                 When    Allowed values          Description
    ===========================    ====    ====================    ==================================

    build_type [RelWithDebInfo]    --      Debug, Release,         CMake build type
                                           RelWithDebInfo,
                                           MinSizeRel
    ipo [off]                      --      on, off                 CMake interprocedural optimization
    python [on]                    --      on, off                 Build the Python bindings

Build Dependencies:
    boost  bzip2  cmake  gmp  mpfr  python

Link Dependencies:
    boost  bzip2  gmp  mpfr

Run Dependencies:
    python
```

From this you can see that there's lots of versions available, and there's a
number of variants listed, along with dependencies.  Let's say I wanted to
build the develop version, and we want ipo to be on, python off, and a
build\_type of Release.  Basically, be as awkward as possible and change
everything:

```bash
spack install piranha @develop +ipo -python build_type=Release
```

There we're just turned on a feature (ipo), turned off a feature (python), and
told it we want the Release build type.  Have a look through the references to
get further information, but this is actually the basics of choosing variants
of packages.

## Compilers

You may want to build a package with a different compiler.  Let's say I have
gcc@11.2.1 available:

```bash
spack install piranha @develop +ipo -python build_type=Release %gcc@11.2.1
```

Now Spack will happily rebuild that specific variant you've asked for, with the
compiler you've asked for.

## Dependencies

You can also specify particular versions and variants of dependencies.  We can
take our previous build even further:

```bash
spack install piranha @develop +ipo -python build_type=Release ^boost@1.62.0+thread %gcc@11.2.1
```

This would build using Boost 1.62.0, with thread support enabled.

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

## References

[Installing packages](https://spack-tutorial.readthedocs.io/en/latest/tutorial_basics.html#installing-packages)
[Module tutorial](https://spack-tutorial.readthedocs.io/en/latest/tutorial_modules.html)
