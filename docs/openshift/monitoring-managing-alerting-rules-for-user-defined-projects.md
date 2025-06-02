:_mod-docs-content-type: CONCEPT
[id="managing-alerting-rules-for-user-defined-projects_{context}"]
# Managing alerting rules for user-defined projects

In OpenShift, you can view, edit, and remove alerting rules in user-defined projects.


[IMPORTANT]
====
Managing alerting rules for user-defined projects is only available in OpenShift version 4.11 and later.
====


.Alerting rule considerations

* The default alerting rules are used specifically for the OpenShift cluster.

* Some alerting rules intentionally have identical names. They send alerts about the same event with different thresholds, different severity, or both.

* Inhibition rules prevent notifications for lower severity alerts that are firing when a higher severity alert is also firing.
