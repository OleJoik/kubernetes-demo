# Kubernetes services

### Table of contents
- What is a Kubernetes Service and when do we need it?
- What are the different types of services?
    - ClusterIP
    - Headless
    - NodePort
    - LoadBalancer
- What are the differences, and when should they be used?

## What is a service (`svc`), and why do we need it?
- Each pod has its own IP adress
- Pods ar ephemeral, and are frequently destroyed
- When a new pod restarts, it gets a new IP adress
- A `service` provides a stable IP adress that stays even when a pod dies
- The service is also a load balancer. If there are several pods, it will forward a request to one of them.
- It provides loose coupling within and outside the cluster

## ClusterIP
- Only accessible within the cluster (not external)
- [default type of a service](service-clusterIP.yaml)
- [Multiple ports in a service](service-multiple-ports.yaml) must be <i>named</i>


## Headless services
- [example](headless-service-example.yaml)
- if a client/pod wants to communicate with 1 specific pod directly
- When deploying stateful applications
- If ClusterIP in a service is set to None, a <i>DNS lookup</i> returns the Pod IP address instead
- Stateful applications/pods have both ClusterIP and headless services.

## NodePort
- [Example](nodeport-service-example.yaml) 
- Allows external traffic on a static port (on <b>worker nodes</b>)
- External requests are directly accessible on worker node, on the `nodePort` that the service specification defines
- the nodePort must be in the range 30000-32767
- The IP adress is from the Worker Node
- This type of service is not very efficient, and not very secure. NOT for production, maybe for quick and dirty testing.
- Creates ClusterIP automatically

## LoadBalancer
- [Example](loadbalancer-service-example.yaml)
- A better alternative then the nodePort service
- The service becomes accessible externally through the cloud providers' LoadBalancer (different implementation for different providers)
- Creates ClusterIP and NodePort automatically

To get the IP adress of pods:
```
kubectl get pod -o wide
```

To list all services:
```
kubectl get svc
```