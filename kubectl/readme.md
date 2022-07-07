# kubectl commands

`kubectl config get-contexts`

`kubectl config use-context CONTEXT_NAME`

`kubectl version -h` ( = help)

`kubectl get nodes`

`kubectl get pod`

`kubectl get pod -o wide` (to get IP:port)

`kubectl get services` == `kubectl get svc`

`kubectl get deployment`

`kubectl get replicaset` (managing replicas of pod)

`kubectl create deployment [NAME] --image=[IMAGE] [options]`

`kubectl edit deployment [NAME]`

`kubectl logs [pod name]`

`kubectl describe pod [pod name]` (info about pod)

`kubectl exec -it [pod name]` (it = interactive terminal, opens bash)

`exit` (leaves interactive terminal)

`kubectl delete delpoyment [name]`

`kubectl apply -f [filename]` (yaml-file. It knows when to create/update deployment)

`kubectl delete -f [filename]` 



