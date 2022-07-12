# Day1

## What is Hypervisor?
- aka virtualization technology
- Virtualization Technology
   - is a combination of Hardware and software solution
  Intel
    - x86 (32 bit processors)
    - x86 ( set of CPU 32-bit CPU Instructions - invented by Intel )
    - x64 64-bit CPU Instructions is cross-licensed by Intel from AMD
    - Virtualization Feature is called VT-X
    
  AMD
    - x86 - Intel's 32 bit CPU Instructions are cross-licensed by AMD
    - x64 (64-bit processors)
    - x64 ( set of 64-bit CPU Instructions invented by AMD )
    - Virtualization Feature is called AMD-V
 - every processor that you find in market, pretty much supports Hypervisors
 
 - Your Laptop/Desktop/Workstation/Server BIOS should also support Virtualization
 - Make sure your Laptop BIOS settings shows up the Virtualization Feature Enabled
 - Virtualization Softwares
   - there are 2 types
     1. Type1 
        - meant for Workstations & Servers
        - aka bare-metal hypervisors
        - they don't need an OS to install this type of Hypervisor
     2. Type2
        - meant to be used in Laptops/Desktops/Workstations
        - it requires Host OS ( Windows, Mac OS-X, Linux )
        - Type2 can only be installed on top of a Host OS
        - 
    - VMWare
     - VMWare Wokstation ( Linux, Mac OS-X & Windows ) - Type 2 - Hypervisor
     - VMWare Player ( Windows ) - Type 2 Hypervisor
     - VMWare vSphere/v-center ( Type 1 - Hypervisor )
     - VMWare Fusion ( Mac OS-X) - Type 2 
   - Microsoft
     - Hyper-v
   - Oracle
     - VirtualBox( Free for Personal & Commercial use )
   - Parallels ( Mac OS-X)
 
 ## Virtual Machine Overview?
 - aka Guest OS
 - each VM get's its own dedicated
    - CPU Cores
    - RAM
    - Storage
    - Virtual Network Card(s)
    - Virtual Graphics Card(s)
 - each VM has a fully functional Operating System
 - has dedicated Kernel
 
 ## Operating System within VM
 - it has dedicated hardware resources
 - it has dedicated Network/Graphics Card
 - has one or more application processes
 - has its own OS Kernel
 - it supports command prompt in case of Windows, shell prompts ( bash, sh in case of Unix/Linux)

## What is a Linux namespace?
- its an isolated virtual sandbox environment
- every container runs in its namespace
- every container has its own port namespace
- every container has its network namespace i.e a dedicated network stack (7 OSI Layers)

 ## Container Technology
 - is a software technology
 - it is an application virtualization technology
 - containers are nothing but application process 
 - containers are not Operating System
 - containers shares the hardware resources available on the Host machine where containers are running
 - each container - runs one single application
 
 ## Container Image
 - a blueprint of a container
 - from developer point of view, Container Image is like a user-defined Class
 - from a single container image, many containers can be created
 - Example
    - ubuntu:16.04 
    - centos:8
 - the above images just have some shell like bash, sh, etc
 - it comes file system ( files and folders )
 - it comes with package manager ( apt-get/apt, yum, etc., )
 - it has no Kernel or OS
 - containers runs on top of some Operating System
 - all containers that runs on the same system shares hardwares and os Kernel
 
 ## Containers
 - is an instance of Container Image
 - from developer point of view, its an Object of some class
 
## Docker Overview
- is a container software from Docker Inc 
- is developed in Go programming language
- comes in 2 flavors
  1. Community Edition (CE) - opensource(Free)
  2. Enterprise Edition (EE) - Requires License/Commercial use
- follows client/server architecture
- client tool (docker)
- server tool (dockerd - application container engine/docker daemon)

## Docker Registry
- has many Docker Images
- It is thru images one can create containers
- Three types
  1. Local Registry ( local folder where Docker Server is running )
  2. Private Registry ( Setup using Sonatype Nexus or JFrog Artifactory )
  3. Remote Registry ( Docker Hub Website maintained by Docker Inc )

## JFrog Artifactory
- this server can be used multiple ways using its web interface
- can be used like a modern FTP Server that hosts application binaries
- application binaries can be download/uploaded manually/programatically
- also servers a repository server
  - it can be setup within your organization as an Ubuntu apt repository server ( application installer )
  - it can be setup within your organization as a CentOS/RHEL yum repository server
- as Private Container Registry
  - you can host your Propriertary Container Images
  - Your Private Images will have your application and its dependent libraries/fraweworks, etc

# Ansible
- comes in two flavors
  1. Ansible Core (Command line interface only ) - OpenSource
  2. Ansible Tower ( RedHat Enterprise Product built on top of Ansible Core)
- configuration management tool like Puppet, Chef, Salt/SaltStack
- developed in Python by Ansible Inc organization
- it is used by DevOps Engineers to automate software installation, configuration management, etc.,
- use may provision containers locally,  provision aws/azure ec2 instances
- the script that write is called Ansible Playbook ( YAML file )

## Ansible Playbook
- can build a Custom Docker Image using Dockerfile you supply
- can provision a container with your Custom Image with basic tools pre-installed
- can install, configure the container post the container is provisioned
- can be used to provision a container locally or on cloud
- can be used to provision an ec2 instance

## Custom Docker Image
- uses some exiting Docker Image as a base Image
- customize by installing additional software tools that you need for your specific applicaiton build/test/etc
-

