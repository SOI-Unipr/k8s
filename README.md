# Kubernetes exercises

## Basic commands

### minikube
```sh
minikube start
minikube dashboard
minikube ssh
```
### Kubectl
```sh
kubectl cluster-info
kubectl create -f <file.yaml>
kubectl get pods
kubectl get nodes
kubectl get svc
kubectl get replicaset
kubectl describe <node>
kubectl describe po <pod>
kubectl logs <pod>
kubectl exec <pod> -it -- sh
kubectl port-forward simple-test <local port>:<pod port>
kubectl delete <pod name>
kubectl delete pods --all
```
### Other
```sh
apk update
apk add busybox-extras # telnet included
apk add mysql-client
apk add curl
```

### exercise 1
> We start with a simple pod. Docker image available here: https://hub.docker.com/r/stefanwalther/docker-test
>connectivity can be tested using
```sh
curl http://localhost:3000/health-check
```

### exercise 2

kubectl get namespaces
-> default
-> unipr-test

how to associate the pod to namespace


labels
kubectl get pods -l env


mysql -u root -p'password' -h 172.17.0.8 -P 3306

### exercise 6 Replica set

>Scale up the replica set using
```sh
kubectl scale rs <replicaset> --replicas=4
```
>You may never need to manipulate ReplicaSet objects: use a Deployment instead, and define your application in the spec section.

### exercise 7 Service

>A Kubernetes Service is a resource you create to make a single, constant point of entry to a group of pods providing the same service. 
>Each service has an IP address and port that never change while the service exists.
>
>After service creation, try to connect to it from an other pods using IP or service name
>look at the environment variables of the service pod, you should see the service cluster ip and port
```sh
kubectl exec simple-pod-client -it -- env|grep SIMPLE_SERVICE_SERVICE
``` 

### exercise 8 NodePort Service
With node port you can expose a service using node port
```
minikube service --url simple-service
```
