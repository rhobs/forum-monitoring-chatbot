[id="creating-alerting-rules-for-user-defined-projects_{context}"]
# Creating alerting rules for user-defined projects

For user-defined projects you can create alerting rules. Those alerting rules will fire alerts based on the values of chosen metrics.

.Prerequisites

* You have enabled monitoring for user-defined projects.
* You are logged in as a user that has the `monitoring-rules-edit` cluster role for the project where you want to create an alerting rule.
* You have installed the OpenShift CLI (`oc`).

.Procedure

. Create a YAML file for alerting rules. In this example, it is called `example-app-alerting-rule.yaml`.

. Add an alerting rule configuration to the YAML file. For example:
+
# [NOTE]
# When you create an alerting rule, a project label is enforced on it if a rule with the same name exists in another project.
+

```yaml
apiVersion: monitoring.coreos.com/v1
kind: PrometheusRule
metadata:
  name: example-alert
  namespace: ns1
spec:
  groups:
  - name: example
    rules:
    - alert: VersionAlert
      expr: version{job="prometheus-example-app"} == 0

```
+
This configuration creates an alerting rule named `example-alert`. The alerting rule fires an alert when the `version` metric exposed by the sample service becomes `0`.
+
# [IMPORTANT]
A user-defined alerting rule can include metrics for its own project and cluster metrics. You cannot include metrics for another user-defined project.

For example, an alerting rule for the user-defined project `ns1` can have metrics from `ns1` and cluster metrics, such as the CPU and memory metrics. However, the rule cannot include metrics from `ns2`.

# Additionally, you cannot create alerting rules for the `openshift-*` core OpenShift projects. OpenShift monitoring by default provides a set of alerting rules for these projects.

. Apply the configuration file to the cluster:
+

```terminal
$ oc apply -f example-app-alerting-rule.yaml

```
+
It takes some time to create the alerting rule.
