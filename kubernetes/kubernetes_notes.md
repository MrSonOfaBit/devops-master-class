# Course Kubernetes Notes


This Readme takes note from the in28Minutes [DevOps for Beginners](https://www.udemy.com/course/devops-with-docker-kubernetes-and-azure-devops) course from udemy. The topic is **Section 3** `DevOps with Docker and Kubernetes on Google Kubernetes Engine`

The project is forked from the github repo [devops-master-class](https://github.com/in28minutes/devops-master-class)

## Introduction

Kubernetes (short K8s) is an open source system for container orchestration. It helps you to automate all the steps you have to made in docker engine by yourself like deploying, scaling and managing containerized applications.

> ⓘ For the leaning course I use microK8s on a ubuntu wls. All examples and approaches are based on the microK8s system. See more under [MicroK8s Documentation](https://microk8s.io/docs). 
> I also used multipass to add multiple nodes to the cluster as small debian virtual machines. [Multipass Docs](https://documentation.ubuntu.com/multipass/latest/?_gl=1*1ybsgaq*_gcl_au*MTAyMTg3Mjk5MC4xNzYyNzczODc4)

---

## Core Functions and benefits
Here are some kubernetes advantages explained in a short term.

1. **Automate Managing**: K8s helps you managing, scaling, deploying your applications. It is useful if you have a large environment of servers, multiple containers and services that you need zo manage and optimize
2. **Orchestrate containers**: You can group containers into logical groups called pods and manage them from cluster of virtual or physical machines
3. **Ensure application state**: Kubernetes will work on maintain your declared state for example having always a number of replicas running from special container
4. **Self-Managing**: If containers fail or nodes die, it can restart and reschedule containers

---

## Kubernetes Cluster
![Kubernetes Cluster Architecture](https://kubernetes.io/images/docs/kubernetes-cluster-architecture.svg)
A Kubernetes Cluster is group of `Worker Nodes` and a `Control Plane` (Master Node). The Control Plane manages the worker nodes while the worker nodes host `pods` in order to run one or more container for each pod.

### Control Plane
The Control Plan manage the state of the cluster. It is responsible for the health of the nodes, for scaling or updating you cluster. There are 5 main components.

#### **kube-apiserver**
This component exposes the `Kubernetes HTTP API`. It validates and configures data for api objects like pods, services, replication controllers and more. 

#### **etcd**
The shortcut stands for `distributed etc directory`. It is the database for the clusters state saving node, pods, secrets etc.

#### **kube-scheduler**
new created pods get a node to run on from this controller.

#### **kube-controller-manager**
It is a collection of controllers watching different parts of the cluster.The state from the etcd is compered with the actual state of the cluster through the api server.
-  **Node Controller:** Respond to node states and reports if they go down
-  **Job Controller:** Create Pods for every job that executes a task
-  **EndpointSlice Controller:** Updates endpoint and manage Ip routes for services
-  **ServiceAccount Controller:** manage ServiceAccounts for pods and sets api token for them

#### **cloud-controller-manager**
Manages and connects your cluster to the clouds provider api. If you run K8s on a local machine the cluster does not have a cloud controller manager.

### Node 
A node can be a virtual or a physical machine. It is managed by the Control Plane and contain services needed to run pods. Normally you have multiple Nodes inside a Cluster but it depends on your environment and amount of workload. 

---

## Workloads
Workloads are applications that run on Kubernetes. These Workloads are run in one or a set of pods.

### Pods
![Kubernetes Pod](https://matthewpalmer.net/kubernetes-app-developer/articles/networking-overview.png)

A Pod is a collection of containers that run on a host. A node can contain multiple pods and a pod contain multiple containers. It is the smallest deployable unit in Kubernetes.

### Deployments
Deployments manage a set of Pods. Your defined state will be applied through the deployment. You can define which image should be used and the amount of pods by define the replica number. 

### ReplicaSet 
The ReplicaSet is managed automatically by the deployment. It maintain a stable amount of replica pods. If one pod gets deleted or is removed, a new pod will be created immediately.  
