:original_name: cce_10_0084.html

.. _cce_10_0084:

Enabling ICMP Security Group Rules
==================================

Scenario
--------

If a workload uses UDP for both load balancing and health check, enable ICMP security group rules for the backend servers.

Procedure
---------

#. Log in to the CCE console, choose **Service List** > **Networking** > **Virtual Private Cloud**, and choose **Access Control** > **Security Groups** in the navigation pane.
#. In the security group list, locate the security group of the cluster. Click the **Inbound Rules** tab page and then **Add Rule**. In the **Add Inbound Rule** dialog box, configure inbound parameters.

   +--------------+-------------+---------------------------------------------------------------------------------------------+-----------------+---------------------------------------------+
   | Cluster Type | ELB Type    | Security Group                                                                              | Protocol & Port | Allowed Source CIDR Block                   |
   +==============+=============+=============================================================================================+=================+=============================================+
   | CCE Standard | Shared      | Node security group, which is named in the format of "{Cluster name}-cce-node-{Random ID}". | All ICMP ports  | 100.125.0.0/16 for the shared load balancer |
   |              |             |                                                                                             |                 |                                             |
   |              |             | If a custom node security group is bound to the cluster, select the target security group.  |                 |                                             |
   +--------------+-------------+---------------------------------------------------------------------------------------------+-----------------+---------------------------------------------+
   |              | Dedicated   | Node security group, which is named in the format of "{Cluster name}-cce-node-{Random ID}". | All ICMP ports  | Backend subnet of the load balancer         |
   |              |             |                                                                                             |                 |                                             |
   |              |             | If a custom node security group is bound to the cluster, select the target security group.  |                 |                                             |
   +--------------+-------------+---------------------------------------------------------------------------------------------+-----------------+---------------------------------------------+

#. Click **OK**.
