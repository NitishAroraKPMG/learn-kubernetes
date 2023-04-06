# learn-kubernetes

Consists of learning commands and practices to learn about Kubernetes.


The first step in learning the Kubernetes is to install the Kubernetes on the local machine. There are two ways to start the Kubernetes in the local machine:-

* [Install Minikube](https://minikube.sigs.k8s.io/docs/start/)
* [Enable kubernetes on the Docker Desktop]()



## Enable kubernetes on Docker Desktop

Open the Docker Desktop GUI application on the machine.



Click on the Gear icon, to open the Settings in the Docker Desktop. Click the **Kubernetes** option in the left side menu.

Select the checkbox “Enable Kubernetes” and click the button “Apply & Restart”.

**Note: It will take some time to restart the Kubernetes.**



There will be Kuberenetes(Web) icon in the botton-left bar of the Docker Desktop stating the status of the Kuberenetes engine. (Green - Running, Yellow - In progress)


## Verify the installation of Kubernetes on the local machine

Run the command:-

`kubectl version`

Will display the version of the kubernetes client(CLI) and the server (Kubernetes engine/api-server)



## Commands in Kubernetes

kubectl \[<command>\] \[<kubernetes_object>\] \[<options/flags>\]


Commands help


1. kubectl create --help
2. kubectl describe --help
3. kubectl run --help
4. kubectl get --help
5. kubectl delete --help




