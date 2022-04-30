# Configurtions for a pod

It is a common practice to configure a pod with environment variables to control runtime behaviour of contrainers.

The task of maintaining environment variables for containers become non-trivial in an enterprise setup.

To resolve this in Kubernetes, there are two primitives `ConfigMaps` and `Secrets`. They help with centralizing configuration data and are injected into a container.


> TIP: Fundamental principle of Continous Delivery is to build once and deploy everywhere. To ensure this configuration data required for a deployment is not maintained as a part of the application code.


`Secrets` are a way to store sensitive data in a container. They are stored in memory and not stored in the file system.

> NOTE: The values are encoded in base64.

`ConfigMaps` and `Secrets` store configuration data as a set of key-value pairs. The key-value are either injected into a container as environment variables or mounted as a Volume.


> TIP: Kubernetes provides a mechanism to create a `ConfigMap` and a `Secret` from a file or define them imperatively.

`ConfigMap` can be consumed by one or mode pods with the help of `envFrom.configMapRef.name`. It is either injected as environment variables or mounted as a volume.

> Note: When mounted as a volume, the data is stored in the file system of the container i.e every key is a file and the value is the content of the file.



----

Now onto `Secret`.




















