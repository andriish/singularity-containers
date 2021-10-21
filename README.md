
# singularity-containers

These singularity build recipies are used to build the singularity
containers stored in /remote/ceph/common/remote/ceph/common/vm/singularity/images.

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

'singularity build --remote <name>.sif <name>.def'

For the remote build one needs a (free) account at 
[sylabs](https://cloud.sylabs.io/builder). Follow the instructions there 
to setup remote access.

### ... with fakroot

The containers are built with

'singularity build --fakeroot <name>.sif <name>.def'

Please ask our IT group to set you up for singularity fakeroot usage. 
This is very convenient, because it is fast and doesn't depend on 
external services (except the docker and distro repos).

Not all builds work well with fakeroot due to limitations of this 
approach hitting corner cases with some packages. Builds from docker 
containers work fine, since with these problematic packages are already 
installed. Builds from source are more problematic, since basic system 
packages are installed which can trigger errors.

### ... on a private machine with root access

The containers are built with

'sudo singularity build <name>.sif <name>.def'

This can be a self-administered laptop or a virtual machine (under 
VirtualBox) under your control with a singularity installation. Needless 
to say this method has less support from our IT group or your local IT 
experts since there are many unknowns with this method.

## Container locations

Please look from your MPP PC under

'/remote/ceph/common/vm/singularity/images/'

to find the singularity images. 

## Starting a container session

Start them with 

'singularity shell /remote/ceph/common/vm/singularity/images/centos8stream-yum.sif

In order to connect a /cvmfs service on the host do

'singularity shell --bind /cvmfs /remote/ceph/common/vm/singularity/images/centos8stream-yum.sif

This requires that the container has the /cvmfs mountpoint created 
during the build.


