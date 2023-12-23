# High Availability Cluster Lab

In this lab is shown how to setup a High Availability (HA) Cluster using Kebernetes using HAProxy to distribute
the load between the master nodes.

# Design

## High Availability

In this example is used the "Stacked etcd Cluster" where etcd is stacked to master node.

## Topology

## HAProxy Load Balancing

## Services

## Requirements

# Configuration

## Master Node

In order to configure first master node, execute the following command:

```shell
sudo kubeadm init --control-plane-endpoint "10.200.1.2:6443" --upload-certs --apiserver-advertise-address=10.200.1.2
```

The output will provide the commands required to be added to other nodes.

```shell
#Your Kubernetes control-plane has initialized successfully!

#To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

#Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

#You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

#You can now join any number of the control-plane node running the following command on each as root:

  kubeadm join 10.200.1.2:6443 --token hvlc8v.opw36dwb0wesnort \
        --discovery-token-ca-cert-hash sha256:8c0c7d69826e89d8bc72c11ee1b6e4c9d49ade1c68bf4696b4b9f9813f8d74a5 \
        --control-plane --certificate-key 17d0e751e834eaa30625974bacc9fed2b6404348d9fe6f3b68ac9e59868de0c3

# Please note that the certificate-key gives access to cluster sensitive data, keep it secret!
# As a safeguard, uploaded-certs will be deleted in two hours; If necessary, you can use

"kubeadm init phase upload-certs --upload-certs" 

# to reload certs afterward.

#Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 10.200.1.2:6443 --token hvlc8v.opw36dwb0wesnort \
        --discovery-token-ca-cert-hash sha256:8c0c7d69826e89d8bc72c11ee1b6e4c9d49ade1c68bf4696b4b9f9813f8d74a5
```

### Container Network Interface
It's required to use a Container Network Interface, in this case use Calico. If CNI is not installed, node will not start.

```shell
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.24.5/manifests/calico.yaml
```

```shell
vagrant@master-node1:~$ kubectl get nodes
NAME           STATUS     ROLES           AGE    VERSION
master-node1   NotReady   control-plane   7m1s   v1.26.11
```


