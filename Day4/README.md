# Day4

## NFS Server
- can be installed on let's say some linux distribution ( Ubuntu, CentOS, RHEL, etc., )
- can export some shared folders
- Example
   - /mnt/shared-folder
   - we need to ensure that NFS Server machine has sufficient disk space to satisfy PVC

## What is Storage Class in OpenShift?
   - it is a way applications get external storage at runtime on demand
   - if your system adminstrator has created let's say NFS Storage Class
   - the storage can create sub-folders within /mnt/shared-folder with specific access-mode with required storage capacity

## Deploying a multi-pod application Wordpress with mysql database

- Any application that needs external storage volume, in OpenShift it can request the OpenShift cluster for storage
- Storage can be requested by Application by creating a PersistentVolumeClaim
- PersistenVolumeClaim (PVC)
  - should mention the access mode
    - ReadWriteOnce
         - All the Pods on the same node in the OpenShift cluster can access this storage
    - ReadWriteMany
         - All the Pods on all nodes in the OpenShift cluster can access this storage
  - storage capacity
      - either in Mi(MB) or Gi(GB)
  - Optionally you may also mention Storage Class
    - AWS S3
    - AWS EBS
    - Azure EBS
    - NFS
- PersistentVolume (PV)
  - System Administrators can create PersistentVolume within OpenShift
    - with different storage capacity like 500Mi, 1Gi, etc.,
    - with different access mode ( ReadWriteOnce, ReadWriteMany, etc.,)
    - optionally, it could be of some StorageClass (AWS EBS, NFS, etc.,)
  - System Administrators can also some StorageClasses within OpenShift
    - Storage class automatically provisions the type of storage requested by the applications
  
 ## Deploying wordpress and mysql with Persistent Volume and Persistent Volume Claim
 
 #### Cloning the repository ( if you haven't done already )
 ```
 cd ~
 git clone https://github.com/tektutor/openshift-july-2022.git
 ```
 
 #### If you already cloned, then do this
 ```
 cd ~
 cd openshift-july-2022
 git pull
 ```
 
#### Deploying mysql using manifest files

Before you start the deployment, 
 - Update the mysql-pv.yml and replace 'jegan' with 'your-project-name'
 - Update the mysql-pvc.yml and replace 'jegan' with 'your-project-name'

```
cd ~/openshift-july-2022/Day4/wordpress
 
oc apply -f mysql-pv.yml
oc apply -f mysql-pvc.yml
oc apply -f mysql-deployment.yml
oc apply -f mysql-service.yml
```
 
#### Deploying wordpress using manifest files
 
Before you start the deployment, 
 - Update the wordpress-pv.yml and replace 'jegan' with 'your-project-name'
 - Update the wordpress-pvc.yml and replace 'jegan' with 'your-project-name'
 - Update wordpress-deployment.yml and replace 'jegan' with 'your-project-name'

```
cd ~/openshift-july-2022/Day4/wordpress
 
oc apply -f wordpress-pv.yml
oc apply -f wordpress-pvc.yml
oc apply -f wordpress-deployment.yml
oc apply -f wordpress-service.yml
 
oc expose svc/wordpress
```

From the OpenShift webconsole => Developer view => Topology navigate to the wordpress route for accessing wordpress blog.

## Deployment vs DeploymentConfig
- In older version of Kubernetes there was no Deployment and Replicaset.  Instead only, ReplicationController was there.
- ReplicationController was supported Scaling up/down and Rolling update in older version of K8s
- At the time, OpenShift introducted DeploymentConfig

- Latest version of Kubernetes, they refactored ReplicationController into two Resources
  1. Deployment
     - managed by DeploymentController
     - supports Rolling Update
  2. ReplicaSet
     - managed by ReplicaSetController
     - supports Scale up/down

- As OpenShift is developed on top of Kubernetes, in newer versions of OpenShift both Deployment and DeployConfig are supported
- For new deployments, see if you can Deployment as opposed to DeploymentConfig

## Ingress

#### Overview
- it is a set of routing(redirection) rules
- the Ingress that we create are picked by Ingress Controller in OpenShift dynamically and configures the HAProxy
  Load Balancer to redirect the calls to different service backends
- in the absence of Ingress Controller and Load Balancer(HAProxy), the Ingress rules will not work
- OpenShift supports Ingress Controller and HAProxy by default

```
cd ~/openshift-july-2022/Day4/ingress

oc apply -f ingress.yml
```
Expected output
<pre>
(jegan@tektutor.org)$ <b>oc apply -f ingress.yml</b>
ingress.networking.k8s.io/tektutor created
</pre>


#### You may try listing the ingress
```
oc get ingress
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc get ingress</b>
NAME       CLASS    HOSTS                            ADDRESS   PORTS   AGE
tektutor   <none>   tektutor.apps.ocp.tektutor.org             80      32s
</pre>


#### You may try describing the ingress
```
oc describe ingress/tektutor
```
Expected output
<pre>
(jegan@tektutor.org)$ <b>oc describe ingress/tektutor</b>
Name:             tektutor
Labels:           <none>
Namespace:        jegan
Address:          
Default backend:  default-http-backend:80 (<error: endpoints "default-http-backend" not found>)
Rules:
  Host                            Path  Backends
  ----                            ----  --------
  tektutor.apps.ocp.tektutor.org  
                                  /wordpress   wordpress:8080 (Pod endpoints)
                                  /dotnet      devfile-sample-dotnet-60-basic-git:8081 (Pod endpoints)
                                  /python      django-psql-example:8080 (Pod endpoints)
                                  /nginx       nginx:8080 (Pod endpoints)
Annotations:                      haproxy.router.openshift.io/rewrite-target: /
Events:                           <none>
</pre>


#### Testing the ingress

Use the below url from your browser
```
tektutor.apps.ocp.tektutor.org
```

Below url will redirect the call to wordpress service
```
tektutor.apps.ocp.tektutor.org/wordress
```

Below url will redirect the call to dotnet service(.net 6 based application)
```
tektutor.apps.ocp.tektutor.org/dotnet
```

Below url will redirect the call to python service(django with postgres application)
```
tektutor.apps.ocp.tektutor.org/python
```

Below url will redirect the call to nginx service(nginx)
```
tektutor.apps.ocp.tektutor.org/nginx
```
