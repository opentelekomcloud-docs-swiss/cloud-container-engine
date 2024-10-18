:original_name: cce_10_0871.html

.. _cce_10_0871:

Using DSS Through a Static PV
=============================

CCE allows you to create a PV using an existing DSS disk. After the PV is created, you can create a PVC and bind it to the PV. This mode applies to scenarios where underlying storage is available.

Prerequisites
-------------

-  You have created a cluster of version v1.21.15-r0, v1.23.14-r0, v1.25.9-r0, v1.27.6-r0, v1.28.4-r0, or later, and :ref:`CCE Container Storage (Everest) <cce_10_0066>` of v2.4.5 or later has been installed in the cluster.
-  To create a cluster using commands, ensure kubectl is used. For details, see :ref:`Connecting to a Cluster Using kubectl <cce_10_0107>`.

-  You have created a DSS disk that meets the following requirements:

   -  The DSS disk cannot be a system disk or shared disk.
   -  The DSS disk must be of the **SCSI** type (the default disk type is **VBD** when you create a DSS disk).
   -  The DSS disk must be available and not used by other resources.
   -  If the DSS disk is encrypted, the key must be available.

Notes and Constraints
---------------------

-  DSS disks cannot be attached across AZs and cannot be used by multiple workloads, multiple pods of the same workload, or multiple tasks. Data sharing of a shared disk is not supported between nodes in a CCE cluster. If a DSS disk is attached to multiple nodes, I/O conflicts and data cache conflicts may occur. Therefore, select only one pod when creating a Deployment that uses DSS.
-  If an HPA policy is used to expand the capacity of a workload with a DSS disk attached, new pods cannot be started because the DSS disk cannot attach to them.

Using an Existing DSS Disk on the Console
-----------------------------------------

#. Log in to the CCE console and click the cluster name to access the cluster console.
#. Statically create a PVC and PV.

   a. Choose **Storage** in the navigation pane. In the right pane, click the **PVCs** tab. Click **Create PVC** in the upper right corner. In the dialog box displayed, configure PVC parameters.

      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                    |
      +===================================+================================================================================================================================================================================+
      | PVC Type                          | In this example, select **DSS**.                                                                                                                                               |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | PVC Name                          | Enter the PVC name, which must be unique in a namespace.                                                                                                                       |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Creation Method                   | -  If underlying storage is available, create a PV or use an existing PV to statically create a PVC.                                                                           |
      |                                   | -  If no underlying storage is available, select **Dynamically provision**. For details, see :ref:`Using DSS Through a Dynamic PV <cce_10_0872>`.                              |
      |                                   |                                                                                                                                                                                |
      |                                   | In this example, select **Create new** to create both a PV and PVC on the console.                                                                                             |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | PV\ :sup:`a`                      | Select an existing PV in the cluster. For details about how to create a PV, see "Creating a storage volume" in :ref:`Related Operations <cce_10_0871__section16505832153318>`. |
      |                                   |                                                                                                                                                                                |
      |                                   | You do not need to specify this parameter in this example.                                                                                                                     |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | DSS\ :sup:`b`                     | Click **Select DSS**. On the displayed page, select the DSS volume that meets your requirements and click **OK**.                                                              |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | PV Name\ :sup:`b`                 | Enter the PV name, which must be unique in the same cluster.                                                                                                                   |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Access Mode\ :sup:`b`             | DSS volumes support only **ReadWriteOnce**, indicating that a storage volume can be mounted to one node in read/write mode.                                                    |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Reclaim Policy\ :sup:`b`          | You can select **Delete** or **Retain** to specify the reclaim policy of the underlying storage when the PVC is deleted.                                                       |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

      .. note::

         a: The parameter is available when **Creation Method** is set to **Use existing**.

         b: The parameter is available when **Creation Method** is set to **Create new**.

   b. Click **Create** to create a PVC and a PV.

      You can choose **Storage** in the navigation pane and view the created PVC and PV on the **PVCs** and **PVs** tab pages, respectively.

