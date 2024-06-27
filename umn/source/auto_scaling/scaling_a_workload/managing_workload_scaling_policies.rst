:original_name: cce_10_0083.html

.. _cce_10_0083:

Managing Workload Scaling Policies
==================================

Scenario
--------

After an HPA or CustomedHPA policy is created, you can update and delete the policy, as well as edit the YAML file.

Checking an HPA Policy
----------------------

You can view the rules, status, and events of an HPA policy and handle exceptions based on the error information displayed.

#. Log in to the CCE console and click the cluster name to access the cluster console.
#. In the navigation pane, choose **Policies**. On the page displayed, click the **HPA Policies** tab and then |image1| next to the target HPA policy.
#. In the expanded area, choose **View Events** in the **Operation** column. If the policy malfunctions, locate and rectify the fault based on the error message displayed on the page.

   .. note::

      You can also view the created HPA policy on the workload details page.

      a. Log in to the CCE console and click the cluster name to access the cluster console.
      b. In the navigation pane, choose **Workloads**. Click the workload name to view its details.
      c. On the workload details page, switch to the **Auto Scaling** tab page to view the HPA policies or CustomedHPA policies. You can also view the scaling policies you configured on the **Policies** page.

   .. table:: **Table 1** Event types and names

      +------------+------------------------------+----------------------------------------------+
      | Event Type | Event Name                   | Description                                  |
      +============+==============================+==============================================+
      | Normal     | SuccessfulRescale            | The scaling is performed successfully.       |
      +------------+------------------------------+----------------------------------------------+
      | Abnormal   | InvalidTargetRange           | Invalid target range.                        |
      +------------+------------------------------+----------------------------------------------+
      |            | InvalidSelector              | Invalid selector.                            |
      +------------+------------------------------+----------------------------------------------+
      |            | FailedGetObjectMetric        | Objects fail to be obtained.                 |
      +------------+------------------------------+----------------------------------------------+
      |            | FailedGetPodsMetric          | Pods fail to be obtained.                    |
      +------------+------------------------------+----------------------------------------------+
      |            | FailedGetResourceMetric      | Resources fail to be obtained.               |
      +------------+------------------------------+----------------------------------------------+
      |            | FailedGetExternalMetric      | External metrics fail to be obtained.        |
      +------------+------------------------------+----------------------------------------------+
      |            | InvalidMetricSourceType      | Invalid metric source type.                  |
      +------------+------------------------------+----------------------------------------------+
      |            | FailedConvertHPA             | HPA conversion failed.                       |
      +------------+------------------------------+----------------------------------------------+
      |            | FailedGetScale               | The scale fails to be obtained.              |
      +------------+------------------------------+----------------------------------------------+
      |            | FailedComputeMetricsReplicas | Failed to calculate metric-defined replicas. |
      +------------+------------------------------+----------------------------------------------+
      |            | FailedGetScaleWindow         | Failed to obtain ScaleWindow.                |
      +------------+------------------------------+----------------------------------------------+
      |            | FailedRescale                | Failed to scale the service.                 |
      +------------+------------------------------+----------------------------------------------+

Viewing a CustomedHPA Policy
----------------------------

You can view the rules and latest status of a CustomedHPA policy and rectify faults based on the error information displayed.

#. Log in to the CCE console and click the cluster name to access the cluster console.
#. In the navigation pane, choose **Policies**. On the page displayed, click the **CustomHPA Policies** tab and then |image2| next to the target CustomHPA policy.
#. In the expanded area, if the policy is abnormal on the **Rules** tab page, click **Details** in **Latest Status** and locate the fault based on the information displayed.

   .. note::

      You can also view the created HPA policy on the workload details page.

      a. Log in to the CCE console and click the cluster name to access the cluster console.
      b. In the navigation pane, choose **Workloads**. Click the workload name to view its details.
      c. On the workload details page, switch to the **Auto Scaling** tab page to view the HPA policies or CustomedHPA policies. You can also view the scaling policies you configured on the **Policies** page.

Editing an HPA or CustomedHPA Policy
------------------------------------

An HPA policy is used as an example.

#. Log in to the CCE console and click the cluster name to access the cluster console.
#. In the navigation pane, choose **Policies**. On the page displayed, click the **HPA Policies** tab. Locate the row containing the target policy and choose **More** > **Edit** in the **Operation** column.
#. On the **Edit HPA Policy** page, configure policy parameters listed in :ref:`Table 1 <cce_10_0208__table8638121213265>`.
#. Click **OK**.

Editing the YAML File (HPA Policy)
----------------------------------

#. Log in to the CCE console and click the cluster name to access the cluster console.
#. In the navigation pane, choose **Policies**. On the page displayed, click the **HPA Policies** tab. Locate the row containing the target policy and click **Edit YAML** in the **Operation** column.
#. In the dialog box displayed, edit or download the YAML file.

Viewing the YAML File (CustomedHPA Policy)
------------------------------------------

#. Log in to the CCE console and click the cluster name to access the cluster console.
#. In the navigation pane, choose **Policies**. On the page displayed, click the **CustomedHPA Policies** tab, locate the row containing the target policy, and choose **More** > **View YAML** in the **Operation** column.
#. In the dialog box displayed, copy and download the YAML file but you are not allowed to modify it.
#. Click the close button in the upper right corner.

Deleting an HPA or CustomedHPA Policy
-------------------------------------

#. Log in to the CCE console and click the cluster name to access the cluster console.
#. In the navigation pane, choose **Policies**. Choose **More** > **Delete** in the **Operation** column of the target policy.
#. In the dialog box displayed, click **Yes**.

.. |image1| image:: /_static/images/en-us_image_0000001851745612.png
.. |image2| image:: /_static/images/en-us_image_0000001851745612.png
