:original_name: cce_bestpractice_00004.html

.. _cce_bestpractice_00004:

Planning CIDR Blocks for a Cluster
==================================

Before creating a cluster on CCE, determine the number of VPCs, number of subnets, container CIDR blocks, and Services for access based on service requirements.

This topic describes the addresses in a CCE cluster in a VPC and how to plan CIDR blocks.

Constraints
-----------

To access a CCE cluster through a VPN, ensure that the VPN does not conflict with the VPC CIDR block where the cluster resides and the container CIDR block.

Basic Concepts
--------------

-  **VPC CIDR Block**

   Virtual Private Cloud (VPC) enables you to provision logically isolated, configurable, and manageable virtual networks for cloud servers, cloud containers, and cloud databases. You have complete control over your virtual network, including selecting your own CIDR block, creating subnets, and configuring security groups. You can also assign EIPs and allocate bandwidth in your VPC for secure and easy access to your business system.

-  **Subnet CIDR Block**

   A subnet is a network that manages ECS network planes. It supports IP address management and DNS. The IP addresses of all ECSs in a subnet belong to the subnet.


   .. figure:: /_static/images/en-us_image_0000001851585668.png
      :alt: **Figure 1** VPC CIDR block architecture

      **Figure 1** VPC CIDR block architecture

   By default, ECSs in all subnets of the same VPC can communicate with one another, while ECSs in different VPCs cannot communicate with each other.

   You can create a peering connection on VPC to enable ECSs in different VPCs to communicate with each other.

-  **Container (Pod) CIDR Block**

   Pod is a Kubernetes concept. Each pod has an IP address.

   When creating a cluster on CCE, you can specify the pod (container) CIDR block, which cannot overlap with the subnet CIDR block. For example, if the subnet CIDR block is 192.168.0.0/16, the container CIDR block cannot be 192.168.0.0/18 or 192.168.1.0/18, because these addresses are included in 192.168.0.0/16.

-  **Service CIDR Block**

   Service is also a Kubernetes concept. Each Service has an address. When creating a cluster on CCE, you can specify the Service CIDR block. Similarly, the Service CIDR block cannot overlap with the subnet CIDR block or the container CIDR block. The Service CIDR block can be used only within a cluster.

Single-VPC Single-Cluster Scenarios
-----------------------------------

**CCE Clusters**: include clusters in VPC network model and container tunnel network model. :ref:`Figure 2 <cce_bestpractice_00004__en-us_topic_0099587154_fig15791152874920>` shows the CIDR block planning of a cluster.

-  VPC CIDR Block: specifies the VPC CIDR block where the cluster resides. The size of this CIDR block affects the maximum number of nodes that can be created in the cluster.
-  Subnet CIDR Block: specifies the subnet CIDR block where the node in the cluster resides. The subnet CIDR block is included in the VPC CIDR block. Different nodes in the same cluster can be allocated to different subnet CIDR blocks.
-  Container CIDR Block: cannot overlap with the subnet CIDR block.
-  Service CIDR Block: cannot overlap with the subnet CIDR block or the container CIDR block.

.. _cce_bestpractice_00004__en-us_topic_0099587154_fig15791152874920:

.. figure:: /_static/images/en-us_image_0000001851744412.png
   :alt: **Figure 2** Network CIDR block planning in single-VPC single-cluster scenarios (CCE cluster)

   **Figure 2** Network CIDR block planning in single-VPC single-cluster scenarios (CCE cluster)

Single-VPC Multi-Cluster Scenarios
----------------------------------

**VPC network model**

Pod packets are forwarded through VPC routes. CCE automatically configures a routing table on the VPC routes to each container CIDR block. The network scale is limited by the VPC route table. :ref:`Figure 3 <cce_bestpractice_00004__en-us_topic_0099587154_fig69527530400>` shows the CIDR block planning of the cluster.

