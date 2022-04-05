# Notes from Kubernetes Patterns book

## Foundation patterns - Predictable Demands


> “Predictable pattern is about how you should declare application requirements, whether they are hard time runtime dependencies or resource requirements.”


Example of runtime dependencies include: 
* CPU / Memory utilization profile
* Storage requirements (Persistant)
* Fixed port requirements

This helps Kubernetes find right place for your app/container in the cluster.

Identifying and providing applicaiton charachterisitics to the underlying platform is crutial for cloudnative applications. 

With the knowledge of resource and runtime dependencies needs of individual containers, the platoform can place the container on a cluster for efficient utilization of resources.

While **Intelligent placement** of containers is one problem to solve, the other one is **Capacity Planning** 


---
**NOTE**

This factors probably will the differentiatig factor from cost effectiveness of one cloud provider to another down the line.

---

### Runtime dependencies

#### Storage

* File storage (Epheremal - EmptyDir)
* Persistent Volumes (Long-lived storage) One example is logs storage. Volume can be declared explictly in container definition

#### Port requirements


---

## Foundation patterns - Declarative Demands

> For a deployment to do its job correctly, it expects containers to be good cloud-native citizens.



What is a good citizen in a distributed system ? 

* Listen and honor lifecycle events
* provide health-check endpoints

### Deployment stratergies

* Rolling deployment - No downtime | Multiple versions live at same time
* Fixed deployment - Small downtime | Only latest version live | progressive upgrade to new version
* Blue green - Small downtime | Only latest version live | Needs twice the application capacity 
* Canary Release (pilot) - No downtime | Scale up new ReplicaSet if stable and scale-down old ReplicaSet


## Service discovery pattern

> Aims to provide a stable endpoint that clients of a service can access the instances providing the service

There is a move from client side discovery of services to allowing platform to manage it.

In k8s world, service consumer just have knowledge of a fixed virtual service end point.
