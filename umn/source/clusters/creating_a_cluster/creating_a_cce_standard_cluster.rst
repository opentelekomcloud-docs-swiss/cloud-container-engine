:original_name: cce_10_0028.html

.. _cce_10_0028:

Creating a CCE Standard Cluster
===============================

On the CCE console, you can easily create Kubernetes clusters. After a cluster is created, the master node is hosted by CCE. You only need to create worker nodes. In this way, you can implement cost-effective O&M and efficient service deployment.

Precautions
-----------

-  After a cluster is created, the following items cannot be changed:

   -  Cluster type
   -  Number of master nodes in the cluster
   -  AZ of a master node
   -  Network configurations of the cluster, such as the VPC, subnet, Service CIDR block, IPv6 settings, and kube-proxy settings
   -  Network model. For example, change **Tunnel network** to **VPC network**.

Step 1: Log In to the CCE Console
---------------------------------

#. Log in to the CCE console.
#. On the **Clusters** page, click **Create** **Cluster** in the upper right corner.

Step 2: Configure the Cluster
-----------------------------

On the **Create** **Cluster** page, configure the parameters.

**Basic Settings**

+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Parameter                         | Description                                                                                                                                                                                                                                                                                |
+===================================+============================================================================================================================================================================================================================================================================================+
| Cluster Name                      | Enter a cluster name. Cluster names under the same account must be unique.                                                                                                                                                                                                                 |
+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Cluster Version                   | Select the Kubernetes version used by the cluster.                                                                                                                                                                                                                                         |
+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Cluster Scale                     | Select a cluster scale for your cluster as required. These values specify the maximum number of nodes that can be managed by the cluster.                                                                                                                                                  |
+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Master Nodes                      | Select the number of master nodes. The master nodes are automatically hosted by CCE and deployed with Kubernetes cluster management components such as kube-apiserver, kube-controller-manager, and kube-scheduler.                                                                        |
|                                   |                                                                                                                                                                                                                                                                                            |
|                                   | -  **3 Masters**: Three master nodes will be created for high cluster availability.                                                                                                                                                                                                        |
|                                   | -  **Single**: Only one master node will be created in your cluster.                                                                                                                                                                                                                       |
|                                   |                                                                                                                                                                                                                                                                                            |
|                                   | You can also select AZs for the master nodes. By default, AZs are allocated automatically for the master nodes.                                                                                                                                                                            |
|                                   |                                                                                                                                                                                                                                                                                            |
|                                   | -  **Automatic**: Master nodes are randomly distributed in different AZs for cluster DR. If there are not enough AZs available, CCE will prioritize assigning nodes in AZs with enough resources to ensure cluster creation. However, this may result in AZ-level DR not being guaranteed. |
|                                   |                                                                                                                                                                                                                                                                                            |
|                                   | -  **Custom**: Master nodes are deployed in specific AZs.                                                                                                                                                                                                                                  |
|                                   |                                                                                                                                                                                                                                                                                            |
|                                   |    If there is one master node in your cluster, you can select one AZ for the master node. If there are multiple master nodes in your cluster, you can select multiple AZs for the master nodes.                                                                                           |
|                                   |                                                                                                                                                                                                                                                                                            |
|                                   |    -  **AZ**: Master nodes are deployed in different AZs for cluster DR.                                                                                                                                                                                                                   |
|                                   |    -  **Host**: Master nodes are deployed on different hosts in the same AZ for cluster DR.                                                                                                                                                                                                |
|                                   |    -  **Custom**: Master nodes are deployed in the AZs you specified.                                                                                                                                                                                                                      |
+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**Network Settings**

The network settings cover nodes, containers, and Services. For details about the cluster networking and container network models, see :ref:`Overview <cce_10_0010>`.

