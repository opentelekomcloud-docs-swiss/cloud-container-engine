:original_name: cce_productdesc_0001.html

.. _cce_productdesc_0001:

What Is CCE?
============

Why CCE?
--------

CCE is a one-stop platform integrating compute (ECS), networking (VPC, EIP, and ELB), storage (EVS, OBS, and SFS), and many other services. Multi-AZ, multi-region disaster recovery ensures high availability of `Kubernetes <https://kubernetes.io/>`__ clusters.

For more information, see :ref:`Product Advantages <cce_productdesc_0003>` and :ref:`Application Scenarios <cce_productdesc_0004>`.

CCE Cluster Types
-----------------

There are multiple CCE products.

+--------------------------+--------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Category                 | Subcategory                    | CCE Standard                                                                                                                                                                             |
+==========================+================================+==========================================================================================================================================================================================+
| Positioning              | ``-``                          | Standard clusters that provide highly reliable and secure containers for commercial use                                                                                                  |
+--------------------------+--------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Application scenario     | ``-``                          | For users who expect to use container clusters to manage applications, obtain elastic computing resources, and enable simplified management on computing, network, and storage resources |
+--------------------------+--------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Specification difference | Network model                  | Cloud-native network 1.0: applies to common, smaller-scale scenarios.                                                                                                                    |
|                          |                                |                                                                                                                                                                                          |
|                          |                                | -  Tunnel network                                                                                                                                                                        |
|                          |                                | -  Virtual Private Cloud (VPC) network                                                                                                                                                   |
+--------------------------+--------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|                          | Network performance            | Overlays the VPC network with the container network, causing certain performance loss.                                                                                                   |
+--------------------------+--------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|                          | Network isolation              | -  Tunnel network model: supports network policies for intra-cluster communications.                                                                                                     |
|                          |                                | -  VPC network model: supports no isolation.                                                                                                                                             |
+--------------------------+--------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|                          | Security isolation             | Runs common containers, isolated by cgroups.                                                                                                                                             |
+--------------------------+--------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|                          | Edge infrastructure management | Not supported                                                                                                                                                                            |
+--------------------------+--------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
