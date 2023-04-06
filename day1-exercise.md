## Solution for lab1


1. Get all the namespaces in k8s 

<details>

<summary></summary>

`kubectl get namespaces`

</details>


2. Create namespace named secret


<details>

<summary></summary>

`kubectl create namespace secret`

</details>


3. Get details of the namespace created

<details>

<summary></summary>

`kubectl describe namespace secret`

</details>


4. Create a pod named nginx-demo with image nginx, using the tag 1.42

<details>

<summary></summary>

`kubectl run nginx-demo --image=nginx:1.42`

</details>


5. Get the status of pod created above and see if it is running (if not debug and try to find the root cause)

<details>

<summary></summary>

`kubectl get pods` (to get all running pods in default namespace)

`kubectl get pods nginx-demo` (to get specific named pod under default namespace)

The status of pod will be ImagePullBackOff or ErrImagePull
the reason being the required image nginx:1.42 is not found.
To get more info about diff between error ImagePullBackOff and ErrImagePull refer this [link](https://komodor.com/learn/how-to-fix-errimagepull-and-imagepullbackoff/) 
    
</details>


6. Delete the pod created above

<details>

<summary></summary>

`kubectl delete pod nginx-demo`

</details>


7. Create a pod named nginx-secret, using the image nginx with tag 1.22-bullseye in secret namespace.

<details>

<summary></summary>

`kubectl run nginx-secret --image nginx:1.22-bullseye -n secret`

</details>


8. Check the status of nginx-secret pod.

<details>

<summary></summary>

`kubectl get pods nginx-secret -n secret` (to get particular named pod created under namespace)

`kubectl get pods -n secret` (to get all pods for namespace secret)

</details>


9. Remove the namespace secret

<details>

<summary></summary>

`kubectl delete ns secret`

</details>


10. Get POD(s) under namespace named secret
(if not try to think what happen to all the resources if namespace is deleted)

<details>

<summary></summary>

`kubectl get pods --all-namespaces` (will not display the pod)
`kubectl get pods nginx-secret -n secret` (will give error "Error from server (NotFound): namespaces "secret" not found")
`kubectl get pods -n secret` (will display message "No resources found in secret namespace.")
    
On deleting the namespace all the resources associated with the namespaces are deleted

</details>
