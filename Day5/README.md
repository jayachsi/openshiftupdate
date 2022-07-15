# Day5

## HELM
- package manager for OpenShift and Kubernetes applications
- it helps us in packaging the Cloud Native (Kubernetes/OpenShift) applications
- the packaged application is called Chart

#### Installing Helm in your system
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

## Docker Network Model
- Docker supports the below types of Network
  1. bridge
  2. host
  3. none
- by default, all containers are added to bridge network
- the bridge network has a subnet 172.17.0.0/16 and the bridge will act like a gateway(172.17.0.1) for all docker containers

## Listing the network types supported by docker
```
docker network ls
```
Expected output
<pre>
(jegan@tektutor.org)$ <b>docker network ls</b>
NETWORK ID     NAME      DRIVER    SCOPE
df0f5f7c81cf   bridge    bridge    local
9371e925f813   host      host      local
7c9fa6ea53f0   none      null      local
</pre>

#### Docker Network Model - Points to remember
- every time a new container is created, if no custom network is mentioned, the container will be connected to docker0 bridge network
- the Docker container get's a Private IP from the 172.17.0.0/16 that is available
- when a new container is created, Docker Application Engine creates a pair of veth interfaces
- one end of the veth interface is connected inside the container, which is seen as eth0 Network Interface Card(NIC) by the container
- other end of the veth interface is connected to the docker0 bridge. This can be seen using ifconfig command on the localhost where docker is installed
- every docker container has a dedicated network stack ( 7 OSI Layers )
- every docker container has a IP Routing Table
- the IP Routing Table is used by Docker to forward network packets for any outbound traffic from container

