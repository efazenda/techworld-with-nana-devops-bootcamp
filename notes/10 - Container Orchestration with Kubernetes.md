Container Orchestration with Kubernetes

* Introduction of K8S
* Main K8s components
* Minikube & kubectl 
* Main kubectl commands 
* YAML Configuration File
* Demo project
* Organize components (Namespaces)
* Configure connectivity (Services)
* Make App available from outside (Ingress)
* Persist data (Volumes)
* ConfigMap & Secrets  as Volume Types
* Deploy stateful Apps (StatefulSet)
* Package Manager of K8S (Helm)
* Extending the K8S API (Operator)
* Introduction to Microservices 
* Demo Project : Deploy a Micro Service Application
* Production & Security Best Practices
* Demo Project : Create a Helm for Microservices
* Demo Project : Deploy Microservices with helmfile
* Authorization in Kubernetes - RBAC

Introduction to Kubernetes
---------------------------

* Opensource container orchestration tool
* Developped by Google
* Help you manage containerized applications in different environements

What problems does Kubernetes solve ? 
What are the tasks of an orchestration tool ? 

* Trend from Monolith to Microservices
* Increase usage of containers

What geatures do orchestration tools offer ?

* HA and no downtime
* Scalability 
* Disaster recovery 

Main Kubernetes Components
-----------------------------

Node and Pod 

Node : Kubernetes node 
Pod : Abstraction over a container (smallest unit of K8S)

A pod usually run one application container inside of it

Kubernetes offer out of the box a virtual network, means each pod have an IP address (private ip address)

Pod are like containers , there are ephemeral.

Services and Ingress

Service is bacically an static IP address that can be attached to each pod 

Even if the pod dies the service will remain and the IP address of the service will still be attached to the pod.

External Service and Internal Service can be created depending of the usage.

If you want to have custom urls and not accessing your application via Ports based on IP of the kubernetes node, the best practice is to use a Ingress resource type.

Ingress -> Service -> Pods 