#. Create an application.

   a. Choose **Workloads** in the navigation pane. In the right pane, click the **StatefulSets** tab.

   b. Click **Create Workload** in the upper right corner. On the displayed page, click **Data Storage** in the **Container Settings** area and click **Add Volume** to select **PVC**.

      Mount and use storage volumes, as shown in :ref:`Table 1 <cce_10_0871__table2529244345>`. For details about other parameters, see :ref:`Workloads <cce_10_0046>`.

      .. _cce_10_0871__table2529244345:

      .. table:: **Table 1** Mounting a storage volume

         +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter                         | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
         +===================================+==============================================================================================================================================================================================================================================================================================================================================================================================================================================================+
         | PVC                               | Select an existing DSS volume.                                                                                                                                                                                                                                                                                                                                                                                                                               |
         |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
         |                                   | A DSS volume can be mounted to only one workload.                                                                                                                                                                                                                                                                                                                                                                                                            |
         +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
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

      .. note::

         A non-shared DSS disk can be attached to only one workload pod. If there are multiple pods, extra pods cannot start properly. Ensure that the number of workload pods is 1 if a DSS disk is attached.

         If multiple workload pods are needed, create a StatefulSet and dynamically mount a PV to each pod. For details, see :ref:`Dynamically Mounting a DSS Disk to a StatefulSet <cce_10_0873>`.

   c. After the configuration, click **Create Workload**.

      After the workload is created, the data in the container mount directory will be persistently stored. Verify the storage by referring to :ref:`Verifying Data Persistence <cce_10_0871__section11593165910013>`.

Using an Existing DSS Disk Through kubectl
------------------------------------------

