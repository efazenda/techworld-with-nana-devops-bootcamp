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

YAML Configuration File
--------------------------

* Metadata
* Specification (specific attributes to the kind)
* Status (automatically generated by K8S)

The status data is stored in etcd database

Format of configuration files is YAML (strict with identation)

Blueprint (template) has is own metadata (Configuration inside a configuration file)

The connection between service and deployment is done with Selector and Labels


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
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080

kubectl get pods -o wide
kubectl get service
kubectl get deployment nginx-deployment -o yaml > deployment-nginx-result.yaml
kubectl delete -f <manifest.yaml>

Complete Demo Project - Deploying Application in Kubernetes Cluster
------------------------------------------------------------

Here the complete deployment of a MongoDB with Mongo Express with external access to the Mongo Express interface.

apiVersion: v1
kind: Secret
metadata:
  name: mongodb-secret
type: Opaque
data:
  mongo-root-username: dXNlcm5hbWU=
  mongo-root-password: cGFzc3dvcmQ=
---
kind: ConfigMap
apiVersion: v1
metadata:
  name: mongodb-configmap
data:
  database_url: mongodb-service
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name:  mongo-express-deployment
  labels:
    app: mongo-express
spec:
  selector:
    matchLabels:
      app: mongo-express
  replicas: 1
  template:
    metadata:
      labels:
        app: mongo-express
    spec:
      containers:
      - name: mongo-express
        image: mongo-express
        env:
          - name: ME_CONFIG_MONGODB_ADMINUSERNAME
            valueFrom:
              secretKeyRef:
                name: mongodb-secret
                key: mongo-root-username
          - name: ME_CONFIG_MONGODB_ADMINPASSWORD
            valueFrom:
              secretKeyRef:
                name: mongodb-secret
                key: mongo-root-password
          - name: ME_CONFIG_MONGODB_SERVER
            valueFrom:
              configMapKeyRef:
                name: mongodb-configmap
                key: database_url
        ports:
          - containerPort: 8081
---
apiVersion: v1
kind: Service
metadata:
  name: mongo-express-service
spec:
  selector:
    app: mongo-express
  type: LoadBalancer
  ports:
    - protocol: TCP
      port: 8081
      targetPort: 8081
      nodePort: 30000
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongodb-deployment
  labels:
    app: mongodb
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongodb
  template:
    metadata:
      labels:
        app: mongodb
    spec:
      containers:
      - name: mongodb
        image: mongo
        ports:
        - containerPort: 27017
        env:
        - name: MONGO_INITDB_ROOT_USERNAME
          valueFrom:
            secretKeyRef:
              name: mongodb-secret
              key: mongo-root-username
        - name: MONGO_INITDB_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mongodb-secret
              key: mongo-root-password
---
apiVersion: v1
kind: Service
metadata:
  name: mongodb-service
spec:
  selector:
    app: mongodb
  ports:
    - protocol: TCP
      port: 27017
      targetPort: 27017

Namespaces Organizing Components 
---------------------------------

What is a namespaces ?

By default Kubernetes gives you somes of them

* kube-system (Do not create resources here)
* kube-public 
* kube-node-lease
* default

kubectl create namespace <nameofnamespace>

* Group resources into namesapces is a best practice
* In case of multiple teams that work on one cluster for the same application
* When You need to have Staging or Production environements.
* Limit access to specific users or temas
* Limit resources at namespace scope

Limits : ConfigMap and Secret can not be used accross namespaces

Volume, Nodes and some other type of resources are not scoped to a namespace.

List resources of a namespaces : 

kubectl get <kind> -n <namespace_name>

There is a tool called kubens which let you point to the namespace you want to work or check which one you are tied to.

To install it on Windows : winget install kubens 

Services - Connecting to Application inside cluster
------------------------------------------------------

* Cluster IP
* Headless 
* NodePort
* LoadBalancer

IP of the pod are ephemeral as they follow the lifecycle of the pod

In  front of each pod we have a service with statis IP address.

The service are also the features of load balancer.

They can be usecul for internal and external communications.

The Cluster IP : 

* Default Type 
* Internal Service
* Specific Port 

Pods are identified  by the selectors (labels of the pods)

To see Service endpoints you can run kubectl get endpoints 

You can have a multiple port service resource, but if so , you will need to name them in the manifest file 

Example : 

apiVersion: v1
kind: Service
metadata:
  name: mongo-express-service
spec:
  selector:
    app: mongo-express
  type: LoadBalancer
  ports:
    - name: mongo-express
      protocol: TCP
      port: 8081
      targetPort: 8081
    - name: mongo-express-exporter
      protocol: TCP
      port: 9216
      targetPort: 9216

The Headless :

* Client wants to communicate with 1 specific Pod directly
* Pods want to talk directly with specific pod

This is usefull in case of deployment of statefulset (databases)

When creating a service just put ClusterIP: none this will be a headless service

Node Port : 

Create a service that is accessible on a static port on each worker node (External Access) (can be from 30000 - 32767)


Load Balancer :

Cloud Provider give LoadBalancer feature, you can also have for example metallb for on premise workload.

