:original_name: cce_10_0873.html

.. _cce_10_0873:

Dynamically Mounting a DSS Disk to a StatefulSet
================================================

Application Scenarios
---------------------

Dynamic mounting is available only for creating a :ref:`StatefulSet <cce_10_0048>`. It is implemented through a volume claim template (`volumeClaimTemplates <https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/#volume-claim-templates>`__ field) and depends on dynamic creation of PVs through StorageClass. In this mode, each pod in a multi-pod StatefulSet is associated with a unique PVC and PV. After a pod is rescheduled, the original data can still be mounted to it based on the PVC name. In the common mounting mode for a Deployment, if ReadWriteMany is supported, multiple pods of the Deployment will be mounted to the same underlying storage.

Prerequisites
-------------

-  You have created a cluster of version v1.21.15-r0, v1.23.14-r0, v1.25.9-r0, v1.27.6-r0, v1.28.4-r0, or later, and :ref:`CCE Container Storage (Everest) <cce_10_0066>` of v2.4.5 or later has been installed in the cluster.
-  To create a cluster using commands, ensure kubectl is used. For details, see :ref:`Connecting to a Cluster Using kubectl <cce_10_0107>`.

Dynamically Mounting a DSS Disk on the Console
----------------------------------------------

#. Log in to the CCE console and click the cluster name to access the cluster console.

#. Choose **Workloads** in the navigation pane. In the right pane, click the **StatefulSets** tab.

#. Click **Create Workload** in the upper right corner. On the displayed page, click **Data Storage** in the **Container Settings** area and click **Add Volume** to select **VolumeClaimTemplate**.

#. Click **Create PVC**. In the dialog box displayed, configure PVC parameters.

   Click **Create**.

   +---------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                             | Description                                                                                                                                                                                                                                             |
   +=======================================+=========================================================================================================================================================================================================================================================+
   | PVC Type                              | In this example, select **DSS**.                                                                                                                                                                                                                        |
   +---------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | PVC Name                              | Enter the name of the PVC. After a PVC is created, a suffix is automatically added based on the number of pods. The format is <*Custom PVC name*>-<*Serial number*>, for example, example-0.                                                            |
   +---------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Creation Method                       | You can select **Dynamically provision** to create a PVC, PV, and underlying storage on the console in cascading mode.                                                                                                                                  |
   +---------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Storage Classes                       | The storage class for DSS disks is **csi-disk-dss**.                                                                                                                                                                                                    |
   +---------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | (Optional) Storage Volume Name Prefix | Available only when the cluster version is v1.23.14-r0, v1.25.9-r0, v1.27.6-r0, v1.28.4-r0, or later, and Everest of v2.4.15 or later is installed in the cluster.                                                                                      |
   |                                       |                                                                                                                                                                                                                                                         |
   |                                       | This parameter specifies the name of the underlying storage that is automatically created. The actual underlying storage name is in the format of "PV name prefix + PVC UID". If this parameter is left blank, the default prefix **pvc** will be used. |
   |                                       |                                                                                                                                                                                                                                                         |
   |                                       | For example, if the PV name prefix is set to **test**, the actual underlying storage name is **test-**\ *{UID}*.                                                                                                                                        |
   +---------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | DSS Pool                              | Select an existing DSS pool.                                                                                                                                                                                                                            |
   +---------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Capacity (GiB)                        | Capacity of the requested storage volume.                                                                                                                                                                                                               |
   +---------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Access Mode                           | DSS volumes support only **ReadWriteOnce**, indicating that a storage volume can be mounted to one node in read/write mode.                                                                                                                             |
   +---------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Encryption                            | Configure whether to encrypt underlying storage. If you select **Enabled (key)**, an encryption key must be configured.                                                                                                                                 |
   +---------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Resource Tag                          | You can add resource tags to classify resources, which is supported only when the Everest version in the cluster is 2.1.39 or later.                                                                                                                    |
   |                                       |                                                                                                                                                                                                                                                         |
   |                                       | You can create **predefined tags** on the TMS console. The predefined tags are available to all resources that support tags. You can use predefined tags to improve the tag creation and resource migration efficiency. For details, see .              |
   |                                       |                                                                                                                                                                                                                                                         |
   |                                       | CCE automatically creates system tags **CCE-Cluster-ID=**\ *{Cluster ID}*, **CCE-Cluster-Name=**\ *{Cluster name}*, and **CCE-Namespace=**\ *{Namespace name}*. These tags cannot be modified.                                                          |
   |                                       |                                                                                                                                                                                                                                                         |
   |                                       | .. note::                                                                                                                                                                                                                                               |
   |                                       |                                                                                                                                                                                                                                                         |
   |                                       |    After a dynamic PV of the DSS type is created, the resource tags cannot be updated on the CCE console. To update DSS resource tags, go to the DSS console.                                                                                           |
   +---------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Enter the path to which the volume is mounted.

   .. table:: **Table 1** Mounting a storage volume

      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
      +===================================+==============================================================================================================================================================================================================================================================================================================================================================================================================================================================+
      | Mount Path                        | Enter a mount path, for example, **/tmp**.                                                                                                                                                                                                                                                                                                                                                                                                                   |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
      |                                   | This parameter specifies a container path to which a data volume will be mounted. Do not mount the volume to a system directory such as **/** or **/var/run**. Otherwise, containers will be malfunctional. Mount the volume to an empty directory. If the directory is not empty, ensure that there are no files that affect container startup. Otherwise, the files will be replaced, leading to container startup failures or workload creation failures. |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
      |                                   | .. important::                                                                                                                                                                                                                                                                                                                                                                                                                                               |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
      |                                   |    NOTICE:                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
      |                                   |    If a volume is mounted to a high-risk directory, use an account with minimum permissions to start the container. Otherwise, high-risk files on the host may be damaged.                                                                                                                                                                                                                                                                                   |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Subpath                           | Enter the subpath of the storage volume and mount a path in the storage volume to the container. In this way, different folders of the same storage volume can be used in a single pod. **tmp**, for example, indicates that data in the mount path of the container is stored in the **tmp** folder of the storage volume. If this parameter is left blank, the root path is used by default.                                                               |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Permission                        | -  **Read-only**: You can only read the data in the mounted volumes.                                                                                                                                                                                                                                                                                                                                                                                         |
      |                                   | -  **Read-write**: You can modify the data volumes mounted to the path. Newly written data will not be migrated if the container is migrated, which may cause data loss.                                                                                                                                                                                                                                                                                     |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   In this example, the disk is mounted to the **/data** path of the container. The container data generated in this path is stored in the DSS disk.

