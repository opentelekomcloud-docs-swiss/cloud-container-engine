:original_name: cce_10_0175.html

.. _cce_10_0175:

Connecting to a Cluster Using an X.509 Certificate
==================================================

Scenario
--------

This section describes how to obtain the cluster certificate from the console and use it access Kubernetes clusters.

Procedure
---------

#. Log in to the CCE console and click the cluster name to access the cluster console.

#. On the **Overview** page, locate the **Connection Info** area, and click **Download** next to **X.509 certificate**.

#. In the **Obtain Certificate** dialog box displayed, select the certificate expiration time and download the X.509 certificate of the cluster as prompted.

   .. important::

      -  The downloaded certificate contains three files: **client.key**, **client.crt**, and **ca.crt**. Keep these files secure.
      -  Certificates are not required for mutual access between containers in a cluster.

#. Call native Kubernetes APIs using the cluster certificate.

   For example, run the **curl** command to call an API to view the pod information. In the following information, *192.168.0.18:5443* indicates the IP address of the API server in the cluster.

   .. code-block::

      curl --cacert ./ca.crt --cert ./client.crt --key ./client.key  https://192.168.0.18:5443/api/v1/namespaces/default/pods/

   For more cluster APIs, see `Kubernetes APIs <https://kubernetes.io/docs/reference/kubernetes-api/>`__.
