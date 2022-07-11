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
