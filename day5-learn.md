## Day 5

Today, we are going to discuss below topic in K8s.

* Deployment


one of the main feature of kubernetes is self-healing i.e.
    1. ensuring required number of pods for a given application are always running
    2. if pod is deleted accidently or intentionally, K8s cluster should maintain the required number of PODS and their state


## Deployment

When moving or deploying application to K8s cluster, we won't directly create pods. 
**Why??..** Since pods don't have the self healing nature.


Create a pod using below command 
`kubectl run nginx-pod --image=nginx`

Now try to delete the pod created in previous command
`kubectl delete pod nginx-pod `

oh! where is the pod??
Here comes the Deployment. Deployment tells kubernetes how to create or modify instances of pods that holds the 
containeraized application.

It can help to scale the number of replica pods, enable the rollout of updated code in a controlled manner or
rollback to earlier version if required.

K8s deployment creates a replicaset and replicaset is responsible for created the desired number of pods.

Deployments provides the self healing nature.


**How to create deployment and make sure desired PODS are always running ?**

Create Deployment with image:nginx:
`kubectl create deployment nginx-dev --image=nginx`

    After running the above command below events would happen
    1. a deployment with name nginx-dev is created 
    2. deployment will create a replicaset with name nginx-dev-<some hash code>
    3. replicaset will create the desired pods with name replicaset-name-<some hash code>

    To view all the resources created using above command 
    `kubectl get all`

Now try to delete any pod created using deployment in previous step 
`kubectl get pods`
`kubectl delete pod nginx-dev-<some-hash-code>`

and now run the below command 
`kubectl get pods`

Ahh! you will see a new pod is created (check the age of the pod it will be few seconds)


### Deployment Strategy

1. **Rolling Update**: This is the default strategy. It will delete and recreate pods gradually without interrupting the application 
availability. This strategy utilizes the maxUnavailable and maxSurge values to control the rolling update process.
    a. **maxUnavailable** defines the maximum number of pods that can be unavailable in the update process.
    b. **maxSurge** defines the maximum number of pods that can be created.

2. **Recreate**:  will delete all the existing pods before creating the new pods

