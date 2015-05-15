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
```

Modify in master/Vagrantfile and worker/Vagrantfile for NFS directory (ex: Local directory path is ~/kubernetes-cluster/continuse)
```
config.vm.synced_folder "~/kubernetes-cluster/continuse", "/continuse", id: "root", :nfs => true, :mount_options =>  ["nolock,vers=3,udp"], :map_uid => 0, :map_gid => 0
```

### Master / Worker Cluster Building
```
$ cd master
$ vagrant up
............
...........
$ cd worker
$ vagrant up
```

### Master Cluster login & Kubernetes Binary file copy
```
$ cd master
$ vagrant ssh master-01

On master-01
$ cd /continuse/bin
$ wget https://github.com/ContinUSE/kubernetes-coreos-cluster/releases/download/v0.16.0/kube.v0.16.0.tgz
$ tar xvfz kube.v0.16.0.tgz
```

### Kubernetes Service Start
```
$ cd /continuse/service
$ fleetctl start kube-apiserver.service
$ fleetctl start kube-controller-manager.service
$ fleetctl start kube-register.service
$ fleetctl start kube-scheduler.service
$ fleetctl start kube-kubelet.service
$ fleetctl start kube-proxy.service
```

### Make a Label
```
$ kubectl label nodes 172.17.8.111 node=test1
$ kubectl label nodes 172.17.8.112 node=test2
```

### Kubernetes Service for GUI Interface
```
$ fleetctl start fleet-ui.service
$ fleetctl start kubernetes-ui.service
```

#### cAdvisor -- Web-based monitoring
```
http://172.17.8.111:4194/containers/
```

### Kubernetes UI -- Web-based
```
http://172.17.8.101:8001/#/dashboard/
```

**KUBERNETES_VERSION** v0.16.0

### Usage

If you want to test something exsamples, start with [Kubernetes examples]
(https://github.com/GoogleCloudPlatform/kubernetes/blob/master/examples/).

