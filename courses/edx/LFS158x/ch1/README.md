# Chapter 1: Container Orchestration
## Container Images
Bundle the application along with it's runtime and dependencies

## Container Orchestrators
### Definition
The tools which group hosts together to form a cluster.

### Reasoning
Can be managed by a set of scripts, but orchestrators give you:

- Bring multiple hosts together and make them part of a cluster
- Schedule containers to run on different hosts
- Help containers running on one host reach out to containers running on other hosts in the cluster
- Bind containers and storage
- Bind containers of similar type to a higher-level construct, like services, so we don't have to deal with individual containers
- Keep resource usage in-check, and optimize it when necessary
- Allow secure access to applications running inside containers.

### Implementations
- Docker Swarm
- Kubernetes
- Mesos Marathon
- Amazon ECS
- Hashicorp Nomad
