Ansible Role: Golang
====================

An Ansible Role that installs the [Go programming language](https://golang.org/).

Requirements
------------

If this is your first time setting up the development environment for this project, then follow
the instructions in [ubuntu-dev-environment.md](./docs/ubuntu-dev-environment.md), instead. The
following instructions will assume that you've already done this and that you are returning to
work on this project after already having worked on it before.

### pyevn

It's recommended to use pyenv to manage your Python version for this project.

Run the following command to instruct pyenv to switch to the version of Python tracked in the
[`.python-version`](./.python-version) file that lives in the root of this project:

```bash
$ eval "$(pyenv init -)"
```

> **Note:** this can be run from any folder in this project. **Additionally:** this command
> will need to be run every time you open a new terminal to work on this project.

### Poetry

Install Ansible and its dependencies with [Poetry](https://python-poetry.org/):

```bash
$ poetry install
```

Role Variables
--------------

Available variables are listed below, along with default values (see
[`defaults/main.yml`](./defaults/main.yml):

```yaml
# By default, the role installs Go via the system package manager.
golang_packages:
  - golang-go
```

Setting `enable: true` effectively instructs this role to install a version-specific Go binary
instead of using the system package manager.

```yaml
golang_binary_install:
  enable: false
  version: ""
```

Optionally, you may verify the binary install using the Checksum value provided by
the Go maintainers. You can find the SHA256 values at https://golang.org/dl/.

```yaml
golang_checksum:
  amd64:
  arm64:
  armv6l:
```

For example, to verify the download of the `go1.15.8.linux-amd64.tar.gz` Go package on a x86-64
architecture, set it like so:

```yaml
golang_checksum:
  amd64: d3379c32a90fdf9382166f8f48034c459a8cc433730bc9476d39d9082c94583b
```

> **Note:** the system architecture will be obtained dynamically by this role.

If `golang_goroot: true` is set, then the `GOROOT` environment variable will be configured.

```yaml
golang_goroot: false
```

You can change the directory of where the Go binary package is installed to by changing
`golang_goroot_parent_dir`:

```yaml
golang_goroot_parent_dir: /usr/local
```

Set `golang_goroot_versioned: true` to instruct this role to append the Golang version
to the `GOROOT` directory. (e.g. `/usr/local/go1.15.6.linux-amd64`).

```yaml
golang_goroot_versioned: false
```

If this is uncommented and set to a value, then the `GOPATH` environment variable will be configured.

```yaml
# golang_gopath: /home/user/go
```

To force the update of the Go package, set `golang_force_update: true`.

```yaml
golang_force_update: false
```

Dependencies
------------

None.

Example Playbook
----------------

```yaml
- hosts: all
  roles:
    - webdavis.golang
```

License
-------

MIT

Author Information
------------------

This role was created in 2021 by [Stephen A. Davis](https://github.com/webdavis).
