## Day 1

Today, we are going to learn the basic concepts of Kubernetes.

* Namespaces
* Pods

## Namespaces

Namespace is the logical grouping of the kubernetes resources like Pods, ConfigMaps/Secrets, Deployment etc.

By default, Kubernetes eco-system comes with the 4 namespaces each having a separate usage of it.

* default: Default namespace in which your pods will be running (if you don’t specify the namespace)
* kube-system: Used to contain all the pods required to run the kuberenetes cluster.
* kube-public: Used to expose the pods to the outside world/shared resources within cluster.


Create Namespace:

`kubectl create namespace first-namespace`

Get Details of namespace:

`kubectl get namespace/first-namespace`

Delete Namespace:

`kubectl delete namespace/first-namespace`

Get all Namespaces:

`kubectl get namespaces`


## Pods


Create Pod:

In order to run our application in Kubernetes, we need dockerize our application and will be running the application as a pod (Running instance of the application).

If you don’t specify the namespace, all pods will be running in the **default** namespace.

`kubectl run <your_pod_name> --image=<application_docker_image_name>`

For example, to run the nginx server :-

`kubectl run nginx --image=nginx`


Few details of Pod:

In order to view the details of pod, run the following command:-

`kubectl get pods/<your_pod_name>`

For example,

`kubectl get pods/nginx`


All details of Pod:

In order to view all details of pod, run the following command:-

`kubectl describe pod/<your_pod_name>`

For example,

`kubectl describe pod/nginx`


Delete Pod:

`kubectl delete pod/<your_pod_name>`

For example,

`kubectl delete pod/nginx`









