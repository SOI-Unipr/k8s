# Kubernetes exercises

## Minikube installation
https://minikube.sigs.k8s.io/docs/start/

## Kubectl installation
https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/
https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/
https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/


## kubectl Cheat Sheet
https://kubernetes.io/docs/reference/kubectl/cheatsheet/

## Basic commands

### minikube
```sh
minikube start
minikube dashboard
minikube ssh # access to the node
```
### kubectl
```sh
kubectl cluster-info
kubectl create -f <file.yaml>
kubectl create namespace <namespace name>
kubectl create -f <file.yaml> -n <namespace name>
kubectl delete -f <file.yaml> -n <namespace name>
kubectl get pods
kubectl get pods -o wide # ip
kubectl get nodes
kubectl get namespaces
kubectl get svc
kubectl get replicaset
kubectl describe <node>
kubectl describe po <pod>
kubectl logs <pod>
kubectl exec <pod> -it -- sh
kubectl port-forward simple-pod <local port>:<pod port>
kubectl delete pod <pod name>
kubectl delete ns <namespace name>
kubectl delete pods --all
```
### others
Install additional apps on alpine linux
```sh
apk update
apk add busybox-extras # telnet included
apk add mysql-client
apk add curl

mysql -u root -p'password' -h <ip> -P 3306

```

Find the ip of the container
```
ip a
```

### exercise 1 - Simple pod
>We start with a simple pod. Docker image available here: https://hub.docker.com/r/stefanwalther/docker-test
>connectivity can be tested (after port forwarding) using
```sh
curl http://localhost:3000/health-check
```
>try to recreate the pod and check the associated ip. 
>check the dashboard

### exercise 2 - Namespace
>Create a new namespace and associate a pod to it.
>Check the namespaces/pods available also in the dashboard.
>How to associate a new pod to namespace?
>what will happen after deleting the namespace?


### exercise 3 - Namespace with LimitRange
>Create a namespace with limit range.
>Check the limit on the console
>Change the limit and use the apply command:
```sh
kubectl apply -f 3.pod-with-namespace-and-limit.yaml
```

### exercise 4 - Labels
>Associate labels to pods.
```sh
kubectl get po --show-labels
kubectl get pods -l env
```
>You can associate labels after pods creation using:
```sh
kubectl label po <pod name> env=debug --overwrite #override existing label env
```
>try to connect the db pod from client pod installing mysql client
tip use the following command to retrieve pod's ips
```
kubectl get pods -o wide
```
### exercise 5 - Liveness
>You can specify a liveness probe for each container in the pod’s specification. Kubernetes will periodically execute the probe and restart the container if the probe fails.
> https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/
> After 10 seconds, view Pod events to verify that liveness probes have failed and the container has been restarted:
```sh
kubectl describe pod liveness-pod
```

### exercise 6 - Replica set
>A ReplicaSet then fulfills its purpose by creating and deleting Pods as needed to reach the desired number. When a ReplicaSet needs to create new Pods, it uses its Pod template.
>Try to delete some pods of the RS and check what happen
>Scale up the replica set using
```sh
kubectl scale rs <replicaset> --replicas=4
```
or change the replicas on the yaml file and then
```
kubectl apply -f <yaml file>
```

>You may never need to manipulate ReplicaSet objects: use a Deployment instead, and define your application in the spec section.

### exercise 7 - Service
>A Kubernetes Service is a resource you create to make a single, constant point of entry to a group of pods providing the same service. 
>Each service has an IP address and port that never change while the service exists.
>By default the service is of type ClusterIP
>
After service creation, try to connect to it from an other pods trough wget command using service IP or service name or from node's curl using
```
minikube ssh
```
look at the environment variables of the service pod, you should see the service cluster ip and port
```sh
kubectl exec simple-pod-client -it -- env|grep SIMPLE_SERVICE_SERVICE
``` 

### exercise 8 - NodePort Service
With node port you can expose a service using node port. Use this command to find the exposed service url:
```sh
minikube service --url simple-service
```
Try to connect to the service from the host: 
curl  http://<host>:<port>


### exercise 9 - Volume
Let's try creating a hostPath volume.
The volume is not running. Why ? *Tip use describe command or ```kubectl get event --field-selector involvedObject.name=my-pod```*
>Warning:
>HostPath volumes present many security risks, and it is a best practice to avoid the use of HostPaths when possible. When a HostPath volume must be used, it should be scoped to only the required file or >directory.

### exercise 10 - ConfigMap
>ConfigMap are object containing key/value pairs with the values ranging from short literals to full config files.
>try to change the configMap and use the apply command: it has no effect on the pod
```sh
kubectl apply -f <yaml file>
```


### exercise 11 - ConfigMap with volume
>try to change the configMap and use the apply command: 
```sh
kubectl apply -f <yaml file>
```
check the /config/hello file:
```
kubectl exec <pod name> -it -- cat /config/hello
```
