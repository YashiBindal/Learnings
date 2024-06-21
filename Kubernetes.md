***## Components of Kubernetes

1. `Node`: A physical/virtual server. A container runtime, K8s Kubelet and Kube Proxy must be installed on every node.
   
2. `Pods`: 
   - Pod is abstraction of container, it creates an environment for container. A pod is a collection of containers that share a network and mount namespace and are scheduled on the same host. A pod is the basic building block of Kubernetes. A pod represents a single instance of a running process in your cluster. Pods contain one or more containers, such as Docker containers. When a pod runs multiple containers, the containers are managed as a single entity and share the resources of the host machine.
   - a pod has a internal IPaddress that is used to connect to other pods in the same cluster. But if a pod goes down and new pod is created, the IP address of the new pod will be different. So, we need a way to connect to the pod without knowing the IP address. This is where service comes in.- while scaling we create a new pod with that container.
   - a pod can has multiple containers..but they are not of the same kind

3. `Service`: 
   - A service is a logical set of pods that work together. A service is defined using YAML (Yet Another Markup Language) or JSON (JavaScript Object Notation) format. A service can be defined as a set of pods that work together, such as one tier of a multi-tier application. The set of pods that constitute a service are defined by a label selector. If no label selector is defined, the service selects all pods in the cluster. The service also defines a policy by which to access the pods, such as single TCP port number. 
   - Services has static IP address that is used to connect to the pods. So, even if the pod goes down and new pod is created, the IP address of the service will be same. So, we can connect to the pod using the service IP address.
   - service also acts as load balancer. It will distribute the load among the pods. So, if one pod is down, the service will distribute the load to other pods. So, the service will always be available. A service can be attached to pods on different nodes.

4. `Ingress`: It allows to access pods/services using a user-friendly URL instead of IP address like 124.2.3.21:8080. It is used to map hostname to the IP address and port. 

5. `ConfigMap`: It is used to store configuration data that can be accessed by the pods. It is used to store data that is not sensitive. For example, database connection string, variable parameters, etc. It helps to decouple the configuration data from the pods. So, if the configuration data changes, we don't need to change the pods. We just need to change the configMap and the pods will automatically get the new configuration data.
   
6. `Secret`: It is used to store sensitive data like DB passwords, API keys, etc. It is similar to configMap but it is used to store sensitive data. It stores data in base64 encoded format.
   
7. `Volumes`: To store persistent data like for databases. It is a external attached hard disk to the pod. So, even if the pod goes down, the data will be persisted in the volume. When the pod comes up again, it will be able to access the data from the volume. Volume can be on local storage i.e. same host or remote storage. K8s does not manage data persistence, you have to configure it to take backups, replication, etc.
   
8. `Deployment`: It is a blueprint for creating pods. You can define replication of pods, how they should be scaled up/down, etc. DBs cannot be replicated using this.
   
9. `Stateful Sets`: Used for configuration of stateful applications like DBs and replicating them, but it keeps data synchronized.
    
10. `Namespaces` : used to isolate different environments. BY default kubernetes create 3 namespaces: default: the one we use, kube-system: to avoid accidental deletion and modification(created at cluster startup), kube-public: resources made available to all users is created.
    
We can create new namespaces as needed and allow resources quotas to them and also access permissions. add namespace tag in metadata section of yaml file to ensure a resource is always created in that name space

11. `Service Account`: For every namespace in Kubernetes, there s a default service account which is created automatically. It has a secret object with token in it.
    
12. `ingress`: Ingress helps your users access your application using a single, externally accessible URL that you can configure to route to different services within your cluster, based on the URL path. At the same time, implement SSL Security as well. Simply put, think of Ingress as a layer seven load balancer built in to the Kubernetes cluster that can be configured using native Kubernetes primitives just like any other object in Kubernetes. Now, remember, even with Ingress, you still need to expose it to make it accessible outside the cluster. So you still have to either publish it as a node port or with a cloud native load balancer, but that is just a one-time configuration.
   - `ingress controller`: acts as load balancer for requests and The Ingress controllers are nginx server that have additional intelligence built into them to monitor the Kubernetes cluster for new definitions or Ingress resources and configure the NGINX server accordingly.
   - `ingress resource`: An Ingress resource is a set of rules and configurations applied on the Ingress controller. You can configure rules to, say, simply forward all incoming traffic to a single application or route traffic to different applications based on the URL.

13. `Network Policies`: to allow deny and control traffic to pods
14. `Persisten volume`: to store persistent data
15. `persistent volume claims`: pods can claim small parts from persistent volume
16. `Storage class`: to remove manual create of persistent volume, with sc, pv are created automatically when a pvc uses sc
17. `Stateful sets`: for cases where you need to deploy pods one by one like in master slave relation or where you need fixed name for your pods. Stateful sets yaml file is same as that of deployment only the kind changes to `StatefulSet`
18. `headless service` : to create a DNS entry of each pod using its name  and a subdomain. This helps DNS service to directly access master node for writes. By setting `clusterIp` field to `None`, we can create a headless service. And to create a A record for pod, set `hostname` field to podname and `subdomain` field to headless service name and you will get a fixed DNS endpoint for that pod. In case of Stateful sets, we do not need to specify hostname and subdomain, it sets them automatically, we only need to specify the servicename to headless servicename explicitly.