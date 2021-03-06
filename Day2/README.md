# OpenShift

## Containers Technology
- Linux Technology
- Linux Kernel Feature
  1. Namespace ( helps isolating one container from other containers )
  2. Control Group (CGroup) - helps in appling resource quota restricts
- Windows Core ( Windows Kernel)

## Container  
- LXC
- Docker
- Podman
- containerd
- Rkt

## Container Engine
- Docker, Podman are Container Engines
- is a high-level tool that depends on Container Runtime to manage containers
- also depends on other tools to manage Container Images
- end-user uses Container Engines not the Container Runtime directly
- Container Engines provide user-friendly commands that  abstracts complex low-level container/image management stuffs
- end-user doesn't have to deal with low-level stuffs, as Container Engines takes care of low-level stuffs internally with the help of Container Engines

## Container Runtime
- example: there is a Container runtime called runC
- runC is used by Docker

## Container Orchestration Platform
- it is used to manage containers

- high-level features supported by Orchestration Platform
  1. Provides a Platform that can ensure HA for your applications
  2. End-user applications(microservices) can be deployed within Orchestration Platform
  3. The applications deployed can be monitored by Orchestration Platforms
     - in case your application isn't responding it will be repaired
  4. The application running in production env can be upgraded without any down-time
  5. Applications instances can be scaled up/down based on user traffic on demand
  6. supports creating services
     1. Internal Services
     2. External Services
  7. Load Balancing
  
- Orchestration Platform Example
  - Docker Swarm 
      - Docker native Orchestration Platform that supports only Docker containers
      - generally not used in Production
      - very commonly used in Dev/QA environments for testing
      - very lightweight
      - setup is easy
      - learning is easy
  - Google Kubernetes
      - supports managing many types of Containers 
      - Any Container Runtime that supports Kubernetes CRI(Container Runtime Interface) is supported by Kubernetes
      - Open source
      - time tested
      - production grade Orchestration Tool
      - mostly accessed via Command Line Interface
      - very robust
      - supports adding new resources typically referred as Custom Resource by defining CRD(Custom Resource Definintion)
      - Custom Resources are referred as CR in short
      - Custom Resource Definintions are referred as CRDs
      - You can create new own CR by writing CRD(YAML)
      - Every Resource in Kubernetes is managed by specific Controllers
      - When you add your own CR, you need to deply Custom Controllers to manage your CR
 
  - RedHat OpenShift ( Enterprise Grade Orchestration Platform requires license )
      - RedHat's distribution of Kubernetes 
      - OpenShift is developed on top of Google Kubernetes with many additional features
      - it supports private container registry out of the box
      - it integrates CI/CD
      - RedHat OpenShift => Google Kubernetess + Many Custom Resources + Many Custom Controllers

## Docker Container Engine
- every container get's a Private IP

## Kubernetes Built-in Resources
1. Deployment ( JSON/YAML Configuration - stored in etcd datastore )
   - represents an application deployed within Openshift
   - manages ReplicaSet
   - desired count
      - no of application pods you wish to run within OpenShift
3. ReplicaSet ( JSON/YAML Configuration - stored in etcd datastore )
   - manages Pod(s)
   - desired count - 3 
   - actual count  - 0
   - ready count
4. Pod ( JSON/YAML Configuration - stored in etcd datastore )
    - smallest unit that is deployable within Kubernetes/OpenShift
    - every Pod get's a Private IP
    - group of related containers 
    - applications run as container within Pod

## OpenShift client tool
- kubectl (REST client) - Kubernetes client also works in OpenShift
- oc (REST client) - OpenShift's exclusive client tool

## RedHat OpenShift Architecture
- self-healing Orchestration Platform
- heals user-defined application Pods
- Cluster
    - group of OpenShift nodes
    - Nodes are Physical Servers or Virtual Machines or AWS/AZure ec2 instances
- Nodes
    1. Master Node
    2. Worker Node
- Master Node
  - Control Plane Components
    1. API Server (Pod)
    2. etcd key/value pair datastore (Pod)
    3. Scheduler (Pod)
    4. Controller Managers (Pod)
       - Deployment Controller
       - ReplicaSet Controller
       - Replication Controller
       - EndPoint Controller
       - Job Controller
       - DaemonSet Controller
       - StatefulSet Controller

  - CRI-O Container Runtime
  - Podman Container Engine
  - kubelet
  - kube-proxy
  - kubeadm
 - Worker Node
   - CRI-O Container Runtime
   - Podman Container Engine
   - kubelet
   - kube-proxy
   - kubeadm

## API Server
   - implements OpenShift Orchestration functionalities as REST API
   - API Server is the only component that communicates with etcd database
   - API Server triggers events whenever some database records are updated/deleted/created in etcd database
   - All the OpenShift components communicate only with API Server
   - OpenShift components won't talk to each other directly 

## Etcd datastore
- this is where the OpenShift cluster state and application status are stored
- Deployment, ReplicaSet, Pod, Service, Job, Daemonset, StatefulSet, etc are stored within this database
- API Server is the only component that is allowed to store/retrieve data from this database

