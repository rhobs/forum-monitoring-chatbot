:_mod-docs-content-type: CONCEPT
[id="about-creating-alerting-rules-for-user-defined-projects_{context}"]
# Creating alerting rules for user-defined projects

In OpenShift, you can create alerting rules for user-defined projects. Those alerting rules will trigger alerts based on the values of the chosen metrics.

If you create alerting rules for a user-defined project, consider the following key behaviors and important limitations when you define the new rules:

* A user-defined alerting rule can include metrics exposed by its own project in addition to the default metrics from core platform monitoring. 
You cannot include metrics from another user-defined project.
+
For example, an alerting rule for the `ns1` user-defined project can use metrics exposed by the `ns1` project in addition to core platform metrics, such as CPU and memory metrics.
However, the rule cannot include metrics from a different `ns2` user-defined project.

* By default, when you create an alerting rule, the `namespace` label is enforced on it even if a rule with the same name exists in another project. To create alerting rules that are not bound to their project of origin, see "Creating cross-project alerting rules for user-defined projects".

* To reduce latency and to minimize the load on core platform monitoring components, you can add the `openshift.io/prometheus-rule-evaluation-scope: leaf-prometheus` label to a rule.
This label forces only the Prometheus instance deployed in the `openshift-user-workload-monitoring` project to evaluate the alerting rule and prevents the Thanos Ruler instance from doing so.
+
# [IMPORTANT]
If an alerting rule has this label, your alerting rule can use only those metrics exposed by your user-defined project.
# Alerting rules you create based on default platform metrics might not trigger alerts.
