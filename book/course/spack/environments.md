# Environments

Environments in Spack feel a whole lot like environments in Conda; they allow
you to bundle up a number of different pieces of software into a single entity,
making a single coherent collection.

## Creating an environment

Here's a minimal workflow for creating an environment:

- Create a blank environment

  ```bash
  spack env create myenv
  ```

- Add a list of software to it

  ```bash
  spack -e myenv add bash@5 python py-numpy py-scipy py-matplotlib openssl@1.1.1t
  ```

- Tweak the config if required

  ```bash
  spack -e myenv config edit
  ```

  Please review the references for more information on options here, but nothing
  needs changing this time.

- Validate the installation

  ```bash
  spack -e myenv concretize
  ```

  [Concretizing](https://spack.readthedocs.io/en/latest/environments.html#spec-concretization)
  the spec resolves all the dependecies and shows you what Spack believes is
  required to satisfy you requirements.

- Build and install the software

  ```bash
  spack -e myenv install
  ```

Note this follows the normal behaviour of Spack, reusing previously built
software, and external software you've told it to use.

## Using an environment

Again, this feels very familiar to people used to Conda; you have to activate
an environment before using it:

```bash
spack env activate myenv
```

Once activated, you can confirm that it's really live:

```bash
$ which bash
/tmp/me/spack/var/spack/environments/myenv/.spack-env/view/bin/bash
$ bash --version
GNU bash, version 5.2.15(1)-release (x86_64-pc-linux-gnu)
Copyright (C) 2022 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>

This is free software; you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
```

### Deactivating an environment

You can leave an environment using:

```bash
spack env deactivate
```

## References

[Using Spack to Replace Homebrew/Conda](https://spack.readthedocs.io/en/latest/replace_conda_homebrew.html)
[Environments](https://spack.readthedocs.io/en/latest/environments.html)