.. table:: **Table 1** Network settings

   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                                                                                     |
   +===================================+=================================================================================================================================================================================+
   | VPC                               | Select the VPC to which the cluster belongs. If no VPC is available, click **Create VPC** to create one. The value cannot be changed after the cluster is created.              |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Subnet                            | Select the subnet to which the master nodes belong. If no subnet is available, click **Create Subnet** to create one. The value cannot be changed after the cluster is created. |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Default Security Group            | Select the security group automatically generated by CCE or use the existing one as the default security group of the node.                                                     |
   |                                   |                                                                                                                                                                                 |
   |                                   | .. important::                                                                                                                                                                  |
   |                                   |                                                                                                                                                                                 |
   |                                   |    NOTICE:                                                                                                                                                                      |
   |                                   |    The default security group must allow traffic from certain ports to ensure normal communication. Otherwise, the node cannot be created.                                      |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | IPv6                              | If enabled, cluster resources, including nodes and workloads, can be accessed through IPv6 CIDR blocks.                                                                         |
   |                                   |                                                                                                                                                                                 |
   |                                   | -  IPv4/IPv6 dual stack is not supported by clusters using the VPC networks.                                                                                                    |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. table:: **Table 2** Network settings

   +-------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | Parameter                                                   | Description                                                                                                         |
   +=============================================================+=====================================================================================================================+
   | Network Model                                               | Select **VPC network** or **Tunnel network** for your CCE standard cluster.                                         |
   |                                                             |                                                                                                                     |
   |                                                             | For more information about their differences, see :ref:`Overview <cce_10_0281>`.                                    |
   +-------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | Container CIDR Block (configured for CCE standard clusters) | Configure the CIDR block used by containers. The value determines the maximum number of containers in your cluster. |
   +-------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------+

.. table:: **Table 3** Service network

   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                                                                                                                                                |
   +===================================+============================================================================================================================================================================================================================================+
   | Service CIDR Block                | Configure the Service CIDR blocks for containers in the same cluster to access each other. The value determines the maximum number of Services you can create. The value cannot be changed after the cluster is created.                   |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Request Forwarding                | Select **IPVS** or **iptables** for your cluster. For details, see :ref:`Comparing iptables and IPVS <cce_10_0349>`.                                                                                                                       |
   |                                   |                                                                                                                                                                                                                                            |
   |                                   | -  iptables is the traditional kube-proxy mode. This mode applies to the scenario where the number of Services is small or a large number of short connections are concurrently sent on the client. IPv6 clusters do not support iptables. |
   |                                   | -  IPVS allows higher throughput and faster forwarding. This mode applies to scenarios where the cluster scale is large or the number of Services is large.                                                                                |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**(Optional) Advanced Settings**

+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Parameter                         | Description                                                                                                                                                                                                                                                                                |
+===================================+============================================================================================================================================================================================================================================================================================+
| Certificate Authentication        | -  If **Automatically generated** is selected, the X509-based authentication mode will be enabled by default. X509 is a commonly used certificate format.                                                                                                                                  |
|                                   |                                                                                                                                                                                                                                                                                            |
|                                   | -  If **Bring your own** is selected, the cluster can identify users based on the header in the request body for authentication.                                                                                                                                                           |
|                                   |                                                                                                                                                                                                                                                                                            |
|                                   |    Upload your CA root certificate, client certificate, and private key.                                                                                                                                                                                                                   |
|                                   |                                                                                                                                                                                                                                                                                            |
|                                   |    .. caution::                                                                                                                                                                                                                                                                            |
|                                   |                                                                                                                                                                                                                                                                                            |
|                                   |       CAUTION:                                                                                                                                                                                                                                                                             |
|                                   |                                                                                                                                                                                                                                                                                            |
|                                   |       -  Upload a file **smaller than 1 MB**. The CA certificate and client certificate can be in **.crt** or **.cer** format. The private key of the client certificate can only be uploaded **unencrypted**.                                                                             |
|                                   |       -  The validity period of the client certificate must be longer than five years.                                                                                                                                                                                                     |
|                                   |       -  The uploaded CA root certificate is used by the authentication proxy and for configuring the kube-apiserver aggregation layer. **If any of the uploaded certificates is invalid, the cluster cannot be created.**                                                                 |
|                                   |       -  Starting from v1.25, Kubernetes no longer supports certificate authentication generated using the SHA1WithRSA or ECDSAWithSHA1 algorithm. The certificate authentication generated using the SHA256 algorithm is supported instead.                                               |
+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| CPU Management                    | If enabled, exclusive CPU cores can be allocated to workload pods. For details, see :ref:`CPU Policy <cce_10_0351>`.                                                                                                                                                                       |
+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Overload Control                  | After this function is enabled, concurrent requests will be dynamically controlled based on the resource demands received by master nodes to ensure the stable running of the master nodes and the cluster. For details, see :ref:`Enabling Overload Control for a Cluster <cce_10_0602>`. |
+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Disk Encryption for Master Nodes  | If enabled, dynamic data and static data on disks can be encrypted, providing powerful security protection for your data.                                                                                                                                                                  |
|                                   |                                                                                                                                                                                                                                                                                            |
|                                   | After encryption, the disk read/write performance deteriorates, and the configuration cannot be modified after the cluster is created.                                                                                                                                                     |
|                                   |                                                                                                                                                                                                                                                                                            |
|                                   | This function is available only for clusters of v1.25 or later.                                                                                                                                                                                                                            |
+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Resource Tag                      | You can add resource tags to classify resources. A maximum of 20 resource tags can be added.                                                                                                                                                                                               |
|                                   |                                                                                                                                                                                                                                                                                            |
|                                   | You can create **predefined tags** on the TMS console. The predefined tags are available to all resources that support tags. You can use predefined tags to improve the tag creation and resource migration efficiency.                                                                    |
+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Description                       | You can enter description for the cluster. A maximum of 200 characters are allowed.                                                                                                                                                                                                        |
+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Step 3: Select Add-ons
----------------------

