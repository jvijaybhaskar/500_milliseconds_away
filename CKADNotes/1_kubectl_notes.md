
The Certified Kubernetes Application Developer (CKAD) program has been developed by the Cloud Native Computing Foundation (CNCF) to help expand the Kubernetes ecosystem.

The primary objective of the exam is to certify that users can design, build, configure, and expose cloud native applications for Kubernetes.

This <><><<>><><>


## Kubectl


Kubectl is a primary tool to interact with Kubernetes clusters over the command line. 

Using `kubectl` you can deploy apps, install and manage resources, and view logs. 

**If you are looking for a detailed list of commands and options available for you, I strongly recommend you look at [kubectl cheat sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/) on the Kubernetes website.** 

This post only covers the different options available for you to interact with resources on the cluster. 

> NOTE: Remember, when in doubt, always use `kubectl help` on your command line. 

---

## Kubectl command pattern

```sh

# Usage pattern
$ kubectl [command] [TYPE] [NAME] [flags]

# command: verbs like get, create, delete, and more that convey the action you want to perform on a object/resource. 

# TYPE: Specifies the resource type that the command is intended for. For example service/svc, pod.

# NAME: Name of the specific object or resource on which the command needs to be executed

# flags: Optional flags for your command. For example -o yaml, --port 

```


---

## Object Management

### Imperative approach 
You can use `kubectl run` or `kubectl create` to create an object on the fly. 

```sh

$ kubectl run frontend --image-nginx --restart=Never --port=80

```

> NOTE: While this approach saves time in bringing up pods, it is not a recommended approach for managing objects in production. 


### Declarative approach

This approach needs a manifest file (mostly a YAML file) to create an object declaratively using `kubectl create` or `kubectl apply` commands. 

> NOTE: This approach is the most recommended way to create objects in production. 

Additionally, you can version control the YAML files for future audits or rollbacks.

```sh
$ kubectl create -f pod.yaml
```


### Hybrid approach

In this approach, you start by using the imperative method to produce a manifest file without creating an object, like so: 

```sh
# Notice the use of --dry-run and -o yaml flag
$ kubectl run frontend --image=ngingx --restart=Never --port=80 -o yaml --dry-run=client > hybrid_pod.yaml

# Edit and save the YAML file and run the below command to create an object
$ kubectl create -f hybrid_pod.yaml

# Describe the pod you just created
$ kubectl describe pod hybrid_pod
```

---

## More useful commands

Kubectl offers more commands to manage k8s objects. This section covers a few:

### Deleting an object

You may want to delete an object for n number of reasons. For this purpose, you can use the `kubectl delete` command like so: 

```sh

# This command allows you to delete an object by providing its names
$ kubectl delete pod frontend

# This commands deletes an object by referring to the manifest file used to create it.
$ kubectl delete -f pod.yaml

```


### Editing a live object

You can edit your object while is it live on the cluster using the `edit` command like so: 

```sh 
kubectl edit pod frontend
```

After saving the object definition, k8s will try and reflect those changes in the live object. 


### Replacing a live object

The replace command will allow you to replace the definition of an existing object declaratively. 

> NOTE: The YAML file provided here must be a complete resource definition.

```sh 
# This command overwrites the live configuration with the one specified in the YAML file.
kubectl replace -f pod.yaml
```


### Updating a live object

Unlike replace, the apply command updates an existing object incrementally or in its entirety.  

> NOTE: The YAML file provided here may be a full definition or a partial one., 

```sh 
# If the object does not exist, this command behaves like a create command. 
kubectl apply -f pod.yaml
```

--- 
## Summary

This post covers only a tiny portion of commands from the `kubectl` universe. Nonetheless, this post provides a quick insight into the fundamental ways you can interact with your cluster. 

There are many more `kubectl` commands that will make you more productive in managing a k8s cluster. 

For a complete list of commands and options available to you, have a look at [kubectl cheat sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/) on the Kubernetes website.

Finally, remember that always use `kubectl help` on your command line when in doubt. 

---
## References

Kubectl cheat sheet
https://kubernetes.io/docs/reference/kubectl/cheatsheet/

CKAD Study guide
https://learning.oreilly.com/library/view/certified-kubernetes-application/9781492083726/ 
