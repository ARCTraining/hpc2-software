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



## Conda environments

## Building your own conda packages
