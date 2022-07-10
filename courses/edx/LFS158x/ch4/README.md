# Chapter 4: Installing Kubernetes
Kubernetes is often installed in one of four main confiugrations:

* **All-in-One Single-Node**

  *In this configuration the Master and Worker components are installed side by side, which is great for learning, but is rather brittle and should not be used in production. `minikube` is a great example of this type of installation*

* **Single-Node etcd, Single-Master, Multi-Worker**

  *In this configuration there is a single Master, which also runs a single node `etcd` instace, and multiple Workers with connect to the Master remotely.*
  
* **Single-Node etcd, Multi-Master, Multi-Worker**

  *In this configuration there are multiple Masters operating in a high availability mode with a shared single-node `etcd` instance and multiple Workers connected to the Masters remotely.*

* **Multi-Node etcd, Multi-Master, Multi-Worker** (recommended for **production**)
  
  *In this configuration there are multiple Masters operating in a high availability mode with multiple Workers connected to the Masters remotely. The main difference in this configuation is that `etcd` is configured in clustered mode outside of the Kubernetes cluster and is connected to remotely* 