:original_name: cce_faq_00038.html

.. _cce_faq_00038:

What Are the Differences Among CCE Storage Classes in Terms of Persistent Storage and Multi-node Mounting?
==========================================================================================================

Container storage provides storage for container workloads. It supports multiple storage classes. A pod can use any amount of storage.

Currently, CCE supports local, EVS, SFS, SFS Turbo, and OBS volumes.

The following table lists the differences among these storage classes.

.. table:: **Table 1** Differences among storage classes

   +---------------+--------------------+-------------------------------------+----------------------------------------------------------------------------------+
   | Storage Class | Persistent Storage | Automatic Migration with Containers | Multi-node Mounting                                                              |
   +===============+====================+=====================================+==================================================================================+
   | Local disks   | Supported          | Not supported                       | Not supported                                                                    |
   +---------------+--------------------+-------------------------------------+----------------------------------------------------------------------------------+
   | EVS           | Supported          | Supported                           | Not supported                                                                    |
   +---------------+--------------------+-------------------------------------+----------------------------------------------------------------------------------+
   | OBS           | Supported          | Supported                           | Supported. This type of volumes can be shared among multiple nodes or workloads. |
   +---------------+--------------------+-------------------------------------+----------------------------------------------------------------------------------+

Selecting a Storage Class
-------------------------

You can use the following types of storage volumes when creating a workload. You are advised to store workload data on EVS volumes. If you store workload data on a local volume, the data cannot be restored when a fault occurs on the node.

-  Local volumes: Mount the file directory of the host where a container is located to a specified container path (corresponding to hostPath in Kubernetes). Alternatively, you can leave the source path empty (corresponding to emptyDir in Kubernetes). If the source path is left empty, a temporary directory of the host will be mounted to the mount point of the container. A specified source path is used when data needs to be persistently stored on the host, while emptyDir is used when temporary storage is needed. A ConfigMap is a type of resource that stores configuration data required by a workload. Its contents are user-defined. A Secret is an object that contains sensitive data such as workload authentication information and keys. Information stored in a Secret is determined by users.
-  EVS volumes: Mount an EVS volume to a container path. When the container is migrated, the mounted EVS volume is migrated together. This storage class is applicable when data needs to be stored permanently.
-  OBS volumes: Create OBS volumes and mount them to a container path. OBS volumes are applicable to scenarios such as cloud workload, data analysis, content analysis, and hotspot objects.
