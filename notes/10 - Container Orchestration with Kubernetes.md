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

ConfigMap and Secret

External Configuration of your application that can be mount to the pod for specific configuration without rebuilding the container image.

Confidential information can be sored in Secret for best practice instead of usage of ConfigMap

The Secret resources are used to stored secret data using base64 encoded data.

In an Kubernetes cluster the security mechanism are not enabled by default.

Volumes 

In order to have persistent data the usage of volumes are necessary, volumes are attached storage to pod, this can be storage on local machine or remote, outside of the K8S cluster.

K8S does not manage data persistance, you need to have a CSI to manage it.

Deployment and StatefulSet

Define a blueprint, specifying the number of replica, this can be done in deployment resource type.

DB can not be replicate with a deployment, because database have a state, this can be done with a StatefulSet (mongodb, mysql, postgresql, elasticsearch)

Kubernetes Architecture 
------------------------

* Master node
* Slave node

Worker node

* each node has multiple pods on it
* 3 process that must be installed on every node (container runtime [docker, containerd], kubelet, kube proxy)

How do you interact with this Kubernetes Cluster ? 

Managing processes are done with master nodes

* API Server (cluster gateway, gatekeeper)
* Scheduler (Schedule resources to the cluster)
* Controller Manager (detect cluster state changes)
* ETCD (Cluster brain) (application data is not stored into etcd)

e.g : Some request -> API Server -> validates request -> other process -> Pod

Usually you should have multiple master nodes (reliability of etcd etc master components.)

To add Woker or Master node into an existing cluster :

1/ Get a new bare server
2/ Install all the master/worker node processes
3/ Add it to the cluster

Minikube and kubectl - Local Kubernetes Cluster
-------------------------------------------------

Minikube will create a K8S cluster using only one physical or virtual node, it will install all the master and worker processes

kubectl usually talk to API Server to talk to the K8S Cluster.

To install minikube on Windows 11 : winget install minikube
To start minikube on Windows 11 (Cmd Admin) : minikube start
To check the status of the cluster : minikube status

Main kubectl commands 
------------------------

kubectl get nodes
kubectl get pod
kubectl get services
kubectl create deployment test --image=nginx
kubectl get deployment
kubectl get replicaset
kubectl edit deployment nginx-depl
kubectl logs <podname>
kubectl exec -it <podname> -- bin/bash
kubectl delete deployment <deployname>
kubectl apply -f <manifest.yaml>

Example of a deployment using manifest file : 

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 2
  selector:
    matchLabels: 
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.16
        ports:
        - containerPort: 80
























