# Chapter 2: Kubernetes (k8s)
## History
### Definition
> Kubernetes is an open-source system for automating deployment, scaling, and management of containerized applications.

### Etmology
Comes from Greek word for *helmsman* or *ship pilot* and can thus be considered a manager for shipping containers

### Origins
Inspired by Google's *Borg*, and as such is written in *Go*. From *Borg* it draws the following core features:

- API Servers
- Pods
- IP-per-Pod
- Services
- Labels

### Features
- **Automatic binpacking**

  *Kubernetes automatically schedules the containers based on resource usage and constraints, without sacrificing the availability.*

- **Self-healing**

  *Kubernetes automatically replaces and reschedules the containers from failed nodes. It also kills and restarts the containers which do not respond to health checks, based on existing rules/policy.*

- **Horizontal scaling**

  *Kubernetes can automatically scale applications based on resource usage like CPU and memory. In some cases, it also supports dynamic scaling based on customer metrics.*

- **Service discovery and Load balancing**

  *Kubernetes groups sets of containers and refers to them via a DNS name. This DNS name is also called a Kubernetes service. Kubernetes can discover these services automatically, and load-balance requests between containers of a given service.*
  
- **Automated rollouts and rollbacks**

  *Kubernetes can roll out and roll back new versions/configurations of an application, without introducing any downtime.*

- **Secrets and configuration management**

  *Kubernetes can manage secrets and configuration details for an application without re-building the respective images. With secrets, we can share confidential information to our application without exposing it to the stack configuration, like on GitHub.*

- **Storage orchestration**

  *With Kubernetes and its plugins, we can automatically mount local, external, and storage solutions to the containers in a seamless manner, based on Software Defined Storage (SDS).*

- **Batch execution**

  *Besides long running jobs, Kubernetes also supports batch execution.*
  
### License
Apache License Version 2.0