Click **Next: Select Add-on**. On the page displayed, select the add-ons to be installed during cluster creation.

**Basic capabilities**

+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Add-on Name                         | Description                                                                                                                                                                                             |
+=====================================+=========================================================================================================================================================================================================+
| CCE Container Network (Yangtse CNI) | This is the basic cluster add-on. It provides network connectivity, Internet access, and security isolation for pods in your cluster.                                                                   |
+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| CCE Container Storage (Everest)     | This add-on (:ref:`CCE Container Storage (Everest) <cce_10_0066>`) is installed by default. It is a cloud native container storage system based on CSI and supports cloud storage services such as EVS. |
+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| CoreDNS                             | This add-on (:ref:`CoreDNS <cce_10_0129>`) is installed by default. It provides DNS resolution for your cluster and can be used to access the in-cloud DNS server.                                      |
+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**Observability**

+---------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Add-on Name                     | Description                                                                                                                                                                                                                                                                                                                                                                                                       |
+=================================+===================================================================================================================================================================================================================================================================================================================================================================================================================+
| Cloud Native Cluster Monitoring | (Optional) If selected, this add-on (:ref:`Cloud Native Cluster Monitoring <cce_10_0406>`) will be automatically installed. Cloud Native Cluster Monitoring collects monitoring metrics for your cluster and reports the metrics to AOM. The agent mode does not support HPA based on custom Prometheus statements. If related functions are required, install this add-on manually after the cluster is created. |
+---------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| CCE Node Problem Detector       | (Optional) If selected, this add-on (:ref:`CCE Node Problem Detector <cce_10_0132>`) will be automatically installed to detect faults and isolate nodes for prompt cluster troubleshooting.                                                                                                                                                                                                                       |
+---------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Step 4: Configure Add-ons
-------------------------

Click **Next: Add-on Configuration**.

**Basic capabilities**

+-------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Add-on Name                         | Description                                                                                                                                                 |
+=====================================+=============================================================================================================================================================+
| CCE Container Network (Yangtse CNI) | This add-on is unconfigurable.                                                                                                                              |
+-------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
| CCE Container Storage (Everest)     | This add-on is unconfigurable. After the cluster is created, choose **Add-ons** in the navigation pane of the cluster console and modify the configuration. |
+-------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
| CoreDNS                             | This add-on is unconfigurable. After the cluster is created, choose **Add-ons** in the navigation pane of the cluster console and modify the configuration. |
+-------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+

**Observability**

+---------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Add-on Name                     | Description                                                                                                                                                 |
+=================================+=============================================================================================================================================================+
| Cloud Native Cluster Monitoring | Select an AOM instance for Cloud Native Cluster Monitoring to report metrics. If no AOM instance is available, click **Creating Instance** to create one.   |
+---------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
| CCE Node Problem Detector       | This add-on is unconfigurable. After the cluster is created, choose **Add-ons** in the navigation pane of the cluster console and modify the configuration. |
+---------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+

Step 5: Confirm the Configuration
---------------------------------

After the parameters are specified, click **Next: Confirm configuration**. The cluster resource list is displayed. Confirm the information and click **Submit**.

It takes about 5 to 10 minutes to create a cluster. You can click **Back to Cluster List** to perform other operations on the cluster or click **Go to Cluster Events** to view the cluster details.

Related Operations
------------------

-  After creating a cluster, you can use the Kubernetes command line (CLI) tool kubectl to connect to the cluster. For details, see :ref:`Connecting to a Cluster Using kubectl <cce_10_0107>`.
-  Add nodes to the cluster. For details, see :ref:`Creating a Node <cce_10_0363>`.
