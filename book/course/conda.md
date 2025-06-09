# Conda package manager

Content from this lesson has been inspired and adapted from a number of sources including:
- [Conda documentation](https://docs.conda.io/en/latest/)
- [Carpentries Incubator: Introduction to Conda for data scientists](https://carpentries-incubator.github.io/introduction-to-conda-for-data-scientists/)

(introduction)=
## Introduction

Conda is an open source package management and environment management system that runs on multiple operating systems (Windows, Linux, macOS). Its features include:

- Quickly installing, running and updating packages and their dependencies.
- Easily creating, saving, loading and switching between environments on your local computer.

While it was created for Python programs, it can package and distribute software for any language. It is a tool that helps you find and install packages, but also lets you manage different software environments where you can install different configurations of packages.
For example, this enables you to install different versions of Python in two separate environments without creating incompatibities in either of those projects.

````{admonition} Conda, Miniconda, Miniforge and Anaconda
:class: note

It's common to be confused when confronted with Conda, Miniconda, Miniforge, Anaconda.org, and Anaconda.com. Conda is specifically the package and environment manager tool itself, and is open source.
Miniconda and Anaconda are distributions provided by [Anaconda.com](https://www.anaconda.com/) which require a commercial license for use except in certain cases, while [Anaconda.org](https://anaconda.org/) is a repository of packages available for download. The *default* channel on the Anaconda repository is also covered by the commercial license; however, the channel *conda-forge* and other community channels are available outside this license. [Miniforge](https://github.com/conda-forge/miniforge) is an open-source install of Conda separate from Anaconda.

_Adapted from [Introduction to Conda for Data Scientists](https://carpentries-incubator.github.io/introduction-to-conda-for-data-scientists/01-getting-started-with-conda/index.html)_

That's a lot of information, so here's the key parts you need to know:

- *Conda* is the name of the software.
- We recommend you install Conda with [Miniforge](https://github.com/conda-forge/miniforge), a fast and open-source distribution.

````

Conda is widely used across scientific computing and data science based domains due it's well populated package ecosystem and environment management capabilities.

- Conda installs prebuilt packages, which allows for installing complicated packages in one step because someone else has built the tool with the right compilers and libraries
- The cross platform nature of Conda allows for users to more easily share the environments. This helps researchers share their computational environment along side their data and analysis, helping improve the reproducibility of their research
- Conda also provides access to widely used machine learning and data science libraries such as TensorFlow, SciPy, NumPy that are available as pre-configured, hardware specific packages (such as GPU-enabled TensorFlow) allowing for code to be as performant as possible

(installing-conda)=
## Installing Conda

### On Aire

We provide a module for miniforge, meaning you don't need to install it
yourself.

[Using conda on Aire](https://arcdocs.leeds.ac.uk/aire/software/interpreters/miniforge.html)

### On another system

You can install Conda from a number of sources. In order to ensure you are using an open-source distribution and the conda-forge channel by default, we recommend you install the [Miniforge](https://github.com/conda-forge/miniforge) distribution.
Installers are available for Windows, MacOS and Linux. You *do not* need administrative rights to install Conda on a machine.

If you have questions or issues installing Conda locally please get in touch via the [Research Computing Contact form](https://leeds.service-now.com/it?id=sc_cat_item&sys_id=7587b2530f675f00a82247ece1050eda).

## Conda environments

As well as managing packages Conda also allows you to create and manage environments.
A Conda environment is a directory that contains a specific set of installed packages and tools.
This allows you to separate the dependencies of different projects cleanly so for example, you can use Python 3.7 in one Conda environment to reproduce a collaborators results but use Python 3.10 in your own projects without any hassle.
Conda makes it easy to switch between different environments and allows you to create and delete them as required.
Conda environments also make it easier to share our environment setup between machines and with collaborators as we can export our environments into a text file.

If you want to find out more about [good dependency management practices in general, please read our documentation](https://arcdocs.leeds.ac.uk/aire/usage/dependency_management.html#dependency-management); we use this material to inform this session but take a more trial-and-error approach here.

```{admonition} The base environment
By default Conda includes the `base` environment.
This contains a starting installation of Python and the dependencies of the Conda tool itself.

Therefore, it's **best practice** to not install packages into the `base` environment and create your own environments into which you install the tools you need.

Installing into the `base` environment can lead to dependency conflicts and prevents you from being able to swap between different versions of packages for different libraries.
```


### General guidelines for handling environments

While following the steps below to build, experiment with, and then create a reproducible environment, you will hopefully notice the following key principles:

- **In general, environments should be treated as disposable and rebuildable**: you should be able to tear down and rebuild your environment quickly and easily (of course, some larger environments with complex installations will be an exception to this rule). Ideally, *you won't have to rebuild*, but being able to will save you an awful lot of heartbreak if and when something goes wrong. We'll see how we can use an `environment.yml` file to do this.
- **Export your exact environment as metadata for analysis results**: it is useful to save a snapshot of your environment to store along any results or outputs produced in that specific environment. 
- **Environments must be stored in your `home` directory and all research output must be stored in `/mnt/scratch/users`**: misuse of the system can affect performance for **all users** and will lead to your jobs being stopped.


(creating-environments)=
### Creating environments

There are two main ways to create a fresh Conda environment:
  1. Creating directly from the command line with a list of required packages;
  2. Creating from an `environment.yml` file that lists required packages.

We will step through examples of both, and compare both techniques.

#### 1. On the fly creation

If you have come across Conda before, this is likely the method of creating environments that you've encountered.

You can create an environment with Conda with the subcommand `conda create`.
When creating an environment we need to give it a name; we recommend giving it a name related to the project you're building it to support. In this example, we use the (unimaginative name) `py39-env` as we're going to be using Python 3.9; you can imagine that if you're working with multiple different versions of Python is could be useful to record this in the environment name, and prefix it with the project title.

```bash
$ conda create --name py39-env python=3.9
```
````{admonition} View full output
:class: dropdown
```
Collecting package metadata (current_repodata.json): done
Solving environment: done


==> WARNING: A newer version of conda exists. <==
  current version: 4.12.0
  latest version: 4.14.0

Please update conda by running

    $ conda update -n base -c defaults conda



## Package Plan ##

  environment location: /home/home01/arcuser/.conda/envs/py39-env

  added / updated specs:
    - python=3.9


The following NEW packages will be INSTALLED:

  _libgcc_mutex      pkgs/main/linux-64::_libgcc_mutex-0.1-main
  _openmp_mutex      pkgs/main/linux-64::_openmp_mutex-5.1-1_gnu
  ca-certificates    pkgs/main/linux-64::ca-certificates-2022.07.19-h06a4308_0
  certifi            pkgs/main/linux-64::certifi-2022.9.14-py39h06a4308_0
  ld_impl_linux-64   pkgs/main/linux-64::ld_impl_linux-64-2.38-h1181459_1
  libffi             pkgs/main/linux-64::libffi-3.3-he6710b0_2
  libgcc-ng          pkgs/main/linux-64::libgcc-ng-11.2.0-h1234567_1
  libgomp            pkgs/main/linux-64::libgomp-11.2.0-h1234567_1
  libstdcxx-ng       pkgs/main/linux-64::libstdcxx-ng-11.2.0-h1234567_1
  ncurses            pkgs/main/linux-64::ncurses-6.3-h5eee18b_3
  openssl            pkgs/main/linux-64::openssl-1.1.1q-h7f8727e_0
  pip                pkgs/main/linux-64::pip-22.1.2-py39h06a4308_0
  python             pkgs/main/linux-64::python-3.9.13-haa1d7c7_1
  readline           pkgs/main/linux-64::readline-8.1.2-h7f8727e_1
  setuptools         pkgs/main/linux-64::setuptools-63.4.1-py39h06a4308_0
  sqlite             pkgs/main/linux-64::sqlite-3.39.2-h5082296_0
  tk                 pkgs/main/linux-64::tk-8.6.12-h1ccaba5_0
  tzdata             pkgs/main/noarch::tzdata-2022c-h04d1e81_0
  wheel              pkgs/main/noarch::wheel-0.37.1-pyhd3eb1b0_0
  xz                 pkgs/main/linux-64::xz-5.2.5-h7f8727e_1
  zlib               pkgs/main/linux-64::zlib-1.2.12-h5eee18b_3


Proceed ([y]/n)? y

Preparing transaction: done
Verifying transaction: done
Executing transaction: done
#
# To activate this environment, use
#
#     $ conda activate py39-env
#
# To deactivate an active environment, use
#
#     $ conda deactivate
```
````

The above command will prompt Conda to create a new environment called `py39-env` and install into it `python` at version 3.9.
We can specify multiple packages when creating a Conda environment by separating each package name with a space.

```bash
$ conda create --name data-sci-env pandas=1.4.2 matplotlib=3.5.1 scikit-learn
```
````{admonition} View full output
:class: dropdown
```
Collecting package metadata (current_repodata.json): done
Solving environment: done


==> WARNING: A newer version of conda exists. <==
  current version: 4.12.0
  latest version: 4.14.0

Please update conda by running

    $ conda update -n base -c defaults conda



## Package Plan ##

  environment location: /home/home01/arcuser/.conda/envs/data-sci-env

  added / updated specs:
    - matplotlib=3.5.1
    - pandas=1.4.2
    - scikit-learn


The following NEW packages will be INSTALLED:

  _libgcc_mutex      pkgs/main/linux-64::_libgcc_mutex-0.1-main
  _openmp_mutex      pkgs/main/linux-64::_openmp_mutex-5.1-1_gnu
  blas               pkgs/main/linux-64::blas-1.0-mkl
  bottleneck         pkgs/main/linux-64::bottleneck-1.3.5-py310ha9d4c09_0
  brotli             pkgs/main/linux-64::brotli-1.0.9-h5eee18b_7
  brotli-bin         pkgs/main/linux-64::brotli-bin-1.0.9-h5eee18b_7
  bzip2              pkgs/main/linux-64::bzip2-1.0.8-h7b6447c_0
  ca-certificates    pkgs/main/linux-64::ca-certificates-2022.07.19-h06a4308_0
  certifi            pkgs/main/linux-64::certifi-2022.9.14-py310h06a4308_0
  cycler             pkgs/main/noarch::cycler-0.11.0-pyhd3eb1b0_0
  dbus               pkgs/main/linux-64::dbus-1.13.18-hb2f20db_0
  expat              pkgs/main/linux-64::expat-2.4.4-h295c915_0
  fftw               pkgs/main/linux-64::fftw-3.3.9-h27cfd23_1
  fontconfig         pkgs/main/linux-64::fontconfig-2.13.1-h6c09931_0
  fonttools          pkgs/main/noarch::fonttools-4.25.0-pyhd3eb1b0_0
  freetype           pkgs/main/linux-64::freetype-2.11.0-h70c0345_0
  giflib             pkgs/main/linux-64::giflib-5.2.1-h7b6447c_0
  glib               pkgs/main/linux-64::glib-2.69.1-h4ff587b_1
  gst-plugins-base   pkgs/main/linux-64::gst-plugins-base-1.14.0-h8213a91_2
  gstreamer          pkgs/main/linux-64::gstreamer-1.14.0-h28cd5cc_2
  icu                pkgs/main/linux-64::icu-58.2-he6710b0_3
  intel-openmp       pkgs/main/linux-64::intel-openmp-2021.4.0-h06a4308_3561
  joblib             pkgs/main/noarch::joblib-1.1.0-pyhd3eb1b0_0
  jpeg               pkgs/main/linux-64::jpeg-9e-h7f8727e_0
  kiwisolver         pkgs/main/linux-64::kiwisolver-1.4.2-py310h295c915_0
  krb5               pkgs/main/linux-64::krb5-1.19.2-hac12032_0
  lcms2              pkgs/main/linux-64::lcms2-2.12-h3be6417_0
  ld_impl_linux-64   pkgs/main/linux-64::ld_impl_linux-64-2.38-h1181459_1
  lerc               pkgs/main/linux-64::lerc-3.0-h295c915_0
  libbrotlicommon    pkgs/main/linux-64::libbrotlicommon-1.0.9-h5eee18b_7
  libbrotlidec       pkgs/main/linux-64::libbrotlidec-1.0.9-h5eee18b_7
  libbrotlienc       pkgs/main/linux-64::libbrotlienc-1.0.9-h5eee18b_7
  libclang           pkgs/main/linux-64::libclang-10.0.1-default_hb85057a_2
  libdeflate         pkgs/main/linux-64::libdeflate-1.8-h7f8727e_5
  libedit            pkgs/main/linux-64::libedit-3.1.20210910-h7f8727e_0
  libevent           pkgs/main/linux-64::libevent-2.1.12-h8f2d780_0
  libffi             pkgs/main/linux-64::libffi-3.3-he6710b0_2
  libgcc-ng          pkgs/main/linux-64::libgcc-ng-11.2.0-h1234567_1
  libgfortran-ng     pkgs/main/linux-64::libgfortran-ng-11.2.0-h00389a5_1
  libgfortran5       pkgs/main/linux-64::libgfortran5-11.2.0-h1234567_1
  libgomp            pkgs/main/linux-64::libgomp-11.2.0-h1234567_1
  libllvm10          pkgs/main/linux-64::libllvm10-10.0.1-hbcb73fb_5
  libpng             pkgs/main/linux-64::libpng-1.6.37-hbc83047_0
  libpq              pkgs/main/linux-64::libpq-12.9-h16c4e8d_3
  libstdcxx-ng       pkgs/main/linux-64::libstdcxx-ng-11.2.0-h1234567_1
  libtiff            pkgs/main/linux-64::libtiff-4.4.0-hecacb30_0
  libuuid            pkgs/main/linux-64::libuuid-1.0.3-h7f8727e_2
  libwebp            pkgs/main/linux-64::libwebp-1.2.2-h55f646e_0
  libwebp-base       pkgs/main/linux-64::libwebp-base-1.2.2-h7f8727e_0
  libxcb             pkgs/main/linux-64::libxcb-1.15-h7f8727e_0
  libxkbcommon       pkgs/main/linux-64::libxkbcommon-1.0.1-hfa300c1_0
  libxml2            pkgs/main/linux-64::libxml2-2.9.14-h74e7548_0
  libxslt            pkgs/main/linux-64::libxslt-1.1.35-h4e12654_0
  lz4-c              pkgs/main/linux-64::lz4-c-1.9.3-h295c915_1
  matplotlib         pkgs/main/linux-64::matplotlib-3.5.1-py310h06a4308_1
  matplotlib-base    pkgs/main/linux-64::matplotlib-base-3.5.1-py310ha18d171_1
  mkl                pkgs/main/linux-64::mkl-2021.4.0-h06a4308_640
  mkl-service        pkgs/main/linux-64::mkl-service-2.4.0-py310h7f8727e_0
  mkl_fft            pkgs/main/linux-64::mkl_fft-1.3.1-py310hd6ae3a3_0
  mkl_random         pkgs/main/linux-64::mkl_random-1.2.2-py310h00e6091_0
  munkres            pkgs/main/noarch::munkres-1.1.4-py_0
  ncurses            pkgs/main/linux-64::ncurses-6.3-h5eee18b_3
  nspr               pkgs/main/linux-64::nspr-4.33-h295c915_0
  nss                pkgs/main/linux-64::nss-3.74-h0370c37_0
  numexpr            pkgs/main/linux-64::numexpr-2.8.3-py310hcea2de6_0
  numpy              pkgs/main/linux-64::numpy-1.21.5-py310h1794996_3
  numpy-base         pkgs/main/linux-64::numpy-base-1.21.5-py310hcba007f_3
  openssl            pkgs/main/linux-64::openssl-1.1.1q-h7f8727e_0
  packaging          pkgs/main/noarch::packaging-21.3-pyhd3eb1b0_0
  pandas             pkgs/main/linux-64::pandas-1.4.2-py310h295c915_0
  pcre               pkgs/main/linux-64::pcre-8.45-h295c915_0
  pillow             pkgs/main/linux-64::pillow-9.2.0-py310hace64e9_1
  pip                pkgs/main/linux-64::pip-22.1.2-py310h06a4308_0
  ply                pkgs/main/linux-64::ply-3.11-py310h06a4308_0
  pyparsing          pkgs/main/linux-64::pyparsing-3.0.9-py310h06a4308_0
  pyqt               pkgs/main/linux-64::pyqt-5.15.7-py310h6a678d5_1
  pyqt5-sip          pkgs/main/linux-64::pyqt5-sip-12.11.0-py310h6a678d5_1
  python             pkgs/main/linux-64::python-3.10.4-h12debd9_0
  python-dateutil    pkgs/main/noarch::python-dateutil-2.8.2-pyhd3eb1b0_0
  pytz               pkgs/main/linux-64::pytz-2022.1-py310h06a4308_0
  qt-main            pkgs/main/linux-64::qt-main-5.15.2-h327a75a_7
  qt-webengine       pkgs/main/linux-64::qt-webengine-5.15.9-hd2b0992_4
  qtwebkit           pkgs/main/linux-64::qtwebkit-5.212-h4eab89a_4
  readline           pkgs/main/linux-64::readline-8.1.2-h7f8727e_1
  scikit-learn       pkgs/main/linux-64::scikit-learn-1.1.1-py310h6a678d5_0
  scipy              pkgs/main/linux-64::scipy-1.7.3-py310h1794996_2
  setuptools         pkgs/main/linux-64::setuptools-63.4.1-py310h06a4308_0
  sip                pkgs/main/linux-64::sip-6.6.2-py310h6a678d5_0
  six                pkgs/main/noarch::six-1.16.0-pyhd3eb1b0_1
  sqlite             pkgs/main/linux-64::sqlite-3.39.2-h5082296_0
  threadpoolctl      pkgs/main/noarch::threadpoolctl-2.2.0-pyh0d69192_0
  tk                 pkgs/main/linux-64::tk-8.6.12-h1ccaba5_0
  toml               pkgs/main/noarch::toml-0.10.2-pyhd3eb1b0_0
  tornado            pkgs/main/linux-64::tornado-6.2-py310h5eee18b_0
  tzdata             pkgs/main/noarch::tzdata-2022c-h04d1e81_0
  wheel              pkgs/main/noarch::wheel-0.37.1-pyhd3eb1b0_0
  xz                 pkgs/main/linux-64::xz-5.2.5-h7f8727e_1
  zlib               pkgs/main/linux-64::zlib-1.2.12-h5eee18b_3
  zstd               pkgs/main/linux-64::zstd-1.5.2-ha4553b6_0


Proceed ([y]/n)? y

Preparing transaction: done
Verifying transaction: done
Executing transaction: /

    Installed package of scikit-learn can be accelerated using scikit-learn-intelex.
    More details are available here: https://intel.github.io/scikit-learn-intelex

    For example:

        $ conda install scikit-learn-intelex
        $ python -m sklearnex my_application.py

done
#
# To activate this environment, use
#
#     $ conda activate data-sci-env
#
# To deactivate an active environment, use
#
#     $ conda deactivate
```
````

With the above command we create a new environment but don't specify to install Python.
However, because we've specified Python packages which depend on Python being installed to run Conda will install the highest version of Python suitable for these packages.

#### 2. Creation from an `environment.yml` file

Instead of providing a list of packages as arguments to the Conda command, you can instead point Conda to a file that lists your dependencies.

First, you need to create an environment file with the dependencies required, saved with the file extension `.yaml` or `.yml` (usually called `environment.yml`, but it doesn't *have* to be):

```yml
name: data-sci-env
dependencies:
- scikit-learn
- matplotlib=3.5.1
- pandas=1.4.2
```

You'll note that this list has the same dependencies as our on-the-fly example previously (`conda create --name data-sci-env pandas=1.4.2 matplotlib=3.5.1 scikit-learn`). This file should be saved in the project directory. 

Then, we can create a new environment by simply pointing Conda at the environment file:

```bash
$ conda env create -f environment.yml
```

*Note that this second example was run much more recently (2025) than the previous example; can you spot some key differences in the output below?*


````{admonition} View full output
:class: dropdown
```
Retrieving notices: done
Channels:
 - conda-forge
Platform: linux-64
Collecting package metadata (repodata.json): done
Solving environment: done


==> WARNING: A newer version of conda exists. <==
    current version: 25.3.1
    latest version: 25.5.1

Please update conda by running

    $ conda update -n base -c conda-forge conda



Downloading and Extracting Packages:

>>>>>>> lots of information on installing packages...

Preparing transaction: done
Verifying transaction: done
Executing transaction: done
#
# To activate this environment, use
#
#     $ conda activate data-sci-env
#
# To deactivate an active environment, use
#
#     $ conda deactivate
```
````

With the above command we create a new environment but don't specify to install Python.
However, because we've specified Python packages which depend on Python being installed to run Conda will install the highest version of Python suitable for these packages.

(activating-environments)=
### Activating environments

Regardless of the method you used to create the environment, in order to use a Conda environment we need to activate it.
Activating our environment does a number of steps that sets the terminal we're using up so that it can see all of the installed packages in the environment, making it ready for use.

```bash
$ conda activate data-sci-env

(data-sci-env)$
```

You use the subcommand `conda activate ENVNAME` for environment activation, where `ENVNAME` is the name of the environment you wish to activate.
You can see it has successfully activated when it returns your prompt with the environment name prepended in brackets.

(deactivating-environments)=
### Deactivating environments

You can deactivate your current environment with another simple subcommand `conda deactivate`.

```bash
(data-sci-env)$ conda deactivate
```

(listing-current-environments)=
### Listing current environments

If you ever want to see your list of current environments on your machine you can you the subcommand `conda env list`.
This will return a list of the available Conda environments you can use and the environment location in your filesystem.

```bash
$ conda env list
```
````{admonition} View full output
:class: dropdown
```
py39-env                   /home/home01/arcuser/.conda/envs/py39-env
data-sci-env               /home/home01/arcusers/.conda/envs/data-sci-env
```
````

(installing-packages)=
### Updating a Conda environment and installing new packages

It's very likely that after creating an environment with a certain list of packages, you'll want to add other packages, or potentially change what version of a package you have installed.

Earlier we created the `data-sci-env` and installed some useful data science packages.
We've discovered we also need the `statsmodels` package for some extra work we want to do so we'll look at how to install this package within our existing environment.

````{admonition} Searching for packages

Conda has a command-line search functionality that we describe below in the section [Use Conda to search for a package](#searching-for-packages); you can also use the [`conda-forge` repository](https://anaconda.org/conda-forge) or [`bioconda` repository](https://anaconda.org/bioconda) to search for packages.
````

Once you have the name (and possibly version) of the package you want to install, again there are two different ways to add these packages, much like there were two ways to create the environment to begin with.

#### 1. On the fly installation of new packages

You can add new packages directly from the command line using the `install` subcommand with the format `conda install PACKAGE`, where `PACKAGE` is the name of the package you wish to install.

To install packages into an existing environment we need to activate it with the [subcommand shown above](activating-environments).

```bash
$ conda activate data-sci-env

(data-sci-env)$ conda install statsmodels
```
````{admonition} View full output
:class: dropdown
```
Collecting package metadata (current_repodata.json): done
Solving environment: done

## Package Plan ##

  environment location: /home/home01/arcuser/.conda/envs/data-sci-env

  added / updated specs:
    - statsmodels


The following packages will be downloaded:

    package                    |            build
    ---------------------------|-----------------
    libopenblas-0.3.21         |pthreads_h78a6416_3        10.1 MB  conda-forge
    numpy-1.23.2               |  py310h53a5b5f_0         7.1 MB  conda-forge
    pandas-1.4.4               |  py310h769672d_0        12.5 MB  conda-forge
    patsy-0.5.2                |     pyhd8ed1ab_0         188 KB  conda-forge
    pytz-2022.2.1              |     pyhd8ed1ab_0         224 KB  conda-forge
    scipy-1.9.1                |  py310hdfbd76f_0        26.2 MB  conda-forge
    statsmodels-0.13.2         |  py310hde88566_0        11.2 MB  conda-forge
    ------------------------------------------------------------
                                           Total:        67.4 MB

The following NEW packages will be INSTALLED:

  libblas            conda-forge/linux-64::libblas-3.9.0-16_linux64_openblas
  libcblas           conda-forge/linux-64::libcblas-3.9.0-16_linux64_openblas
  libgfortran-ng     conda-forge/linux-64::libgfortran-ng-12.1.0-h69a702a_16
  libgfortran5       conda-forge/linux-64::libgfortran5-12.1.0-hdcd56e2_16
  liblapack          conda-forge/linux-64::liblapack-3.9.0-16_linux64_openblas
  libopenblas        conda-forge/linux-64::libopenblas-0.3.21-pthreads_h78a6416_3
  libstdcxx-ng       conda-forge/linux-64::libstdcxx-ng-12.1.0-ha89aaad_16
  numpy              conda-forge/linux-64::numpy-1.23.2-py310h53a5b5f_0
  packaging          conda-forge/noarch::packaging-21.3-pyhd8ed1ab_0
  pandas             conda-forge/linux-64::pandas-1.4.4-py310h769672d_0
  patsy              conda-forge/noarch::patsy-0.5.2-pyhd8ed1ab_0
  pyparsing          conda-forge/noarch::pyparsing-3.0.9-pyhd8ed1ab_0
  python-dateutil    conda-forge/noarch::python-dateutil-2.8.2-pyhd8ed1ab_0
  python_abi         conda-forge/linux-64::python_abi-3.10-2_cp310
  pytz               conda-forge/noarch::pytz-2022.2.1-pyhd8ed1ab_0
  scipy              conda-forge/linux-64::scipy-1.9.1-py310hdfbd76f_0
  six                conda-forge/noarch::six-1.16.0-pyh6c4a22f_0
  statsmodels        conda-forge/linux-64::statsmodels-0.13.2-py310hde88566_0


Proceed ([y]/n)?
```
````

Conda will always prompt the user if we're happy to proceed with the installation and specifies all the other packages that will be installed or updated that are required for our specified package.
We confirm we wish to proceed by entering `y` and pressing Return.

````{admonition} View full output
:class: dropdown
```
Proceed ([y]/n)? y

Downloading and Extracting Packages
pytz-2022.2.1        | 224 KB    | ##################################### | 100%
libopenblas-0.3.21   | 10.1 MB   | ##################################### | 100%
scipy-1.9.1          | 26.2 MB   | ##################################### | 100%
patsy-0.5.2          | 188 KB    | ##################################### | 100%
statsmodels-0.13.2   | 11.2 MB   | ##################################### | 100%
pandas-1.4.4         | 12.5 MB   | ##################################### | 100%
numpy-1.23.2         | 7.1 MB    | ##################################### | 100%
Preparing transaction: done
Verifying transaction: done
Executing transaction: done
```
````

This installs any packages that are currently not installed (Conda caches packages locally in case they are required by other packages, this speeds up installs but uses more disk space to maintain this cache).

#### 2. Updating from an `environment.yml` file

To update our environment using our environment file, we need to edit the `environment.yml` to include the new packages:

```yml
name: data-sci-env
dependencies:
- scikit-learn
- matplotlib=3.5.1
- pandas=1.4.2
- statsmodels
```

Now, to update the environment from this file, we us the `update` subcommand:

```bash
$ conda env update --file environment.yml --prune
```

Note that the environment does **not** need to be active to do this. You should pin any versions of libraries (such as `matplotlib=3.5.1`) that you don't want to update.

````{admonition} View full output
:class: dropdown
```
FutureWarning: `remote_definition` is deprecated and will be removed in 25.9. Use `conda env create --file=URL` instead.
  action(self, namespace, argument_values, option_string)^

Channels:
 - conda-forge
Platform: linux-64
Collecting package metadata (repodata.json): done
Solving environment: done


==> WARNING: A newer version of conda exists. <==
    current version: 25.3.1
    latest version: 25.5.1

Please update conda by running

    $ conda update -n base -c conda-forge conda



Downloading and Extracting Packages:

Preparing transaction: done
Verifying transaction: done
Executing transaction: done
#
# To activate this environment, use
#
#     $ conda activate data-sci-env
#
# To deactivate an active environment, use
#
#     $ conda deactivate
```
````

*Note that there is a `FutureWarning` that can safely be ignored as it is not intended to flag use of `environment.yml` files.* 

This ensures that we have an up-to-date record of what we have installed in our project folder.

The `--prune` argument here clears out old unused libraries and is key to keeping your `.conda` folder a reasonable size. **Please ensure you use the prune command to prevent environment bloat**.

(removing-a-conda-environment)=
### Removing a Conda environment

It is also possible to delete a Conda environment through the `remove` subcommand.
This [command is outlined below](removing-packages) in relation to removing specific packages but can also be used to delete an entire Conda environment.

To remove the `py39-env` we created earlier we use the command:

```bash
$ conda remove --name py39-env --all
```
````{admonition} View full output
:class: dropdown
```
Remove all packages in environment /home/home01/arcuser/.conda/envs/py39-env:


## Package Plan ##

  environment location: /home/home01/arcuser/.conda/envs/py39-env


The following packages will be REMOVED:

  _libgcc_mutex-0.1-main
  _openmp_mutex-5.1-1_gnu
  ca-certificates-2022.07.19-h06a4308_0
  certifi-2022.9.14-py39h06a4308_0
  ld_impl_linux-64-2.38-h1181459_1
  libffi-3.3-he6710b0_2
  libgcc-ng-11.2.0-h1234567_1
  libgomp-11.2.0-h1234567_1
  libstdcxx-ng-11.2.0-h1234567_1
  ncurses-6.3-h5eee18b_3
  openssl-1.1.1q-h7f8727e_0
  pip-22.1.2-py39h06a4308_0
  python-3.9.13-haa1d7c7_1
  readline-8.1.2-h7f8727e_1
  setuptools-63.4.1-py39h06a4308_0
  sqlite-3.39.2-h5082296_0
  tk-8.6.12-h1ccaba5_0
  tzdata-2022c-h04d1e81_0
  wheel-0.37.1-pyhd3eb1b0_0
  xz-5.2.5-h7f8727e_1
  zlib-1.2.12-h5eee18b_3


Proceed ([y]/n)?
```
````

Conda checks for user confirmation that we wish to proceed and outlines for us exactly which packages are being removed.
On proceeding with removing the environment all associated environment files and packages are deleted.

```{important}
Using `conda remove` to delete an environment is irreversible.
You cannot undo deletion of an environment to the exact state it was in before deletion.
However, if you have exported details of your environment it is possible to recreate it.
```

(recording-conda-environments)=
### Recording your Conda environments

Recording dependencies is crucial for reproducibility.
In order to record the exact versions of all dependencies used in your project (as opposed to the limited list you manually installed with your `envrionment.yml` file), from inside your active conda environment, you can run the following export command:

```bash
$ conda activate data-sci-env

(data-sci-env)$ conda env export > env-record.yml
```

This can be run as part of a batch job and included in your submission script;  so that it's saved out alongside your other output data files:

```bash
conda env export > /mnt/scratch/users/your-user-name/env-record.yaml
```

**This exported environment file is mainly useful as a record for the sake of reproducibility, not for *reusability*. Your `environment.yml` file is a far better basis for rebuilding or sharing environments.**

This record will include background library dependencies (libraries you did not explicitly install, that were loaded automatically) and details of builds. This file, while technically an `environment.yml` file, will likely not be able to rebuild your environment on a machine other than the machine it was created on.

It's important to consider the balance of reproducibility and portability: `conda env export` captures the exact specification of an environment including all installed packages, their dependencies and package hashes.
Sometimes this level of detail should be included to ensure maximum reproduciblity of a project and when looking to validate results, but it's important to also balance being able to allow people to reproduce your work on other systems. The next section talks about portability or re*use*ability more.

(sharing-conda-environments)=
### Sharing Conda environments

The Conda `environment.yml` file is the key to sharing conda environments across systems.

If you created your Conda environment from a `.yml` file (and have kept it up-to-date by using it and the `update` command to install new packages), you can share this file with collaborators, and they can use the instructions above to create an environment from file.

If you instead used the on-the-fly creation method and *don't* have an `environment.yml`, it will take a little bit more work. As we stated in the last section, using `conda env export` will export all installed packages, their dependencies, and package hashes, and will be unlikely to install without error on a different system. So how can we produce a reuseable `environment.yml` file?

**If you follow the above steps for building your conda environment from a `.yml` file, this step is not necessary. However, if you want to salvage, share, or back-up an environment that you built using repeated `conda install package-name` commands, this allows you to create an `environment.yml` file.**

Activate your environment and run a modified export:

```bash
$ conda activate data-sci-env

(data-sci-env)$ conda env export --from-history > environment_export.yml
```

This will export a list of only the libraries that you explicitly installed (and not all the background dependencies), and only the pinned versions you requested. This is not useful as a record of your exact environment, but is a good backup for rebuilding or sharing your environment. **Note that this will not add any pip dependencies: to find out more about pip dependencies.** We won't get into mixing in pip dependencies today, but please read our documentation for [how to export a reuseable environment file including pip dependencies](https://arcdocs.leeds.ac.uk/aire/usage/dependency_management.html#pip-dependencies).

(searching-for-packages)=
## Using Conda to search for packages

We can use the `search` command in Conda to find available package versions:
```bash
$ conda search python
```
````{admonition} View full output
:class: dropdown
```
Loading channels: done
# Name                       Version           Build  Channel
python                        2.7.13     hac47a24_15  pkgs/main
python                        2.7.13     heccc3f1_16  pkgs/main
python                        2.7.13     hfff3488_13  pkgs/main
python                        2.7.14     h1571d57_29  pkgs/main
python                        2.7.14     h1571d57_30  pkgs/main
python                        2.7.14     h1571d57_31  pkgs/main
python                        2.7.14     h1aa7481_19  pkgs/main
python                        2.7.14     h435b27a_18  pkgs/main
python                        2.7.14     h89e7a4a_22  pkgs/main
python                        2.7.14     h91f54f5_26  pkgs/main
python                        2.7.14     h931c8b0_15  pkgs/main
python                        2.7.14     h9b67528_20  pkgs/main
python                        2.7.14     ha6fc286_23  pkgs/main
python                        2.7.14     hc2b0042_21  pkgs/main
python                        2.7.14     hdd48546_24  pkgs/main
python                        2.7.14     hf918d8d_16  pkgs/main
python                        2.7.15      h1571d57_0  pkgs/main
python                        2.7.15      h77bded6_1  pkgs/main
python                        2.7.15      h77bded6_2  pkgs/main
python                        2.7.15      h9bab390_2  pkgs/main
python                        2.7.15      h9bab390_4  pkgs/main
python                        2.7.15      h9bab390_6  pkgs/main
python                        2.7.16      h8b3fad2_1  pkgs/main
python                        2.7.16      h8b3fad2_2  pkgs/main
python                        2.7.16      h8b3fad2_3  pkgs/main
python                        2.7.16      h8b3fad2_4  pkgs/main
python                        2.7.16      h8b3fad2_5  pkgs/main
python                        2.7.16      h9bab390_0  pkgs/main
python                        2.7.16      h9bab390_6  pkgs/main
python                        2.7.16      h9bab390_7  pkgs/main
python                        2.7.17      h9bab390_0  pkgs/main
python                        2.7.18      h02575d3_0  pkgs/main
python                        2.7.18      h15b4118_1  pkgs/main
python                        2.7.18      ha1903f6_2  pkgs/main
python                         3.5.4     h00c01ad_19  pkgs/main
python                         3.5.4     h0b4c808_22  pkgs/main
python                         3.5.4     h2170f06_12  pkgs/main
python                         3.5.4     h3075507_18  pkgs/main
python                         3.5.4     h417fded_24  pkgs/main
python                         3.5.4     h56e0582_23  pkgs/main
python                         3.5.4     h72f0b78_15  pkgs/main
python                         3.5.4     hb43c6bb_21  pkgs/main
python                         3.5.4     hc053d89_14  pkgs/main
python                         3.5.4     hc3d631a_27  pkgs/main
python                         3.5.4     he2c66cf_20  pkgs/main
python                         3.5.5      hc3d631a_0  pkgs/main
python                         3.5.5      hc3d631a_1  pkgs/main
python                         3.5.5      hc3d631a_3  pkgs/main
python                         3.5.5      hc3d631a_4  pkgs/main
python                         3.5.6      h12debd9_1  pkgs/main
python                         3.5.6      hc3d631a_0  pkgs/main
python                         3.6.2     h02fb82a_12  pkgs/main
python                         3.6.2     h0b30769_14  pkgs/main
python                         3.6.2     h33255ae_18  pkgs/main
python                         3.6.2     hca45abc_19  pkgs/main
python                         3.6.2     hdfe5801_15  pkgs/main
python                         3.6.3      h0ef2715_3  pkgs/main
python                         3.6.3      h1284df2_4  pkgs/main
python                         3.6.3      h6c0c0dc_5  pkgs/main
python                         3.6.3      hc9025b9_1  pkgs/main
python                         3.6.3      hcad60d5_0  pkgs/main
python                         3.6.3      hefd0734_2  pkgs/main
python                         3.6.4      hc3d631a_0  pkgs/main
python                         3.6.4      hc3d631a_1  pkgs/main
python                         3.6.4      hc3d631a_3  pkgs/main
python                         3.6.5      hc3d631a_0  pkgs/main
python                         3.6.5      hc3d631a_1  pkgs/main
python                         3.6.5      hc3d631a_2  pkgs/main
python                         3.6.6      h6e4f718_2  pkgs/main
python                         3.6.6      hc3d631a_0  pkgs/main
python                         3.6.7      h0371630_0  pkgs/main
python                         3.6.8      h0371630_0  pkgs/main
python                         3.6.9      h265db76_0  pkgs/main
python                        3.6.10      h0371630_0  pkgs/main
python                        3.6.10      h191fe78_1  pkgs/main
python                        3.6.10      h7579374_2  pkgs/main
python                        3.6.10      hcf32534_1  pkgs/main
python                        3.6.12      hcff3b4d_2  pkgs/main
python                        3.6.13      h12debd9_1  pkgs/main
python                        3.6.13      hdb3f193_0  pkgs/main
python                         3.7.0      h6e4f718_3  pkgs/main
python                         3.7.0      hc3d631a_0  pkgs/main
python                         3.7.1      h0371630_3  pkgs/main
python                         3.7.1      h0371630_7  pkgs/main
python                         3.7.2      h0371630_0  pkgs/main
python                         3.7.3      h0371630_0  pkgs/main
python                         3.7.4      h265db76_0  pkgs/main
python                         3.7.4      h265db76_1  pkgs/main
python                         3.7.5      h0371630_0  pkgs/main
python                         3.7.6      h0371630_2  pkgs/main
python                         3.7.7 h191fe78_0_cpython  pkgs/main
python                         3.7.7 hcf32534_0_cpython  pkgs/main
python                         3.7.7      hcff3b4d_4  pkgs/main
python                         3.7.7      hcff3b4d_5  pkgs/main
python                         3.7.9      h7579374_0  pkgs/main
python                        3.7.10      h12debd9_4  pkgs/main
python                        3.7.10      hdb3f193_0  pkgs/main
python                        3.7.11      h12debd9_0  pkgs/main
python                        3.7.13      h12debd9_0  pkgs/main
python                         3.8.0      h0371630_0  pkgs/main
python                         3.8.0      h0371630_1  pkgs/main
python                         3.8.0      h0371630_2  pkgs/main
python                         3.8.1      h0371630_1  pkgs/main
python                         3.8.2      h191fe78_0  pkgs/main
python                         3.8.2      hcf32534_0  pkgs/main
python                         3.8.2     hcff3b4d_13  pkgs/main
python                         3.8.2     hcff3b4d_14  pkgs/main
python                         3.8.3      hcff3b4d_0  pkgs/main
python                         3.8.3      hcff3b4d_2  pkgs/main
python                         3.8.5      h7579374_1  pkgs/main
python                         3.8.5      hcff3b4d_0  pkgs/main
python                         3.8.8      hdb3f193_4  pkgs/main
python                         3.8.8      hdb3f193_5  pkgs/main
python                        3.8.10      h12debd9_8  pkgs/main
python                        3.8.10      hdb3f193_7  pkgs/main
python                        3.8.11 h12debd9_0_cpython  pkgs/main
python                        3.8.12      h12debd9_0  pkgs/main
python                        3.8.13      h12debd9_0  pkgs/main
python                         3.9.0      hcff3b4d_1  pkgs/main
python                         3.9.0      hdb3f193_2  pkgs/main
python                         3.9.1      hdb3f193_2  pkgs/main
python                         3.9.2      hdb3f193_0  pkgs/main
python                         3.9.4      hdb3f193_0  pkgs/main
python                         3.9.5      h12debd9_4  pkgs/main
python                         3.9.5      hdb3f193_3  pkgs/main
python                         3.9.6      h12debd9_0  pkgs/main
python                         3.9.6      h12debd9_1  pkgs/main
python                         3.9.7      h12debd9_1  pkgs/main
python                        3.9.11      h12debd9_1  pkgs/main
python                        3.9.11      h12debd9_2  pkgs/main
python                        3.9.12      h12debd9_0  pkgs/main
python                        3.9.12      h12debd9_1  pkgs/main
python                        3.9.13      haa1d7c7_1  pkgs/main
python                        3.10.0      h12debd9_0  pkgs/main
python                        3.10.0      h12debd9_1  pkgs/main
python                        3.10.0      h12debd9_2  pkgs/main
python                        3.10.0      h12debd9_4  pkgs/main
python                        3.10.0      h12debd9_5  pkgs/main
python                        3.10.0      h151d27f_3  pkgs/main
python                        3.10.3      h12debd9_5  pkgs/main
python                        3.10.4      h12debd9_0  pkgs/main
```
````

This command searches for packages based on the argument provided.
It searches in package repositories called [Conda Channels](https://docs.conda.io/projects/conda/en/stable/user-guide/concepts/channels.html) which are remote websites where built Conda packages have been uploaded to.
By default Conda installed with Miniforge uses the [`conda-forge` channel](https://conda-forge.org/).
If you are using a different install of Conda, you may need to specify this channel. Alternatively, you may need to point to the Bioconda channel.

```bash
$ conda search 'python[channel=conda-forge]'
```
````{admonition} View full output
:class: dropdown
```
Loading channels: done
# Name                       Version           Build  Channel
python                         1.0.1               0  conda-forge
python                           1.2               0  conda-forge
python                           1.3               0  conda-forge
python                           1.4               0  conda-forge
python                         1.5.2               0  conda-forge
python                           1.6               0  conda-forge
python                           2.0               0  conda-forge
python                         2.6.9               0  conda-forge
python                        2.7.12               0  conda-forge
python                        2.7.12               1  conda-forge
python                        2.7.12               2  conda-forge
python                        2.7.13               0  conda-forge
python                        2.7.13               1  conda-forge
python                        2.7.14               0  conda-forge
python                        2.7.14               1  conda-forge
python                        2.7.14               2  conda-forge
python                        2.7.14               3  conda-forge
python                        2.7.14               4  conda-forge
python                        2.7.14               5  conda-forge
python                        2.7.15               0  conda-forge
python                        2.7.15      h33da82c_1  conda-forge
python                        2.7.15      h33da82c_3  conda-forge
python                        2.7.15      h33da82c_4  conda-forge
python                        2.7.15      h33da82c_5  conda-forge
python                        2.7.15      h33da82c_6  conda-forge
python                        2.7.15   h5a48372_1009  conda-forge
python                        2.7.15 h5a48372_1010_cpython  conda-forge
python                        2.7.15 h5a48372_1011_cpython  conda-forge
python                        2.7.15   h721da81_1008  conda-forge
python                        2.7.15   h938d71a_1001  conda-forge
python                        2.7.15   h938d71a_1002  conda-forge
python                        2.7.15   h938d71a_1003  conda-forge
python                        2.7.15   h938d71a_1004  conda-forge
python                        2.7.15   h938d71a_1005  conda-forge
python                        2.7.15   h938d71a_1006  conda-forge
python                        2.7.15      h9fef7bc_0  conda-forge
python                         3.4.5               0  conda-forge
python                         3.4.5               1  conda-forge
python                         3.4.5               2  conda-forge
python                         3.5.1               0  conda-forge
python                         3.5.1               1  conda-forge
python                         3.5.2               0  conda-forge
python                         3.5.2               1  conda-forge
python                         3.5.2               2  conda-forge
python                         3.5.2               4  conda-forge
python                         3.5.2               5  conda-forge
python                         3.5.3               0  conda-forge
python                         3.5.3               1  conda-forge
python                         3.5.3               2  conda-forge
python                         3.5.3               3  conda-forge
python                         3.5.4               0  conda-forge
python                         3.5.4               1  conda-forge
python                         3.5.4               2  conda-forge
python                         3.5.4               3  conda-forge
python                         3.5.5               0  conda-forge
python                         3.5.5               1  conda-forge
python                         3.5.5      h5001a0f_2  conda-forge
python                       3.6.0a3               0  conda-forge
python                       3.6.0a4               0  conda-forge
python                       3.6.0b1               0  conda-forge
python                       3.6.0b2               0  conda-forge
python                       3.6.0b2               1  conda-forge
python                       3.6.0b3               0  conda-forge
python                       3.6.0b4               0  conda-forge
python                      3.6.0rc1               0  conda-forge
python                         3.6.0               0  conda-forge
python                         3.6.0               1  conda-forge
python                         3.6.0               2  conda-forge
python                         3.6.1               0  conda-forge
python                         3.6.1               1  conda-forge
python                         3.6.1               2  conda-forge
python                         3.6.1               3  conda-forge
python                         3.6.2               0  conda-forge
python                         3.6.3               0  conda-forge
python                         3.6.3               1  conda-forge
python                         3.6.3               2  conda-forge
python                         3.6.3               3  conda-forge
python                         3.6.3               4  conda-forge
python                         3.6.4               0  conda-forge
python                         3.6.5               0  conda-forge
python                         3.6.5               1  conda-forge
python                         3.6.6      h5001a0f_0  conda-forge
python                         3.6.6      h5001a0f_2  conda-forge
python                         3.6.6      h5001a0f_3  conda-forge
python                         3.6.6   hd21baee_1000  conda-forge
python                         3.6.6   hd21baee_1001  conda-forge
python                         3.6.6   hd21baee_1002  conda-forge
python                         3.6.6   hd21baee_1003  conda-forge
python                         3.6.7   h357f687_1005  conda-forge
python                         3.6.7   h357f687_1006  conda-forge
python                         3.6.7 h357f687_1008_cpython  conda-forge
python                         3.6.7   h381d211_1004  conda-forge
python                         3.6.7      h5001a0f_0  conda-forge
python                         3.6.7      h5001a0f_1  conda-forge
python                         3.6.7   hd21baee_1000  conda-forge
python                         3.6.7   hd21baee_1001  conda-forge
python                         3.6.7   hd21baee_1002  conda-forge
python                         3.6.9       0_73_pypy  conda-forge
python                         3.6.9       2_73_pypy  conda-forge
python                         3.6.9       3_73_pypy  conda-forge
python                         3.6.9 h9d8adfe_0_cpython  conda-forge
python                        3.6.10 h8356626_1010_cpython  conda-forge
python                        3.6.10 h8356626_1011_cpython  conda-forge
python                        3.6.10 h9d8adfe_1009_cpython  conda-forge
python                        3.6.10 he5300dc_1010_cpython  conda-forge
python                        3.6.10 he5300dc_1011_cpython  conda-forge
python                        3.6.11 h425cb1d_0_cpython  conda-forge
python                        3.6.11 h425cb1d_1_cpython  conda-forge
python                        3.6.11 h425cb1d_2_cpython  conda-forge
python                        3.6.11 h4d41432_0_cpython  conda-forge
python                        3.6.11 h4d41432_1_cpython  conda-forge
python                        3.6.11 h4d41432_2_cpython  conda-forge
python                        3.6.11 h6f2ec95_0_cpython  conda-forge
python                        3.6.11 h6f2ec95_1_cpython  conda-forge
python                        3.6.11 h6f2ec95_2_cpython  conda-forge
python                        3.6.11 hffdb5ce_3_cpython  conda-forge
python                        3.6.12       4_73_pypy  conda-forge
python                        3.6.12       5_73_pypy  conda-forge
python                        3.6.12 hffdb5ce_0_cpython  conda-forge
python                        3.6.13 hb7a2778_1_cpython  conda-forge
python                        3.6.13 hb7a2778_2_cpython  conda-forge
python                        3.6.13 hffdb5ce_0_cpython  conda-forge
python                        3.6.15 hb7a2778_0_cpython  conda-forge
python                         3.7.0      h5001a0f_0  conda-forge
python                         3.7.0      h5001a0f_1  conda-forge
python                         3.7.0      h5001a0f_4  conda-forge
python                         3.7.0      h5001a0f_6  conda-forge
python                         3.7.0   hd21baee_1002  conda-forge
python                         3.7.0   hd21baee_1003  conda-forge
python                         3.7.0   hd21baee_1004  conda-forge
python                         3.7.0   hd21baee_1005  conda-forge
python                         3.7.0   hd21baee_1006  conda-forge
python                         3.7.0      hfd72cd7_0  conda-forge
python                         3.7.1   h381d211_1002  conda-forge
python                         3.7.1   h381d211_1003  conda-forge
python                         3.7.1      h5001a0f_0  conda-forge
python                         3.7.1   hd21baee_1000  conda-forge
python                         3.7.1   hd21baee_1001  conda-forge
python                         3.7.2      h381d211_0  conda-forge
python                         3.7.3      h33d41f4_1  conda-forge
python                         3.7.3      h357f687_2  conda-forge
python                         3.7.3      h5b0a415_0  conda-forge
python                         3.7.5 hffdb5ce_0_cpython  conda-forge
python                         3.7.6 cpython_h8356626_6  conda-forge
python                         3.7.6 cpython_he5300dc_6  conda-forge
python                         3.7.6      h357f687_1  conda-forge
python                         3.7.6      h357f687_2  conda-forge
python                         3.7.6 h357f687_3_cpython  conda-forge
python                         3.7.6 h357f687_4_cpython  conda-forge
python                         3.7.6 h8356626_5_cpython  conda-forge
python                         3.7.6 he5300dc_5_cpython  conda-forge
python                         3.7.8 h425cb1d_0_cpython  conda-forge
python                         3.7.8 h425cb1d_1_cpython  conda-forge
python                         3.7.8 h4d41432_0_cpython  conda-forge
python                         3.7.8 h4d41432_1_cpython  conda-forge
python                         3.7.8 h6238437_2_cpython  conda-forge
python                         3.7.8 h6f2ec95_0_cpython  conda-forge
python                         3.7.8 h6f2ec95_1_cpython  conda-forge
python                         3.7.8 h8bdb77d_2_cpython  conda-forge
python                         3.7.8 hdad413e_2_cpython  conda-forge
python                         3.7.8 hffdb5ce_3_cpython  conda-forge
python                         3.7.9       4_73_pypy  conda-forge
python                         3.7.9       5_73_pypy  conda-forge
python                         3.7.9 hffdb5ce_0_cpython  conda-forge
python                         3.7.9 hffdb5ce_100_cpython  conda-forge
python                        3.7.10       0_73_pypy  conda-forge
python                        3.7.10       1_73_pypy  conda-forge
python                        3.7.10 hb7a2778_101_cpython  conda-forge
python                        3.7.10 hb7a2778_102_cpython  conda-forge
python                        3.7.10 hb7a2778_103_cpython  conda-forge
python                        3.7.10 hb7a2778_104_cpython  conda-forge
python                        3.7.10 hf930737_102_cpython  conda-forge
python                        3.7.10 hf930737_103_cpython  conda-forge
python                        3.7.10 hf930737_104_cpython  conda-forge
python                        3.7.10 hffdb5ce_100_cpython  conda-forge
python                        3.7.12       0_73_pypy  conda-forge
python                        3.7.12 hb7a2778_100_cpython  conda-forge
python                        3.7.12 hf930737_100_cpython  conda-forge
python                         3.8.0      h357f687_0  conda-forge
python                         3.8.0      h357f687_1  conda-forge
python                         3.8.0      h357f687_2  conda-forge
python                         3.8.0      h357f687_3  conda-forge
python                         3.8.0      h357f687_4  conda-forge
python                         3.8.0      h357f687_5  conda-forge
python                         3.8.1      h357f687_1  conda-forge
python                         3.8.1      h357f687_2  conda-forge
python                         3.8.2 h8356626_5_cpython  conda-forge
python                         3.8.2 h8356626_6_cpython  conda-forge
python                         3.8.2 h8356626_7_cpython  conda-forge
python                         3.8.2 h9d8adfe_4_cpython  conda-forge
python                         3.8.2 he5300dc_5_cpython  conda-forge
python                         3.8.2 he5300dc_6_cpython  conda-forge
python                         3.8.2 he5300dc_7_cpython  conda-forge
python                         3.8.3 cpython_h8356626_0  conda-forge
python                         3.8.3 cpython_he5300dc_0  conda-forge
python                         3.8.4 cpython_h425cb1d_0  conda-forge
python                         3.8.4 cpython_h6f2ec95_0  conda-forge
python                         3.8.5 cpython_h425cb1d_0  conda-forge
python                         3.8.5 cpython_h4d41432_0  conda-forge
python                         3.8.5 cpython_h6f2ec95_0  conda-forge
python                         3.8.5      h1103e12_2  conda-forge
python                         3.8.5 h1103e12_3_cpython  conda-forge
python                         3.8.5 h1103e12_4_cpython  conda-forge
python                         3.8.5 h1103e12_5_cpython  conda-forge
python                         3.8.5 h1103e12_6_cpython  conda-forge
python                         3.8.5 h1103e12_7_cpython  conda-forge
python                         3.8.5 h1103e12_8_cpython  conda-forge
python                         3.8.5 h1103e12_9_cpython  conda-forge
python                         3.8.5 h425cb1d_1_cpython  conda-forge
python                         3.8.5 h425cb1d_2_cpython  conda-forge
python                         3.8.5 h4d41432_1_cpython  conda-forge
python                         3.8.5 h4d41432_2_cpython  conda-forge
python                         3.8.5 h6f2ec95_1_cpython  conda-forge
python                         3.8.5 h6f2ec95_2_cpython  conda-forge
python                         3.8.6 h852b56e_0_cpython  conda-forge
python                         3.8.6 hffdb5ce_1_cpython  conda-forge
python                         3.8.6 hffdb5ce_2_cpython  conda-forge
python                         3.8.6 hffdb5ce_3_cpython  conda-forge
python                         3.8.6 hffdb5ce_4_cpython  conda-forge
python                         3.8.6 hffdb5ce_5_cpython  conda-forge
python                         3.8.8 hffdb5ce_0_cpython  conda-forge
python                        3.8.10 h49503c6_1_cpython  conda-forge
python                        3.8.10 hb7a2778_2_cpython  conda-forge
python                        3.8.10 hffdb5ce_0_cpython  conda-forge
python                        3.8.12       0_73_pypy  conda-forge
python                        3.8.12 h0744224_3_cpython  conda-forge
python                        3.8.12 ha38a3c6_3_cpython  conda-forge
python                        3.8.12 hb7a2778_0_cpython  conda-forge
python                        3.8.12 hb7a2778_1_cpython  conda-forge
python                        3.8.12 hb7a2778_2_cpython  conda-forge
python                        3.8.12 hf930737_1_cpython  conda-forge
python                        3.8.12 hf930737_2_cpython  conda-forge
python                        3.8.13       0_73_pypy  conda-forge
python                        3.8.13 h582c2e5_0_cpython  conda-forge
python                        3.8.13 ha86cf86_0_cpython  conda-forge
python                         3.9.0 h2a148a8_4_cpython  conda-forge
python                         3.9.0 h852b56e_1_cpython  conda-forge
python                         3.9.0 h852b56e_2_cpython  conda-forge
python                         3.9.0 h852b56e_3_cpython  conda-forge
python                         3.9.0 hffdb5ce_5_cpython  conda-forge
python                         3.9.1 hffdb5ce_0_cpython  conda-forge
python                         3.9.1 hffdb5ce_1_cpython  conda-forge
python                         3.9.1 hffdb5ce_2_cpython  conda-forge
python                         3.9.1 hffdb5ce_3_cpython  conda-forge
python                         3.9.1 hffdb5ce_4_cpython  conda-forge
python                         3.9.1 hffdb5ce_5_cpython  conda-forge
python                         3.9.2 hffdb5ce_0_cpython  conda-forge
python                         3.9.4 hffdb5ce_0_cpython  conda-forge
python                         3.9.5 h49503c6_0_cpython  conda-forge
python                         3.9.6 h49503c6_0_cpython  conda-forge
python                         3.9.6 h49503c6_1_cpython  conda-forge
python                         3.9.7 h49503c6_0_cpython  conda-forge
python                         3.9.7 hb7a2778_1_cpython  conda-forge
python                         3.9.7 hb7a2778_2_cpython  conda-forge
python                         3.9.7 hb7a2778_3_cpython  conda-forge
python                         3.9.7 hf930737_3_cpython  conda-forge
python                         3.9.9 h543edf9_0_cpython  conda-forge
python                         3.9.9 h62f1059_0_cpython  conda-forge
python                        3.9.10       0_73_pypy  conda-forge
python                        3.9.10 h85951f9_0_cpython  conda-forge
python                        3.9.10 h85951f9_1_cpython  conda-forge
python                        3.9.10 h85951f9_2_cpython  conda-forge
python                        3.9.10 hc74c709_0_cpython  conda-forge
python                        3.9.10 hc74c709_1_cpython  conda-forge
python                        3.9.10 hc74c709_2_cpython  conda-forge
python                        3.9.12       0_73_pypy  conda-forge
python                        3.9.12 h2660328_0_cpython  conda-forge
python                        3.9.12 h2660328_1_cpython  conda-forge
python                        3.9.12 h9a8a25e_0_cpython  conda-forge
python                        3.9.12 h9a8a25e_1_cpython  conda-forge
python                        3.9.13 h2660328_0_cpython  conda-forge
python                        3.9.13 h9a8a25e_0_cpython  conda-forge
python                        3.10.0 h543edf9_1_cpython  conda-forge
python                        3.10.0 h543edf9_2_cpython  conda-forge
python                        3.10.0 h543edf9_3_cpython  conda-forge
python                        3.10.0 h62f1059_1_cpython  conda-forge
python                        3.10.0 h62f1059_2_cpython  conda-forge
python                        3.10.0 h62f1059_3_cpython  conda-forge
python                        3.10.1 h543edf9_0_cpython  conda-forge
python                        3.10.1 h543edf9_1_cpython  conda-forge
python                        3.10.1 h543edf9_2_cpython  conda-forge
python                        3.10.1 h62f1059_0_cpython  conda-forge
python                        3.10.1 h62f1059_1_cpython  conda-forge
python                        3.10.1 h62f1059_2_cpython  conda-forge
python                        3.10.2 h543edf9_0_cpython  conda-forge
python                        3.10.2 h62f1059_0_cpython  conda-forge
python                        3.10.2 h85951f9_1_cpython  conda-forge
python                        3.10.2 h85951f9_2_cpython  conda-forge
python                        3.10.2 h85951f9_3_cpython  conda-forge
python                        3.10.2 h85951f9_4_cpython  conda-forge
python                        3.10.2 hc74c709_1_cpython  conda-forge
python                        3.10.2 hc74c709_2_cpython  conda-forge
python                        3.10.2 hc74c709_3_cpython  conda-forge
python                        3.10.2 hc74c709_4_cpython  conda-forge
python                        3.10.4 h2660328_0_cpython  conda-forge
python                        3.10.4 h9a8a25e_0_cpython  conda-forge
python                        3.10.5 h582c2e5_0_cpython  conda-forge
python                        3.10.5 ha86cf86_0_cpython  conda-forge
python                        3.10.6 h582c2e5_0_cpython  conda-forge
python                        3.10.6 ha86cf86_0_cpython  conda-forge
```
````
You can also search for specific version requirements with `conda search`:

```bash
$ conda search 'python>=3.8'
```
````{admonition} View full output
:class: dropdown
```
Loading channels: done
# Name                       Version           Build  Channel
python                         3.8.0      h0371630_0  pkgs/main
python                         3.8.0      h0371630_1  pkgs/main
python                         3.8.0      h0371630_2  pkgs/main
python                         3.8.1      h0371630_1  pkgs/main
python                         3.8.2      h191fe78_0  pkgs/main
python                         3.8.2      hcf32534_0  pkgs/main
python                         3.8.2     hcff3b4d_13  pkgs/main
python                         3.8.2     hcff3b4d_14  pkgs/main
python                         3.8.3      hcff3b4d_0  pkgs/main
python                         3.8.3      hcff3b4d_2  pkgs/main
python                         3.8.5      h7579374_1  pkgs/main
python                         3.8.5      hcff3b4d_0  pkgs/main
python                         3.8.8      hdb3f193_4  pkgs/main
python                         3.8.8      hdb3f193_5  pkgs/main
python                        3.8.10      h12debd9_8  pkgs/main
python                        3.8.10      hdb3f193_7  pkgs/main
python                        3.8.11 h12debd9_0_cpython  pkgs/main
python                        3.8.12      h12debd9_0  pkgs/main
python                        3.8.13      h12debd9_0  pkgs/main
python                         3.9.0      hcff3b4d_1  pkgs/main
python                         3.9.0      hdb3f193_2  pkgs/main
python                         3.9.1      hdb3f193_2  pkgs/main
python                         3.9.2      hdb3f193_0  pkgs/main
python                         3.9.4      hdb3f193_0  pkgs/main
python                         3.9.5      h12debd9_4  pkgs/main
python                         3.9.5      hdb3f193_3  pkgs/main
python                         3.9.6      h12debd9_0  pkgs/main
python                         3.9.6      h12debd9_1  pkgs/main
python                         3.9.7      h12debd9_1  pkgs/main
python                        3.9.11      h12debd9_1  pkgs/main
python                        3.9.11      h12debd9_2  pkgs/main
python                        3.9.12      h12debd9_0  pkgs/main
python                        3.9.12      h12debd9_1  pkgs/main
python                        3.9.13      haa1d7c7_1  pkgs/main
python                        3.10.0      h12debd9_0  pkgs/main
python                        3.10.0      h12debd9_1  pkgs/main
python                        3.10.0      h12debd9_2  pkgs/main
python                        3.10.0      h12debd9_4  pkgs/main
python                        3.10.0      h12debd9_5  pkgs/main
python                        3.10.0      h151d27f_3  pkgs/main
python                        3.10.3      h12debd9_5  pkgs/main
python                        3.10.4      h12debd9_0  pkgs/main
```
````

You can combine the two conditions shown above (searching a specific channel and for a specific version):

```bash
$ conda search 'python[channel=conda-forge]>=3.8'
```
````{admonition} View full output
:class: dropdown
```
Loading channels: done
# Name                       Version           Build  Channel
python                         3.8.0      h357f687_0  conda-forge
python                         3.8.0      h357f687_1  conda-forge
python                         3.8.0      h357f687_2  conda-forge
python                         3.8.0      h357f687_3  conda-forge
python                         3.8.0      h357f687_4  conda-forge
python                         3.8.0      h357f687_5  conda-forge
python                         3.8.1      h357f687_1  conda-forge
python                         3.8.1      h357f687_2  conda-forge
python                         3.8.2 h8356626_5_cpython  conda-forge
python                         3.8.2 h8356626_6_cpython  conda-forge
python                         3.8.2 h8356626_7_cpython  conda-forge
python                         3.8.2 h9d8adfe_4_cpython  conda-forge
python                         3.8.2 he5300dc_5_cpython  conda-forge
python                         3.8.2 he5300dc_6_cpython  conda-forge
python                         3.8.2 he5300dc_7_cpython  conda-forge
python                         3.8.3 cpython_h8356626_0  conda-forge
python                         3.8.3 cpython_he5300dc_0  conda-forge
python                         3.8.4 cpython_h425cb1d_0  conda-forge
python                         3.8.4 cpython_h6f2ec95_0  conda-forge
python                         3.8.5 cpython_h425cb1d_0  conda-forge
python                         3.8.5 cpython_h4d41432_0  conda-forge
python                         3.8.5 cpython_h6f2ec95_0  conda-forge
python                         3.8.5      h1103e12_2  conda-forge
python                         3.8.5 h1103e12_3_cpython  conda-forge
python                         3.8.5 h1103e12_4_cpython  conda-forge
python                         3.8.5 h1103e12_5_cpython  conda-forge
python                         3.8.5 h1103e12_6_cpython  conda-forge
python                         3.8.5 h1103e12_7_cpython  conda-forge
python                         3.8.5 h1103e12_8_cpython  conda-forge
python                         3.8.5 h1103e12_9_cpython  conda-forge
python                         3.8.5 h425cb1d_1_cpython  conda-forge
python                         3.8.5 h425cb1d_2_cpython  conda-forge
python                         3.8.5 h4d41432_1_cpython  conda-forge
python                         3.8.5 h4d41432_2_cpython  conda-forge
python                         3.8.5 h6f2ec95_1_cpython  conda-forge
python                         3.8.5 h6f2ec95_2_cpython  conda-forge
python                         3.8.6 h852b56e_0_cpython  conda-forge
python                         3.8.6 hffdb5ce_1_cpython  conda-forge
python                         3.8.6 hffdb5ce_2_cpython  conda-forge
python                         3.8.6 hffdb5ce_3_cpython  conda-forge
python                         3.8.6 hffdb5ce_4_cpython  conda-forge
python                         3.8.6 hffdb5ce_5_cpython  conda-forge
python                         3.8.8 hffdb5ce_0_cpython  conda-forge
python                        3.8.10 h49503c6_1_cpython  conda-forge
python                        3.8.10 hb7a2778_2_cpython  conda-forge
python                        3.8.10 hffdb5ce_0_cpython  conda-forge
python                        3.8.12       0_73_pypy  conda-forge
python                        3.8.12 h0744224_3_cpython  conda-forge
python                        3.8.12 ha38a3c6_3_cpython  conda-forge
python                        3.8.12 hb7a2778_0_cpython  conda-forge
python                        3.8.12 hb7a2778_1_cpython  conda-forge
python                        3.8.12 hb7a2778_2_cpython  conda-forge
python                        3.8.12 hf930737_1_cpython  conda-forge
python                        3.8.12 hf930737_2_cpython  conda-forge
python                        3.8.13       0_73_pypy  conda-forge
python                        3.8.13 h582c2e5_0_cpython  conda-forge
python                        3.8.13 ha86cf86_0_cpython  conda-forge
python                         3.9.0 h2a148a8_4_cpython  conda-forge
python                         3.9.0 h852b56e_1_cpython  conda-forge
python                         3.9.0 h852b56e_2_cpython  conda-forge
python                         3.9.0 h852b56e_3_cpython  conda-forge
python                         3.9.0 hffdb5ce_5_cpython  conda-forge
python                         3.9.1 hffdb5ce_0_cpython  conda-forge
python                         3.9.1 hffdb5ce_1_cpython  conda-forge
python                         3.9.1 hffdb5ce_2_cpython  conda-forge
python                         3.9.1 hffdb5ce_3_cpython  conda-forge
python                         3.9.1 hffdb5ce_4_cpython  conda-forge
python                         3.9.1 hffdb5ce_5_cpython  conda-forge
python                         3.9.2 hffdb5ce_0_cpython  conda-forge
python                         3.9.4 hffdb5ce_0_cpython  conda-forge
python                         3.9.5 h49503c6_0_cpython  conda-forge
python                         3.9.6 h49503c6_0_cpython  conda-forge
python                         3.9.6 h49503c6_1_cpython  conda-forge
python                         3.9.7 h49503c6_0_cpython  conda-forge
python                         3.9.7 hb7a2778_1_cpython  conda-forge
python                         3.9.7 hb7a2778_2_cpython  conda-forge
python                         3.9.7 hb7a2778_3_cpython  conda-forge
python                         3.9.7 hf930737_3_cpython  conda-forge
python                         3.9.9 h543edf9_0_cpython  conda-forge
python                         3.9.9 h62f1059_0_cpython  conda-forge
python                        3.9.10       0_73_pypy  conda-forge
python                        3.9.10 h85951f9_0_cpython  conda-forge
python                        3.9.10 h85951f9_1_cpython  conda-forge
python                        3.9.10 h85951f9_2_cpython  conda-forge
python                        3.9.10 hc74c709_0_cpython  conda-forge
python                        3.9.10 hc74c709_1_cpython  conda-forge
python                        3.9.10 hc74c709_2_cpython  conda-forge
python                        3.9.12       0_73_pypy  conda-forge
python                        3.9.12 h2660328_0_cpython  conda-forge
python                        3.9.12 h2660328_1_cpython  conda-forge
python                        3.9.12 h9a8a25e_0_cpython  conda-forge
python                        3.9.12 h9a8a25e_1_cpython  conda-forge
python                        3.9.13 h2660328_0_cpython  conda-forge
python                        3.9.13 h9a8a25e_0_cpython  conda-forge
python                        3.10.0 h543edf9_1_cpython  conda-forge
python                        3.10.0 h543edf9_2_cpython  conda-forge
python                        3.10.0 h543edf9_3_cpython  conda-forge
python                        3.10.0 h62f1059_1_cpython  conda-forge
python                        3.10.0 h62f1059_2_cpython  conda-forge
python                        3.10.0 h62f1059_3_cpython  conda-forge
python                        3.10.1 h543edf9_0_cpython  conda-forge
python                        3.10.1 h543edf9_1_cpython  conda-forge
python                        3.10.1 h543edf9_2_cpython  conda-forge
python                        3.10.1 h62f1059_0_cpython  conda-forge
python                        3.10.1 h62f1059_1_cpython  conda-forge
python                        3.10.1 h62f1059_2_cpython  conda-forge
python                        3.10.2 h543edf9_0_cpython  conda-forge
python                        3.10.2 h62f1059_0_cpython  conda-forge
python                        3.10.2 h85951f9_1_cpython  conda-forge
python                        3.10.2 h85951f9_2_cpython  conda-forge
python                        3.10.2 h85951f9_3_cpython  conda-forge
python                        3.10.2 h85951f9_4_cpython  conda-forge
python                        3.10.2 hc74c709_1_cpython  conda-forge
python                        3.10.2 hc74c709_2_cpython  conda-forge
python                        3.10.2 hc74c709_3_cpython  conda-forge
python                        3.10.2 hc74c709_4_cpython  conda-forge
python                        3.10.4 h2660328_0_cpython  conda-forge
python                        3.10.4 h9a8a25e_0_cpython  conda-forge
python                        3.10.5 h582c2e5_0_cpython  conda-forge
python                        3.10.5 ha86cf86_0_cpython  conda-forge
python                        3.10.6 h582c2e5_0_cpython  conda-forge
python                        3.10.6 ha86cf86_0_cpython  conda-forge
```
````


(removing-packages)=
### Removing packages

Another crucial aspect of managing an environment involves removing packages.
Conda includes the `remove` subcommand for this operation, which allows you to specify a list of packages you wish to remove.
You can do this within an activated environment, or specify to Conda the environment from which you want to remove packages.

When creating our `data-sci-env` we installed `pandas=1.4.2`, let's imagine we made a mistake here and wanted a different version.
We could remove this version of pandas with the following command:

```bash
$ conda remove -n data-sci-env pandas
```
````{admonition} View full output
:class: dropdown
```
Collecting package metadata (repodata.json): done
Solving environment: done

## Package Plan ##

  environment location: /home/home01/arcuser/.conda/envs/data-sci-env

  removed specs:
    - pandas


The following packages will be downloaded:

    package                    |            build
    ---------------------------|-----------------
    numpy-1.22.3               |  py310hfa59a62_0          10 KB
    numpy-base-1.22.3          |  py310h9585f30_0        11.8 MB
    ------------------------------------------------------------
                                           Total:        11.8 MB

The following packages will be REMOVED:

  bottleneck-1.3.5-py310ha9d4c09_0
  numexpr-2.8.3-py310hcea2de6_0
  pandas-1.4.2-py310h295c915_0
  pytz-2022.1-py310h06a4308_0

The following packages will be UPDATED:

  numpy                              1.21.5-py310h1794996_3 --> 1.22.3-py310hfa59a62_0
  numpy-base                         1.21.5-py310hcba007f_3 --> 1.22.3-py310h9585f30_0


Proceed ([y]/n)?
```
````

When removing packages as with installing them Conda will ask for user confirmation to proceed.
As you can see in the above example, removing one package may also lead to the removal of additional packages and can cause other packages to update.

With these changes made we can now install a newer version of pandas using `conda install`.

Of course, this can also be easily done by updating our `environment.yml` file to remove the package, and running the `update` command shown above with the flag `--prune`.

(updating-a-package)=
### Updating a package

The above example is slightly artificial as removing a package to install a more recent version is a long-winded way of doing things with Conda.
If we want to update a package to a more recent version Conda provides the `update` subcommand to achieve this.
Crucially, `conda update` will update a package to its most recent version and can't be used to specific a particular version.

Let's say we wanted to update the `matplotlib` library to the most recent version in our `data-sci-env`.

```bash
$ conda activate data-sci-env

(data-sci-env)$ conda update matplotlib
```
````{admonition} View full output
:class: dropdown
```
Collecting package metadata (current_repodata.json): done
Solving environment: done


## Package Plan ##

  environment location: /home/home01/arcuser/.conda/envs/data-sci-env

  added / updated specs:
    - matplotlib


The following packages will be downloaded:

    package                    |            build
    ---------------------------|-----------------
    matplotlib-3.5.2           |  py310h06a4308_0           7 KB
    matplotlib-base-3.5.2      |  py310hf590b9c_0         7.8 MB
    ------------------------------------------------------------
                                           Total:         7.8 MB

The following packages will be UPDATED:

  matplotlib                          3.5.1-py310h06a4308_1 --> 3.5.2-py310h06a4308_0
  matplotlib-base                     3.5.1-py310ha18d171_1 --> 3.5.2-py310hf590b9c_0


Proceed ([y]/n)?
```
````

When requesting to update a package Conda will also update other dependencies of the package that you wish to update, and can potentially install new packages that are required.

Again, this can also be easily done by updating our `environment.yml` file to change the version of a specific package, and running the `update` command shown above with the flag `--prune`.

## Summary

```{important}

- [Introduces Conda](introduction) as a cross-platform package and environment manager
- Highlights options for [how to install Conda](installing-conda)
- Introduces Conda environments for separating specific package dependencies on a project-by-project basis
  - How to [create an environment using Conda](creating-environments)
  - How to use a created Conda environment through [environment activation](activating-environments)
  - Leaving an environment through [deactivation](deactivating-environments)
  - [Listing available Conda environments](listing-current-environments)
  - [Deleting Conda environments](removing-a-conda-environment)
  - [Exporting and sharing Conda Environments](sharing-conda-environments)
- Shows how Conda can be used for managing packages
  - Using Conda to [search Conda repositories for a package](searching-for-packages)
  - Using Conda to [install a package](installing-packages)
  - Using Conda to [remove a package](removing-packages)
  - Using Conda to [update a package](updating-a-package)

```
