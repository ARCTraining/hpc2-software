# A template jupyter book documentation repository

This is a template repository that can be used when generating documentation using [Jupyter-books](https://github.com/executablebooks/jupyter-book) and hosting the site via GitHub pages.

## Working with this project locally

You can get this project working locally by using the environment.yml file to create a conda environment that contains all the dependencies required to get started.

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

Jupyterbook now supports [Windows](https://jupyterbook.org/en/stable/advanced/windows.html) although the steps for configuring a development environment using Vagrant are available below:

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