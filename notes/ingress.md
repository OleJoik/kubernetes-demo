# Kubernetes ingress

### Table of contents

- What is ingress?
- When do you need Ingress?
- Ingress YAML Configuration
- Ingress Controller 


## What is ingress

Similar to an external service. Allows external connections through IP:PORT

## When do you need it?
However, in a real world case you probably want to connect a domain name to the service, and in those cases you need <b>Ingress</b>.

In this case you still use a service, but its INTERNAL. Ingress redirects to service, and service redirects to pod.

## YAML Configuration
See comments in code file.
Important points:
- define the kind of component (Ingress)
- Map a domain to an internal service
- Paths defines any part after the domain:
http://myapp.com/PATHS-STUFF
- Point to the name and port of an internal service
- Internal service has no nodePort (like an external would have)

### The hostname (YAML "host")
- Must be a valid domain adress
- Map domain name to the Node's IP adress, which is the entrypoint
- Might also map the hostname to a server outside of the kubernetes cluster (API GATEWAY?)

## Ingress controller
- also called "ingress implementation"/"Ingress Controller Pod"
- Alternatives: 
    - K8s Nginx Ingress Controller 
    - Third party implementations
- Entrypoint to the cluster
- The ingress controller
- Evaluates and processes ingress rules
- "Which ingress applies to this specific request?"
- Manages redirections (to correct ingress)

### Cloud Load Balancer
- Depending on the environment in which your cluster runs (gcp, aws, azure, ...), this may already be implemented for you.
- Then you don't have to implement a load balancer
- Alternative: Bare Metal: Have a seperate server to provide an entry point to the cluster. Can be an external proxy server. 
- Public IP adress and open ports.
- Entrypoint to cluster
- No server in K8s cluster is accessible from outside!


### Minicube setup
This command starts the K8s Nginx implementation of ingress controller...
```
minikube addons enable ingress
```


To see the pod running in minikube, do...
```
kubectl get pod -n kube-system
```