# Example
- let's say you have a spring-boot java application
- to compile this spring java application
  - you need a build system( VM/Container/Ec2 Instance )
  - the build machine must have the below 
    - Java Development Kit
    - Ant/Maven/Gradle Build tool is required
    - your application source code

## CI/CD DevOps Pipeline
- when Java/C# application source code is committed into GitHub or similar version control
- Jenkins Job can be setup to monitor your application code repository within GitHub
- Jenkins Job constantly monitors for code changes committed in a particular code repo
- When Jenkins Job detects code changes, it can trigger an Ansible Playbook to provision the build machine 
- the newly configured build system will be used to perform the application build
- As part of application build, it compilers, perform automated tests, package and the binaries will be
  deployed into JFrog Artifactory
- As part of the CI/CD pipeline, it is also possible to create custom Docker images from some base Docker Image, copying
  your application binaries into it along with its dependencies(lib)
- From the JFrog custom Docker Image you can deploy your containerized applications
  
# Docker Commands

## Docker High Level Architecture

![Docker Architecture](DockerHighLevelArchitectureUpdated.png)

![Docker Image Architecture](DockerLayers.png)

## Checking docker version
```
docker --version
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>docker --version</b>
Docker version 20.10.7, build 20.10.7-0ubuntu5~18.04.3
</pre>

## Listing Docker Images in your Local Docker Registry
```
docker images
```

Expected output
<pre>
jegan@tektutor.org)$ docker images
REPOSITORY                                           TAG                           IMAGE ID       CREATED         SIZE
</pre>

## Downloading an image from Docker Hub to Local Docker Registry
```
docker pull ubuntu:22.04
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>docker pull ubuntu:22.04</b>
22.04: Pulling from library/ubuntu
Digest: sha256:b6b83d3c331794420340093eb706a6f152d9c1fa51b262d9bf34594887c2c7ac
Status: Downloaded newer image for ubuntu:22.04
docker.io/library/ubuntu:22.04
</pre>

Listing the images once again to see the downloaded images
```
docker images
```


## Listing docker images from Docker Hub
```
docker search centos
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>docker search centos</b>
NAME                                         DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
centos                                       The official build of CentOS.                   7231      [OK]       
kasmweb/centos-7-desktop                     CentOS 7 desktop for Kasm Workspaces            21                   
continuumio/centos5_gcc5_base                                                                3                    
dokken/centos-7                              CentOS 7 image for kitchen-dokken               2                    
dokken/centos-stream-9                                                                       1                    
couchbase/centos7-systemd                    centos7-systemd images with additional debug…   1                    [OK]
spack/centos7                                CentOS 7 with Spack preinstalled                1                    
dokken/centos-stream-8                                                                       0                    
dokken/centos-6                              CentOS 6 image for kitchen-dokken               0                    
dokken/centos-8                              CentOS 8 image for kitchen-dokken               0                    
spack/centos6                                CentOS 6 with Spack preinstalled                0                    
datadog/centos-i386                                                                          0                    
bitnami/centos-extras-base                                                                   0                    
corpusops/centos                             centos corpusops baseimage                      0                    
couchbase/centos-72-java-sdk                                                                 0                    
couchbase/centos-72-jenkins-core                                                             0                    
bitnami/centos-base-buildpack                Centos base compilation image                   0                    [OK]
couchbase/centos-69-sdk-nodevtoolset-build                                                   0                    
fnndsc/centos-python3                        Source for a slim Centos-based Python3 image…   0                    [OK]
couchbase/centos-69-sdk-build                                                                0                    
couchbase/centos-70-sdk-build                                                                0                    
spack/centos-stream                                                                          0                    
galaxy/centos-wheel                                                                          0                    
galaxy/centos32-wheel                                                                        0                    
galaxy/centos32                                                                              0                    
</pre>

## Creating your first container
```
docker run hello-world:latest
```

<pre>
(jegan@tektutor.org)$ <b>docker run hello-world:latest</b>

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
 </pre>


## Create ubuntu containers in background/deattached/daemon mode
```
docker run -dit --name ubuntu1 --hostname ubuntu1 ubuntu:16.04 /bin/bash
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>docker run -dit --name ubuntu1 --hostname ubuntu1 ubuntu:16.04 /bin/bash</b>
9d6cfd1926872920dbade1b47ab7a6aab4f69986b997ced6e912824a39d5682c
(jegan@tektutor.org)$ docker ps
CONTAINER ID   IMAGE          COMMAND       CREATED         STATUS         PORTS     NAMES
9d6cfd192687   ubuntu:16.04   "/bin/bash"   5 seconds ago   Up 5 seconds             ubuntu1
</pre>

## Listing only running containers
```
docker ps
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>docker ps</b>
CONTAINER ID   IMAGE          COMMAND       CREATED         STATUS         PORTS     NAMES
9d6cfd192687   ubuntu:16.04   "/bin/bash"   5 seconds ago   Up 5 seconds             ubuntu1
</pre>

## Listing all containers irrespective of their running state
```
docker stop ubuntu1
docker ps
docker ps -a
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>docker stop ubuntu1</b>
ubuntu1
(jegan@tektutor.org)$ <b>docker ps</b>
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
(jegan@tektutor.org)$ <b>docker ps -a</b>
CONTAINER ID   IMAGE                COMMAND       CREATED          STATUS                      PORTS     NAMES
9d6cfd192687   ubuntu:16.04         "/bin/bash"   11 minutes ago   Exited (0) 7 seconds ago              ubuntu1
</pre>

