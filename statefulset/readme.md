# Kubernetes StatefulSet
Component for stateful applications

Examples of <i>stateful</i> applications -> deployed with kubernetes statefulSet (`sts`)
- Databases
- MySQL
- MongoDB
- elasticsearch
- applications that store data (tracks state by setting it in some storage)

A <i>stateless</i> application -> deployed with kubernetes deployment
- does not keep a record of state
- each request is completely new

Both application types manage pods based on a container specification

## Deplyment vs StatefulSet
- Replicating stateful applications is more difficult
- Has other requirements

### Deployment pods
- Pods are identical and interchangable
- Created in random order with random hashes
- one Service that load balances to any pod
- typical name: `mysql-c9e93d4e783165d`

### StatefulSet pods
- Can't be created/deleted at the same time
- Can't be randomly addressed
- Created from the same specification, but are not interchangable
- Persistent identifier across re-scheduling
- replica pods are not identical - they have their own `pod identity`
- This `pod identity` has the form `{StatefulSet Name}-{ordinal}`
- typical name: `mysql-0`, `mysql-1`, ...
- Creation and deletion happens in order 0-1-2-3 and 3-2-1-0 respectively. 
- Any new pod "copies" the state from the "last indexed pod"
- StatefulSet pods have 2 endpoints:
1) Load balancer from service `svc1`
2) Individual service names `mysql-0.svc2`, `mysql-1.svc2`, ... where svc2 is a name defined in the StatefulSet


### Data consistency
- two pods cannot simultaneously write data to its state because it will lead to data inconsistencies
- a mechanism makes sure only one pod is allowed to write data. Reading is fine for any pod.
- The pod responsible for updating the data is called `master`. The others are called `workers` or `slaves`
- Each pod have their own replicas of the storage, but do not reference the original data. So stateful pods have to continuously synchronize the data.
- Even though pods synchronize data, the data will be lost if all pods die.
- Database applications should therefore use persistent volumes to store the data.
- Pod state, including their unique pod identity is stored in persistant volumes, and if a pod dies, or is replaced by another, it gets the state and ID from persistant volumes.

## Summing up the difficulties
- Its a complex setup
- kubernetes helps you, but you still have to do a lot of setup in yaml-files.
    - configuring the cloning and data synchronization
    - Make remote storage available
    - Managing and back-up