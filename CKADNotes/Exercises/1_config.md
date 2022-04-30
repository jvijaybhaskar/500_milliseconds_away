

## Creating a `ConfigMap`


### Imperative approach
```sh
kubectl create configmap app-config --from-literal=key1=value1 --from-literal=key2=value2
```



```sh
# Environment variables loaded from file with environment variables

kubectl create configmap db-config --from-env-file=db-config.env
```



```sh
# Environment variables loaded from file 

kubectl create configmap additional-config --from-file=more-config.txt
```

```sh
# Environment variables loaded from folder container files

kubectl create configmap additional-config --from-file=prod-config
```


### Creating `ConfigMap` using yaml files

```sh
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  key1: value1
  key2: value2
```

----

## Consuming `ConfigMap` as environetment variables


```sh
#injecting ConfigMap into a container

apiVersion: v1
kind: Pod
metadata:
  name: hazelcast
spec:
  containers:
  - name: hazelcast
    image: hazelcast/hazelcast
    envFrom:
    - configMapRef:
        name: app-config
```

After creationg a pod, you can inspect the environment variables by executing the remote Unix `env` command inside the container. 

```sh
kubectl exec -it hazelcast -- env
```


### Mounting a `ConfigMap` as a volume

```sh
apiVersion: v1
kind: Pod  
metadata:
  name: hazelcast
spec:
  containers:
  - name: hazelcast
    image: hazelcast/hazelcast
    volumeMounts:
    - name: config-volume
      mountPath: /etc/config
  volumes:
  - name: config-volume
    configMap:
      name: app-config
```

To verify how the key-value pairs are represented, open an interactive shell into the container and execute the following command:

```sh
kubectl exec -it hazelcast -- /bin/sh

# list the keys represented as files inside the container
ls -1 /etc/config
db_url
service_id

# cat the contents of the files to view the values
cat /etc/config/db_url
cat /etc/config/service_id
```


----
