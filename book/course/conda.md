# Conda package manager

Content from this lesson has been inspired and adapted from a number of sources including:
- [Conda documentation](https://docs.conda.io/en/latest/)
- [Carpentries Incubator: Introduction to conda for data scientists](https://carpentries-incubator.github.io/introduction-to-conda-for-data-scientists/)

## Introduction

Conda is an open source package management and environment management system that runs on multiple operating systems (Windows, Linux, macOS). Its features include:

- Conda quickly installs, runs and updates packages and their dependencies. 
- Conda easily creates, saves, loads and switches between environments on your local computer. 
- It was created for Python programs, but it can package and distribute software for any language.

Conda is a tool that helps find and install packages, but also lets you manage different software environments where you can install different configurations of packages. 
For example, this enables you to install different versions of Python in two separate environments without creating incompatibities in either of those projects.

````{admonition} Conda, Miniconda and Anaconda
:class: note

It's common to be confused when confronted with Conda, Miniconda and Anaconda. Conda is specifically the package and environment manager tool itself. 
Miniconda is a distribution of Python that includes the Conda package manager and a few other core packages. 
Anaconda is another distribution of Python that includes the Conda package manager but also includes a number of widely used Python packages and other Anaconda features such as the [Anaconda Navigator](https://docs.anaconda.com/anaconda/navigator/). 

```{image} ../assets/img/course/conda/miniconda_v_anaconda.png
:alt: Miniconda versus Anaconda, reproduced from https://carpentries-incubator.github.io/introduction-to-conda-for-data-scientists/01-getting-started-with-conda/index.html
:width: 400px
:align: center
```

_Reproduced from [Introduction to Conda for Data Scientists](https://carpentries-incubator.github.io/introduction-to-conda-for-data-scientists/01-getting-started-with-conda/index.html)_

````

Conda is widely used across scientific computing and data science based domains due it's well populated package ecosystem and environment management capabilities. 

- Conda installs prebuilt packages, which allows for installing complicated packages in one step because someone else has built the tool with the right compilers and libraries
- The cross platform nature of Conda allows for users to more easily share the environments. This helps researchers share their computational environment along side their data and analysis, helping improve the reproducibility of their research
- Conda also provides access to widely used machine learning and data science libraries such as TensorFlow, SciPy, NumPy that are available as pre-configured, hardware specific packages (such as GPU-enabled TensorFlow) allowing for code to be as performant as possible

## Installing conda

You can install conda from a number of sources:
- As part of the [Anaconda Python distribution](https://www.anaconda.com/products/distribution)
- As part of [Miniconda](https://docs.conda.io/en/latest/miniconda.html)
- As part of the conda-forge channel [Miniforge](https://github.com/conda-forge/miniforge) distribution

Conda is cross-platform, therefore all these distributions have installers for both Windows, MacOS and Linux. 
For Miniconda, you visit the [Miniconda page](https://docs.conda.io/en/latest/miniconda.html) on the conda website, select the installer corresponding to your operating system and run the downloaded file on your machine. 
When installing Miniconda you may be prompted to select various settings during installation, our recommendation is to leave these settings as the defaults if you're unsure.

If you have questions or issues installing conda locally please get in touch via the [Research Computing Contact form](https://leeds.service-now.com/it?id=sc_cat_item&sys_id=7587b2530f675f00a82247ece1050eda).

## Conda environments

As well as managing packages Conda also allows you to create and manage environments. 
A Conda environment is a directory that contains a specific set of installed packages and tools. 
This allows you to separate the dependencies of different projects cleanly so for example, you can use Python 3.7 in one Conda environment to reproduce a collaborators results but use Python 3.10 in your own projects without any hassle. 
Conda makes it easy to switch between different environments and allows you to create and delete them as required. 
Conda environments also make it easier to share our environment setup between machines and with collaborators as we can export our environments into a text file.

```{note} The base environment
By default conda includes the `base` environment. 
This contains a starting installation of python and the dependencies of the conda tool itself. 
Therefore, it's **best practice** to not install packages into the `base` environment and create your own environments into which you install the tools you need.
```

### Creating environments

You can create an environment with conda with the subcommand `conda create`. 
When creating an environment we need to give it a name, we recommend giving it a name related to the project you're going to use the environment for.

```bash
$ conda create --name py39-env python=3.9
```
The above command will prompt conda to create a new environment called `py39-env` and install into it `python` at version 3.9. 
We can specify multiple packages when creating a conda environment by separating each package name with a space.

```bash
$ conda create --name data-sci-env pandas=1.4.2 matplotlib=3.5.1 scikit-learn
```

With the above command we create a new environment but don't specify to install Python. 
However, because we've specified Python packages which depend on Python being installed to run conda will install the high version of Python suitable for these packages.

### Activating environments

To use a conda environment we need to activate it. 
Activating our environment does a number of steps that sets the terminal we're using up so that it can see all of the installed packages in the environment, making it ready for use.

```bash
$ conda activate data-sci-env

(data-sci-env)$ 
```

You use the subcommand `conda activate ENVNAME` for environment activation, where `ENVNAME` is the name of the environment you wish to activate. 
You can see it has successfully activated when it returns your prompt with the environment name prepended in brackets.

### Deactivating environments

You can deactivate your current environment with another simple subcommand `conda deactivate`.

```bash
(data-sci-env)$ conda deactivate

$
```

### Listing current environments

If you ever want to see your list of current environments on your machine you can you the subcommand `conda env list`. 
This will return a list of the available conda environments you can use and the environment location in your filesystem.

```bash
$ conda env list

py39-env                   /home/home01/arcuser/.conda/envs/py39-env
data-sci-env               /home/home01/arcusers/.conda/envs/data-sci-env

```

### Removing a Conda environment

It is also possible to delete a Conda environment through the `remove` subcommand. 
This [command is outlined below](#removing-packages) in relation to removing specific packages but can also be used to delete an entire Conda environment.

To remove the `py39-env` we created earlier we use the command:

```bash
$ conda remove --name py39-env --all
```
```output
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

Conda checks for user confirmation that we wish to proceed and outlines for us exactly which packages are being removed. 
On proceeding with removing the environment all associated environment files and packages are deleted.

```{important}
Using `conda remove` to delete an environment is irreversible.
You cannot undo deletion of an environment to the exact state it was in before deletion.
However, if you have exported details of your environment it is possible to recreate it.
```

### Sharing Conda environments

If you need to share a Conda environment with others or between machines its possible to use Conda to export a file containing a specification of packages installed in that environment.
With this environment file and Conda installed on another device its possible to recreate the environment with the same specifications.

Let's assume we want to share our `data-sci-env` Conda environment with others. To do this we first need to create the `environment.yml` file containing our environment specification. 
You can create a very detailed specification that includes operating system specific hashes with the command:

```bash
$ conda activate data-sci-env

(data-sci-env)$ conda env export > environment.yml
```

Above, we activate the environment we want to create an `environment.yml` file from and then use the command `conda env export`.
This outputs the environment specification to the standard output in the terminal so to capture and write this to a file we redirect the output to `environment.yml`.

This command also exports a line called `prefix:` specifying the directory location of the environment on your filesystem. 
This isn't required when sharing your environment and should be removed, you can do this manually or use `grep` when exporting your environment.

```bash
(data-sci-env)$ conda env export | grep -v ^prefix: > environment.yml
```

We can share the `environment.yml` file with collaborators and/or commit the file to version control to ensure people can recreate the required Conda environment.

You can recreate a Conda environment from a file with the following command:

```bash
$ conda env create -f environment.yml
```

Here we're specifying Conda create a new environment and using the `-f` option to specify that it creates the environment using a file with an environment specification.
We pass the file path to the environment file as the argument following `-f`.

#### Creating a cross platform environment file

As noted above using `conda env export` creates a highly specific environment file, this often causes difficulties when sharing environments across operating systems as the `environment.yml` contains operating system specific hashes for each package.

There are two possible methods of creating a more flexible `environment.yml`.

##### 1. Using `conda env export --from-history`

By default `conda env export` exports an environments entire specification, including dependencies of packages you `conda install` and their associated hashes. 
If you use `conda env export --from-history` Conda only exports packages explicitly installed with `conda install`. 
It does not include dependencies of those packages and therefore allows different operating systems to more flexibly install package dependencies.

For the above example with `data-sci-env` we would export a more flexible `environment.yml` with:
```bash
(data-sci-env)$ conda env export --from-history | grep -v ^prefix: > environment.yml
```

##### 2. Manually create an `environment.yml`

The other option is to manually specify the `environment.yml` file. 
This is often more fiddly than just exporting an environment but can be preferable to ensure all the desired dependencies of your project are captured.
Environment files are written in YAML, a markup language, and have the standard pattern of:
```yaml
name: data-sci-env
channels:
- defaults
dependencies:
- scikit-learn
- matplotlib=3.5.1
- pandas=1.4.3
```
Where you specify the environment name, a list of Conda channels used to install packages, and under dependencies a list of packages to be installed. You can also include version specification within the `environment.yml` allowing you to 

Understanding the differences between weays to create environment files is important when you come to deciding on how best to share your project. 
It's important to consider the balance of reproducibility and portability, `conda env export` captures the exact specification of an environment including all installed packages, their dependencies and package hashes.
Sometimes this level of detail should be included to ensure maximum reproduciblity of a project, when looking to validate results, but it's important to also balance being able to allow people to reproduce your work on other systems.


## Using conda to install packages

With the Conda command line tool searching for and installing packages is can be performed with the following subcommands:
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

This command searches for packages based on the argument provided. 
It searches in package repositories called [Conda Channels](https://docs.conda.io/projects/conda/en/stable/user-guide/concepts/channels.html) which are remote websites where built Conda packages have been uploaded to. 
By default Conda uses the `defaults` channel which points to the Anaconda maintained package repository https://repo.anaconda.com/pkgs/main and https://repo.anaconda.com/pkgs/r. 
Other channels are also available such as [`conda-forge`](https://conda-forge.org/) and we can specify when installing packages or when searching which channels we wish to search.

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

Earlier we created the `data-sci-env` and installed some useful data science packages. 
We've discovered we also need the `statsmodels` package for some extra work we want to do so we'll look at using `conda install` to install this package within our existing environment.

To install packages into an existing environment we need to activate it with the [subcommand shown above](./#activating-environments).

```bash
$ conda activate data-sci-env

(data-sci-env)$
```

```bash
(data-sci-env)$ conda install statsmodels
```
```output
(data-sci-env)$ conda install statsmodels
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

Here conda asks us if we're happy to proceed with the installation and specifies all the other packages that will be installed or updated that are required for our specified package. 
We confirm we wish to proceed by entering `y` and pressing Return.

```output
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

This installs any packages that are currently not installed (conda caches packages locally incase they are required by other packages, this speeds up installs but uses more disk space to maintain this cache).
