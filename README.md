# kubernetes-coreos-cluster-hands-on
[Kubernetes](https://github.com/GoogleCloudPlatform/kubernetes)
cluster setup with [Vagrant](https://www.vagrantup.com) and
[CoreOS](https://coreos.com).

## Pre-requisites

* [Vagrant](https://www.vagrantup.com)
* [Virtualbox](https://www.virtualbox.org)

## Master and Worker Cluster Setup 

### Download Scripts
Master Cluster has only one node as master-01, and Worker Cluster has two nodes as node-01, node-02
```
$ ls
continuse      master      worker
$ cd master
$ vagrant up
...........

$ cd ../worker
$ vagrant up
.........
```

### Master CLuster login & Service Start
```
$ cd master
$ vagrant ssh master-01

On master-01
$ cd /continuse
```

**KUBERNETES_VERSION** v0.16.0

### Usage

If you want to test something exsamples, start with [Kubernetes examples]
(https://github.com/GoogleCloudPlatform/kubernetes/blob/master/examples/).

