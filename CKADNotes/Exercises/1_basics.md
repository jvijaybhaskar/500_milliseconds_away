


## Create a new Pod named nginx running the image nginx:1.17.10. Expose the container port 80. The Pod should live in the namespace named ckad.


```
kubectl create namespace ckad

kubectl run nginx --image=nginx:1.17.10 --restart=Never \
    --port=80 --env="DNS_DOMAIN=cluster" --labels="app=nginx,env=prod" \
    --namespace=ckad

```


