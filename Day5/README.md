# Day5

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

## OpenShift Network Model