## Scheduler
- this component is reponsible to identify a healthy node where Pod(application instance) can be deployed
- keeps looking for Added, Modified, Deleted Pod events
- the scheduling recommendation will sent to API Server via REST call

## Controller Managers(s)
- Deployment Controller
   - Takes Deployment as an Input
   - It Creates ReplicaSet under the Deployment
   - Deployment manages the Deployment

- ReplicaSet Controller
  - accepts ReplicaSet as Input
  - It creates the desired number of Pods

## kubelet
- OpenShift Container Agent
- this runs on every node ie. Master and Worker Node
- kubelet is the component which communicates with CRI-O Container Runtime
- kubelet creates the Pods on the node where it runs
- kubelet receives events from API Server to create, delete, update Pods
- kubelet once the Pod is created, it constantly monitors and reports the status of Pods to API Server as heart-beat REST calls

![OpenShift Architecture](OpenShiftArchitecture.png)
![OpenShift Architecture](RedHatOpenShiftArchitecture.png)
![OpenShift Architecture](master-node.png)


## What is a Custom Resource in Kubernetes/OpenShift?

## What are Controllers in Kubernetes/OpenShift?
- application that monitors the health of application Pods
- Controllers ensure a desired number of application Pods are always running
- When Controllers notices that a particular Pod instance is not responding, Controllers will replace
  the bad non-responding application instance(Pod) with a new application instance(Pod)
- Controllers also take care of scaling up/down 
   - When user-traffic increase, they can help add more Pods to load-balance and serve the end users better
   - self-healing of itself and user application


# OpenShift Commands

## Create your first project
```
oc new-project jegan
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc new-project jegan</b>
Now using project "jegan" on server "https://api.ocp.tektutor.org:6443".

You can add applications to this project with the 'new-app' command. For example, try:

    oc new-app rails-postgresql-example

to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:

    kubectl create deployment hello-node --image=k8s.gcr.io/e2e-test-images/agnhost:2.33 -- /agnhost serve-hostname
</pre>

## Finding the currently active project
```
oc project
```
Expected output
<pre>
(jegan@tektutor.org)$ <b>oc project</b>
Using project "jegan" on server "https://api.ocp.tektutor.org:6443".
</pre>

## Listing projects
```
oc get projects
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc get projects</b>
NAME                                               DISPLAY NAME   STATUS
default                                                           Active
<b>jegan                                                             Active</b>
kube-node-lease                                                   Active
kube-public                                                       Active
kube-system                                                       Active
openshift                                                         Active
openshift-apiserver                                               Active
openshift-apiserver-operator                                      Active
openshift-authentication                                          Active
openshift-authentication-operator                                 Active
openshift-cloud-controller-manager                                Active
openshift-cloud-controller-manager-operator                       Active
openshift-cloud-credential-operator                               Active
openshift-cloud-network-config-controller                         Active
openshift-cluster-csi-drivers                                     Active
openshift-cluster-machine-approver                                Active
openshift-cluster-node-tuning-operator                            Active
openshift-cluster-samples-operator                                Active
openshift-cluster-storage-operator                                Active
openshift-cluster-version                                         Active
openshift-config                                                  Active
openshift-config-managed                                          Active
openshift-config-operator                                         Active
openshift-console                                                 Active
openshift-console-operator                                        Active
openshift-console-user-settings                                   Active
openshift-controller-manager                                      Active
openshift-controller-manager-operator                             Active
openshift-dns                                                     Active
openshift-dns-operator                                            Active
openshift-etcd                                                    Active
openshift-etcd-operator                                           Active
openshift-host-network                                            Active
openshift-image-registry                                          Active
openshift-infra                                                   Active
openshift-ingress                                                 Active
openshift-ingress-canary                                          Active
openshift-ingress-operator                                        Active
openshift-insights                                                Active
openshift-kni-infra                                               Active
openshift-kube-apiserver                                          Active
openshift-kube-apiserver-operator                                 Active
openshift-kube-controller-manager                                 Active
openshift-kube-controller-manager-operator                        Active
openshift-kube-scheduler                                          Active
openshift-kube-scheduler-operator                                 Active
openshift-kube-storage-version-migrator                           Active
openshift-kube-storage-version-migrator-operator                  Active
openshift-machine-api                                             Active
openshift-machine-config-operator                                 Active
openshift-marketplace                                             Active
openshift-monitoring                                              Active
openshift-multus                                                  Active
openshift-network-diagnostics                                     Active
openshift-network-operator                                        Active
openshift-node                                                    Active
openshift-oauth-apiserver                                         Active
openshift-openstack-infra                                         Active
openshift-operator-lifecycle-manager                              Active
openshift-operators                                               Active
openshift-ovirt-infra                                             Active
openshift-sdn                                                     Active
openshift-service-ca                                              Active
openshift-service-ca-operator                                     Active
openshift-user-workload-monitoring                                Active
openshift-vsphere-infra                                           Active
</pre>

