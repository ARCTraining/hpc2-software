# Spack

I'll introduce Spack by quoting the first paragraph of their documentation:

> [Spack](https://spack.io/) is a package manager for supercomputers, Linux,
> and macOS. It makes installing scientific software easy. Spack isn’t tied to
> a particular language; you can build a software stack in Python or R, link to
> libraries written in C, C++, or Fortran, and easily swap compilers or target
> specific microarchitectures.

So what does this mean in practice?  You've already learnt about build systems
like [Autotools](autotools) and [CMake](cmake), and may be familiar with module
systems, as covered in the section on [Building personal modules on
HPC](personalmodules).  You may also have worked through the section on
[containers](containers) and wondered if there was an easier way of building a
potentially deep stack of software for a particular project.

You may have already tried [Conda](conda) and found it wonderful, but for the
fact that the piece of software you want isn't built with the options you
wanted, so you can't use it.  Or maybe you just can't make the conda installed
version work with the MPI setup on the HPC cluster you're using.

In all these cases, Spack has the potential to make your life a little bit
easier, and the learning curve is such that you can incrementally learn about
Spack, and find it useful almost straight away.

As such, I'm going to introduce Spack with a series of examples, that work you
way up what you can do with Spack, without going too far down into the details.
If you do want to learn more about Spack, the [official Spack
documentation](https://spack.readthedocs.io/en/latest/) is mostly excellent.

- [Installing Spack](spack/installing)
- [Building a test piece of software](spack/testinstall)
- [Changing the way it names modules](spack/modules)
- [How you can tell it about bits of software you already have](spack/existing)
- [An advanced software build example](spack/advanced)
- [Building containers with Spack](spack/containers)
- [Writing a Spack recipe to build a bit of software not already handled by Spack](spack/recipes)
