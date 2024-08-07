:original_name: cce_10_0036.html

.. _cce_10_0036:

Stopping a Node
===============

Scenario
--------

After a node in the cluster is stopped, services on the node are also stopped. Before stopping a node, ensure that discontinuity of the services on the node will not result in adverse impacts.

Constraints
-----------

-  Deleting a node will lead to pod migration, which may affect services. Therefore, delete nodes during off-peak hours.
-  Unexpected risks may occur during the operation. Back up related data in advance.
-  While the node is being deleted, the backend will set the node to the unschedulable state.
-  Only worker nodes can be stopped.

Procedure
---------

#. Log in to the CCE console and click the cluster name to access the cluster console.
#. In the navigation pane, choose **Nodes**. On the displayed page, click the **Nodes** tab.
#. Locate the target node and click its name.
#. In the upper right corner of the ECS details page, click **Stop**. In the displayed dialog box, click **Yes**.