## Creating a nginx web server in foreground/interactive mode
```
docker run nginx:latest
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>docker run nginx:latest</b>
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2022/07/11 09:51:19 [notice] 1#1: using the "epoll" event method
2022/07/11 09:51:19 [notice] 1#1: nginx/1.21.6
2022/07/11 09:51:19 [notice] 1#1: built by gcc 10.2.1 20210110 (Debian 10.2.1-6) 
2022/07/11 09:51:19 [notice] 1#1: OS: Linux 5.4.0-120-generic
2022/07/11 09:51:19 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2022/07/11 09:51:19 [notice] 1#1: start worker processes
2022/07/11 09:51:19 [notice] 1#1: start worker process 31
2022/07/11 09:51:19 [notice] 1#1: start worker process 32
2022/07/11 09:51:19 [notice] 1#1: start worker process 33
2022/07/11 09:51:19 [notice] 1#1: start worker process 34
2022/07/11 09:51:19 [notice] 1#1: start worker process 35
2022/07/11 09:51:19 [notice] 1#1: start worker process 36
2022/07/11 09:51:19 [notice] 1#1: start worker process 37
2022/07/11 09:51:19 [notice] 1#1: start worker process 38
2022/07/11 09:51:19 [notice] 1#1: start worker process 39
2022/07/11 09:51:19 [notice] 1#1: start worker process 40
2022/07/11 09:51:19 [notice] 1#1: start worker process 41
2022/07/11 09:51:19 [notice] 1#1: start worker process 42
2022/07/11 09:51:19 [notice] 1#1: start worker process 43
2022/07/11 09:51:19 [notice] 1#1: start worker process 44
2022/07/11 09:51:19 [notice] 1#1: start worker process 45
2022/07/11 09:51:19 [notice] 1#1: start worker process 46
2022/07/11 09:51:19 [notice] 1#1: start worker process 47
2022/07/11 09:51:19 [notice] 1#1: start worker process 48
2022/07/11 09:51:19 [notice] 1#1: start worker process 49
2022/07/11 09:51:19 [notice] 1#1: start worker process 50
2022/07/11 09:51:19 [notice] 1#1: start worker process 51
2022/07/11 09:51:19 [notice] 1#1: start worker process 52
2022/07/11 09:51:19 [notice] 1#1: start worker process 53
2022/07/11 09:51:19 [notice] 1#1: start worker process 54
2022/07/11 09:51:19 [notice] 1#1: start worker process 55
2022/07/11 09:51:19 [notice] 1#1: start worker process 56
2022/07/11 09:51:19 [notice] 1#1: start worker process 57
2022/07/11 09:51:19 [notice] 1#1: start worker process 58
2022/07/11 09:51:19 [notice] 1#1: start worker process 59
2022/07/11 09:51:19 [notice] 1#1: start worker process 60
2022/07/11 09:51:19 [notice] 1#1: start worker process 61
2022/07/11 09:51:19 [notice] 1#1: start worker process 62
2022/07/11 09:51:19 [notice] 1#1: start worker process 63
2022/07/11 09:51:19 [notice] 1#1: start worker process 64
2022/07/11 09:51:19 [notice] 1#1: start worker process 65
2022/07/11 09:51:19 [notice] 1#1: start worker process 66
2022/07/11 09:51:19 [notice] 1#1: start worker process 67
2022/07/11 09:51:19 [notice] 1#1: start worker process 68
2022/07/11 09:51:19 [notice] 1#1: start worker process 69
2022/07/11 09:51:19 [notice] 1#1: start worker process 70
2022/07/11 09:51:19 [notice] 1#1: start worker process 71
2022/07/11 09:51:19 [notice] 1#1: start worker process 72
2022/07/11 09:51:19 [notice] 1#1: start worker process 73
2022/07/11 09:51:19 [notice] 1#1: start worker process 74
2022/07/11 09:51:19 [notice] 1#1: start worker process 75
2022/07/11 09:51:19 [notice] 1#1: start worker process 76
2022/07/11 09:51:19 [notice] 1#1: start worker process 77
2022/07/11 09:51:19 [notice] 1#1: start worker process 78
2022/07/11 09:53:05 [notice] 1#1: signal 28 (SIGWINCH) received
2022/07/11 09:53:32 [notice] 1#1: signal 28 (SIGWINCH) received
172.17.0.1 - - [11/Jul/2022:09:55:34 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.58.0" "-"
172.17.0.1 - - [11/Jul/2022:09:55:48 +0000] "GET / HTTP/1.1" 200 615 "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36" "-"
2022/07/11 09:55:48 [error] 32#32: *2 open() "/usr/share/nginx/html/favicon.ico" failed (2: No such file or directory), client: 172.17.0.1, server: localhost, request: "GET /favicon.ico HTTP/1.1", host: "172.17.0.3", referrer: "http://172.17.0.3/"
172.17.0.1 - - [11/Jul/2022:09:55:48 +0000] "GET /favicon.ico HTTP/1.1" 404 555 "http://172.17.0.3/" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36" "-"
</pre>

To list the nginx container, you need to open another terminal tab as we launched nginx web server in interactive mode
```
docker ps
```

Find the IP address of the nginx container
```
docker inspect <nginx-container-id> | grep IPA
docker inspect <nginx-container-name> | grep IPA
```

Access the nginx web page
```
curl 172.17.0.3
```
In the above command, 172.17.0.3 is the nginx container IP, it might vary for you.


## Deleting a container that is not running
```
docker rm <container-id>
docker rm <container-name>
```

## Deleting a running container gracefully
```
docker stop <container-name>
docker rm <container-name>
```

## Deleting a running container forcibly
```
docker rm -f <container-name>
```

## Getting inside a running container
```
docker exec -it <container-name> /bin/bash
```
Some containers won't support bash, you may replace /bin/bash with /bin/sh in that case.

## Deleting multiple exited containers
```
docker rm <container-name-1> <container-name-2> <container-name-3>
```

## Deleting multiple exited containers without using their names
```
docker rm $(docker ps -aq)
```

## Deleting all containers in your system forcibly
```
docker rm -f $(docker ps -aq)
```

## Deleting all containers in your system gracefully
```
docker stop $(docker ps -q)
docker rm $(docker ps -aq)
```

## Renaming a container
```
docker rename <old-container-name> <new-container-name>
```

## Restarting a container

This might be useful, let's say you update some web server config file, etc.,
To apply config changes, you may have to reboot the container, in such cases this command comes in handy.

```
docker restart <container-name>
```

IP addresses of container are subject to change when they are either stop and started or restarted.

## Creating 3 nginx web servers in background mode
```
docker run -d --name web1 --hostname web1 nginx:latest
docker run -d --name web2 --hostname web2 nginx:latest
docker run -d --name web3 --hostname web3 nginx:latest
```

Finding the IP Addresses of web1, web2 and web3 respectively
```
docker inspect -f {{.NetworkSettings.IPAddress}} web1
docker inspect -f {{.NetworkSettings.IPAddress}} web2
docker inspect -f {{.NetworkSettings.IPAddress}} web3
```

Accessing web1, web2 and web3 container nginx pages
```
curl <web1-container-ip>
curl <web2-container-ip>
curl <web3-container-ip>
```

## Deleting an image from local docker registry
```
docker rmi hello-world:latest
```

## Volume Mounting - storging mysql db and table recorded in an external storage
```
mkdir -p /tmp/mysql
docker run -d --name db1 --hostname db1 -e MYSQL_ROOT_PASSWORD=root@123 -v /tmp/mysql:/var/lib/mysql mysql:latest
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>docker run -d --name db1 --hostname db1 -e MYSQL_ROOT_PASSWORD=root@123 -v /tmp/mysql:/var/lib/mysql mysql:latest</b>
020f56e867ddf4330153778fa9eb5dfaadf34418af9a9ae1f4fe36ee75c55ce7
(jegan@tektutor.org)$ <b>docker ps</b>
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS        PORTS                 NAMES
020f56e867dd   mysql:latest   "docker-entrypoint.s…"   3 seconds ago   Up 1 second   3306/tcp, 33060/tcp   db1
(jegan@tektutor.org)$ <b>docker exec -it db1 sh</b>
# <b>mysql -u root -p</b>
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.28 MySQL Community Server - GPL

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.00 sec)

