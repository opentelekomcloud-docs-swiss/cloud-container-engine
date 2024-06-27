:original_name: cce_10_0281.html

.. _cce_10_0281:

Overview
========

The container network assigns IP addresses to pods in a cluster and provides networking services. In CCE, you can select the following network models for your cluster:

-  :ref:`Tunnel network <cce_10_0282>`
-  :ref:`VPC network <cce_10_0283>`

Network Model Comparison
------------------------

:ref:`Table 1 <cce_10_0281__en-us_topic_0146398798_table715802210336>` describes the differences of network models supported by CCE.

.. caution::

   After a cluster is created, the network model cannot be changed.

.. _cce_10_0281__en-us_topic_0146398798_table715802210336:

.. table:: **Table 1** Network model comparison

   +------------------------+-----------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Dimension              | Tunnel Network                                                                                                                    | VPC Network                                                                                                                                                          |
   +========================+===================================================================================================================================+======================================================================================================================================================================+
   | Application scenarios  | -  Common container service scenarios                                                                                             | -  Scenarios that have high requirements on network latency and bandwidth                                                                                            |
   |                        | -  Scenarios that do not have high requirements on network latency and bandwidth                                                  | -  Containers can communicate with VMs using a microservice registration framework, such as Dubbo and CSE.                                                           |
   +------------------------+-----------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Core technology        | OVS                                                                                                                               | IPvlan and VPC route                                                                                                                                                 |
   +------------------------+-----------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Applicable clusters    | CCE standard cluster                                                                                                              | CCE standard cluster                                                                                                                                                 |
   +------------------------+-----------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Network isolation      | Kubernetes native NetworkPolicy for pods                                                                                          | No                                                                                                                                                                   |
   +------------------------+-----------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Passthrough networking | No                                                                                                                                | No                                                                                                                                                                   |
   +------------------------+-----------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | IP address management  | -  The container CIDR block is allocated separately.                                                                              | -  The container CIDR block is allocated separately.                                                                                                                 |
   |                        | -  CIDR blocks are divided by node and can be dynamically allocated (CIDR blocks can be dynamically added after being allocated.) | -  CIDR blocks are divided by node and statically allocated (the CIDR block cannot be changed after a node is created).                                              |
   +------------------------+-----------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Network performance    | Performance loss due to VXLAN encapsulation                                                                                       | No tunnel encapsulation. Cross-node packets are forwarded through VPC routers, delivering performance equivalent to that of the host network.                        |
   +------------------------+-----------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Networking scale       | A maximum of 2000 nodes are supported.                                                                                            | Suitable for small- and medium-scale networks due to the limitation on VPC routing tables. It is recommended that the number of nodes be less than or equal to 1000. |
   |                        |                                                                                                                                   |                                                                                                                                                                      |
   |                        |                                                                                                                                   | Each time a node is added to the cluster, a route is added to the VPC routing tables. Therefore, the cluster scale is limited by the VPC routing tables.             |
   +------------------------+-----------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. important::

   #. The scale of a cluster that uses the VPC network model is limited by the custom routes of the VPC. Therefore, estimate the number of required nodes before creating a cluster.
   #. By default, VPC routing network supports direct communication between containers and hosts in the same VPC. If a peering connection policy is configured between the VPC and another VPC, the containers can directly communicate with hosts on the peer VPC. In addition, in hybrid networking scenarios such as Direct Connect and VPN, communication between containers and hosts on the peer end can also be achieved with proper planning.
   #. Do not change the mask of the primary CIDR block on the VPC after a cluster is created. Otherwise, the network will be abnormal.
