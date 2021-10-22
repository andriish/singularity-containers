
# singularity-containers

These singularity build recipies are used to build the singularity 
containers stored in 
/remote/ceph/common/remote/ceph/common/vm/singularity/images.

## Different container builds

With postfix -yum or -debootstrap the container is built from the 
sources of the distro, w/o such a postfix the build starts from the 
distro docker containers. Just have a look inside the .def file to see 
the difference.

## Building the containers

Container building generally needs system previledges, because some 
pieces in the image need to have root permissions in order for the 
container to work fine. With singularity there are three ways to deal 
with this.

### ... remotely

The containers are built with

    $> singularity build --remote <name>.sif <name>.def

For the remote build one needs a (free) account at 
[sylabs](https://cloud.sylabs.io/builder). Follow the instructions there 
to setup remote access.

### ... with fakroot

The containers are built with

    $> singularity build --fakeroot <name>.sif <name>.def

Please ask our IT group to set you up for singularity fakeroot usage. 
This is very convenient, because it is fast and doesn't depend on 
external services (except of course the docker and/or distro repos).

Not all builds work well with fakeroot due to limitations of this 
approach hitting corner cases with some packages. Builds from docker 
containers work fine, since with these problematic packages are already 
installed. Builds from source are more problematic, since basic system 
packages are installed which can trigger errors.

### ... on a private machine with root access

The containers are built with

    $> sudo singularity build <name>.sif <name>.def

This can be a self-administered laptop or a virtual machine (under 
VirtualBox) under your control with a singularity installation. Needless 
to say this method has less support from our IT group or your local IT 
experts since there are many unknowns with this method.

## Container locations

Please look from your MPP PC under

    /remote/ceph/common/vm/singularity/images/

to find the singularity images.

## Starting a container session

Start them with

    $> singularity shell 
    /remote/ceph/common/vm/singularity/images/centos8stream-yum.sif

In order to connect a /cvmfs service on the host do

    $> singularity shell --bind /cvmfs 
    /remote/ceph/common/vm/singularity/images/centos8stream-yum.sif

This requires that the container has the /cvmfs mountpoint created 
during the build.

## Considerations for building your own container

The container allows to keep your development environment as stable as 
you want it, since you can keep it as long as you want. If the container 
needs to be rebuilt, because e.g. some extra package should be included, 
the environment in the container could change slightly too. This can 
happen if the underlying docker image was updated, or, if building from 
distro sources, updates (usually security patches) for some packages 
were included. If the container is based a fixed release of a distro 
(e.g. ubuntu 20.04, or centos 8 stream), such changes should be 
invisible to users. But you have been warned.

For the question about what to include in the container, and what to 
build from sources inside the running container, consider which packages 
are required, but will not need changes in the course of your work. 
These packages should be included in the container image. Software, 
which you expect to modify, should be built from sources inside the 
container against the installed packages.

## HEPrpms COPR repository

We maintain a repository, managed by Andrii Verbytskyi, including more 
than 60 packages with software for HEP and related fields, see Andriis 
COPR [repo](https://copr.fedorainfracloud.org/coprs/averbyts/HEPrpms) 
for details. COPR is the package build service of the RedHat fedora 
project. Builds are available for centos8 (stream), and recent versions 
of fedora. For centos7 (or CERN centos7 "cc7") this COPR 
[repo](https://copr.fedorainfracloud.org/coprs/averbyts/fastjet/) is 
used, because some modern HEP projects don't work on centos7.
See e.g. in "root6-cernlib-centos8stream.def" how this repo
is used.

# Contributing

Any contributions, bug fixes, enhancements are very welcome. You are 
welcome to store your (or your groups) container definitions here. This 
could help others solving their problems. 

In order to do so, please fork this repo on github, clone the forked 
repo, and define a branch, e.g. with your name if can't think of anything 
better. Then add+commit your contributions followed by a push to
github. At last, create a pull request on github. This allows us
to understand and merge your changes in a clean way. 

# Any questions or problems?

Please contact your groups IT expert or our IT group.

