:_mod-docs-content-type: PROCEDURE
[id="reviewing-monitoring-dashboards-developer_{context}"]
# Reviewing monitoring dashboards as a developer

In the *Developer* perspective, you can view dashboards relating to a selected project.

# [NOTE]
# In the *Developer* perspective, you can view dashboards for only one project at a time.

.Prerequisites

* You have access to the cluster as a developer or as a user.
* You have view permissions for the project that you are viewing the dashboard for.

.Procedure

. In the Developer perspective in the OpenShift web console, click *Observe* and go to the *Dashboards* tab.

. Select a project from the *Project:* drop-down list.

. Select a dashboard from the *Dashboard* drop-down list to see the filtered metrics.
+
# [NOTE]
# All dashboards produce additional sub-menus when selected, except *Kubernetes / Compute Resources / Namespace (Pods)*.
+
. Optional: Select a time range for the graphs in the *Time Range* list.

__ Select a pre-defined time period.

__ Set a custom time range by clicking *Custom time range* in the *Time Range* list.
+
.. Input or select the *From* and *To* dates and times.
+
.. Click *Save* to save the custom time range.

. Optional: Select a *Refresh Interval*.

. Hover over each of the graphs within a dashboard to display detailed information about specific items.
