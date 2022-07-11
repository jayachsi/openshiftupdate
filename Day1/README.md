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

 
