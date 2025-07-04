:_mod-docs-content-type: PROCEDURE
[id="creating-alerting-rules-for-user-defined-projects_{context}"]
# Creating alerting rules for user-defined projects

You can create alerting rules for user-defined projects. Those alerting rules will trigger alerts based on the values of the chosen metrics.

# [NOTE]
# To help users understand the impact and cause of the alert, ensure that your alerting rule contains an alert message and severity value.

.Prerequisites

* You have enabled monitoring for user-defined projects.
* You are logged in as a cluster administrator or as a user that has the `monitoring-rules-edit` cluster role for the project where you want to create an alerting rule.
* You have installed the OpenShift CLI (`oc`).

.Procedure

. Create a YAML file for alerting rules. In this example, it is called `example-app-alerting-rule.yaml`.

. Add an alerting rule configuration to the YAML file.
The following example creates a new alerting rule named `example-alert`. The alerting rule fires an alert when the `version` metric exposed by the sample service becomes `0`:
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
    - alert: VersionAlert # <1>
      for: 1m # <2>
      expr: version{job="prometheus-example-app"} == 0 # <3>
      labels:
        severity: warning # <4>
      annotations:
        message: This is an example alert. # <5>

```
<1> The name of the alerting rule you want to create.
<2> The duration for which the condition should be true before an alert is fired.
<3> The PromQL query expression that defines the new rule.
<4> The severity that alerting rule assigns to the alert.
<5> The message associated with the alert.

. Apply the configuration file to the cluster:
+

```terminal
$ oc apply -f example-app-alerting-rule.yaml

```
