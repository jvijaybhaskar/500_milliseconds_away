
## Pods

The most important primitive in k8s API is the Pod.

What does it do?
A Pod lets you run a containerize application.

Typically there is a one is to one mapping between a POD and a container. However, there are cases where there are more than one container per Pod. 

What else can a Pod do? 
Think of Pod as a wrapper for containers. i.e apart from running containers, it can consume/expose services like persistent storage, configuration data and more to containers. Addtionally it can expose more functions by mediating with other Kubernetes objects.  <**@TODO need more examples**>

## Containers

In the previous section you might notice that mention of containers more than five times. Lets take a few minutes to explore the container landscale. 

A **container** is a running instance of an application that encompasses the software (incuding source code/binary, dependencies, tools, operating system) its runtime environment and configuration.

The process of a building an application into a container is called **containerization**. 
A **Dockerfile** explicitly spells out what needs to happen during the containerization process.[ Think of it as a blueprint or a recepie!]

The artifact that gets generated at the end of the conterization process is an **image** which is hosted in a **repository** (like Docker Hub, Quay.io). 


<**@TODO Need a diagram to visualize**>

> Note: You might want to deep dive into containers. 


---

## Ready for a hands on ? 

<@TODO: COnterize an application using docker commands/push to a registry and run > 


---
## Pod lifecycle 

Essensially Kubernets is a state engine <@TODO: Link to what it is>. 
The control loops mechanism  asyncronously monitors the statuses of objects and adjusts it to maintain the balance. 

From application maintenance perspective it is important to understand the various phases of the Pod  lifecycle and act accordingly.




Pending | Running | Successed | Failed | Unknown



## Getting logs out of Pod



## Environemnt variables


## Namespaces