## Switching to a project
```
oc project jegan
```
Expected output
<pre>
(jegan@tektutor.org)$ <b>oc project jegan</b>
Already on project "jegan" on server "https://api.ocp.tektutor.org:6443".
(jegan@tektutor.org)$ <b>oc project default</b>
Now using project "default" on server "https://api.ocp.tektutor.org:6443".
(jegan@tektutor.org)$ <b>oc project jegan</b>
Now using project "jegan" on server "https://api.ocp.tektutor.org:6443".
</pre>

## Deleting a project
The below command will delete all the deployments, services, etc within the project, including the project.
```
oc delete project jegan
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc delete project jegan</b>
project.project.openshift.io "jegan" deleted
</pre>


## Create your first deployment in RedHat OpenShift
```
oc create deployment nginx --image=nginx:latest --replicas=2
```
In the above command
<pre>
oc - is the openshift client tool
create - is used create different types of OpenShift resources
deployment - resource type
nginx - name of our deployment, user-defined
image=nginx:latest - represents which container image it should to deploy the application Pods
replicas - indicates how many Pods should be created using nginx:latest container image
</pre>

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc create deployment nginx --image=nginx:latest --replicas=2</b>
deployment.apps/nginx created
</pre>

## Listing the application deployments
```
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc get deployments</b>
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   0/3     3            0           11s
(jegan@tektutor.org)$ <b>oc get deployment</b>
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   0/3     3            0           7s
(jegan@tektutor.org)$ <b>oc get deploy</b>
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   0/3     3            0           13s
</pre>

## Listing replicasets
```
oc get replicasets
oc get replicaset
oc get rs
```

Expected ouptut
<pre>
(jegan@tektutor.org)$ <b>oc get replicasets</b>
NAME               DESIRED   CURRENT   READY   AGE
nginx-7c658794b9   3         3         0       3m31s
(jegan@tektutor.org)$ <b>oc get replicaset</b>
NAME               DESIRED   CURRENT   READY   AGE
nginx-7c658794b9   3         3         0       3m36s
(jegan@tektutor.org)$ <b>oc get rs</b>
NAME               DESIRED   CURRENT   READY   AGE
nginx-7c658794b9   3         3         0       3m37s
</pre>


## Deleting a deployment
```
oc delete deploy/nginx
```

<pre>
(jegan@tektutor.org)$ <b>oc delete deploy/nginx</b>
deployment.apps "nginx" deleted
</pre>

## Creating nginx deployment using bitnami image
```
oc create deployment nginx --image=bitnami/nginx:latest --replicas=3
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc create deployment nginx --image=bitnami/nginx:latest --replicas=3</b>
deployment.apps/nginx created
</pre>

## Listing deployments, replicaset and pods all at once
```
oc get deploy,rs,po
```

Expected output
<pre>
jegan@tektutor.org)$ <b>oc get deploy,rs,po</b>
NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   3/3     3            3           72s

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-78644964b4   3         3         3       72s

NAME                         READY   STATUS    RESTARTS   AGE
pod/nginx-78644964b4-dtvkq   1/1     Running   0          72s
pod/nginx-78644964b4-gbm4p   1/1     Running   0          72s
pod/nginx-78644964b4-w48ks   1/1     Running   0          72s
</pre>

## Port-forward a Pod to access its web page
```
oc port-forward pod/nginx-78644964b4-n95ww 9000:8080
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc port-forward pod/nginx-78644964b4-n95ww 9000:8080</b>
Forwarding from 127.0.0.1:9000 -> 8080
Forwarding from [::1]:9000 -> 8080
Handling connection for 9000
</pre>

## Creating ClusterIP Internal Service
```
oc expose deploy/nginx --type=ClusterIP --port=8080
oc get services
oc get service
oc get svc
oc describe svc/nginx
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc expose deploy/nginx --type=ClusterIP --port=8080</b>
service/nginx exposed
(jegan@tektutor.org)$ <b>oc get services</b>
NAME    TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
nginx   ClusterIP   172.30.14.93   <none>        8080/TCP   7s
(jegan@tektutor.org)$ <b>oc get service</b>
NAME    TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
nginx   ClusterIP   172.30.14.93   <none>        8080/TCP   11s
(jegan@tektutor.org)$ <b>oc get svc</b>
NAME    TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
nginx   ClusterIP   172.30.14.93   <none>        8080/TCP   13s
(jegan@tektutor.org)$ <b>oc describe svc/nginx</b>
Name:              nginx
Namespace:         jegan
Labels:            app=nginx
Annotations:       <none>
Selector:          app=nginx
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                172.30.14.93
IPs:               172.30.14.93
Port:              <unset>  8080/TCP
TargetPort:        8080/TCP
Endpoints:         10.128.0.76:8080,10.128.0.77:8080,10.128.0.78:8080 + 17 more...
Session Affinity:  None
Events:            <none>
</pre>

## Getting inside a Pod shell given a deployment name
```
oc rsh deploy/nginx
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc rsh deploy/nginx</b>
$ ls
50x.html  index.html
$ curl 172.30.14.93:8080
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
</pre>
