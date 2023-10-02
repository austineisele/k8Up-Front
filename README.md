# k8Up-Front
Scripts for setting up a Kubernetes cluster using configuration files.


To use you must have kubeadm installed on an admin station

###To Use###:

##Part 1: kubeadm configuration##
We need to create the configuration values and certificates in order to pass to our control plane and workder nodes. To do this, we will be working on an admin station. Part 1 is *all* done on an admin station. ***Note***: If you plan on committing this to a repository, use something like transcrypt to encrypt any sensitive information. 

1. *Creating varaibles.* The first section creates variables that will be used to configure the init.yaml and the join.yaml files. Run:
```
cd scripts
sudo chmod ./*
./init_variables
```

2. *Generating tokens and applying the paramters.* Next we use kubeadm's token generation and apply this to the init.yaml template. Run:
```
./create_token
```
3. *Generating Init Certificates.* Normally the kubeadm init command creates certificates. In order to use up front initialization, we need to generate certificates and store then in our admin's cert directory. This also creates the CA certificate Hash (see [docs](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-join/#token-based-discovery-with-ca-pinning)). Run:
```
./create_certs
```
4. *Generate the kubeconfig.* The kubeconfig file is next. This will set up front your .kube/config file for cluster contexts, along with certificates. Run:
```
./create_kube_config
```

###**Part 2: Node set ups**
1. *Set up nodes*: Install the initial tools and configurations needed to install Kubernetes on all the nodes that you will be using for your cluster. This includes: 
- CRI-O as the container runtime.
- Kuberenetes tools: kubelet, kubeadm, and kubectl
Run:
```
./all_nodes
```
2. *Set up controlplane*: Pull the configuration images, and initialize the cluster with kubeadm. Then initialize a CNI (this program uses Calico). Run:
```
./controlplane
```i 