#. Dynamically mount and use storage volumes. For details about other parameters, see :ref:`Creating a StatefulSet <cce_10_0048>`. After the configuration, click **Create Workload**.

   After the workload is created, the data in the container mount directory will be persistently stored. Verify the storage by referring to :ref:`Verifying Data Persistence <cce_10_0873__section18332151425712>`.

Dynamically Mounting a DSS Volume Through kubectl
-------------------------------------------------

#. Use kubectl to access the cluster.

#. Create a file named **statefulset-dss.yaml**. In this example, the disk is mounted to the **/data** path.

   .. code-block::

      apiVersion: apps/v1
      kind: StatefulSet
      metadata:
        name: statefulset-dss
        namespace: default
      spec:
        selector:
          matchLabels:
            app: statefulset-dss
        template:
          metadata:
            labels:
              app: statefulset-dss
          spec:
            containers:
              - name: container-1
                image: nginx:latest
                volumeMounts:
                  - name: pvc-disk           # The value must be the same as that in the volumeClaimTemplates field.
                    mountPath: /data         # Location where the storage volume is mounted
            imagePullSecrets:
              - name: default-secret
        serviceName: statefulset-dss         # Headless Service name
        replicas: 2
        volumeClaimTemplates:
          - apiVersion: v1
            kind: PersistentVolumeClaim
            metadata:
              name: pvc-disk
              namespace: default
              annotations:
                everest.io/disk-volume-type: SAS    # Disk type
                everest.io/csi.dedicated-storage-id: <dss_id>     # ID of the DSS storage pool
                everest.io/crypt-key-id: <your_key_id>    # (Optional) Encryption key ID. Mandatory for an encrypted disk.
                everest.io/enterprise-project-id: <your_project_id>  # (Optional) Enterprise project ID. If an enterprise project is specified, use the same enterprise project when creating a PVC. Otherwise, the PVC cannot be bound to a PV.
                everest.io/disk-volume-tags: '{"key1":"value1","key2":"value2"}' # (Optional) Custom resource tags
                everest.io/csi.volume-name-prefix: test  # (Optional) PV name prefix of the automatically created underlying storage
              labels:
                failure-domain.beta.kubernetes.io/region: <your_region>   # Region of the node where the application is to be deployed
                failure-domain.beta.kubernetes.io/zone: <your_zone>       # AZ of the node where the application is to be deployed
            spec:
              accessModes:
                - ReadWriteOnce               # The value must be ReadWriteOnce for DSS.
              resources:
                requests:
                  storage: 10Gi               # Disk capacity, ranging from 1 to 32768
              storageClassName: csi-disk      # StorageClass is DSS.
      ---
      apiVersion: v1
      kind: Service
      metadata:
        name: statefulset-dss   # Headless Service name
        namespace: default
        labels:
          app: statefulset-dss
      spec:
        selector:
          app: statefulset-dss
        clusterIP: None
        ports:
          - name: statefulset-dss
            targetPort: 80
            nodePort: 0
            port: 80
            protocol: TCP
        type: ClusterIP

   .. table:: **Table 2** Key parameters

      +------------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                                | Mandatory             | Description                                                                                                                                                                                                                                                               |
      +==========================================+=======================+===========================================================================================================================================================================================================================================================================+
      | failure-domain.beta.kubernetes.io/region | Yes                   | Region where the cluster is located.                                                                                                                                                                                                                                      |
      +------------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | failure-domain.beta.kubernetes.io/zone   | Yes                   | AZ where the DSS volume is created. It must be the same as the AZ planned for the workload.                                                                                                                                                                               |
      +------------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | everest.io/disk-volume-type              | Yes                   | Disk type. All letters are in uppercase.                                                                                                                                                                                                                                  |
      |                                          |                       |                                                                                                                                                                                                                                                                           |
      |                                          |                       | -  **SAS**: high I/O                                                                                                                                                                                                                                                      |
      |                                          |                       | -  **SSD**: ultra-high I/O                                                                                                                                                                                                                                                |
      +------------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | everest.io/csi.dedicated-storage-id      | Yes                   | ID of the DSS storage pool where the DSS disk resides.                                                                                                                                                                                                                    |
      |                                          |                       |                                                                                                                                                                                                                                                                           |
      |                                          |                       | To obtain a DSS storage pool ID, log in to the **Cloud Server Console**. In the navigation pane, choose **Dedicated Distributed Storage Service** > **Storage Pools** and click the name of the target storage pool. On the resource pool details page, copy the pool ID. |
      +------------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | everest.io/crypt-key-id                  | No                    | Mandatory when the DSS disk is encrypted. Enter the encryption key ID selected during disk creation.                                                                                                                                                                      |
      |                                          |                       |                                                                                                                                                                                                                                                                           |
      |                                          |                       | To obtain a key ID, log in to the DEW console, locate the key to be encrypted, and copy the key ID.                                                                                                                                                                       |
      +------------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | everest.io/disk-volume-tags              | No                    | This field is optional. It is supported when the Everest version in the cluster is 2.1.39 or later.                                                                                                                                                                       |
      |                                          |                       |                                                                                                                                                                                                                                                                           |
      |                                          |                       | You can add resource tags to classify resources.                                                                                                                                                                                                                          |
      |                                          |                       |                                                                                                                                                                                                                                                                           |
      |                                          |                       | You can create **predefined tags** on the TMS console. The predefined tags are available to all resources that support tags. You can use predefined tags to improve the tag creation and resource migration efficiency.                                                   |
      |                                          |                       |                                                                                                                                                                                                                                                                           |
      |                                          |                       | CCE automatically creates system tags **CCE-Cluster-ID=**\ *{Cluster ID}*, **CCE-Cluster-Name=**\ *{Cluster name}*, and **CCE-Namespace=**\ *{Namespace name}*. These tags cannot be modified.                                                                            |
      +------------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | everest.io/csi.volume-name-prefix        | No                    | (Optional) This parameter is available only when the cluster version is v1.23.14-r0, v1.25.9-r0, v1.27.6-r0, v1.28.4-r0, or later, and Everest of v2.4.15 or later is installed in the cluster.                                                                           |
      |                                          |                       |                                                                                                                                                                                                                                                                           |
      |                                          |                       | This parameter specifies the name of the underlying storage that is automatically created. The actual underlying storage name is in the format of "PV name prefix + PVC UID". If this parameter is left blank, the default prefix **pvc** will be used.                   |
      |                                          |                       |                                                                                                                                                                                                                                                                           |
      |                                          |                       | Enter 1 to 26 characters that cannot start or end with a hyphen (-). Only lowercase letters, digits, and hyphens (-) are allowed.                                                                                                                                         |
      |                                          |                       |                                                                                                                                                                                                                                                                           |
      |                                          |                       | For example, if the PV name prefix is set to **test**, the actual underlying storage name is **test-**\ *{UID}*.                                                                                                                                                          |
      +------------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | storage                                  | Yes                   | Requested PVC capacity, in Gi. The value ranges from **1** to **32768**.                                                                                                                                                                                                  |
      +------------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | storageClassName                         | Yes                   | The storage class for DSS disks is **csi-disk-dss**.                                                                                                                                                                                                                      |
      +------------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Run the following command to create a workload to which the DSS volume is mounted:

   .. code-block::

      kubectl apply -f statefulset-dss.yaml

   After the workload is created, the data in the container mount directory will be persistently stored. Verify the storage by referring to :ref:`Verifying Data Persistence <cce_10_0873__section18332151425712>`.