-  VPC CIDR Block: specifies the VPC CIDR block where the cluster resides. The size of this CIDR block affects the maximum number of nodes that can be created in the cluster.
-  Subnet CIDR Block: The subnet CIDR block in each cluster cannot overlap with the container CIDR block.
-  Container CIDR Block: If multiple VPC network model clusters exist in a single VPC, the container CIDR blocks of all clusters cannot overlap because the clusters use the same routing table. In this case, if the node security group allows container CIDR block from the peer cluster, pods in one cluster can directly access pods in another cluster through the pod IP addresses.
-  Service CIDR Block: can be used only in clusters. Therefore, the Service CIDR blocks of different clusters can overlap, but cannot overlap with the subnet CIDR block and container CIDR block of the cluster.

.. _cce_bestpractice_00004__en-us_topic_0099587154_fig69527530400:

.. figure:: /_static/images/en-us_image_0000001897905069.png
   :alt: **Figure 3** VPC network - multi-cluster scenario

   **Figure 3** VPC network - multi-cluster scenario

**Tunnel network model**

Though at some cost of performance, the tunnel encapsulation enables higher interoperability and compatibility with advanced features (such as network policy-based isolation), meeting the requirements of most applications. :ref:`Figure 4 <cce_bestpractice_00004__en-us_topic_0099587154_fig8672112184219>` shows the CIDR block planning of the cluster.

-  VPC CIDR Block: specifies the VPC CIDR block where the cluster resides. The size of this CIDR block affects the maximum number of nodes that can be created in the cluster.
-  Subnet CIDR Block: The subnet CIDR block in each cluster cannot overlap with the container CIDR block.
-  Container CIDR Block: The container CIDR blocks of all clusters can overlap. In this case, pods in different clusters cannot be directly accessed through pod IP addresses. Pods in different clusters need to access each other through Services. The LoadBlancer Services are recommended.
-  Service CIDR Block: can be used only in clusters. Therefore, the Service CIDR blocks of different clusters can overlap, but cannot overlap with the subnet CIDR block and container CIDR block of the cluster.

.. _cce_bestpractice_00004__en-us_topic_0099587154_fig8672112184219:

.. figure:: /_static/images/en-us_image_0000001898024565.png
   :alt: **Figure 4** Tunnel network - multi-cluster scenario

   **Figure 4** Tunnel network - multi-cluster scenario

**Clusters using different networks**

When a VPC contains clusters created with different network models, comply with the following rules when creating a cluster:

-  VPC CIDR Block: In this scenario, all clusters are located in the same VPC CIDR block. Ensure that there are sufficient available IP addresses in the VPC.
-  Subnet CIDR Block: Ensure that the subnet CIDR block does not overlap with the container CIDR block.
-  Container CIDR Block: Ensure that the container CIDR blocks of clusters in **VPC network model** do not overlap.
-  Service CIDR Block: The Service CIDR blocks of all clusters can overlap, but cannot overlap with the subnet CIDR block and container CIDR block of the cluster.

Cross-VPC Cluster Interconnection
---------------------------------

If VPCs cannot communicate with each other, a VPC peering connection is used to ensure communication between VPCs. When two VPC networks are interconnected, you can configure the packets to be sent to the peer VPC in the route table.

**Clusters using VPC networks**

To allow clusters that use VPC networks to access each other across VPCs, add routes to the two ends of the VPC peering after a VPC peering connection is created.


.. figure:: /_static/images/en-us_image_0000001897905025.png
   :alt: **Figure 5** VPC network - VPC interconnection scenario

   **Figure 5** VPC network - VPC interconnection scenario

When creating a VPC peering connection between containers across VPCs, pay attention to the following points:

