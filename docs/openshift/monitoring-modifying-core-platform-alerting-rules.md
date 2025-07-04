:_mod-docs-content-type: PROCEDURE
[id="modifying-core-platform-alerting-rules_{context}"]
# Modifying core platform alerting rules

As a cluster administrator, you can modify core platform alerts before Alertmanager routes them to a receiver.
For example, you can change the severity label of an alert, add a custom label, or exclude an alert from being sent to Alertmanager.

.Prerequisites

* You have access to the cluster as a user with the `cluster-admin` cluster role.
* You have installed the OpenShift CLI (`oc`).

.Procedure

. Create a new YAML configuration file named `example-modified-alerting-rule.yaml`.

. Add an `AlertRelabelConfig` resource to the YAML file.
The following example modifies the `severity` setting to `critical` for the default platform `watchdog` alerting rule:
+

```yaml
apiVersion: monitoring.openshift.io/v1
kind: AlertRelabelConfig
metadata:
  name: watchdog
  namespace: openshift-monitoring # <1>
spec:
  configs:
  - sourceLabels: [alertname,severity] # <2>
    regex: "Watchdog;none" # <3>
    targetLabel: severity # <4>
    replacement: critical # <5>
    action: Replace # <6>

```
<1> Ensure that the namespace is `openshift-monitoring`.
<2> The source labels for the values you want to modify.
<3> The regular expression against which the value of `sourceLabels` is matched.
<4> The target label of the value you want to modify.
<5> The new value to replace the target label.
<6> The relabel action that replaces the old value based on regex matching.
The default action is `Replace`.
Other possible values are `Keep`, `Drop`, `HashMod`, `LabelMap`, `LabelDrop`, and `LabelKeep`.
+
# [IMPORTANT]
# You must create the `AlertRelabelConfig` object in the `openshift-monitoring` namespace. Otherwise, the alert label will not change.

. Apply the configuration file to the cluster:
+

```terminal
$ oc apply -f example-modified-alerting-rule.yaml

```