.. _cce_10_0873__section18332151425712:

Verifying Data Persistence
--------------------------

#. View the deployed application and DSS volume files.

   a. Run the following command to view the created pod:

      .. code-block::

         kubectl get pod | grep statefulset-dss

      Expected output:

      .. code-block::

         statefulset-dss-0          1/1     Running   0             45s
         statefulset-dss-1          1/1     Running   0             28s

   b. Run the following command to check whether the DSS volume has been mounted to the **/data** path:

      .. code-block::

         kubectl exec statefulset-dss-0 -- df | grep data

      Expected output:

      .. code-block::

         /dev/sdd              10255636     36888  10202364   0% /data

   c. Run the following command to check the files in the **/data** path:

      .. code-block::

         kubectl exec statefulset-dss-0 -- ls /data

      Expected output:

      .. code-block::

         lost+found

#. Run the following command to create a file named **static** in the **/data** path:

   .. code-block::

      kubectl exec statefulset-dss-0 --  touch /data/static

#. Run the following command to check the files in the **/data** path:

   .. code-block::

      kubectl exec statefulset-dss-0 -- ls /data

   Expected output:

   .. code-block::

      lost+found
      static

#. Run the following command to delete the pod named **web-dss-auto-0**:

   .. code-block::

      kubectl delete pod statefulset-dss-0

   Expected output:

   .. code-block::

      pod "statefulset-dss-0" deleted

