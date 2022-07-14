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
  
   
