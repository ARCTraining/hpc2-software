[![Project Status: WIP – Initial development is in progress, but there has not yet been a stable, usable release suitable for the public.](https://www.repostatus.org/badges/latest/wip.svg)](https://www.repostatus.org/#wip)

# 👷‍♀️ HPC2: Installing and Managing Applications on the HPC 👷‍♂️

**This project is a work in progress and is not yet in a stable state.**

The home for all course notes for HPC2: Installing and managing application on the HPC Research Computing course.

## Lesson Format

We are currently basing our lessons on the Software Carpentry format (e.g. https://swcarpentry.github.io/shell-novice/) which sets a good example of language and structure to follow.
Currently these lessons are 25 minutes long (to facilitate a 10 minute break each hour).
The current lesson format to follow as a guide:

* Overview (questions/objectives)
* Intro paragraph
* Worked Examples
* Exercises
* Key points

## Contributing to this project

### Working with this project locally

You will need to install the [conda package management tool](https://docs.conda.io/en/latest/) before you can get this project working locally.
You can use the `environment.yml` file included to create a conda environment that contains all the dependencies required to get started.

```{bash}
$ git clone https://github.com/ARCTraining/template-jb-docs.git

$ conda env create -f environment.yml
```

To build the html content locally you can use the `jupyter-book` command line tool:

```{bash}
# navigate to the repository root
$ cd template-jb-docs
# sometimes worth running jupyter-book clean book/ to remove old files
$ jupyter-book build book/
```
### Windows

Jupyterbook now supports [Windows](https://jupyterbook.org/en/stable/advanced/windows.html) so the above steps can also be used on a Windows terminal.

#### Set up a development environment using Vagrant

To aid with this we have created a `Vagrantfile` that can allow Windows users who have a virtualisation provider installed (such as [VirtualBox](https://www.virtualbox.org/)) and [Vagrant](https://www.vagrantup.com/) installed to create a headless virtual Linux machine that will build the jupyter book. You can do this with the following steps once you've installed a virtualisation provider and vagrant:
```
# within git-bash or powershell
$ cd template-jb-docs
$ vagrant up

# to rebuild the site after changes with the vagrant box running
$ vagrant reload --provision

# don't forget to destroy the box when you're done
$ vagrant destroy
```

This will build the jupyter-book html files on your Windows file system (by navigating via /vagrant) so your local build will still persist after you've destroyed your vagrant box.
