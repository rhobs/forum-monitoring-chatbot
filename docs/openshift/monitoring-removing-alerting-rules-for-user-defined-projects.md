:_mod-docs-content-type: PROCEDURE
[id="removing-alerting-rules-for-user-defined-projects_{context}"]
# Removing alerting rules for user-defined projects

You can remove alerting rules for user-defined projects.

.Prerequisites

* You have enabled monitoring for user-defined projects.
* You are logged in as a cluster administrator or as a user that has the `monitoring-rules-edit` cluster role for the project where you want to create an alerting rule.
* You have installed the OpenShift CLI (`oc`).

.Procedure

* To remove rule `<alerting_rule>` in `<namespace>`, run the following:
+

```terminal
$ oc -n <namespace> delete prometheusrule <alerting_rule>

```
