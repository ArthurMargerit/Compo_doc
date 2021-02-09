# Build Guix Docker

GNU Guix is a cross-platform package manager and a tool to instantiate and manage Unix-like operating systems, based on the Nix package manager with Guile Scheme APIs and specializes in providing exclusively free software. [Website](https://guix.gnu.org/)

## Install Guix
Install the guix system, guix Qenu image or guix binary package.
- [binary_install](https://guix.gnu.org/manual/en/html_node/Binary-Installation.html)
- [system_install](https://guix.gnu.org/manual/en/html_node/System-Installation.html)
- [vm_install](https://guix.gnu.org/manual/en/html_node/Running-Guix-in-a-VM.html)

## Install non-gnu Package

Guix doesn't provide by default all the package needed by Compo. You need to download this (git repository)[].

```bash
git clone GIT_REPO
```

## Build docker image

Guix provide many tool to build docker image, chroot image, vm , lxc container, systems etc...

Docker:
-------
```bash
guix pack -f docker -m (TODO)
```

On the last line you will find the Docker image.
(refer to the docker install)
