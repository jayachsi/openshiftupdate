# OpenShift

## Containers Technology
- Linux Technology
- Linux Kernel Feature
  1. Namespace ( helps isolating one container from other containers )
  2. Control Group (CGroup) - apply resource quota restricts
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
- is purpose is to make things easy for end-user
- end-user uses Container Engines not the Container Runtime directly
- Container Engines provide user-friendly commands that  abstracts complex low-level container/image management stuffs
- end-user doesn't have to deal with low-level stuffs, as Container Engines takes care of low-level stufss internally with the help of Container Engines

## Container Runtime
- example: there is runtime called runC
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
      - Which Containers supports CNI ( Container native interface )
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
      
## What is a Custom Resource in Kubernetes/OpenShift?

## What are Controllers in Kubernetes/OpenShift?
- application that monitors the health of application Pods
- Controllers ensure a desired number of application Pods are always running
- When Controllers notices that a particular Pod instance is not responding, Controllers will replace
  the bad non-responding application instance(Pod) with a new application instance(Pod)
- Controllers also take care of scaling up/down 
   - When user-traffic increase, they can help add more Pods to load-balance and serve the end users better
   - self-healing of itself and user application
