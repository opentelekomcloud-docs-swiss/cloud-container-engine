:original_name: cce_10_0184.html

.. _cce_10_0184:

Synchronizing Data with Cloud Servers
=====================================

Scenario
--------

Each node in a cluster is a cloud server or physical machine. After a cluster node is created, you can change the cloud server name or specifications as required.

Some information about CCE nodes is maintained independently from the ECS console. After you change the name, EIP, or specifications of an ECS on the ECS console, you need to **synchronize the ECS information** to the corresponding node on the CCE console. After the synchronization, information on both consoles is consistent.

Notes and Constraints
---------------------

-  Data, including the VM status, ECS names, number of CPUs, size of memory, ECS specifications, and public IP addresses, can be synchronized.

   If an ECS name is specified as the Kubernetes node name, the change of the ECS name cannot be synchronized to the CCE console.

-  Data, such as the OS and image ID, cannot be synchronized. (Such parameters cannot be modified on the ECS console.)

Procedure
---------

#. Log in to the CCE console.

#. Click the cluster name and access the cluster console. Choose **Nodes** in the navigation pane.

#. Choose **More** > **Sync Server Data** next to the node.


   .. figure:: /_static/images/en-us_image_0000001517743520.png
      :alt: **Figure 1** Synchronizing server data

      **Figure 1** Synchronizing server data

   After the synchronization is complete, the **ECS data synchronization requested** message is displayed in the upper right corner.
