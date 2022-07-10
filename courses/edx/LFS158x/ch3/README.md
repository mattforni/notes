# Chapter 3: Kubernetes Architecture
## Master Node(s)
The **Master Node(s)** manage the Kubernetes cluster by acting as the gateway for all adminstrative tasks and can be accessed via CLI, GUI or API.

### Components
* **API Server**
  
  *All administrative tasks are submitted via the API Server via REST commands. The API Server validates and processes these requests, updating the state of the cluster in the distributed key-value store upon completion.*
  
* **Secheduler**

  *Work is scheduled to different Pods and Services based on resource usage and  user/operator prescribed constraints, data locality, affinity, etc.*
  
* **Controller Manager**

  *Several non-terminating control loops monitor and regulate the current state of the cluster via the API Server. Should any loop find that the desired state of the objects it manages do not meet the desired state, it takes corrective steps and then resumes normal operations.*
  
* **etcd**

  *Each cluster contains either a local or distributed key-value store that stores the state of the cluster.*
 
## Worker Node(s)
The **Worker Node(s)** are machines (VM, physical server, etc.) which run applications using Pods. A Pod is the core scheduling unit in Kubernetes which consists of a logical collection of one or more containers which are **always scheduled together**.

### Components
* **Container Runtime (CR)**

  *A Worker Node requires a Container Runtime (e.g. Docker, rkt, etc.) to schedule pods.*
  
* **kubelet**

  *The kubelet communicates between the Master Node and the Worker Node by:*

  1. Receiving Pod definitions and running the associated containers associated via the CR.
  2. Ensuring the containers with make up the Pod are healthy at all times.

* **kube-proxy**

  *The network proxy which runs on each node and listens to the API Server so it may create and delete Service endpoints as Pods come into and out of service.*

## Key-Value Store (etcd)
**etcd** is a distributed key-value store, written in Go, which uses the Raft Consensus algorithm to relibably stores the state (e.g. subnets, ConfigMaps, Secrets, etc.) of the Kubernetes cluster at any given time.

## Networking
### Unique IPs
Each Pod requires a unique IP address, which the CR offloads to Container Network Interface (CNI).

### Container-to-Container Communication
The CR creates a unique network identifier for each container that it starts which is referred to as a Network Namespace. These Network Namespaces can be shared across multiple containers, or with the Host Operating System. Within a Pod, Network Namespaces are shared so that containers may communicate via localhost.