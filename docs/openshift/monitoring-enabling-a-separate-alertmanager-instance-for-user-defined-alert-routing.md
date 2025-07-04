:_mod-docs-content-type: PROCEDURE
[id="enabling-a-separate-alertmanager-instance-for-user-defined-alert-routing_{context}"]
# Enabling a separate Alertmanager instance for user-defined alert routing

In some clusters, you might want to deploy a dedicated Alertmanager instance for user-defined projects, which can help reduce the load on the default platform Alertmanager instance and can better separate user-defined alerts from default platform alerts.

In OpenShift, you may want to deploy a dedicated Alertmanager instance for user-defined projects, which provides user-defined alerts separate from default platform alerts.

In these cases, you can optionally enable a separate instance of Alertmanager to send alerts for user-defined projects only.

.Prerequisites

* You have access to the cluster as a user with the `dedicated-admin` role.
* The `user-workload-monitoring-config` `ConfigMap` object exists. This object is created by default when the cluster is created.

* You have access to the cluster as a user with the `cluster-admin` cluster role.
* You have enabled monitoring for user-defined projects.

* You have installed the {oc-first}.

.Procedure

. Edit the `user-workload-monitoring-config` `ConfigMap` object:
+

```terminal
$ oc -n openshift-user-workload-monitoring edit configmap user-workload-monitoring-config

```
+
. Add `enabled: true` and `enableAlertmanagerConfig: true` in the `alertmanager` section under `data/config.yaml`:
+

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: user-workload-monitoring-config
  namespace: openshift-user-workload-monitoring
data:
  config.yaml: |
    alertmanager:
      enabled: true # <1>
      enableAlertmanagerConfig: true # <2>

```
<1> Set the `enabled` value to `true` to enable a dedicated instance of the Alertmanager for user-defined projects in a cluster. Set the value to `false` or omit the key entirely to disable the Alertmanager for user-defined projects.
If you set this value to `false` or if the key is omitted, user-defined alerts are routed to the default platform Alertmanager instance.
<2> Set the `enableAlertmanagerConfig` value to `true` to enable users to define their own alert routing configurations with `AlertmanagerConfig` objects.
+
. Save the file to apply the changes. The dedicated instance of Alertmanager for user-defined projects starts automatically.

.Verification

* Verify that the `user-workload` Alertmanager instance has started:
+

```terminal
$ oc -n openshift-user-workload-monitoring get alertmanager

```
+
.Example output
+

```terminal
NAME            VERSION   REPLICAS   AGE
user-workload   0.24.0    2          100s

```

* Verify that the `alert-manager-user-workload` pods are running:
+

```terminal
$ oc -n openshift-user-workload-monitoring get pods

```
+
.Example output
+

```terminal
NAME                                   READY   STATUS    RESTARTS   AGE
alertmanager-user-workload-0           6/6     Running   0          38s
alertmanager-user-workload-1           6/6     Running   0          38s
...

```
