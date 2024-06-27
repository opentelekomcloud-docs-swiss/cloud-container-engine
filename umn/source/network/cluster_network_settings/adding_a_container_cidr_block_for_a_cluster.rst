:original_name: cce_10_0680.html

.. _cce_10_0680:

Adding a Container CIDR Block for a Cluster
===========================================

Scenario
--------

If the container CIDR block set during CCE cluster creation is insufficient, you can add a container CIDR block for the cluster.

Constraints
-----------

-  This function applies to CCE standard clusters of v1.19 or later, but not to clusters using container tunnel networking.
-  The container CIDR block or container subnet cannot be deleted after being added. Exercise caution when performing this operation.

Adding a Container CIDR Block for a CCE Standard Cluster
--------------------------------------------------------

#. Log in to the CCE console and click the cluster name to access the cluster console.
#. On the **Overview** page, locate the **Networking Configuration** area and click **Add**.
#. Configure the container CIDR block to be added. You can click |image1| to add multiple container CIDR blocks at a time.

   .. note::

      New container CIDR blocks cannot conflict with service CIDR blocks, VPC CIDR blocks, and existing container CIDR blocks.

#. Click **OK**.

.. |image1| image:: /_static/images/en-us_image_0000001898025869.png
