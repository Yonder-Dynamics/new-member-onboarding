# Docker

Docker is a tool for managing the execution environment of programs. The
intended use case is maintenance of stable `build` and `runtime` environments,
which allows programs to run very reliably on any Linux machine with docker
installed.

The unit of environment is called a `container`. Docker is a background
program (a `daemon`) that builds and starts these containers.

A container is created by running a program with a limited view of the
host computer's file system and operating system. The file system visible
to the "containerized" program can resemble that of any Linux based operating
system, and allows programs written for other Linux distros to run on any
Linux distro.

The advantage of containers over virtual machines (VMs) is in speed and
resources. Containers reuse the host system kernel, allowing their programs
to run at native speed with no virtualization overhead. In addition,
much of the work to build and run a container is creating its filesystem,
which is much faster than starting up a VM. Thus containerized services can
scale larger and faster than services in a VM.

New members should install docker using [this guide](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce).
For those unfamiliar with the Ubuntu environment, this guide add's a special
docker package repository to your computer's package manager, allowing docker
versions to be updated without a middleman (the package repository manager).
Tutorials for other distros can also be found:
* [CentOS](https://docs.docker.com/install/linux/docker-ce/centos/#install-docker-ce)
* [Debian](https://docs.docker.com/install/linux/docker-ce/debian/#install-docker-ce)
* [Fedora](https://docs.docker.com/install/linux/docker-ce/fedora/#install-docker-ce)

Once docker has been installed, go through the
[getting started guide](https://docs.docker.com/get-started/), at least up to
part 3. We could get into swarms and stacks, but that is a more advanced use
case ;).