-  The VPC to which the clusters belong must not overlap. In each cluster, the subnet CIDR block cannot overlap with the container CIDR block.
-  The container CIDR blocks of clusters at both ends cannot overlap, but the Service CIDR blocks can.
-  If the request end cluster uses the VPC network, check whether the node security group in the destination cluster allows the container CIDR block of the request end cluster. If yes, pods in one cluster can directly access pods in another cluster through the pod IP address. Similarly, if nodes running in the clusters at the two ends of the VPC peering connection need to access each other, the node security group must allow the VPC CIDR block of the peer cluster.
-  You need to add routes for accessing the peer network CIDR block to the VPC routing tables at both ends. For example, you need to add a route for accessing the CIDR block of VPC 2 to the route table of VPC 1, and add a route for accessing VPC 1 to the route table of VPC 2.

   -  **Add the VPC CIDR block of the peer cluster**: After the route of the VPC CIDR block is added, a pod in a cluster can access another cluster node. For example, the pod can access the port of a NodePort Service.
   -  **Add peer container CIDR block**: After the route of the container CIDR block is added, a pod can directly access pods in another cluster through the container IP addresses.

**Clusters using tunnel networks**

To allow clusters that use tunnel networks to access each other across VPCs, add routes to the two ends of the VPC peering after a VPC peering connection is created.


.. figure:: /_static/images/en-us_image_0000001897905033.png
   :alt: **Figure 6** Tunnel network - VPC interconnection scenario

   **Figure 6** Tunnel network - VPC interconnection scenario

Pay attention to the following:

-  The VPCs of the peer clusters must not overlap.
-  The container CIDR blocks of all clusters can overlap, so do the Service CIDR blocks.
-  If the request end cluster uses the tunnel network, check whether the node security group in the destination cluster allows the VPC CIDR block (including the node subnets) of the request end cluster. If yes, nodes in one cluster can access nodes in another cluster. However, pods in different clusters cannot be directly accessed using pod IP addresses. Access between pods in different clusters requires Services. The LoadBlancer Services are recommended.
-  The VPC CIDR block route of the peer cluster must be added to the VPC routing tables of both ends. For example, you need to add a route for accessing the CIDR block of VPC 2 to the route table of VPC 1, and add a route for accessing VPC 1 to the route table of VPC 2. After the route of the VPC CIDR block is added, the pod can access another cluster node, for example, accessing the port of a NodePort Service.

**Clusters using different networks**

If clusters using different networks need to communicate with each other across VPCs, every one of them may serve as the request end or destination end. Pay attention to the following:

-  The VPC CIDR block to which the cluster belongs cannot overlap with the VPC CIDR block of the peer cluster.

-  Cluster subnet CIDR blocks cannot overlap with the container CIDR blocks.

-  Container CIDR blocks in different clusters cannot overlap with each other.

-  If pods or nodes in different clusters need to access each other, the security groups of the clusters on both ends must allow the corresponding CIDR blocks based on the following rules:

   -  If the request end cluster uses the VPC network, the node security group of the destination cluster must allow the VPC CIDR block (including the node subnets and container CIDR block) of the request end cluster.
   -  If the request end cluster uses the tunnel network, the node security group of the destination cluster must allow the VPC CIDR block (including the node subnets) of the request end cluster.

-  The VPC CIDR block route of the peer cluster must be added to the VPC routing tables of both ends. For example, you need to add a route for accessing the CIDR block of VPC 2 to the route table of VPC 1, and add a route for accessing VPC 1 to the route table of VPC 2. After the route of the VPC CIDR block is added, the pod can access another cluster node, for example, accessing the port of a NodePort Service.

   If a cluster uses the VPC network, the VPC routing tables at both ends must contain its container CIDR block. After the container CIDR block route is added, the pod can directly access pods in another cluster through the container IP addresses.

VPC-IDC Scenarios
-----------------

Similar to the VPC interconnection scenario, some CIDR blocks in the VPC are routed to the IDC. The pod IP addresses of CCE clusters cannot overlap with the addresses within these CIDR blocks. To access the pod IP addresses in the cluster in the IDC, configure the route table to the private line VBR on the IDC.
