# Day3

Many ways applications can be deployed in OpenShift

1. Via YAML
2. Using Container Image
3. Using Dockerfile
4. Using application source

1. Using YAML file
   Assumptions:
   1. Container Image is ready
   2. Application build not required
   3. Image build not required

   OpenShift will deploy your application based on YAML declartive instructions

2. Using Container Image
   Assumptions:
    1. No application build is required
    2. No Docker Image build is required

   OpenShift will pull the Container Image and deploy the application into OpenShift

3. Using Dockerfile
   Assumption is, you application binary is already there, no application build is required

   OpenShift will follow the instructions in the Dockerfile you supplied from the GitHub/GitLab,etc
   OpenShift builds the custom image, injects your application binary into the image

   OpenShift will then deploy your application into OpenShift


4.Using application source - S2I (Source to Image)
  - GitHub/GitLab/version control

  - OpenShift will detect your programming language
  - Let's say your application is a Java application
  - OpenShift will make use of a Java Docker Image with Maven/Gradle
  - OpenShift will create a Pod (Build Machine)
  - Your application will be compiled within the Pod
  - Your application binary i.e jar/war/ear file will be created

  - Build a Custom Image for Java based application
  - Copy your application binary into the Custom Image

  - Using the custom image that has your application, it will deploy into OpenShift

## Creating a NodePort external service

#### Let's delete any existing deployment we have
```
oc delete deploy/nginx svc/nginx route/nginx
```

#### Let's create a new deployment
```
oc create deploy nginx --image=bitnami/nginx:latest --replicas=3
oc expose deploy/nginx --type=NodePort --port=8080
```

#### Let's access the NodePort service
```
curl master-1.ocp.tektutor.org:31505
curl master-2.ocp.tektutor.org:31505
curl master-3.ocp.tektutor.org:31505
curl worker-1.ocp.tektutor.org:31505
curl worker-2.ocp.tektutor.org:31505
```

Expected output
<pre>
(jegan@tektutor.org)$ curl 192.168.122.87:31505
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

## Viewing the etcd datastore records from OpenShift webconsole
You need to select master-1 or master-2 or master-3 etcd Pod and switch to the Terminal

List deployments in all namespaces/projects
```
ETCDCTL_API=3 etcdctl get "/kubernetes.io/deployments" --prefix --keys-only| sed '/^\s*$/d'
```

## Listing the deployment in your project
```
ETCDCTL_API=3 etcdctl get "/kubernetes.io/deployments/jegan" --prefix --keys-only| sed '/^\s*$/d'
```
In the above command, you need to replace 'jegan' with 'your-project-name'


## Listing all pods under your project
```
ETCDCTL_API=3 etcdctl get "/kubernetes.io/pods/jegan" --prefix --keys-only| sed '/^\s*$/d'
```

## Listing all services in your project
```
ETCDCTL_API=3 etcdctl get "/kubernetes.io/services/endpoints/jegan" --prefix --keys-only| sed '/^\s*$/d'
```

## Creating a LoadBalancer Service
```
oc create deploy nginx --image=bitnami/nginx --replicas=3
oc expose deploy/nginx --type=LoadBalancer --port=8080
```

You need to install MetalLB operator to enable the LoadBalancer service in bare-metal OpenShift setup
<pre>
https://medium.com/tektutor/using-metallb-loadbalancer-with-bare-metal-openshift-onprem-4230944bfa35
</pre>


## Creating a deployment in declarative style
```
oc create deploy nginx --image=bitnami/nginx:latest --replicas=3 --dry-run=client -o yaml > nginx-deploy.yml
```

#### Create nginx deployment using the above generated manifest(yaml) file
```
oc apply -f nginx-deploy.yml
```

## Creating a ClusterIP internal service in declarative sytle
```
oc delete svc/nginx
oc expose deploy/nginx --type=ClusterIP --port=8080 --dry-run=client -o yaml > nginx-clusterip-svc.yml
oc apply -f nginx-clusterip-svc.yml
```

## Creating a NodePort external service in declarative sytle
```
oc delete svc/nginx
oc expose deploy/nginx --type=NodePort --port=8080 --dry-run=client -o yaml > nginx-nodeport-svc.yml
oc apply -f nginx-nodeport-svc.yml
```

## Creating a LoadBalancer external service in declarative sytle
```
oc delete svc/nginx
oc expose deploy/nginx --type=LoadBalancer --port=8080 --dry-run=client -o yaml > nginx-lb-svc.yml
oc apply -f nginx-lb-svc.yml
```

## Creating replicaset declaratively

In case, you haven't cloned the repo so far, you may do now
```
cd ~
git clone https://github.com/tektutor/openshift-july-2022.git
```

In case, you have already cloned then you may pull delta changes
```
cd ~/openshift-july-2022
git pull

cd Day3/manifests
```

Delete previous nginx deploy(if any)
```
oc delete deploy/nginx
```

You can create replicaset declaratively now
```
cd Day3/manifests
oc get deploy,rs,po
oc apply -f nginx-rs.yml
oc get po
```

## Creating Pod in declarative style

Let's delete the replicaset declartively
```
cd ~/openshift-july-2022/Day3/manifests
oc delete -f nginx-rs.yml
```

Create the pod
```
oc apply -f nginx-pod.yml
```

## Performing rolling update
Let's delete any existing deployments that we have
```
cd ~/openshift-july-2022
git pull
cd Day3/manifests

oc delete -f nginx-deploy.yml
```

Let's edit the nginx-deploy.yml and update the bitnami/nginx:latest to bitnami/nginx:1.21
```
cd ~/openshift-july-2022
cd Day3/manifests

oc apply -f nginx-deploy.yml
```

You may verify the version of container in the deployment
```
oc edit deploy/nginx
oc rollout status deploy/nginx
oc rollout history deploy/nginx

```
The expectation is the image version should be bitnami/nginx:1.21


Let's edit the nginx-deploy.yml and update the bitnami/nginx:1.21 to bitnami/nginx:1.22
```
cd ~/openshift-july-2022
cd Day3/manifests

oc apply -f nginx-deploy.yml
```

You may verify the version of container in the deployment
```
oc edit deploy/nginx
oc rollout status deploy/nginx
oc rollout history deploy/nginx
```
The expectation is the image version should be bitnami/nginx:1.22


Let's edit the nginx-deploy.yml and update the bitnami/nginx:1.22 to bitnami/nginx:1.23
```
cd ~/openshift-july-2022
cd Day3/manifests

oc apply -f nginx-deploy.yml
```

You may verify the version of container in the deployment
```
oc edit deploy/nginx
```
The expectation is the image version should be bitnami/nginx:1.23