#. Use kubectl to access the cluster.
#. Create a PV. If a PV has been created in your cluster, skip this step.

   a. .. _cce_10_0871__li162841212145314:

      Create the **pv-dss.yaml** file.

      .. code-block::

         apiVersion: v1
         kind: PersistentVolume
         metadata:
           annotations:
             pv.kubernetes.io/provisioned-by: everest-csi-provisioner
             everest.io/reclaim-policy: retain-volume-only         # (Optional) The underlying volume is retained when the PV is deleted.
           name: pv-dss    # PV name
           labels:
             failure-domain.beta.kubernetes.io/region: <your_region>   # Region of the node where the application is to be deployed
             failure-domain.beta.kubernetes.io/zone: <your_zone>       # AZ of the node where the application is to be deployed
         spec:
           accessModes:
             - ReadWriteOnce     # Access mode, which must be ReadWriteOnce for DSS
           capacity:
             storage: 10Gi       # Disk capacity, in the unit of GiB. The value ranges from 1 to 32768.
           csi:
             driver: disk.csi.everest.io     # Dependent storage driver for the mounting
             fsType: ext4    # Must be the same as that of the original file system of the disk.
             volumeAttributes:
               everest.io/disk-mode: SCSI           # Device type of the DSS disk. Only SCSI is supported.
               everest.io/disk-volume-type: SAS     # Disk type
               everest.io/csi.dedicated-storage-id: <dss_id>     # ID of the DSS storage pool
               storage.kubernetes.io/csiProvisionerIdentity: everest-csi-provisioner
               everest.io/crypt-key-id: <your_key_id>    # (Optional) Encryption key ID. Mandatory for an encrypted disk.

           persistentVolumeReclaimPolicy: Delete    # Reclaim policy
           storageClassName: csi-disk-dss              # Storage class name of the DSS disk

      .. table:: **Table 2** Key parameters

         +-----------------------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter                                     | Mandatory             | Description                                                                                                                                                                                                                                                                                                                         |
         +===============================================+=======================+=====================================================================================================================================================================================================================================================================================================================================+
         | everest.io/reclaim-policy: retain-volume-only | No                    | Optional.                                                                                                                                                                                                                                                                                                                           |
         |                                               |                       |                                                                                                                                                                                                                                                                                                                                     |
         |                                               |                       | Only **retain-volume-only** is supported.                                                                                                                                                                                                                                                                                           |
         |                                               |                       |                                                                                                                                                                                                                                                                                                                                     |
         |                                               |                       | This parameter is valid only when the Everest version is 1.2.9 or later and the reclaim policy is **Delete**. If the reclaim policy is **Delete** and the current value is **retain-volume-only**, the associated PV is deleted while the underlying storage volume is retained, when a PVC is deleted.                             |
         +-----------------------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | failure-domain.beta.kubernetes.io/region      | Yes                   | Region where the cluster is located.                                                                                                                                                                                                                                                                                                |
         +-----------------------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | failure-domain.beta.kubernetes.io/zone        | Yes                   | AZ where the DSS volume is created. It must be the same as the AZ planned for the workload.                                                                                                                                                                                                                                         |
         +-----------------------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | fsType                                        | Yes                   | File system type, which defaults to **ext4**.                                                                                                                                                                                                                                                                                       |
         +-----------------------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | everest.io/disk-volume-type                   | Yes                   | Disk type. All letters are in uppercase.                                                                                                                                                                                                                                                                                            |
         |                                               |                       |                                                                                                                                                                                                                                                                                                                                     |
         |                                               |                       | -  **SAS**: high I/O                                                                                                                                                                                                                                                                                                                |
         |                                               |                       | -  **SSD**: ultra-high I/O                                                                                                                                                                                                                                                                                                          |
         +-----------------------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | everest.io/csi.dedicated-storage-id           | Yes                   | ID of the DSS storage pool where the DSS disk resides.                                                                                                                                                                                                                                                                              |
         |                                               |                       |                                                                                                                                                                                                                                                                                                                                     |
         |                                               |                       | To obtain a DSS storage pool ID, log in to the **Cloud Server Console**. In the navigation pane, choose **Dedicated Distributed Storage Service** > **Storage Pools** and click the name of the target storage pool. On the resource pool details page, copy the pool ID.                                                           |
         +-----------------------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | everest.io/crypt-key-id                       | No                    | Mandatory when the disk is encrypted. Enter the encryption key ID selected during disk creation.                                                                                                                                                                                                                                    |
         |                                               |                       |                                                                                                                                                                                                                                                                                                                                     |
         |                                               |                       | To obtain an encryption key ID, log in to the **Cloud Server Console**. In the navigation pane, choose **Dedicated Distributed Storage Service** > **Disks**. Click the name of the target disk to go to its details page. On the **Summary** tab page, copy the value of **KMS Key ID** in the **Configuration Information** area. |
         +-----------------------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | persistentVolumeReclaimPolicy                 | Yes                   | The **Delete** and **Retain** reclaim policies are supported. If high data security is required, select **Retain** to prevent data from being deleted by mistake.                                                                                                                                                                   |
         |                                               |                       |                                                                                                                                                                                                                                                                                                                                     |
         |                                               |                       | **Delete**:                                                                                                                                                                                                                                                                                                                         |
         |                                               |                       |                                                                                                                                                                                                                                                                                                                                     |
         |                                               |                       | -  If **everest.io/reclaim-policy** is not specified, both the PV and DSS disk will be deleted when a PVC is deleted.                                                                                                                                                                                                               |
         |                                               |                       | -  If **everest.io/reclaim-policy** is set to **retain-volume-only**, when a PVC is deleted, the PV will be deleted but the DSS disk will be retained.                                                                                                                                                                              |
         |                                               |                       |                                                                                                                                                                                                                                                                                                                                     |
         |                                               |                       | **Retain**: When a PVC is deleted, both the PV and underlying storage resources will be retained. You need to manually delete these resources. After the PVC is deleted, the PV is in the **Released** state and cannot be bound to a PVC again.                                                                                    |
         +-----------------------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | storageClassName                              | Yes                   | The storage class for DSS disks is **csi-disk-dss**.                                                                                                                                                                                                                                                                                |
         +-----------------------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   b. Run the following command to create a PV:

      .. code-block::

         kubectl apply -f pv-dss.yaml