#### Finding more details about bridge device
```
docker network inspect bridge
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>docker network inspect bridge</b>
[
    {
        "Name": "bridge",
        "Id": "df0f5f7c81cf25494c9d446913eaf5a1acec94ee69bc38ad98ea3bc6bcee073e",
        "Created": "2022-07-14T20:17:13.057160332+05:30",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16",
                    "Gateway": "172.17.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {}
    }
]
</pre>

#### Creating a custom network my-net-1
```
docker network create my-net-1 --subnet 172.18.0.0/16
```
Expected output
<pre>
(jegan@tektutor.org)$ <b>docker network create my-net-1 --subnet 172.18.0.0/16</b>
73aadf83287bd494c9cb43eda2d3d61bb7dcee30e01f217c0389146db520ea7c
</pre>

#### Creating a custom network my-net-2
```
docker network create my-net-2 --subnet 172.19.0.0/16
```
Expected output
<pre>
(jegan@tektutor.org)$ <b>docker network create my-net-2 --subnet 172.19.0.0/16</b>
0d8127f235c403fb99d0778e93c96db1054ed1ca6acb01d147898ae42320d087
</pre>

#### Create a new ubuntu1 container and connect it to my-net-1 network
```
docker run -dit --name ubuntu1 --hostname ubuntu1 --network my-net-1 ubuntu:16.04 /bin/bash
```
Expected output
<pre>
(jegan@tektutor.org)$ <b>docker run -dit --name ubuntu1 --hostname ubuntu1 --network my-net-1 ubuntu:16.04 /bin/bash</b>
e2980aefc095823b4074f594e4e8ab9c91470b9e9be261d697efc10c4912baa7
</pre>

#### Create a new ubuntu2 container and connect it to my-net-2 network
```
docker run -dit --name ubuntu2 --hostname ubuntu2 --network my-net-2 ubuntu:16.04 /bin/bash
```
Expected output
<pre>
(jegan@tektutor.org)$<b>docker run -dit --name ubuntu2 --hostname ubuntu2 --network my-net-2 ubuntu:16.04 /bin/bash</b>
e8b3a9148948f5d7ace8f32d6dfe904b9cd0ab56366a437c1009e463d2620e6d
</pre>

#### Connect ubuntu1 container to my-net-2 as well
```
docker network connect my-net-2 ubuntu1
```
Expected output
<pre>
(jegan@tektutor.org)$ <b>docker network connect my-net-2 ubuntu1</b>
</pre>

![Docker Network Model](DockerNetworking.png)


#### Inspect ubuntu1 and see the IP address
```
docker inspect ubuntu1 | grep IPA
```

Expected output
<pre>
(jegan@tektutor.org)$ docker inspect ubuntu1|grep IPA
            "SecondaryIPAddresses": null,
            "IPAddress": "",
                    "IPAMConfig": null,
                    "IPAddress": "172.18.0.2",
                    "IPAMConfig": {},
                    "IPAddress": "172.19.0.3",
</pre>
ubuntu1 container has two IP addresses as it is connected to 2 networks, it has 2 Network Interface Cards(NICs).

#### Try getting inside the ubuntu1 container and install some network tools
```
docker exec -it ubuntu1 bash
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>docker exec -it ubuntu1 bash</b>
root@ubuntu1:/# <b>apt update && apt install iputils-ping net-tools</b>
Get:1 http://security.ubuntu.com/ubuntu xenial-security InRelease [99.8 kB]
Get:2 http://archive.ubuntu.com/ubuntu xenial InRelease [247 kB]
Get:3 http://security.ubuntu.com/ubuntu xenial-security/main amd64 Packages [2051 kB]
Get:4 http://archive.ubuntu.com/ubuntu xenial-updates InRelease [99.8 kB]         
Get:5 http://archive.ubuntu.com/ubuntu xenial-backports InRelease [97.4 kB]                
Get:6 http://archive.ubuntu.com/ubuntu xenial/main amd64 Packages [1558 kB]                  
Get:7 http://security.ubuntu.com/ubuntu xenial-security/restricted amd64 Packages [15.9 kB] 
Get:8 http://security.ubuntu.com/ubuntu xenial-security/universe amd64 Packages [984 kB]   
Get:9 http://security.ubuntu.com/ubuntu xenial-security/multiverse amd64 Packages [8820 B]
Get:10 http://archive.ubuntu.com/ubuntu xenial/restricted amd64 Packages [14.1 kB]
Get:11 http://archive.ubuntu.com/ubuntu xenial/universe amd64 Packages [9827 kB]
Get:12 http://archive.ubuntu.com/ubuntu xenial/multiverse amd64 Packages [176 kB]
Get:13 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 Packages [2560 kB]
Get:14 http://archive.ubuntu.com/ubuntu xenial-updates/restricted amd64 Packages [16.4 kB]
Get:15 http://archive.ubuntu.com/ubuntu xenial-updates/universe amd64 Packages [1544 kB]
Get:16 http://archive.ubuntu.com/ubuntu xenial-updates/multiverse amd64 Packages [26.2 kB]
Get:17 http://archive.ubuntu.com/ubuntu xenial-backports/main amd64 Packages [10.9 kB]
Get:18 http://archive.ubuntu.com/ubuntu xenial-backports/universe amd64 Packages [12.7 kB]
Fetched 19.3 MB in 5s (3510 kB/s)                           
Reading package lists... Done
Building dependency tree       
Reading state information... Done
All packages are up to date.
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following additional packages will be installed:
  libffi6 libgmp10 libgnutls-openssl27 libgnutls30 libhogweed4 libidn11 libnettle6 libp11-kit0 libtasn1-6
Suggested packages:
  gnutls-bin
The following NEW packages will be installed:
  iputils-ping libffi6 libgmp10 libgnutls-openssl27 libgnutls30 libhogweed4 libidn11 libnettle6 libp11-kit0 libtasn1-6 net-tools