mysql> CREATE DATABASE tektutor;
Query OK, 1 row affected (0.01 sec)

mysql> USE tektutor;
Database changed
mysql> CREATE TABLE training ( id int, name varchar(60), duration varchar(60) );
Query OK, 0 rows affected (0.02 sec)

mysql> INSERT INTO training values ( 1, "
mysql> INSERT INTO training values ( 1, "Linux Device Drivers", "5 days" );
Query OK, 1 row affected (0.02 sec)

mysql> INSERT INTO training values ( 2, "Advanced OpenShift", "5 days" );
Query OK, 1 row affected (0.01 sec)

mysql> SELECT * FROM training;
+------+----------------------+----------+
| id   | name                 | duration |
+------+----------------------+----------+
|    1 | Linux Device Drivers | 5 days   |
|    2 | Advanced OpenShift   | 5 days   |
+------+----------------------+----------+
2 rows in set (0.00 sec)

mysql> exit
Bye
# exit
</pre>


## Load Balancer example
```
docker run -d --name web1 --hostname web1 nginx:latest
docker run -d --name web2 --hostname web2 nginx:latest
docker run -d --name web3 --hostname web3 nginx:latest

docker run -d --name lb --hostname lb -p 80:80 nginx:latest
```
The port on the left represents host machine where the container is running.
The port on the right side represents the container port where nginx is listening.

List the containers and see if all containers are in running state
```
docker ps
```

### We need to copy the nginx.conf from lb container to our local machine to configure it as Load Balancer
```
docker cp lb:/etc/nginx/nginx.conf .
```

Update the nginx.conf file locally with the below content
<pre>
user  nginx;
worker_processes  auto;

error_log  /var/log/nginx/error.log notice;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}

http {
    upstream backend {
        server 172.17.0.2:80;
        server 172.17.0.3:80;
        server 172.17.0.4:80;
    }
    
    server {
        location / {
            proxy_pass http://backend;
        }
    }
}
</pre>

### Find your web1, web2 and web3 container IPs
```
docker inspect -f {{.NetworkSettings.IPAddress}} web1
docker inspect -f {{.NetworkSettings.IPAddress}} web2
docker inspect -f {{.NetworkSettings.IPAddress}} web3
```

