:original_name: cce_bestpractice_00222.html

.. _cce_bestpractice_00222:

Creating an IPv4/IPv6 Dual-Stack Cluster in CCE
===============================================

This section describes how to set up a VPC with IPv6 CIDR block and create a cluster and nodes with an IPv6 address in the VPC, so that the nodes can access the Internet.

Overview
--------

IPv6 addresses are used to deal with the problem of IPv4 address exhaustion. If a worker node (such as an ECS) in the current cluster uses IPv4, the node can run in dual-stack mode after IPv6 is enabled. Specifically, the node has both IPv4 and IPv6 addresses, which can be used to access the intranet or public network.

Application Scenarios
---------------------

-  If your application needs to provide Services for users who use IPv6 clients, you can use IPv6 EIPs or the IPv4 and IPv6 dual-stack function.
-  If your application needs to both provide Services for users who use IPv6 clients and analyze the access request data, you can use only the IPv4 and IPv6 dual-stack function.
-  If internal communication is required between your application systems or between your application system and another system (such as the database system), you can use only the IPv4 and IPv6 dual-stack function.

Constraints
-----------

-  Clusters that support IPv4/IPv6 dual stack:

   +-----------------+--------------------------+-----------------+-------------------------------------------------------------------------+
   | Cluster Type    | Cluster Network Model    | Version         | Remarks                                                                 |
   +=================+==========================+=================+=========================================================================+
   | CCE cluster     | Container tunnel network | v1.15 or later  | IPv4/IPv6 dual stack will be generally available for clusters of v1.23. |
   |                 |                          |                 |                                                                         |
   |                 |                          |                 | ELB dual stack is not supported.                                        |
   +-----------------+--------------------------+-----------------+-------------------------------------------------------------------------+

-  Worker nodes and master nodes in Kubernetes clusters use IPv4 addresses to communicate with each other.
-  Only one IPv6 address can be bound to each NIC.
-  When IPv4/IPv6 dual stack is enabled for the cluster, DHCP unlimited lease cannot be enabled for the selected node subnet.
-  If a dual-stack cluster is used, do not change the load balancer protocol version on the ELB console.

Step 1: Create a VPC
--------------------

Before creating your VPCs, determine how many VPCs, the number of subnets, and what IP address ranges you will need.

.. note::

   -  The basic operations for IPv4 and IPv6 dual-stack networks are the same as those for IPv4 networks. Only some parameters are different.

Perform the following operations to create a VPC named **vpc-ipv6** and its default subnet named **subnet-ipv6**.

#. Log in to the management console.

#. Click |image1| in the upper left corner of the management console and select a region and a project.

#. Choose **Networking** > **Virtual Private Cloud**.

#. Click **Create VPC**.