0 upgraded, 11 newly installed, 0 to remove and 0 not upgraded.
Need to get 1482 kB of archives.
After this operation, 4510 kB of additional disk space will be used.
<b>Do you want to continue? [Y/n] Y</b>
Get:1 http://archive.ubuntu.com/ubuntu xenial/main amd64 libgmp10 amd64 2:6.1.0+dfsg-2 [240 kB]
Get:2 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libnettle6 amd64 3.2-1ubuntu0.16.04.2 [93.7 kB]
Get:3 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libhogweed4 amd64 3.2-1ubuntu0.16.04.2 [136 kB]
Get:4 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libidn11 amd64 1.32-3ubuntu1.2 [46.5 kB]
Get:5 http://archive.ubuntu.com/ubuntu xenial/main amd64 libffi6 amd64 3.2.1-4 [17.8 kB]
Get:6 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libp11-kit0 amd64 0.23.2-5~ubuntu16.04.2 [107 kB]
Get:7 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libtasn1-6 amd64 4.7-3ubuntu0.16.04.3 [43.5 kB]
Get:8 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libgnutls30 amd64 3.4.10-4ubuntu1.9 [548 kB]
Get:9 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libgnutls-openssl27 amd64 3.4.10-4ubuntu1.9 [21.9 kB]
Get:10 http://archive.ubuntu.com/ubuntu xenial/main amd64 iputils-ping amd64 3:20121221-5ubuntu2 [52.7 kB]
Get:11 http://archive.ubuntu.com/ubuntu xenial/main amd64 net-tools amd64 1.60-26ubuntu1 [175 kB]
Fetched 1482 kB in 6s (224 kB/s)                                                                                                                        
debconf: delaying package configuration, since apt-utils is not installed
Selecting previously unselected package libgmp10:amd64.
(Reading database ... 4785 files and directories currently installed.)
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
Selecting previously unselected package net-tools.
Preparing to unpack .../net-tools_1.60-26ubuntu1_amd64.deb ...
Unpacking net-tools (1.60-26ubuntu1) ...
Processing triggers for libc-bin (2.23-0ubuntu11.3) ...
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
Setting up net-tools (1.60-26ubuntu1) ...
Processing triggers for libc-bin (2.23-0ubuntu11.3) ...
</pre>