In the above file, you need to replace the IPs with your web1, web2 and web3 IP addresses respectively.
<pre>
172.17.0.2 - nginx web server 1
172.17.0.3 - nginx web server 2
172.17.0.4 - nginx web server 3
</pre>

### Copy the nginx.conf file from local machine to lb container
```
docker cp nginx.conf lb:/etc/nginx/nginx.conf
```

### Restart the lb container to apply config changes
```
docker restart lb
```

### Verify if the lb container is still running after restart
```
docker ps
```

### Let's configure the web1, web2 and web3 index.html to identify which web server is serving us(Optional)
```
echo "Server 1" > index.html
docker cp index.html web1:/usr/share/nginx/html/index.html

echo "Server 2" > index.html
docker cp index.html web2:/usr/share/nginx/html/index.html

echo "Server 3" > index.html
docker cp index.html web3:/usr/share/nginx/html/index.html
```

### Let's test the lb 
```
curl http://localhost:80
```
If the above curl is not updating the output in round-robin fashion, try localhost from RPS lab machine web browser


## Lab - Checking application logs
```
docker run -d --name nginx1 --hostname nginx1 nginx:latest
docker logs nginx1
```

## Lab - Creating custom docker image

#### Create a file name Dockerfile with the below content
```
FROM ubuntu:16.04

RUN apt update && apt install -y net-tools 
RUN apt update && apt install -y iputils-ping vim tree
```