#. After the deletion, the StatefulSet controller automatically creates a replica with the same name. Run the following command to check whether the files in the **/data** path have been modified:

   .. code-block::

      kubectl exec statefulset-dss-0 -- ls /data

   Expected output:

   .. code-block::

      lost+found
      static

   The **static** file is retained, indicating that the data in the DSS volume can be stored persistently.

Related Operations
------------------

You can also perform the operations listed in :ref:`Table 3 <cce_10_0873__cce_10_0872_table1619535674020>`.

.. _cce_10_0873__cce_10_0872_table1619535674020:

.. table:: **Table 3** Related operations

   +---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Operation                             | Description                                                                                                                                | Procedure                                                                                                                                                                       |
   +=======================================+============================================================================================================================================+=================================================================================================================================================================================+
   | Expanding the capacity of DSS storage | Quickly expand the capacity of an attached DSS disk on the CCE console.                                                                    | #. Choose **Storage** in the navigation pane. In the right pane, click the **PVCs** tab. Click **More** in the **Operation** column of the target PVC and select **Scale-out**. |
   |                                       |                                                                                                                                            | #. Enter the capacity to be added and click **OK**.                                                                                                                             |
   +---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Viewing events                        | View event names, event types, number of occurrences, Kubernetes events, first occurrence time, and last occurrence time of the PVC or PV. | #. Choose **Storage** in the navigation pane. In the right pane, click the **PVCs** or **PVs** tab.                                                                             |
   |                                       |                                                                                                                                            | #. Click **View Events** in the **Operation** column of the target PVC or PV to view events generated within one hour (events are retained for one hour).                       |
   +---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Viewing a YAML file                   | View, copy, or download the YAML file of a PVC or PV.                                                                                      | #. Choose **Storage** in the navigation pane. In the right pane, click the **PVCs** or **PVs** tab.                                                                             |
   |                                       |                                                                                                                                            | #. Click **View YAML** in the **Operation** column of the target PVC or PV to view or download the YAML.                                                                        |
   +---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
