## Configmap volumes

In this tutorial we are going to see how we can use config maps as volumes. 

Topic of discussion

### Configmap

Configmaps are great way to store non essential information in key-value pair, Pods can utilise this information as enviornment variables, command-line params and as configration files as volumes.

We can pass on files as configs using configmap volumes, which later can be used by application image inside the pod.

### Creating configmaps

First step will be to create a configmap, 

We can use the following command inorder to create a configmap

`kubectl create configmap my-config --from-file=my-config.txt --from-literal=extra-param=extra-value --from-literal=another-param=another-value`

In the above command we are creating config in two ways
1. We can create from the literal directly by passing a key value.
2. We can pass a config file to the config map. 

We can check the created config map by using following command

`kubectl get configmap my-config`

We can see the details for the configmap.

### Passing configmaps to pods

Please follow the following yaml file


```
apiVersion: v1
kind: Pod
metadata:
  name: kuard-config
spec:
  containers:
    - name: configmap-container
      image: gcr.io/kuar-demo/kuard-amd64:blue
      #imagePullPolicy: Always
      command:
        - "/kuard"
        - "$(EXTRA_PARAM)"
      env:
        # An example of an environment variable used inside the container
        - name: ANOTHER_PARAM
          valueFrom:
            configMapKeyRef:
              name: my-config
              key: another-param
        # An example of an environment variable passed to the command to start
        # the container (above).
        - name: EXTRA_PARAM
          valueFrom:
            configMapKeyRef:
              name: my-config
              key: extra-param
  restartPolicy: Always
```

You will see in the env we have given our config map params which were created from literal. 

We are using a already build image kuard for this demo which gives a lots of facalities out of the box. We can directly apply the file and expose the port by using following commands to see the pod details.

`kubectl apply -f configmap.yaml`

then forward the port to created pod

`kubectl port-forward kuard-config 8080`

Now if you will go to `http:\\localhost:8080` under env section you can see the variables.

### Passing configmaps as volumes in kubernates

Check the following yaml code. You will notice we have create a volume using config map. Once volume is created we have assigned that volume to our pod.

```
apiVersion: v1
kind: Pod
metadata:
  name: kuard-config
spec:
  containers:
    - name: test-container
      image: gcr.io/kuar-demo/kuard-amd64:blue
      #imagePullPolicy: Always
      command:
        - "/kuard"
        - "$(EXTRA_PARAM)"
      env:
        # An example of an environment variable used inside the container
        - name: ANOTHER_PARAM
          valueFrom:
            configMapKeyRef:
              name: my-config
              key: another-param
        # An example of an environment variable passed to the command to start
        # the container (above).
        - name: EXTRA_PARAM
          valueFrom:
            configMapKeyRef:
              name: my-config
              key: extra-param
      volumeMounts:
        # Mounting the ConfigMap as a set of files
        - name: config-volume
          mountPath: /config
  volumes:
    - name: config-volume
      configMap:
        name: my-config
  restartPolicy: Always
```

There are only just few additions from the previous yaml file.
Once we have applied the yaml, In our pod we will be able to see a config folder which will have all the config values as files
It will include both type of values 1. Created from literal 2. Created using the file
Our application can easily use config from volume.