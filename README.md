Project to provision an OpenHPC cluster via Vagrant using the
CRI_XCBC (XSEDE basic cluster) Ansible provisioning framework.

The Vagrantfile takes inspiration from the [vagrantcluster](https://github.com/cluening/vagrantcluster)
project but is oriented toward deploying only a master node 
and using standard OHPC tools to provision the cluster, and 
therfore favors the CRI_XCBC approach to ansible scripts just 
for the master.

The Vagrantfile is stripped to the core (rather that carry all
the cruft of a vagrant init).  It leverages work from a 
[pilot project](https://gitlab.rc.uab.edu/ravi89/ohpc_vagrant)
(primaryly the development of an updated centos 7.5 image)
but prefers a clean repo slate.  

## Project Setup

After cloning this project you need to initialize the submodule
from with in the git repo
```
git submodule init
git submodule update
```

Alternatively you can provide the `--recurse-submodules` command 
during the initial clone.

## Cluster Setup

After setting up the project above create your single node OpenHPC
cluster with vagrant:
```
vagrant up
```

The ansible config will bring the master node to the point where its
ready to ingest compute nodes via wwnodescan and prompt to you
start a compute node.  You can create a compute node and start it with
the helper scripts:

Create node c0 (choose whatever name makes sense, c0 matches the config):
```
compute_create c0
```

When prompted start node c0:
```
compute_start c0
```

If you want to stop the node:
```
compute_stop c0
```

If you want to get rid of the compute node VM:
```
compute_destroy c0
```

Note, the compute scripts work directly with the VirtualBox hypervisor.  The
machine created is a basic, lightweight diskless compute node the boots
via iPXE from the OpenHPC master.   You may need to adjust the path to the
ipxe.iso in compute_create to match your local environment.

## Cluster Check

After the `vagrant up` completes you can can log into the cluster with `vagrant ssh`.

To confirm the system is operational run `sinfo` and you should see the following text:
```
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
low*         up 2-00:00:00      1   idle c0
```

You can run a test command on the compute node via slurm using:

```
srun hostname
```

This should return the name `c0`.

With these tests confirmed you have a working OpenHPC cluster running slurm.
