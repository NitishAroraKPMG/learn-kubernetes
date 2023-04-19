## Lab 5


1. Create a deployment nginx-dep-lab-5 use image nginx

<details>

<summary></summary>

`kubectl create deployment nginx-dep-lab-5 --image=nginx`

</details>


2. Delete any pod created in previous command 


<details>

<summary></summary>

`kubectl delete pod nginx-dep-lab-5-<some hash code>`

</details>


3. Fetch all the pods 

<details>

<summary></summary>

`kubectl get pods`
have you seen the new pod created again? can you think why this happened?
</details>



4. Edit the deployment file and change the replicas to 10 

<details>

<summary></summary>

`kubectl edit deployment nginx-dep-lab-5`
it will open the editor and now change the replicas under spec to 5
save the file

</details>


5. Fetch all the resources in K8s cluster. Have you seen any changes?

<details>

<summary></summary>

`kubectl get all` (to fetch all resources on the cluster)

You will see 5 replicas of the POD is running
    
</details>


6. Get details of the deployemnt

<details>

<summary></summary>

`kubectl describe deployment nginx-dep-lab-5`
Execute command and see the events section and replicas section

</details>


7. Edit the deployment file for nginx-dep-lab-5 and change the deployment strategy to Recreate

<details>

<summary></summary>

`kubectl edit deployment nginx-dep-lab-5`
It will open the file in editor (on terminal)
under spec edit the strategy to below 
`strategy:`
`   type: Recreate`
`  `
</details>

8. Change the image of the deployment to nginx using tag 1.14
<details>

<summary></summary>

`kubectl edit deployment nginx-dep-lab-5`
It will open the file in editor (on terminal)
change the container image to nginx:1.14
save the file
</details>

9. Fetch all the resources 
<details>

<summary></summary>

`kubectl get all` (to fetch all resources on the cluster)

a new replica set is created with desired state (see the age of the replicaset to ensure whats the latest replicaset) 
As all the previous running pods in older replicaset are scaled to 0
and then a new replicaset is created with the required number of pods
</details>