#. Create a PVC.

   a. Create the **pvc-dss.yaml** file.

      .. code-block::

         apiVersion: v1
         kind: PersistentVolumeClaim
         metadata:
           name: pvc-dss
           namespace: default
           annotations:
             everest.io/disk-volume-type: SAS    # Disk type
             everest.io/csi.dedicated-storage-id: <dss_id>     # ID of the DSS storage pool
             everest.io/crypt-key-id: <your_key_id>    # (Optional) Encryption key ID. Mandatory for an encrypted disk.

           labels:
             failure-domain.beta.kubernetes.io/region: <your_region>   # Region of the node where the application is to be deployed
             failure-domain.beta.kubernetes.io/zone: <your_zone>       # AZ of the node where the application is to be deployed
         spec:
           accessModes:
           - ReadWriteOnce               # The value must be ReadWriteOnce for DSS.
           resources:
             requests:
               storage: 10Gi             # Disk capacity, ranging from 1 to 32768. The value must be the same as the storage size of the existing PV.
           storageClassName: csi-disk-dss    # StorageClass is DSS.
           volumeName: pv-dss            # PV name

      .. table:: **Table 3** Key parameters

         +------------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter                                | Mandatory             | Description                                                                                                                                                                                                                                                               |
         +==========================================+=======================+===========================================================================================================================================================================================================================================================================+
         | failure-domain.beta.kubernetes.io/region | Yes                   | Region where the cluster is located.                                                                                                                                                                                                                                      |
         +------------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | failure-domain.beta.kubernetes.io/zone   | Yes                   | AZ where the disk is created. It must be the same as the AZ planned for the workload.                                                                                                                                                                                     |
         +------------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | everest.io/csi.dedicated-storage-id      | Yes                   | ID of the DSS storage pool where the DSS disk resides.                                                                                                                                                                                                                    |
         |                                          |                       |                                                                                                                                                                                                                                                                           |
         |                                          |                       | To obtain a DSS storage pool ID, log in to the **Cloud Server Console**. In the navigation pane, choose **Dedicated Distributed Storage Service** > **Storage Pools** and click the name of the target storage pool. On the resource pool details page, copy the pool ID. |
         +------------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | storage                                  | Yes                   | Requested capacity in the PVC, in Gi.                                                                                                                                                                                                                                     |
         |                                          |                       |                                                                                                                                                                                                                                                                           |
         |                                          |                       | The value must be the same as the storage size of the existing PV.                                                                                                                                                                                                        |
         +------------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | volumeName                               | Yes                   | PV name, which must be the same as the PV name in :ref:`1 <cce_10_0871__li162841212145314>`.                                                                                                                                                                              |
         +------------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | storageClassName                         | Yes                   | Storage class name, which must be the same as the storage class of the PV in :ref:`1 <cce_10_0871__li162841212145314>`.                                                                                                                                                   |
         |                                          |                       |                                                                                                                                                                                                                                                                           |
         |                                          |                       | The storage class for DSS disks is **csi-disk-dss**.                                                                                                                                                                                                                      |
         +------------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   b. Run the following command to create a PVC:

      .. code-block::

         kubectl apply -f pvc-dss.yaml

#. Create an application.

   a. Create a file named **web-dss.yaml**. In this example, the disk is mounted to the **/data** path.

      .. code-block::

         apiVersion: apps/v1
         kind: StatefulSet
         metadata:
           name: web-dss
           namespace: default
         spec:
           replicas: 1
           selector:
             matchLabels:
               app: web-dss
           serviceName: web-dss   # Headless Service name
           template:
             metadata:
               labels:
                 app: web-dss
             spec:
               containers:
               - name: container-1
                 image: nginx:latest
                 volumeMounts:
                 - name: pvc-disk-dss    # Volume name, which must be the same as the volume name in the volumes field
                   mountPath: /data  # Location where the storage volume is mounted
               imagePullSecrets:
                 - name: default-secret
               volumes:
                 - name: pvc-disk-dss    # Volume name, which can be customized
                   persistentVolumeClaim:
                     claimName: pvc-dss    # Name of the created PVC
         ---
         apiVersion: v1
         kind: Service
         metadata:
           name: web-dss   # Headless Service name
           namespace: default
           labels:
             app: web-dss
         spec:
           selector:
             app: web-dss
           clusterIP: None
           ports:
             - name: web-dss
               targetPort: 80
               nodePort: 0
               port: 80
               protocol: TCP
           type: ClusterIP

   b. Run the following command to create a workload to which the DSS volume is mounted:

      .. code-block::

         kubectl apply -f web-dss.yaml

      After the workload is created, the data in the container mount directory will be persistently stored. Verify the storage by referring to :ref:`Verifying Data Persistence <cce_10_0871__section11593165910013>`.

.. _cce_10_0871__section11593165910013:

Verifying Data Persistence
--------------------------