#. Set the VPC and subnet parameters.

   When configuring a subnet, select **Enable** for **IPv6 CIDR Block** to automatically allocate an IPv6 CIDR block to the subnet. IPv6 cannot be disabled after the subnet is created. Currently, you are not allowed to specify a custom IPv6 CIDR block.

   .. table:: **Table 1** VPC configuration parameters

      +-------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------+
      | Parameter               | Description                                                                                                                                                                                                                                                                                                                           | Example Value            |
      +=========================+=======================================================================================================================================================================================================================================================================================================================================+==========================+
      | Region                  | Specifies the desired region. Regions are geographic areas that are physically isolated from each other. The networks inside different regions are not connected to each other, so resources cannot be shared across different regions. For lower network latency and faster access to your resources, select the region nearest you. | ``-``                    |
      +-------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------+
      | Name                    | VPC name.                                                                                                                                                                                                                                                                                                                             | vpc-ipv6                 |
      +-------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------+
      | IPv4 CIDR Block         | Specifies the Classless Inter-Domain Routing (CIDR) block of the VPC. The CIDR block of a subnet can be the same as the CIDR block for the VPC (for a single subnet in the VPC) or a subset (for multiple subnets in the VPC).                                                                                                        | 192.168.0.0/16           |
      |                         |                                                                                                                                                                                                                                                                                                                                       |                          |
      |                         | The following CIDR blocks are supported:                                                                                                                                                                                                                                                                                              |                          |
      |                         |                                                                                                                                                                                                                                                                                                                                       |                          |
      |                         | 10.0.0.0/8-24                                                                                                                                                                                                                                                                                                                         |                          |
      |                         |                                                                                                                                                                                                                                                                                                                                       |                          |
      |                         | 172.16.0.0/12-24                                                                                                                                                                                                                                                                                                                      |                          |
      |                         |                                                                                                                                                                                                                                                                                                                                       |                          |
      |                         | 192.168.0.0/16-24                                                                                                                                                                                                                                                                                                                     |                          |
      +-------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------+
      | Tag (Advanced Settings) | Specifies the VPC tag, which consists of a key and value pair. You can add a maximum of ten tags for each VPC.                                                                                                                                                                                                                        | -  **Tag key**: vpc_key1 |
      |                         |                                                                                                                                                                                                                                                                                                                                       | -  **Key value**: vpc-01 |
      |                         | The tag key and value must meet the requirements listed in :ref:`Table 3 <cce_bestpractice_00222__en-us_topic_0226102195_en-us_topic_0213478735_en-us_topic_0118066459_table63360804153019>`.                                                                                                                                         |                          |
      +-------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------+

   .. table:: **Table 2** Subnet parameters

      +------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------+
      | Parameter              | Description                                                                                                                                                                                                                          | Example Value               |
      +========================+======================================================================================================================================================================================================================================+=============================+
      | Name                   | Specifies the subnet name.                                                                                                                                                                                                           | subnet-ipv6                 |
      +------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------+
      | IPv4 CIDR Block        | Specifies the IPv4 CIDR block for the subnet. This value must be within the VPC CIDR range.                                                                                                                                          | 192.168.0.0/24              |
      +------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------+
      | IPv6 CIDR Block        | Select **Enable** for **IPv6 CIDR Block**. An IPv6 CIDR block will be automatically assigned to the subnet. IPv6 cannot be disabled after the subnet is created. Currently, you are not allowed to specify a custom IPv6 CIDR block. | N/A                         |
      +------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------+
      | Associated Route Table | Specifies the default route table to which the subnet will be associated. You can change the route table to a custom route table.                                                                                                    | Default                     |
      +------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------+
      | Advanced Settings      |                                                                                                                                                                                                                                      |                             |
      +------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------+
      | Gateway                | Specifies the gateway address of the subnet.                                                                                                                                                                                         | 192.168.0.1                 |
      |                        |                                                                                                                                                                                                                                      |                             |
      |                        | This IP address is used to communicate with other subnets.                                                                                                                                                                           |                             |
      +------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------+
      | DNS Server Address     | By default, two DNS server addresses are configured. You can change them if necessary. When multiple IP addresses are available, separate them with a comma (,).                                                                     | 100.125.x.x                 |
      +------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------+
      | Tag                    | Specifies the subnet tag, which consists of a key and value pair. You can add a maximum of ten tags to each subnet.                                                                                                                  | -  **Tag key**: subnet_key1 |
      |                        |                                                                                                                                                                                                                                      | -  **Key value**: subnet-01 |
      |                        | The tag key and value must meet the requirements listed in :ref:`Table 4 <cce_bestpractice_00222__en-us_topic_0226102195_en-us_topic_0213478735_en-us_topic_0118066459_table4168255153519>`.                                         |                             |
      +------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------+

   .. _cce_bestpractice_00222__en-us_topic_0226102195_en-us_topic_0213478735_en-us_topic_0118066459_table63360804153019:

   .. table:: **Table 3** VPC tag key and value requirements

      +-----------------------+--------------------------------------------------------------------------------+-----------------------+
      | Parameter             | Requirement                                                                    | Example Value         |
      +=======================+================================================================================+=======================+
      | Tag key               | -  Cannot be left blank.                                                       | vpc_key1              |
      |                       | -  Must be unique in a VPC.                                                    |                       |
      |                       | -  Can contain a maximum of 36 characters.                                     |                       |
      |                       | -  Can contain letters, digits, underscores (_), and hyphens (-).              |                       |
      +-----------------------+--------------------------------------------------------------------------------+-----------------------+
      | Tag value             | -  Can contain a maximum of 43 characters.                                     | vpc-01                |
      |                       | -  Can contain letters, digits, underscores (_), periods (.), and hyphens (-). |                       |
      +-----------------------+--------------------------------------------------------------------------------+-----------------------+

   .. _cce_bestpractice_00222__en-us_topic_0226102195_en-us_topic_0213478735_en-us_topic_0118066459_table4168255153519:

   .. table:: **Table 4** Subnet tag key and value requirements

      +-----------------------+--------------------------------------------------------------------------------+-----------------------+
      | Parameter             | Requirement                                                                    | Example Value         |
      +=======================+================================================================================+=======================+
      | Tag key               | -  Cannot be left blank.                                                       | subnet_key1           |
      |                       | -  Must be unique for each subnet.                                             |                       |
      |                       | -  Can contain a maximum of 36 characters.                                     |                       |
      |                       | -  Can contain letters, digits, underscores (_), and hyphens (-).              |                       |
      +-----------------------+--------------------------------------------------------------------------------+-----------------------+
      | Tag value             | -  Can contain a maximum of 43 characters.                                     | subnet-01             |
      |                       | -  Can contain letters, digits, underscores (_), periods (.), and hyphens (-). |                       |
      +-----------------------+--------------------------------------------------------------------------------+-----------------------+