#### Get inside ubuntu2 container and install network tools
```
docker exec -it ubuntu2 bash
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>docker exec -it ubuntu2 bash</b>
root@ubuntu2:/# <b>apt update && apt install -y net-tools iputils-ping</b>
Get:1 http://security.ubuntu.com/ubuntu xenial-security InRelease [99.8 kB]
Get:2 http://archive.ubuntu.com/ubuntu xenial InRelease [247 kB]
Get:3 http://security.ubuntu.com/ubuntu xenial-security/main amd64 Packages [2051 kB]
Get:4 http://archive.ubuntu.com/ubuntu xenial-updates InRelease [99.8 kB]         
Get:5 http://archive.ubuntu.com/ubuntu xenial-backports InRelease [97.4 kB]        
Get:6 http://archive.ubuntu.com/ubuntu xenial/main amd64 Packages [1558 kB]        
Get:7 http://archive.ubuntu.com/ubuntu xenial/restricted amd64 Packages [14.1 kB]                                                                       
Get:8 http://archive.ubuntu.com/ubuntu xenial/universe amd64 Packages [9827 kB]                                                                         
Get:9 http://security.ubuntu.com/ubuntu xenial-security/restricted amd64 Packages [15.9 kB]                                                             
Get:10 http://security.ubuntu.com/ubuntu xenial-security/universe amd64 Packages [984 kB]                                                               
Get:11 http://security.ubuntu.com/ubuntu xenial-security/multiverse amd64 Packages [8820 B]                                                             
Get:12 http://archive.ubuntu.com/ubuntu xenial/multiverse amd64 Packages [176 kB]                                                                       
Get:13 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 Packages [2560 kB]                                                                    
Get:14 http://archive.ubuntu.com/ubuntu xenial-updates/restricted amd64 Packages [16.4 kB]                                                              
Get:15 http://archive.ubuntu.com/ubuntu xenial-updates/universe amd64 Packages [1544 kB]                                                                
Get:16 http://archive.ubuntu.com/ubuntu xenial-updates/multiverse amd64 Packages [26.2 kB]                                                              
Get:17 http://archive.ubuntu.com/ubuntu xenial-backports/main amd64 Packages [10.9 kB]                                                                  
Get:18 http://archive.ubuntu.com/ubuntu xenial-backports/universe amd64 Packages [12.7 kB]                                                              
Fetched 19.3 MB in 40s (474 kB/s)                                                                                                                       
Reading package lists... Done
Building dependency tree       
Reading state information... Done
All packages are up to date.
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following additional packages will be installed:
  libffi6 libgmp10 libgnutls-openssl27 libgnutls30 libhogweed4 libidn11 libnettle6 libp11-kit0 libtasn1-6
Suggested packages:
  gnutls-bin
The following NEW packages will be installed:
  iputils-ping libffi6 libgmp10 libgnutls-openssl27 libgnutls30 libhogweed4 libidn11 libnettle6 libp11-kit0 libtasn1-6 net-tools
0 upgraded, 11 newly installed, 0 to remove and 0 not upgraded.
Need to get 1482 kB of archives.
After this operation, 4510 kB of additional disk space will be used.
Get:1 http://archive.ubuntu.com/ubuntu xenial/main amd64 libgmp10 amd64 2:6.1.0+dfsg-2 [240 kB]
Get:2 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libnettle6 amd64 3.2-1ubuntu0.16.04.2 [93.7 kB]
Get:3 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libhogweed4 amd64 3.2-1ubuntu0.16.04.2 [136 kB]
Get:4 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libidn11 amd64 1.32-3ubuntu1.2 [46.5 kB]
Get:5 http://archive.ubuntu.com/ubuntu xenial/main amd64 libffi6 amd64 3.2.1-4 [17.8 kB]
Get:6 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libp11-kit0 amd64 0.23.2-5~ubuntu16.04.2 [107 kB]
Get:7 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libtasn1-6 amd64 4.7-3ubuntu0.16.04.3 [43.5 kB]
Get:8 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libgnutls30 amd64 3.4.10-4ubuntu1.9 [548 kB]
Get:9 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libgnutls-openssl27 amd64 3.4.10-4ubuntu1.9 [21.9 kB]
Get:10 http://archive.ubuntu.com/ubuntu xenial/main amd64 iputils-ping amd64 3:20121221-5ubuntu2 [52.7 kB]
Get:11 http://archive.ubuntu.com/ubuntu xenial/main amd64 net-tools amd64 1.60-26ubuntu1 [175 kB]
Fetched 1482 kB in 3s (456 kB/s)   
debconf: delaying package configuration, since apt-utils is not installed
Selecting previously unselected package libgmp10:amd64.
(Reading database ... 4785 files and directories currently installed.)
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
Selecting previously unselected package net-tools.
Preparing to unpack .../net-tools_1.60-26ubuntu1_amd64.deb ...
Unpacking net-tools (1.60-26ubuntu1) ...
Processing triggers for libc-bin (2.23-0ubuntu11.3) ...
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
Setting up net-tools (1.60-26ubuntu1) ...
Processing triggers for libc-bin (2.23-0ubuntu11.3) ...
root@ubuntu2:/# 
</pre>

#### Get inside ubuntu1 container,find its IP address and list routing table
```
docker exec -it ubuntu1 bash
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>docker exec -it ubuntu1 bash</b>
root@ubuntu1:/# <b>ifconfig</b>
eth0      Link encap:Ethernet  HWaddr 02:42:ac:12:00:02  
          inet addr:172.18.0.2  Bcast:172.18.255.255  Mask:255.255.0.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:4996 errors:0 dropped:0 overruns:0 frame:0
          TX packets:3949 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:21180907 (21.1 MB)  TX bytes:266513 (266.5 KB)

eth1      Link encap:Ethernet  HWaddr 02:42:ac:13:00:03  
          inet addr:172.19.0.3  Bcast:172.19.255.255  Mask:255.255.0.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:86 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:15142 (15.1 KB)  TX bytes:0 (0.0 B)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:6 errors:0 dropped:0 overruns:0 frame:0
          TX packets:6 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:946 (946.0 B)  TX bytes:946 (946.0 B)

root@ubuntu1:/# <b>route</b>
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         172.18.0.1      0.0.0.0         UG    0      0        0 eth0
172.18.0.0      *               255.255.0.0     U     0      0        0 eth0
172.19.0.0      *               255.255.0.0     U     0      0        0 eth1
root@ubuntu1:/#          
</pre>