Loadbalancer service is an extension of Nodeport Service
Nodeport Service is an extension of ClusterIP Service

Ingress - Connecting to Applications outside cluster.
-----------------------------------------------------

Example : 

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: myapp-ingress
spec:
  rules:
  - host: myapp.com
    http:
      paths:
      - backend:
          serviceName: myapp-service-internal
          servicePort: 8080

Before using Ingress, you need to install an ingress controller

The ingress controller will do :

* evaluates all the rules
* manages redirections
* entrypoint to cluster

You can have multiple path for an application and this can be defined to the ingress resource of your application 

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: myapp-ingress
spec:
  rules:
  - host: myapp.com
    http:
      paths:
      - path: /analytics
        backend:
          serviceName: myapp-analytics-service
          servicePort: 8080
      - path: /shopping
        backend:
          serviceName: myapp-shopping-service
          servicePort: 8080

For multiple sub-domains or domains

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: virtualhost-ingress
spec:
  rules:
  - host: analytics.myapp.com
    http:
      paths:
        backend:
          serviceName: myapp-analytics-service
          servicePort: 8080
  - host: shopping.myapp.com
    http:
      paths:
        backend:
          serviceName: myapp-shopping-service
          servicePort: 8080      

Configuring TLS Certificate

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: myapp-ingress
spec:
  tls:
  - hosts:
    - myapp.com
    secretName: myapp-sercret-tls
  rules:
  - host: myapp.com
    http:
      paths:
      - path: /analytics
        backend:
          serviceName: myapp-analytics-service
          servicePort: 8080
      - path: /shopping
        backend:
          serviceName: myapp-shopping-service
          servicePort: 8080
---
apiVersion: v1
kind: Secret
metadata:
  name: myapp-sercret-tls
data:
  tls.crt: base64 encoded cert
  tls.key: base64 encoded key
type: kubernetes.io/tls

The Secret must be located in the same namespace of the ingress.


Volumes - Persisting Application Data
-----------------------------------------------------

How to perist data in Kubernetes using volumes ? 

* Peristent Volume 
* Persistent Volume Claim
* Storage Class

As pod are ephemeral , the data will be lost if pod restart.

Storage must be available on all nodes as you don't know where the pod will restart.

Storage needs to survive even if the cluster crashes

A persistent volume is a cluster resource that can be created via YAMl file 

kind: PersistentVolume

The storage is not managed by Kubernetes Cluster , you need to provide it to the cluster.

apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-name
spec:
  capacity:
    storage: 5 Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce
  persistentVolumeClaimPolicy: Recycle
  storageClass: slow
  mountOptions:
    - hard
    - nfsvers-4.8
  nfs:
    path: /dir/path/on/nfs/server
    server: nfs-server-ip-address-or-fqdn

Depending on storage type , the spec attributes will differ.

Persistent Volumes are not namespaced resources 

Local versus Remote Volume Types 

Local volume types violate 2 and 3 requirement for data persistence :

* Being tied to 1 specific node
* Surviving cluster crashes

Who creates the persistent volumes ?

PV are resources that need to be there BEFORE the pod that depends on it is created.

Application ask Peristent Volumes via Persistent Volume Claims resources

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-name
spec:
  storageClassName: manual
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10 Gi

Use that PVC in pods configuration

apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: myfrontend
      image: nginx
      volumeMounts:
      - mountPath: "/var/www/html"
        name: mypd
  volumes:
    - name: mypd
      persistentVolumeClaim:
        claimName: pvc-name

Levels of Volume abstractions

Pod requests the volume through the PV claim
Claim tries to find a volume in cluster
Volume has the actual storage backend

The PVC must be in the same namespace of the pod resource

Volumes are mounted to the pods and after mounted to the specifics containers inside the pods 

ConfigMap & Secret are volumes types 

These are not created via PV and PVC

Storage Class provisions persistent volumes dynamically when pvc claims it

Example of a Storage Class (AWS)

apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: storage-class-name
provisioner: kubernetes.io/aws-ebs
parameters:
  type: io1
  iosperGB: "10"
  fsType: ext4

There are internal provisioner and external provisioner


ConfigMap & Secret Volume Types 
----------------------------------------

Many configurations files are used by applications, to pass these configuration files to Kubernetes Pod that contain the application, we will need to use ConfigMap and Secret resources for confidential data.

apiVersion: v1
kind: ConfigMap
metadata:
  name: mongodb-configmap
data:
  db_host: mongodb-service
---
apiVersion: v1
kind: Secret
metadata:
  name: mongodb-secret
type: Opaque
data:
  username: <base64 encoded>
  password: <base64 encoded>
---


ConfigMap and Secret for mounting files 

* individual key value pairs 
* create files

apiVersion: v1
kind: ConfigMap
metadata:
  name: mosquitto-config-files
data:
  mosquitto.conf: |
    log_dest stdout
    log_type all
    log_timestamp true
    listener 9801


apiVersion: v1
kind: Secret
metadata:
  name: mosquitto-secret-file
type: Opaque
data:
  secret.file: |
    <base64 encoded string>












































