:_mod-docs-content-type: PROCEDURE
[id="reviewing-monitoring-dashboards-admin_{context}"]
# Reviewing monitoring dashboards as a cluster administrator

In the *Administrator* perspective, you can view dashboards relating to core OpenShift cluster components.

.Prerequisites


* You have access to the cluster as a user with the `cluster-admin` cluster role.


* You have access to the cluster as a user with the `dedicated-admin` role.


.Procedure

. In the *Administrator* perspective of the OpenShift web console, go to *Observe* -> *Dashboards*.

. Choose a dashboard in the *Dashboard* list. Some dashboards, such as *etcd* and *Prometheus* dashboards, produce additional sub-menus when selected.

. Optional: Select a time range for the graphs in the *Time Range* list.

** Select a pre-defined time period.

** Set a custom time range by clicking *Custom time range* in the *Time Range* list.
+
.. Input or select the *From* and *To* dates and times.
+
.. Click *Save* to save the custom time range.

. Optional: Select a *Refresh Interval*.

. Hover over each of the graphs within a dashboard to display detailed information about specific items.