#### Get inside ubuntu2 container,find its IP address and list routing table
```
docker exec -it ubuntu2 bash
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>docker exec -it ubuntu2 bash</b>
root@ubuntu2:/# <b>ifconfig</b>
eth0      Link encap:Ethernet  HWaddr 02:42:ac:13:00:02  
          inet addr:172.19.0.2  Bcast:172.19.255.255  Mask:255.255.0.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:10958 errors:0 dropped:0 overruns:0 frame:0
          TX packets:6998 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:21576879 (21.5 MB)  TX bytes:467819 (467.8 KB)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:6 errors:0 dropped:0 overruns:0 frame:0
          TX packets:6 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:762 (762.0 B)  TX bytes:762 (762.0 B)

root@ubuntu2:/# <b>route</b>
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         172.19.0.1      0.0.0.0         UG    0      0        0 eth0
172.19.0.0      *               255.255.0.0     U     0      0        0 eth0
</pre>

## OpenShift Network Model

You may find this article useful
<pre>
https://www.suse.com/c/rancher_blog/comparing-kubernetes-cni-providers-flannel-calico-canal-and-weave/
</pre>

- RedHat OpenShift follows Kubernetes Network Model as OpenShift is just Kubernetes with extra features

- Kubernetes Network Model specification says
  - kubelet container agent should be able to communicate with all pods on the node where kubelet is running
  - all Pods from every nodes should be able to communicate with each other

- The challenge is, Pod are assigned with Private IP, hence their IPs are accessible only within the node they are running
- But Kubernetes Network requirement is that, a Pod P1 running in Node N1 should be able to communicate with another Pod P2 running in Node N2
- In an ideal world this isn't possible, as the Pod P2 IP from Node N1 is invalid(inaccessible). Pod P1 IP from Node N2 is invalid(inaccessible as they are private to that machine)
- Kubernetes Network specifications are implemented by third-party Network vendors
- There are many Kubernetes Network add-ons, however the below are the most popular/common ones used in the industry
  1. calico
  2. flannel
  3. weave

#### Calico
- Project calico is developed and maintained by a company called Tigera
- Calico Kubernetes CNI is based on BGP routing protocol
- Apart from implementing Kubernetes Network Specifications, it also supports defining Network Policies for your application Pods
- Offers the best network performance as it doesn't encapsulate/de-encapsulate packets while forwarding/consuming network packets

#### Flannel
- Flannel is developed by CoreOS
- Flannel is based on Overlay Network
- Flannel deploys a flanneld Pod on every nodes as a DaemonSet
- flanneld on each nodes communicates and learns about neighbout node network topology
- Flannel wraps the outgoing packets in an UDP packet using source and destination IPs as the OpenShift Node IPs
- Flannel de-encapsulates the incoming UDP packet before forwarding to the destination Pods running on the node
- Flannel is easy install and understand
- Flannel doesn't support Network Policy
- Flannel performance isn't that great compared to Calico

#### Weave
- Weave is developed by WeaveWorks
- creates a mesh of overlay networks
- installs a routing component on every node
- the routing components on each node exchanges network topology information
- Weave learns fast routing paths to each node as there might be many ways to reach one node from other
- Weave also has features to communicate with nodes for which there is no know route
- Weave supports Network Policies

OpenShift multus CNI, allows using many different Network CNIs in the same cluster unlike Kubernetes which supports only one Network CNI at a time.


## Installing RedHat Ansible Automation Platform in RedHat OpenShift
<pre>
RedHat OpenShift webconsole => Administrator => Operators => OperatorHub => Search Ansible Automation Platform => Install
</pre>

#### Configuring RedHat Ansible Automation Platform
This depends on Persistent Volume of 8Gi capacity.  I named my automation controller as 'awx'.

<pre>
RedHat OpenShift webconsole => Installed Operators => Ansible Automation Platform => Automation Controller => Create Automation Controller
</pre>

## Retrive RedHat Ansible Automation Platform admin user Password from OpenShift
```
oc get secret/awx-admin-password -o yaml -n ansible-automation-platform
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc get secret/awx-admin-password -o yaml -n ansible-automation-platform</b>
apiVersion: v1
data:
  <b>password: Y2QxVnA3TDlCejZDV1hDZzRiZGxRUnVIRUJCRnZraVo=</b>
