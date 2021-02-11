# Ubuntu Dev Environment Installation and Setup

This project tracks its Python dependencies using Poetry. As such, to work on this project, the
following steps are recommended in order to set up a Python virtual environment.

This document assumes you are using an Ubuntu 20.04+ machine; though, it should be easy
enough to adapt the installation instructions for a different OS.

> **Note:** If you are not running Ubuntu 20.04+, then you will need to adapt the steps
> provided here (and in the Playbook) for your system.

## pyenv

It's recommended to use [pyenv](https://github.com/pyenv/pyenv) to manage Python versions.

However, some build dependencies are required for pyenv to install versions of Python. On
Ubuntu 20.04+ systems, they can be installed as follows:

```bash
$ sudo apt update
$ sudo apt install --no-install-recommends make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
```

Install `git`:

```bash
$ sudo apt install git
```

Now clone the pyenv project using `git`:

```bash
$ git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```

Run the following commands to add `pyenv` to your shell environments `$PATH`:

```bash
$ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
$ echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
```

Then source your `~/.bashrc`, like so:

```bash
$ source ~/.bashrc
```

Install the version of Python tracked in [`.python-version`](../.python-version):

```bash
$ cat .python-version | pyenv install
```

Now switch to that Python version, like so:

```bash
$ eval "$(pyenv init -)"
```

## Poetry

This project has some dependencies that are installed via [Poetry](https://python-poetry.org/).
The Poetry project provides a custom installer for Linux; it can be run as follows:

```bash
$ curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python -
```

To make the `poetry` executable availabe on your current shells `$PATH`, run:

```bash
$ source ~/.poetry/env
```

Poetry uses the [`pyproject.toml`](../pyproject.toml) and [`poetry.lock`](../poetry.lock)
files to track dependencies. Install the project dependencies, like so:

```bash
$ poetry install
```

### Two ways to use the Poetry environment

You can either spawn a new shell with the Python virtual environment instantiated, like so:

```bash
$ poetry shell
$ molecule test
```

Or you can run commands ad-hoc style by prepending them with `poetry run`, like so:

```bash
$ poetry run molecule test
```

## Running the Molecule tests

```bash
$ poetry run molecule test
```

To run different Molecule plays, prepend `molecule` with the `MOLECULE_PLAYBOOK` environment
variable and set its value to the desired YAML playbook, like so:

```bash
$ MOLECULE_PLAYBOOK=binary_install.yml poetry run molecule test
```