#. View the deployed application and DSS volume files.

   a. Run the following command to view the created pod:

      .. code-block::

         kubectl get pod | grep web-dss

      Expected output:

      .. code-block::

         web-dss-0                  1/1     Running   0               38s

   b. Run the following command to check whether the DSS volume has been mounted to the **/data** path:

      .. code-block::

         kubectl exec web-dss-0 -- df | grep data

      Expected output:

      .. code-block::

         /dev/sdc              10255636     36888  10202364   0% /data

   c. Run the following command to check the files in the **/data** path:

      .. code-block::

         kubectl exec web-dss-0 -- ls /data

      Expected output:

      .. code-block::

         lost+found

#. Run the following command to create a file named **static** in the **/data** path:

   .. code-block::

      kubectl exec web-dss-0 --  touch /data/static

#. Run the following command to check the files in the **/data** path:

   .. code-block::

      kubectl exec web-dss-0 -- ls /data

   Expected output:

   .. code-block::

      lost+found
      static

#. Run the following command to delete the pod named **web-dss-0**:

   .. code-block::

      kubectl delete pod web-dss-0

   Expected output:

   .. code-block::

      pod "web-dss-0" deleted

#. After the deletion, the StatefulSet controller automatically creates a replica with the same name. Run the following command to check whether the files in the **/data** path have been modified:

   .. code-block::

      kubectl exec web-dss-0 -- ls /data

   Expected output:

   .. code-block::

      lost+found
      static

   The **static** file is retained, indicating that the data in the DSS volume can be stored persistently.

.. _cce_10_0871__section16505832153318:

Related Operations
------------------

You can also perform the operations listed in :ref:`Table 4 <cce_10_0871__table1619535674020>`.

.. _cce_10_0871__table1619535674020:

.. table:: **Table 4** Related operations

   +---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Operation                             | Description                                                                                                                                | Procedure                                                                                                                                                                                               |
   +=======================================+============================================================================================================================================+=========================================================================================================================================================================================================+
   | Creating a storage volume (PV)        | Create a PV on the CCE console.                                                                                                            | #. Choose **Storage** in the navigation pane. In the right pane, click the **PVs** tab. Click **Create PersistentVolume** in the upper right corner. In the dialog box displayed, configure parameters. |
   |                                       |                                                                                                                                            |                                                                                                                                                                                                         |
   |                                       |                                                                                                                                            |    -  **Volume Type**: Select **DSS**.                                                                                                                                                                  |
   |                                       |                                                                                                                                            |    -  **DSS**: Click **Select DSS**. On the displayed page, select the disk that meets your requirements and click **OK**.                                                                              |
   |                                       |                                                                                                                                            |    -  **PV Name**: Enter the PV name, which must be unique in a cluster.                                                                                                                                |
   |                                       |                                                                                                                                            |    -  **Access Mode**: DSS volumes support only **ReadWriteOnce**, indicating that a storage volume can be mounted to one node in read/write mode.                                                      |
   |                                       |                                                                                                                                            |    -  **Reclaim Policy**: **Delete** or **Retain** is supported.                                                                                                                                        |
   |                                       |                                                                                                                                            |                                                                                                                                                                                                         |
   |                                       |                                                                                                                                            | #. Click **Create**.                                                                                                                                                                                    |
   +---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Expanding the capacity of DSS storage | Quickly expand the capacity of an attached DSS disk on the CCE console.                                                                    | #. Choose **Storage** in the navigation pane. In the right pane, click the **PVCs** tab. Click **More** in the **Operation** column of the target PVC and select **Scale-out**.                         |
   |                                       |                                                                                                                                            | #. Enter the capacity to be added and click **OK**.                                                                                                                                                     |
   +---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Viewing events                        | View event names, event types, number of occurrences, Kubernetes events, first occurrence time, and last occurrence time of the PVC or PV. | #. Choose **Storage** in the navigation pane. In the right pane, click the **PVCs** or **PVs** tab.                                                                                                     |
   |                                       |                                                                                                                                            | #. Click **View Events** in the **Operation** column of the target PVC or PV to view events generated within one hour (events are retained for one hour).                                               |
   +---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Viewing a YAML file                   | View, copy, or download the YAML file of a PVC or PV.                                                                                      | #. Choose **Storage** in the navigation pane. In the right pane, click the **PVCs** or **PVs** tab.                                                                                                     |
   |                                       |                                                                                                                                            | #. Click **View YAML** in the **Operation** column of the target PVC or PV to view or download the YAML.                                                                                                |
   +---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