#### Build the custom image
```
docker built -t tektutor/ubuntu:16.04 .
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>docker build -t tektutor/ubuntu:16.04 .</b>
Sending build context to Docker daemon  2.048kB
Step 1/3 : FROM ubuntu:16.04
 ---> b6f507652425
Step 2/3 : RUN apt update && apt install -y net-tools
 ---> Running in ca8754908b88

WARNING: apt does not have a stable CLI interface. Use with caution in scripts.

Get:1 http://security.ubuntu.com/ubuntu xenial-security InRelease [99.8 kB]
Get:2 http://archive.ubuntu.com/ubuntu xenial InRelease [247 kB]
Get:3 http://security.ubuntu.com/ubuntu xenial-security/main amd64 Packages [2051 kB]
Get:4 http://archive.ubuntu.com/ubuntu xenial-updates InRelease [99.8 kB]
Get:5 http://archive.ubuntu.com/ubuntu xenial-backports InRelease [97.4 kB]
Get:6 http://security.ubuntu.com/ubuntu xenial-security/restricted amd64 Packages [15.9 kB]
Get:7 http://security.ubuntu.com/ubuntu xenial-security/universe amd64 Packages [984 kB]
Get:8 http://security.ubuntu.com/ubuntu xenial-security/multiverse amd64 Packages [8820 B]
Get:9 http://archive.ubuntu.com/ubuntu xenial/main amd64 Packages [1558 kB]
Get:10 http://archive.ubuntu.com/ubuntu xenial/restricted amd64 Packages [14.1 kB]
Get:11 http://archive.ubuntu.com/ubuntu xenial/universe amd64 Packages [9827 kB]
Get:12 http://archive.ubuntu.com/ubuntu xenial/multiverse amd64 Packages [176 kB]
Get:13 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 Packages [2560 kB]
Get:14 http://archive.ubuntu.com/ubuntu xenial-updates/restricted amd64 Packages [16.4 kB]
Get:15 http://archive.ubuntu.com/ubuntu xenial-updates/universe amd64 Packages [1544 kB]
Get:16 http://archive.ubuntu.com/ubuntu xenial-updates/multiverse amd64 Packages [26.2 kB]
Get:17 http://archive.ubuntu.com/ubuntu xenial-backports/main amd64 Packages [10.9 kB]
Get:18 http://archive.ubuntu.com/ubuntu xenial-backports/universe amd64 Packages [12.7 kB]
Fetched 19.3 MB in 5s (3689 kB/s)
Reading package lists...
Building dependency tree...
Reading state information...
All packages are up to date.

WARNING: apt does not have a stable CLI interface. Use with caution in scripts.

Reading package lists...
Building dependency tree...
Reading state information...
The following NEW packages will be installed:
  net-tools
0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
Need to get 175 kB of archives.
After this operation, 725 kB of additional disk space will be used.
Get:1 http://archive.ubuntu.com/ubuntu xenial/main amd64 net-tools amd64 1.60-26ubuntu1 [175 kB]
debconf: delaying package configuration, since apt-utils is not installed
Fetched 175 kB in 1s (123 kB/s)
Selecting previously unselected package net-tools.
(Reading database ... 4785 files and directories currently installed.)
Preparing to unpack .../net-tools_1.60-26ubuntu1_amd64.deb ...
Unpacking net-tools (1.60-26ubuntu1) ...
Setting up net-tools (1.60-26ubuntu1) ...
Removing intermediate container ca8754908b88
 ---> ae52a7a15be2
Step 3/3 : RUN apt update && apt install -y ipuitls-ping vim tree
 ---> Running in 24ba897c9eff

WARNING: apt does not have a stable CLI interface. Use with caution in scripts.

Hit:1 http://security.ubuntu.com/ubuntu xenial-security InRelease
Hit:2 http://archive.ubuntu.com/ubuntu xenial InRelease
Hit:3 http://archive.ubuntu.com/ubuntu xenial-updates InRelease
Hit:4 http://archive.ubuntu.com/ubuntu xenial-backports InRelease
Reading package lists...
Building dependency tree...
Reading state information...
All packages are up to date.

WARNING: apt does not have a stable CLI interface. Use with caution in scripts.

Reading package lists...
Building dependency tree...
Reading state information...
E: Unable to locate package ipuitls-ping
The command '/bin/sh -c apt update && apt install -y ipuitls-ping vim tree' returned a non-zero code: 100
(jegan@tektutor.org)$ vim Dockerfile
(jegan@tektutor.org)$ docker build -t tektutor/ubuntu:16.04 .
Sending build context to Docker daemon  2.048kB
Step 1/3 : FROM ubuntu:16.04
 ---> b6f507652425
Step 2/3 : RUN apt update && apt install -y net-tools
 ---> Using cache
 ---> ae52a7a15be2
Step 3/3 : RUN apt update && apt install -y iputils-ping vim tree
 ---> Running in a4924c94fdb1

WARNING: apt does not have a stable CLI interface. Use with caution in scripts.

Hit:1 http://archive.ubuntu.com/ubuntu xenial InRelease
Hit:2 http://security.ubuntu.com/ubuntu xenial-security InRelease
Hit:3 http://archive.ubuntu.com/ubuntu xenial-updates InRelease
Hit:4 http://archive.ubuntu.com/ubuntu xenial-backports InRelease
Reading package lists...
Building dependency tree...
Reading state information...
All packages are up to date.

WARNING: apt does not have a stable CLI interface. Use with caution in scripts.

Reading package lists...
Building dependency tree...
Reading state information...
The following additional packages will be installed:
  file libexpat1 libffi6 libgmp10 libgnutls-openssl27 libgnutls30 libgpm2
  libhogweed4 libidn11 libmagic1 libmpdec2 libnettle6 libp11-kit0 libpython3.5
  libpython3.5-minimal libpython3.5-stdlib libsqlite3-0 libssl1.0.0 libtasn1-6
  mime-support vim-common vim-runtime
Suggested packages:
  gnutls-bin gpm ctags vim-doc vim-scripts vim-gnome-py2 | vim-gtk-py2
  | vim-gtk3-py2 | vim-athena-py2 | vim-nox-py2
The following NEW packages will be installed:
  file iputils-ping libexpat1 libffi6 libgmp10 libgnutls-openssl27 libgnutls30
  libgpm2 libhogweed4 libidn11 libmagic1 libmpdec2 libnettle6 libp11-kit0
  libpython3.5 libpython3.5-minimal libpython3.5-stdlib libsqlite3-0
  libssl1.0.0 libtasn1-6 mime-support tree vim vim-common vim-runtime
0 upgraded, 25 newly installed, 0 to remove and 0 not upgraded.
Need to get 13.6 MB of archives.
After this operation, 62.3 MB of additional disk space will be used.
Get:1 http://archive.ubuntu.com/ubuntu xenial/main amd64 libgpm2 amd64 1.20.4-6.1 [16.5 kB]
Get:2 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libmagic1 amd64 1:5.25-2ubuntu1.4 [216 kB]
Get:3 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 file amd64 1:5.25-2ubuntu1.4 [21.2 kB]
Get:4 http://archive.ubuntu.com/ubuntu xenial/main amd64 libgmp10 amd64 2:6.1.0+dfsg-2 [240 kB]
Get:5 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libnettle6 amd64 3.2-1ubuntu0.16.04.2 [93.7 kB]
Get:6 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libhogweed4 amd64 3.2-1ubuntu0.16.04.2 [136 kB]
Get:7 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libidn11 amd64 1.32-3ubuntu1.2 [46.5 kB]
Get:8 http://archive.ubuntu.com/ubuntu xenial/main amd64 libffi6 amd64 3.2.1-4 [17.8 kB]
Get:9 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libp11-kit0 amd64 0.23.2-5~ubuntu16.04.2 [107 kB]
Get:10 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libtasn1-6 amd64 4.7-3ubuntu0.16.04.3 [43.5 kB]
Get:11 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libgnutls30 amd64 3.4.10-4ubuntu1.9 [548 kB]
Get:12 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libgnutls-openssl27 amd64 3.4.10-4ubuntu1.9 [21.9 kB]
Get:13 http://archive.ubuntu.com/ubuntu xenial/main amd64 iputils-ping amd64 3:20121221-5ubuntu2 [52.7 kB]
Get:14 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libexpat1 amd64 2.1.0-7ubuntu0.16.04.5 [71.5 kB]
Get:15 http://archive.ubuntu.com/ubuntu xenial/main amd64 libmpdec2 amd64 2.4.2-1 [82.6 kB]
Get:16 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libssl1.0.0 amd64 1.0.2g-1ubuntu4.20 [1083 kB]
Get:17 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libpython3.5-minimal amd64 3.5.2-2ubuntu0~16.04.13 [524 kB]
Get:18 http://archive.ubuntu.com/ubuntu xenial/main amd64 mime-support all 3.59ubuntu1 [31.0 kB]
Get:19 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libsqlite3-0 amd64 3.11.0-1ubuntu1.5 [398 kB]
Get:20 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libpython3.5-stdlib amd64 3.5.2-2ubuntu0~16.04.13 [2135 kB]
Get:21 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 vim-common amd64 2:7.4.1689-3ubuntu1.5 [104 kB]
Get:22 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libpython3.5 amd64 3.5.2-2ubuntu0~16.04.13 [1360 kB]
Get:23 http://archive.ubuntu.com/ubuntu xenial/universe amd64 tree amd64 1.7.0-3 [40.6 kB]
Get:24 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 vim-runtime all 2:7.4.1689-3ubuntu1.5 [5169 kB]
Get:25 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 vim amd64 2:7.4.1689-3ubuntu1.5 [1036 kB]
debconf: delaying package configuration, since apt-utils is not installed
Fetched 13.6 MB in 2s (4634 kB/s)
Selecting previously unselected package libgpm2:amd64.
(Reading database ... 4833 files and directories currently installed.)
Preparing to unpack .../libgpm2_1.20.4-6.1_amd64.deb ...
Unpacking libgpm2:amd64 (1.20.4-6.1) ...
Selecting previously unselected package libmagic1:amd64.
Preparing to unpack .../libmagic1_1%3a5.25-2ubuntu1.4_amd64.deb ...
Unpacking libmagic1:amd64 (1:5.25-2ubuntu1.4) ...
Selecting previously unselected package file.
Preparing to unpack .../file_1%3a5.25-2ubuntu1.4_amd64.deb ...
Unpacking file (1:5.25-2ubuntu1.4) ...
Selecting previously unselected package libgmp10:amd64.
Preparing to unpack .../libgmp10_2%3a6.1.0+dfsg-2_amd64.deb ...
Unpacking libgmp10:amd64 (2:6.1.0+dfsg-2) ...
Selecting previously unselected package libnettle6:amd64.
Preparing to unpack .../libnettle6_3.2-1ubuntu0.16.04.2_amd64.deb ...
Unpacking libnettle6:amd64 (3.2-1ubuntu0.16.04.2) ...
Selecting previously unselected package libhogweed4:amd64.
Preparing to unpack .../libhogweed4_3.2-1ubuntu0.16.04.2_amd64.deb ...
Unpacking libhogweed4:amd64 (3.2-1ubuntu0.16.04.2) ...
Selecting previously unselected package libidn11:amd64.
Preparing to unpack .../libidn11_1.32-3ubuntu1.2_amd64.deb ...
Unpacking libidn11:amd64 (1.32-3ubuntu1.2) ...
Selecting previously unselected package libffi6:amd64.
Preparing to unpack .../libffi6_3.2.1-4_amd64.deb ...
Unpacking libffi6:amd64 (3.2.1-4) ...
Selecting previously unselected package libp11-kit0:amd64.
Preparing to unpack .../libp11-kit0_0.23.2-5~ubuntu16.04.2_amd64.deb ...
Unpacking libp11-kit0:amd64 (0.23.2-5~ubuntu16.04.2) ...
Selecting previously unselected package libtasn1-6:amd64.
Preparing to unpack .../libtasn1-6_4.7-3ubuntu0.16.04.3_amd64.deb ...
Unpacking libtasn1-6:amd64 (4.7-3ubuntu0.16.04.3) ...
Selecting previously unselected package libgnutls30:amd64.
Preparing to unpack .../libgnutls30_3.4.10-4ubuntu1.9_amd64.deb ...
Unpacking libgnutls30:amd64 (3.4.10-4ubuntu1.9) ...
Selecting previously unselected package libgnutls-openssl27:amd64.
Preparing to unpack .../libgnutls-openssl27_3.4.10-4ubuntu1.9_amd64.deb ...
Unpacking libgnutls-openssl27:amd64 (3.4.10-4ubuntu1.9) ...
Selecting previously unselected package iputils-ping.
Preparing to unpack .../iputils-ping_3%3a20121221-5ubuntu2_amd64.deb ...
Unpacking iputils-ping (3:20121221-5ubuntu2) ...
Selecting previously unselected package libexpat1:amd64.
Preparing to unpack .../libexpat1_2.1.0-7ubuntu0.16.04.5_amd64.deb ...
Unpacking libexpat1:amd64 (2.1.0-7ubuntu0.16.04.5) ...
Selecting previously unselected package libmpdec2:amd64.
Preparing to unpack .../libmpdec2_2.4.2-1_amd64.deb ...
Unpacking libmpdec2:amd64 (2.4.2-1) ...
Selecting previously unselected package libssl1.0.0:amd64.
Preparing to unpack .../libssl1.0.0_1.0.2g-1ubuntu4.20_amd64.deb ...
Unpacking libssl1.0.0:amd64 (1.0.2g-1ubuntu4.20) ...
Selecting previously unselected package libpython3.5-minimal:amd64.
Preparing to unpack .../libpython3.5-minimal_3.5.2-2ubuntu0~16.04.13_amd64.deb ...
Unpacking libpython3.5-minimal:amd64 (3.5.2-2ubuntu0~16.04.13) ...
Selecting previously unselected package mime-support.
Preparing to unpack .../mime-support_3.59ubuntu1_all.deb ...
Unpacking mime-support (3.59ubuntu1) ...
Selecting previously unselected package libsqlite3-0:amd64.
Preparing to unpack .../libsqlite3-0_3.11.0-1ubuntu1.5_amd64.deb ...
Unpacking libsqlite3-0:amd64 (3.11.0-1ubuntu1.5) ...
Selecting previously unselected package libpython3.5-stdlib:amd64.
Preparing to unpack .../libpython3.5-stdlib_3.5.2-2ubuntu0~16.04.13_amd64.deb ...
Unpacking libpython3.5-stdlib:amd64 (3.5.2-2ubuntu0~16.04.13) ...
Selecting previously unselected package vim-common.
Preparing to unpack .../vim-common_2%3a7.4.1689-3ubuntu1.5_amd64.deb ...
Unpacking vim-common (2:7.4.1689-3ubuntu1.5) ...
Selecting previously unselected package libpython3.5:amd64.
Preparing to unpack .../libpython3.5_3.5.2-2ubuntu0~16.04.13_amd64.deb ...
Unpacking libpython3.5:amd64 (3.5.2-2ubuntu0~16.04.13) ...
Selecting previously unselected package tree.
Preparing to unpack .../tree_1.7.0-3_amd64.deb ...
Unpacking tree (1.7.0-3) ...
Selecting previously unselected package vim-runtime.
Preparing to unpack .../vim-runtime_2%3a7.4.1689-3ubuntu1.5_all.deb ...
Adding 'diversion of /usr/share/vim/vim74/doc/help.txt to /usr/share/vim/vim74/doc/help.txt.vim-tiny by vim-runtime'
Adding 'diversion of /usr/share/vim/vim74/doc/tags to /usr/share/vim/vim74/doc/tags.vim-tiny by vim-runtime'
Unpacking vim-runtime (2:7.4.1689-3ubuntu1.5) ...
Selecting previously unselected package vim.
Preparing to unpack .../vim_2%3a7.4.1689-3ubuntu1.5_amd64.deb ...
Unpacking vim (2:7.4.1689-3ubuntu1.5) ...
Processing triggers for libc-bin (2.23-0ubuntu11.3) ...
Setting up libgpm2:amd64 (1.20.4-6.1) ...
Setting up libmagic1:amd64 (1:5.25-2ubuntu1.4) ...
Setting up file (1:5.25-2ubuntu1.4) ...
Setting up libgmp10:amd64 (2:6.1.0+dfsg-2) ...
Setting up libnettle6:amd64 (3.2-1ubuntu0.16.04.2) ...
Setting up libhogweed4:amd64 (3.2-1ubuntu0.16.04.2) ...
Setting up libidn11:amd64 (1.32-3ubuntu1.2) ...
Setting up libffi6:amd64 (3.2.1-4) ...
Setting up libp11-kit0:amd64 (0.23.2-5~ubuntu16.04.2) ...
Setting up libtasn1-6:amd64 (4.7-3ubuntu0.16.04.3) ...
Setting up libgnutls30:amd64 (3.4.10-4ubuntu1.9) ...
Setting up libgnutls-openssl27:amd64 (3.4.10-4ubuntu1.9) ...
Setting up iputils-ping (3:20121221-5ubuntu2) ...
Setcap is not installed, falling back to setuid
Setting up libexpat1:amd64 (2.1.0-7ubuntu0.16.04.5) ...
Setting up libmpdec2:amd64 (2.4.2-1) ...
Setting up libssl1.0.0:amd64 (1.0.2g-1ubuntu4.20) ...
debconf: unable to initialize frontend: Dialog
debconf: (TERM is not set, so the dialog frontend is not usable.)
debconf: falling back to frontend: Readline
debconf: unable to initialize frontend: Readline
debconf: (Can't locate Term/ReadLine.pm in @INC (you may need to install the Term::ReadLine module) (@INC contains: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.22.1 /usr/local/share/perl/5.22.1 /usr/lib/x86_64-linux-gnu/perl5/5.22 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl/5.22 /usr/share/perl/5.22 /usr/local/lib/site_perl /usr/lib/x86_64-linux-gnu/perl-base .) at /usr/share/perl5/Debconf/FrontEnd/Readline.pm line 7.)
debconf: falling back to frontend: Teletype
Setting up libpython3.5-minimal:amd64 (3.5.2-2ubuntu0~16.04.13) ...
Setting up mime-support (3.59ubuntu1) ...
Setting up libsqlite3-0:amd64 (3.11.0-1ubuntu1.5) ...
Setting up libpython3.5-stdlib:amd64 (3.5.2-2ubuntu0~16.04.13) ...
Setting up vim-common (2:7.4.1689-3ubuntu1.5) ...
Setting up libpython3.5:amd64 (3.5.2-2ubuntu0~16.04.13) ...
Setting up tree (1.7.0-3) ...
Setting up vim-runtime (2:7.4.1689-3ubuntu1.5) ...
Setting up vim (2:7.4.1689-3ubuntu1.5) ...
update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/vim (vim) in auto mode
update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/vimdiff (vimdiff) in auto mode
update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/rvim (rvim) in auto mode
update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/rview (rview) in auto mode
update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/vi (vi) in auto mode
update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/view (view) in auto mode
update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/ex (ex) in auto mode
update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/editor (editor) in auto mode
Processing triggers for libc-bin (2.23-0ubuntu11.3) ...
Removing intermediate container a4924c94fdb1
 ---> 5d734013958b
Successfully built 5d734013958b
Successfully tagged tektutor/ubuntu:16.04
</pre>

#### Creating a container from our custom docker image
```
docker run -dit --name c1 --hostname c1 tektutor/ubuntu:16.04 bash
```

#### Get inside the container c1
```
docker exec -it c1 bash
```

Within the container shell, see if the below commands work
<pre>
tree
ifconfig
ping localhost
</pre>
