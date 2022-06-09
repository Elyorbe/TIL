## Pods
Lowest unit of deployment within Kubernetes, and is essentially a group of containers.
A pod allows a microservice application container to be grouped with other "sidecar" containers 
that may provide system services like logging, monitoring, or communication management.
Containers within a pod share a filesystem and network namespace. A single container can be deployed,
but it is always deployed within a pod.

## Services
Kubernetes service provide load balancing, naming, and discovery to isolate one microservice from another.
Services are backed by `Deployments`, which, in turn, are responsible for details associated with maintaining
the desired number of instances of a pod to be running with the system. Services, deployments, and pods are
connected together in Kubernetes through the use of `labels`, both for naming and selecting.
