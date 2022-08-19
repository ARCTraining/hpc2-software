# Environments

WORK IN PROGRESS

Environments in Spack feel a whole lot like environments in Conda; they allow
you to bundle up a number of different pieces of software into a single blob,
making a single coherent collection.

## Creating an environment

- Create an environment
  ```bash
  spack env create myenv
  ```

- Add a list of software to it
  ```bash
  spack -e myenv add bash@5 python py-numpy py-scipy py-matplotlib
  ```

- Tweak the config if required
  ```bash
  spack -e myenv config edit

- Validate the installation
  ```bash
  spack -e myenv concretize
  ```

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
blah blah
$ bash --version
blah blah
```

## References

[Using Spack to Replace Homebrew/Conda](https://spack.readthedocs.io/en/latest/replace_conda_homebrew.html)
