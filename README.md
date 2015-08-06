# kubernetes-coreos-cluster-hands-on
[Kubernetes](https://github.com/GoogleCloudPlatform/kubernetes)
cluster setup with [Vagrant](https://www.vagrantup.com) and
[CoreOS](https://coreos.com).

## Pre-requisites

* [Vagrant](https://www.vagrantup.com)
* [Virtualbox](https://www.virtualbox.org)
* For more information about virtualbox setup, see the test environments section in https://github.com/ContinUSE/openstack-on-coreos

## Controller and Kube Cluster Setup 

### Download Scripts
Controller Cluster has 3 nodes as controller-01/controller-02/controller-03, and Kube Cluster has 3 nodes as kube-01-01, kube-02, kube-03.
```
$ ls
README.md*	continuse/	controller/	examples/	kube/
```

Modify in controller/Vagrantfile and kube/Vagrantfile for NFS directory (ex: Local directory path is ~/kubernetes-cluster/continuse)
```
config.vm.synced_folder "~/kubernetes-cluster/continuse", "/continuse", id: "root", :nfs => true, :mount_options =>  ["nolock,vers=3,udp"], :map_uid => 0, :map_gid => 0
```

### Controller / Kube Cluster Building
```
$ cd controller
$ vagrant up
............
...........
$ cd kube
$ vagrant up
```

### Controller Cluster login & Kubernetes Binary file copy
```
$ cd controller
$ vagrant ssh controller-01

On controller-01
$ cd /continuse/kube/bin
$ wget https://github.com/ContinUSE/kubernetes-coreos-cluster/releases/download/v1.0.1/kube_1.0.1.tar
$ tar xvfz kube_1.0.1.tar
```

### Make Service Account Key file
```
On controller-01
$ cd /continuse/kube/bin
$ openssl genrsa -out kube-serviceaccount.key 2048
```

### Kubernetes Service Start
```
$ cd /continuse/kube/service
$ fleetctl start kube-apiserver.service
$ fleetctl start kube-controller-manager.service
$ fleetctl start kube-scheduler.service
$ fleetctl start kube-kubelet.service
$ fleetctl start kube-proxy.service
```

### Kubernetes Service for GUI Interface
```
$ kubectl create -f kube-ui-rc.yaml --namespace=kube-system
$ kubectl create -f kube-ui-svc.yaml --namespace=kube-system
```

### Kubernetes UI -- Web-based
```
http://192.168.10.12:8080/ui/           IP address of Kubernetes API Server Running
```

#### cAdvisor -- Web-based monitoring
```
http://192.168.10.71:4194/containers/    Each Kube Cluster node
```

**KUBERNETES_VERSION** v1..0.1

### Usage

If you want to test something examples, start with [Kubernetes examples]
(https://github.com/GoogleCloudPlatform/kubernetes/blob/master/examples/).
And I am trying yo make examples. See with [My Examples] (https://github.com/ContinUSE/kubernetes-coreos-cluster/tree/master/examples).