#. Click **Create Now**.

Step 2: Create a CCE Cluster
----------------------------

**Creating a CCE cluster**

#. Log in to the CCE console and create a cluster.

   Complete the network settings as follows.

   -  **Network Model**: Select **Tunnel network**.
   -  **VPC**: Select the created VPC **vpc-ipv6**.
   -  **Subnet**: Select a subnet with IPv6 enabled.
   -  **IPv6**: Enable this function. After this function is enabled, cluster resources, including nodes and workloads, can be accessed through IPv6 CIDR blocks.
   -  **Container CIDR Block**: A proper mask must be set for the container CIDR block. The mask determines the number of available nodes in the cluster. If the mask of the container CIDR block in the cluster is set improperly, there will be only a small number of available nodes in the cluster.

#. Create a node.

   The CCE console displays the nodes that support IPv6. You can directly select a node.

   After the creation is complete, access the cluster details page. Then, click the node name to go to the ECS details page and view the automatically allocated IPv6 address.

Step 3: Apply for a Shared Bandwidth and Adding an IPv6 Address to It
---------------------------------------------------------------------

By default, the IPv6 address can only be used for private network communication. If you want to use this IPv6 address to access the Internet or be accessed by IPv6 clients on the Internet, apply for a shared bandwidth and add the IPv6 address to it.

If you already have a shared bandwidth, you can add the IPv6 address to the shared bandwidth without applying for one.

**Applying a Shared Bandwidth**

#. Log in to the management console.
#. Click |image2| in the upper left corner of the management console and select a region and a project.
#. Choose **Service List** > **Networking** > **Virtual Private Cloud**.
#. In the navigation pane, choose **Elastic IP and Bandwidth** > **Shared Bandwidths**.
#. In the upper right corner, click **Assign Shared Bandwidth**. On the displayed page, configure parameters following instructions.

   .. table:: **Table 5** Parameters

      +-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
      | Parameter | Description                                                                                                                                                                                                                                                                                                                           | Example Value |
      +===========+=======================================================================================================================================================================================================================================================================================================================================+===============+
      | Region    | Specifies the desired region. Regions are geographic areas that are physically isolated from each other. The networks inside different regions are not connected to each other, so resources cannot be shared across different regions. For lower network latency and faster access to your resources, select the region nearest you. | ``-``         |
      +-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
      | Bandwidth | Specifies the shared bandwidth size in Mbit/s. The minimum bandwidth that can be purchased is 5 Mbit/s.                                                                                                                                                                                                                               | 10            |
      +-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
      | Name      | Specifies the name of the shared bandwidth.                                                                                                                                                                                                                                                                                           | Bandwidth-001 |
      +-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+

#. Click **Create Now**.

**Adding an IPv6 Address to a Shared Bandwidth**

#. On the shared bandwidth list page, locate the row containing the target shared bandwidth and click **Add Public IP Address** in the **Operation** column.
#. Add the IPv6 address to the shared bandwidth.
#. Click **OK**.

**Verifying the Result**

Log in to an ECS and ping an IPv6 address on the Internet to verify the connectivity. **ping6 ipv6.baidu.com** is used as an example here. The execution result is displayed in :ref:`Figure 1 <cce_bestpractice_00222__en-us_topic_0226102195_en-us_topic_0213478735_en-us_topic_0118066459_fig12339172511196>`.

.. _cce_bestpractice_00222__en-us_topic_0226102195_en-us_topic_0213478735_en-us_topic_0118066459_fig12339172511196:

.. figure:: /_static/images/en-us_image_0000001981274817.png
   :alt: **Figure 1** Result verification

   **Figure 1** Result verification

.. |image1| image:: /_static/images/en-us_image_0000001950315312.png
.. |image2| image:: /_static/images/en-us_image_0000001981274869.png
