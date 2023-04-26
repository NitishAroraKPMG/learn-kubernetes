# Day 3

## We have probes PPT in the day 3 folder for more details

# Probes in pods

When we have our server live and running there are certain scenarios where our server might not work as expected.

e.g.
1. Pod working from a very long time, it might become un-responsive.
2. Our application code has a lots of bugs which causes server to crash frequently.
3. Our application is doing a very heavy and long processing due to which server is not ready to accept new requests.

In all above given scenarios, If a user sent a request to the given pod. It will only show an error to the user. So there is no point sending traffic to the pod. 
Most of the time problem can be solved using a simple restart and if server is doing any processing we simply have to stop sending requests to the given pod.

In order to handle given scenario kubernates provides us with prods. If a probe fails system will stop sending the requests to the server or it will restart it depending upon the type of probe. But all the things will be handled automatically. 

### Following YMAL is being used for the probes

```
apiVersion: v1
kind: Pod
metadata:
  name: kuard-config
spec:
  containers:
    - name: test-container
      image: gcr.io/kuar-demo/kuard-amd64:blue
      livenessProbe:
        httpGet:
          path: /healthy
          port: 8080
        initialDelaySeconds: 5
        periodSeconds: 5
      readinessProbe:
        httpGet:
          path: /ready
          port: 8080
        initialDelaySeconds: 5
        periodSeconds: 5
  restartPolicy: Always
```

We can directly apply the file and expose the port by using following commands to see the pod details.

`kubectl apply -f configmap.yaml`

then forward the port to created pod

`kubectl port-forward kuard-config 8080`

### Types of probes

We have mainly three types of probes

1. Liveness Probe
This probe will be responsible of checking if the server is live or not. If this probe fails server will be stopped and restarted considering the restart policy of the pod. If restart policy is set to Always or On Error. System will restart the pod. But if policy is set to Never. System will simply kill the pod and will not create it again.

Following is the code for liveness probe

```
livenessProbe:
        httpGet:
          path: /healthy
          port: 8080
        initialDelaySeconds: 5
        periodSeconds: 5
```

`initialDelaySeconds` will delay the number of seconds before server sends a request.
`periodSeconds` will define after how many seconds request shall be sent again.

Once the YAML is applied and running we can go to `http://localhost:8080` and check the liveness probe section. We can manually fail the probe. Once probe is failed we can see in pod details that pod is killed and created again.

2. Readiness Probe
This probe will be responsible for server to check if server is ready to accept the traffic or not. If service returned header code between 200 - 400 then it will consider it as success and will keep sending traffic to the pod. if header status code is greater than 400 then system will mark such pods as no ready and will stop sending the traffic until pod is ready again. That is the differentce between Liveness and Readiness Probe.

Readiness probe is also same with some minor difference.

```
readinessProbe:
        httpGet:
          path: /ready
          port: 8080
        initialDelaySeconds: 5
        periodSeconds: 5
```

Once the YAML is applied and running we can go to `http://localhost:8080` and check the liveness probe section. We can manually fail the probe. Once probe is failed, Ready status of the pod is changed from 1 to 0.

3. Startup Probe
There can be scenarios when we want to complete the startup process of pod and only then check for Liveness and Readiness Probe because it doesn't make sense to kill the pod before it comples the startup. So in that case we can use the startup probe. Once startup probe is defined it will stop the liveness probe and Readiness probe takeover until it is completed. 

We take a worst case scenario for the pod startup and define `failureThreshold` so `failureThreshold * periodSeconds` shall be equal to seconds taken in worst case scenario.


### Other settings we can use in the probes

1. InitialDelayseconds : Set a delay between the time the container starts and the first time the probe is executed. Defaults to zero seconds.
2. PeriodInSeconds: Defines how frequently the probe will be executed after the initial delay. Defaults to ten seconds.timeoutSeconds: Number of seconds after which the probe times out. Defaults to 1 second. Minimum value is 1.
3. successThreshold: Minimum consecutive successes for the probe to be considered successful after having failed. Defaults to 1. Must be 1 for liveness and startup Probes. Minimum value is 1.
4. failureThreshold: After a probe fails failureThreshold times in a row, Kubernetes considers that the overall check has failed: the container is not ready / healthy / live. For the case of a startup or liveness probe, if at least failureThreshold probes have failed, Kubernetes treats the container as unhealthy and triggers a restart for that specific container. 
5. terminationGracePeriodSeconds: configure a grace period for the kubelet to wait between triggering a shut down of the failed container, 

