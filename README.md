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

mysql -u root -p'password' -h <ip> -P 3306

```

### exercise 1 - Simple pod
>We start with a simple pod. Docker image available here: https://hub.docker.com/r/stefanwalther/docker-test
>connectivity can be tested (after port forwarding) using
```sh
curl http://localhost:3000/health-check
```
>try to recreate the pod and check the associated ip
>check the dashboard

### exercise 2 - Namespace
>Create a new namespace and associate a pod to it.
>Check the namespaces available
>What happen deleting the namespace?

how to associate the pod to namespace

### exercise 3 - Namespace with LimitRange
>Create a namespace with limit range.
>Check the limit on the console
>Change the limit and use the apple command:
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
kubectl label po simple-test env=debug --overwrite #override existing label env
```
>try to connect the db pod from client pod installing mysql client

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
>You may never need to manipulate ReplicaSet objects: use a Deployment instead, and define your application in the spec section.

### exercise 7 - Service
>A Kubernetes Service is a resource you create to make a single, constant point of entry to a group of pods providing the same service. 
>Each service has an IP address and port that never change while the service exists.
>By default the service is of type ClusterIP
>
>After service creation, try to connect to it from an other pods using IP or service name
>look at the environment variables of the service pod, you should see the service cluster ip and port
```sh
kubectl exec simple-pod-client -it -- env|grep SIMPLE_SERVICE_SERVICE
``` 

### exercise 8 - NodePort Service
With node port you can expose a service using node port. Use this command to find the exposed service url:
```sh
minikube service --url simple-service
```

### exercise 9 - Volume
>Let's try creating a hostPath volume.
>The volume is not running. Why ?
>Warning:
>HostPath volumes present many security risks, and it is a best practice to avoid the use of HostPaths when possible. When a HostPath volume must be used, it should be scoped to only the required file or >directory.

### exercise 10 - ConfigMap
>ConfigMap are object containing key/value pairs with the values ranging from short literals to full config files.
>try to change the configMap and use the apply command insted of recreate configMap (and pod)
```sh
kubectl apply -f <yaml file>
```