# Conda package manager

Content from this lesson has been inspired and adapted from a number of sources including:
- [Conda documentation](https://docs.conda.io/en/latest/)
- [Carpentries Incubator: Introduction to conda for data scientists](https://carpentries-incubator.github.io/introduction-to-conda-for-data-scientists/)

## Introduction

Conda is an open source package management and environment management system that runs on multiple operating systems (Windows, Linux, macOS). It's features include:

- Conda quickly installs, runs and updates packages and their dependencies. 
- Conda easily creates, saves, loads and switches between environments on your local computer. 
- It was created for Python programs, but it can package and distribute software for any language.

Conda is a tool that helps find and install packages, but also lets you manage different software environments where you can install different configurations of packages. For example, this enables you to install different versions of Python in two separate environments without creating incompatibities in either of those projects.

````{admonition} Conda, Miniconda and Anaconda
:class: note

It's common to be confused when confronted with Conda, Miniconda and Anaconda. Conda is specifically the package and environment manager tool itself. Miniconda is a distribution of Python that includes the Conda package manager and a few other core packages. Anaconda is another distribution of Python that includes the Conda package manager but also includes a number of widely used Python packages and other Anaconda features such as the [Anaconda Navigator](https://docs.anaconda.com/anaconda/navigator/). 

```{image} ../assets/img/course/conda/miniconda_v_anaconda.png
:alt: Miniconda versus Anaconda, reproduced from https://carpentries-incubator.github.io/introduction-to-conda-for-data-scientists/01-getting-started-with-conda/index.html
:width: 400px
:align: center
```

_Reproduced from [Introduction to Conda for Data Scientists](https://carpentries-incubator.github.io/introduction-to-conda-for-data-scientists/01-getting-started-with-conda/index.html)_

````

Conda is widely used across scientific computing and data science based domains due it's well populated package ecosystem and environment management capabilities. 

- Conda installs prebuilt packages, which allows for installing complicated packages in one-step because someone else has built the tool with the right compilers and libraries
- The cross platform nature of Conda allows for users to more easily share the environments. This helps researchers share their computational environment along side their data and analysis helping improve the reproducibility of their research
- Conda also provides access to widely used machine learning and data science libraries such as TensorFlow, SciPy, NumPy that are available as pre-configured, hardware specific packages (such as GPU-enabled TensorFlow) allowing for code to be as performant as possible

## Installing conda

You can install conda from a number of sources:
- As part of the [Anaconda Python distribution](https://www.anaconda.com/products/distribution)
- As part of [Miniconda](https://docs.conda.io/en/latest/miniconda.html)
- As part of the conda-forge channel [Miniforge](https://github.com/conda-forge/miniforge) distribution

Conda is cross-platform, therefore all these distributions have installers for both Windows, MacOS and Linux. For Miniconda, you visit the [Miniconda page](https://docs.conda.io/en/latest/miniconda.html) on the conda website, select the installer corresponding to your operating system and run the downloaded file on your machine. When installing Miniconda you may be prompted to select various settings during installation, our recommendation is to leave these settings as the defaults if you're unsure.

If you have questions or issues installing conda locally please get in touch via the [Research Computing Contact form](https://leeds.service-now.com/it?id=sc_cat_item&sys_id=7587b2530f675f00a82247ece1050eda).

## Using conda to install packages

With the Conda command line tool searching for and installing packages is can be performed with the following commands:
- `conda search`
- `conda install`

### Searching for packages

```bash
$ conda search python
```
```output
Loading channels: done
# Name                       Version           Build  Channel             
python                        2.7.13     hac47a24_15  pkgs/main           
python                        2.7.13     heccc3f1_16  pkgs/main           
...                           ...        ...          ...          
python                        3.10.3      h12debd9_5  pkgs/main           
python                        3.10.4      h12debd9_0  pkgs/main 
```

This command searches for packages based on the argument provided. It searches in package repositories called [Conda Channels](https://docs.conda.io/projects/conda/en/stable/user-guide/concepts/channels.html) which are remote websites where built Conda packages have been uploaded to. By default Conda uses the `defaults` channel which points to the Anaconda maintained package repository https://repo.anaconda.com/pkgs/main and https://repo.anaconda.com/pkgs/r. Other channels are also available such as [`conda-forge`](https://conda-forge.org/) and we can specify when installing packages or when searching which channels we wish to search.

```bash
$ conda search 'python[channel=conda-forge]'
```
```output
Loading channels: done
# Name                       Version           Build  Channel             
python                         1.0.1               0  conda-forge         
python                           1.2               0  conda-forge         
...                           ...                ...  ...        
python                        3.10.5 h582c2e5_0_cpython  conda-forge         
python                        3.10.5 ha86cf86_0_cpython  conda-forge
```

You can also search for specific version requirements with `conda search`:

```bash
$ conda search 'python>=3.8'
```
```output
Loading channels: done
# Name                       Version           Build  Channel             
python                         3.8.0      h0371630_0  pkgs/main           
python                         3.8.0      h0371630_1  pkgs/main           
...                           ...                ...  ...        
python                        3.10.3      h12debd9_5  pkgs/main           
python                        3.10.4      h12debd9_0  pkgs/main
```

You can combine the two conditions shown above (searching a specific channel and for a specific version):

```bash
$ conda search 'python[channel=conda-forge]>=3.8'
```
```output
Loading channels: done
# Name                       Version           Build  Channel             
python                         3.8.0      h357f687_0  conda-forge         
python                         3.8.0      h357f687_1  conda-forge         
...                           ...                ...  ...       
python                        3.10.5 h582c2e5_0_cpython  conda-forge         
python                        3.10.5 ha86cf86_0_cpython  conda-forge 
```

### Installing packages

Installing packages via conda is performed using the `install` subcommand with the format `conda install PACKAGE`, where `PACKAGE` is the name of the package you wish to install.

```bash
$ conda install python
```
```output
$ conda install python
Collecting package metadata (current_repodata.json): done
Solving environment: done

## Package Plan ##

  environment location: /home/home01/medacola/.conda/envs/test

  added / updated specs:
    - python


The following packages will be downloaded:

    package                    |            build
    ---------------------------|-----------------
    libsqlite-3.39.3           |       h753d276_0         789 KB  conda-forge
    python-3.10.6              |ha86cf86_0_cpython        29.0 MB  conda-forge
    setuptools-65.3.0          |     pyhd8ed1ab_1         782 KB  conda-forge
    tzdata-2022c               |       h191b570_0         119 KB  conda-forge
    xz-5.2.6                   |       h166bdaf_0         409 KB  conda-forge
    ------------------------------------------------------------
                                           Total:        31.0 MB

The following NEW packages will be INSTALLED:

  _libgcc_mutex      conda-forge/linux-64::_libgcc_mutex-0.1-conda_forge
  _openmp_mutex      conda-forge/linux-64::_openmp_mutex-4.5-2_gnu
  bzip2              conda-forge/linux-64::bzip2-1.0.8-h7f98852_4
  ca-certificates    conda-forge/linux-64::ca-certificates-2022.6.15-ha878542_0
  ld_impl_linux-64   conda-forge/linux-64::ld_impl_linux-64-2.36.1-hea4e1c9_2
  libffi             conda-forge/linux-64::libffi-3.4.2-h7f98852_5
  libgcc-ng          conda-forge/linux-64::libgcc-ng-12.1.0-h8d9b700_16
  libgomp            conda-forge/linux-64::libgomp-12.1.0-h8d9b700_16
  libnsl             conda-forge/linux-64::libnsl-2.0.0-h7f98852_0
  libsqlite          conda-forge/linux-64::libsqlite-3.39.3-h753d276_0
  libuuid            conda-forge/linux-64::libuuid-2.32.1-h7f98852_1000
  libzlib            conda-forge/linux-64::libzlib-1.2.12-h166bdaf_2
  ncurses            conda-forge/linux-64::ncurses-6.3-h27087fc_1
  openssl            conda-forge/linux-64::openssl-3.0.5-h166bdaf_1
  pip                conda-forge/noarch::pip-22.2.2-pyhd8ed1ab_0
  python             conda-forge/linux-64::python-3.10.6-ha86cf86_0_cpython
  readline           conda-forge/linux-64::readline-8.1.2-h0f457ee_0
  setuptools         conda-forge/noarch::setuptools-65.3.0-pyhd8ed1ab_1
  tk                 conda-forge/linux-64::tk-8.6.12-h27826a3_0
  tzdata             conda-forge/noarch::tzdata-2022c-h191b570_0
  wheel              conda-forge/noarch::wheel-0.37.1-pyhd8ed1ab_0
  xz                 conda-forge/linux-64::xz-5.2.6-h166bdaf_0


Proceed ([y]/n)?

```

Here conda asks us if we're happy to proceed with the installation and specifies all the other packages that will be installed or updated that are required for our specified package. We confirm we wish to proceed by entering `y` and pressing Return.

```output
Proceed ([y]/n)? y

Downloading and Extracting Packages
tzdata-2022c         | 119 KB    | ############################################################################ | 100%
setuptools-65.3.0    | 782 KB    | ############################################################################ | 100%
python-3.10.6        | 29.0 MB   | ############################################################################ | 100%
libsqlite-3.39.3     | 789 KB    | ############################################################################ | 100%
xz-5.2.6             | 409 KB    | ############################################################################ | 100%
Preparing transaction: done
Verifying transaction: done
Executing transaction: done
```

This installs any packages that are currently not installed (conda caches packages locally incase they are required by other packages, this speeds up installs but uses more disk space to maintain this cache).