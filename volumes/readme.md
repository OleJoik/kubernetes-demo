# Kubernetes volumes
Data persistance in kubernetes

Think of it as an external plugin to your cluster

### 3 main types
- Persistent volume
- Pesistent volume claim
- Storage class

## Storage requirements
1) Storage doesn't depend on pod lifecycle

2) Storage must be available on all nodes

3) storage need to survive even if the cluster crashes


## Persistent volumes
- cluster Resources - not in namespaces, accessible to the whole cluster.
- created via yaml files
    
```
- kind
- spec
    - storage amount
    - ...
```

- Needs actual physical storage, which can be on the local disk of the kubernetes cluster, cload-storage or an external nfs server.

- Persistent volumes are interfaces to the actual storage, that may come from many places. YAML files define the which resource the volume uses for storage.

### some example yaml files
- [Google cloud storage](persistant-volume-examples/persistent-volume-gcp-example.yaml)
- [Local storage](persistant-volume-examples/persistent-volume-local-storage.yaml)
- [External NFS server](persistant-volume-examples/persistent-volume-nfs-example.yaml)

Kubernetes docs gives details about [all types of persistent volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#types-of-persistent-volumes)

### Local vs Remove volume types
- Each has its own use case!
- Local volumes violate requirements 2 and 3 (above), as they are...
    - Tied to 1 specific node
    - Won't survive a cluster crash
- For DB persistence, use remote storage!

### Who and when to create Persistant Volumes (PV) 
- PV are resources that need to be there before that pods that depend on it.

### User types
#### Administrator
- sets up and maintains the cluster. 
- Makes sure the system has enough resources.
- System administrator / devops engineer.

- configures the storage
- creates consistent volumes

#### K8s User
- Deploys applications
- Uses the storage in the applications
- Applications then have to `claim` the persistant volume

## Persistent Volume claim (PVC component)
A [claim](persistant-volume-claims/pv-claim-example.yaml) to a persistent volume, used by the applications/pods

The claim will then be used in the pod [specification](persistant-volume-claims/pv-claim-pod-example.yaml)

Claims are namespaced, and must be in the same namespace as the pod using the claim.

The <i>pod</i> requests the volume through the PV <i>claim</i>. The claim tries to find a volume in the cluster (any volume that has a correct "type", not any specific one). The <i>volume</i> has the actual storage backend

## Mounting ConfigMap/Secret as volumes
1) Create a ConfigMap and/or Secret component
2) Mount it into your pod/container such as in [this example](volume-configmap-example.yaml)


## Storage class (SC)
[SC components](storage-class/storage-class.yaml) provisions persistent volumes dynamically (...on K8s user request - when [PVC claims it](storage-class/persistent-volume-claim.yaml)!)

StorageBackend is defined in the SC component, in the `provisioner` attribute. It tells kubernetes which provisioner should be used to create the persistent volume. Each storage backend has its own provisioner, internal (such as `kubernetes.io`) or external. In addition you have to give parameters for the storage we want to request for the PV.