kind: Secret
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: '{"apiVersion":"v1","kind":"Secret","metadata":{"labels":{"app.kubernetes.io/component":"automationcontroller","app.kubernetes.io/managed-by":"automationcontroller-operator","app.kubernetes.io/name":"awx","app.kubernetes.io/operator-version":"","app.kubernetes.io/part-of":"awx"},"name":"awx-admin-password","namespace":"ansible-automation-platform"},"stringData":{"password":"cd1Vp7L9Bz6CWXCg4bdlQRuHEBBFvkiZ"}}'
  creationTimestamp: "2022-07-14T17:05:02Z"
  labels:
    app.kubernetes.io/component: automationcontroller
    app.kubernetes.io/managed-by: automationcontroller-operator
    app.kubernetes.io/name: awx
    app.kubernetes.io/operator-version: ""
    app.kubernetes.io/part-of: awx
  name: awx-admin-password
  namespace: ansible-automation-platform
  resourceVersion: "2375617"
  uid: 3ae527be-0ba3-43bd-8eaf-1b595b07ae24
type: Opaque
</pre>

## Deploying Jenkins within RedHat OpenShift
```
oc new-project jegan
oc new-app jenkins-ephemeral
```

## Securing your application routes with TLS Termination

## What is a Microservice?
- indepenently deployment application that represents one functionality
- microservice aren't supposed to be aware of other microservices
- microservices communicate with other via some message queue (Rabbit MQ) or streams (Apache kafka)
- can be implemented in many different langauages
- every microservice supports REST API
- normally they are containarized and deployed in Orchestration platforms like Kubernetes/OpenShift
- they are cloud native applications
- distribution computing

## Monolithic vs Microservice
- Monolithic applications
  - they use centralized database typically RDBMS( Oracle, mysql, etc., )
  - supports many features
  - RDBMS databases 
    - support ACID property by default
    - transaction Atomicity 
    -  either all transaction are commited or rolled back if transaction is incomplete
  - easy to deploy
  - easy to troubleshoot
  - drawbacks
    - even a small change, requires building and testing the entire application before it can be released
    - frequent releases are not possible
    - not easy to scale up
- Microservice architecture
  - Benefits
    - a single product may have several 100s or 1000s of independent microservices
    - each microservice technically can be developed in different programming language stack based on need
    - they can scaled up/down independently
    - they can be released independently
    - any change in one microservice most will have 0 impacts on other microservices
    - change in one microservie doesn't require compiling, packaging, testing and release the entire product is not required
    - nosql database are preferred over the traditional RDBMS
 - Drawabacks
   - troubleshoot is a nightmare
   - checking logs is very difficult, we need sophiscated tools like ELK, EFK, Splunk, etc to check the logs from a centralized location
   - deploying different microservices
   - we can't use traditional databases like RDBMS ( Oracle, MySQL )
   - 
   
Customer
Address
PaymentMethod

Customer has one or more Address
  - name
  - id
  Address
   - Address1
   - Address2
   - Address3
  PaymentMethods
   - PaymentMethod1
   - PaymentMethod2
   - PaymentMethod3
   

## Benefits of nosql databases
 - data is stored flat as documents (JSON, YAML, etc,)
 
## Drawbacks of nosql databases
 - most of them doesn't support ACID property
 - doesn't transaction management 
 

## Creating Credential Type in Ansible Automation Platform
Paste the below under Input Configuration
<pre>
fields:
  - id: kube_config
    type: string
    label: kubeconfig
    secret: true
    multiline: true
required:
  - kube_config
</pre>

Paste the below under Inject Configuration
<pre>
env:
  K8S_AUTH_KUBECONFIG: '{{ tower.filename.kubeconfig }}'
file:
  template.kubeconfig: '{{ kube_config }}'
</pre>



