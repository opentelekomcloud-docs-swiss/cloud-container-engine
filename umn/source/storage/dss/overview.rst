:original_name: cce_10_0870.html

.. _cce_10_0870:

Overview
========

Dedicated Distributed Storage Service (DSS) provides dedicated physical storage resources. With data redundancy and cache acceleration technologies, DSS delivers highly reliable, durable, low-latency, and stable storage resources. CCE allows you to mount DSS storage volumes to containers.

DSS Performance
---------------

The key indicators to measure the performance of DSS storage pools include IOPS, throughput, and I/O latency.

-  IOPS: the number of input/output operations per second
-  Throughput: the amount of data read from and written into a storage pool per second
-  I/O latency: the minimum interval between two consecutive I/O operations

.. table:: **Table 1** DSS performance specifications

   +-----------------------------------------------+--------------------------------+-------------------------------+
   | Parameter                                     | High I/O                       | Ultra-high I/O                |
   +===============================================+================================+===============================+
   | IOPS                                          | 1500 IOPS/TB                   | 8000 IOPS/TB                  |
   +-----------------------------------------------+--------------------------------+-------------------------------+
   | I/O latency (single queue, 4 KiB data blocks) | 1 ms to 3 ms                   | 1 ms                          |
   +-----------------------------------------------+--------------------------------+-------------------------------+
   | Typical application scenarios                 | Common development and testing | -  Transcoding services       |
   |                                               |                                | -  I/O-intensive services     |
   |                                               |                                |                               |
   |                                               |                                |    -  NoSQL                   |
   |                                               |                                |    -  SQL Server              |
   |                                               |                                |    -  PostgreSQL              |
   |                                               |                                |                               |
   |                                               |                                | -  Latency-sensitive services |
   |                                               |                                |                               |
   |                                               |                                |    -  Redis                   |
   |                                               |                                |    -  Memcache                |
   +-----------------------------------------------+--------------------------------+-------------------------------+

Application Scenarios
---------------------

DSS supports the following mounting modes based on application scenarios:

-  :ref:`Using DSS Through a Static PV <cce_10_0871>`: static creation mode, where you use an existing volume to create a PV and then mount storage to the workload through a PVC. This mode applies to scenarios where disks are available.
-  :ref:`Using DSS Through a Dynamic PV <cce_10_0872>`: dynamic creation mode, in which you do not need to create disks beforehand. Instead, specify a StorageClass when creating a PVC. Then, a disk and PV will be created automatically. This mode applies to scenarios where no disk is available.
-  :ref:`Dynamically Mounting a DSS Disk to a StatefulSet <cce_10_0873>`: available only for StatefulSets. In this mode, each pod is associated with a unique PVC and PV. After a pod is rescheduled, the original data can still be mounted to it based on the PVC name. This mode applies to StatefulSets with multiple pods.
