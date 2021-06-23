
## Kubernetes
## K3s Summary
Provisioning a Kubernetes cluster is known as the bootstrapping process. When creating a cluster, it is essential to ensure that each node had the necessary components installed. It is possible to manually provision a cluster, however, this implied the distribution and execution of each component independently (e.g. kube-apiserver, kube-scheduler, kubelet, etc.). This is a highly tedious task that has a higher risk of misconfiguration.  

As a result, multiple tools emerged to handle the bootstrapping of a cluster automatically. For example:  

Production-grade clusters  
* [kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)
* [Kubespray](https://github.com/kubernetes-sigs/kubespray)
* [Kops](https://github.com/kubernetes/kops)
* [K3s](https://k3s.io/)
Developmnet-grade clusters  
* [kind](https://kind.sigs.k8s.io/docs/user/quick-start/)
* [minikube](https://minikube.sigs.k8s.io/docs/start/)
* [k3d](https://classroom.udacity.com/nanodegrees/nd064-1/parts/30cb07da-8fd4-4438-a209-b3457adb5d82/modules/7b21dfa4-aac8-4d24-82c5-65325e6dc691/lessons/d9fa86b3-301d-4966-86f8-a2f34a5a7ca3/concepts/link%20to%20https://k3d.io/)

A good introduction to k3d, written by k3d's creator Thorsten Klein, can be found here  

A comprehensive overview of currently existing lightweight kubernetes distros can be found here  

## Create Vagrant Box And Install Kubernetes with k3s  
This demo is a step-by-step guide on how to create a vagrant box and install a Kubernetes cluster using k3s. To follow this demo, reference the Vagrantfile from the course repository.  

A nice introduction to Vagrant can be found in this article  

Before following the demo:  

Make sure to have VirtualBox 6.1.16 or higher installed.  
You also need to install vagrant on your machine.  

To run a Vagrant File:
```
# Inspect available vagrant boxes 
vagrant status 

# create a vagrant box using the Vagrantfile in the current directory
vagrant up

# SSH into the vagrant box
# Note: this command uses the .vagrant folder to identify the details of the vagrant box
vagrant ssh
```
In VM run this commands below:
```
# To install k3s
curl -sfL https://get.k3s.io | sh

# To see nodes in Kubernetes is necessary is sudo user
sudo su 
kubectl get nodes
```
## Kubeconfig Summary
To access a Kubernetes cluster a kubeconfig file is required. A kubeconfig file has all the necessary cluster metadata and authentication details, that grants the user permission to query the cluster objects. Usually, the kubeconfig file is stored locally under the ~/.kube/config file. However, k3s places the kubeconfig file within /etc/rancher/k3s/k3s.yaml path. Additionally, the location of a kubeconfig file can be set through the --kubeconfig kubectl flag or via the KUBECONFIG environmental variable.  

**A Kubeconfig file has 3 main distinct sections:**  

**Cluster** - encapsulates the metadata for a cluster, such as the name of the cluster, API server endpoint, and certificate authority used to check the identity of the user.  
**User** - contains the user details that want access to the cluster, including the user name, and any authentication metadata, such as username, password, token or client, and key certificates.  
**Context** - links a user to a cluster. If the user credentials are valid and the cluster is up, access to resources is granted. Also, a current-context can be specified, which instructs which context (cluster and user) should be used to query the cluster.
Here is an example of a kubeconfig file:  
```
apiVersion: v1
# define the cluster metadata 
clusters:
- cluster:
    certificate-authority-data: {{ CA }}
    server: https://127.0.0.1:63668
  name: udacity-cluster
# define the user details 
users:
# `udacity-user` user authenticates using client and key certificates 
- name: udacity-user
  user:
    client-certificate-data: {{ CERT }}
    client-key-data: {{ KEY }}
# `green-user` user authenticates using a token
- name: green-user
  user:
    token: {{ TOKEN }}
# define the contexts 
contexts:
- context:
    cluster: udacity-cluster
    user: udacity-user
  name: udacity-context
# set the current context
current-context: udacity-context
```

Once you start handling multiple clusters, you'll find a lot of useful information in this [article](https://community.suse.com/posts/scheduled/cluster-this-is-your-admin-do-you-read)  

## Kubeconfig Walkthrough
In this demo, the instructor uses a cluster bootstrapped with [kind](https://kind.sigs.k8s.io/docs/user/quick-start/). Throughout this course, the students will use k3s to provision a cluser. However, in this demo kind is used to highlight how different tools provision the kubeconfig files.  

If the students chose to follows this demo, these are the instructions to create a cluster using kind:  

Note: kind can be installed directly on your local machine  

Ensure Docker is installed and running. Use the `docker --version` command to verify if Docker is installed.
Install kind by using the [official installation documentation](https://kind.sigs.k8s.io/docs/user/quick-start/#installation)
Create a kind cluster using the `kind create cluster --name demo` command  
Throughout the demo, the following kubectl commands are used:  
```
# Inspect  the endpoints for the cluster and installed add-ons 
kubectl cluster-info

# List all the nodes in the cluster. 
# To get a more detailed view of the nodes, the `-o wide` flag can be passed
kubectl get nodes [-o wide] 

# Describe a cluster node.
# Typical configuration: node IP, capacity (CPU and memory), a list of running pods on the node, podCIDR, etc.
kubectl describe node {{ NODE NAME }}
```
## New terms
* Kubeconfig - a metadata file that grants a user access to a Kubernetes cluster
## Further reading
* [Organizing Cluster Access Using kubeconfig Files](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/)