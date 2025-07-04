:_mod-docs-content-type: PROCEDURE
[id="modifying-the-retention-time-for-thanos-ruler-metrics-data_{context}"]
# Modifying the retention time for Thanos Ruler metrics data

By default, for user-defined projects, Thanos Ruler automatically retains metrics data for 24 hours. You can modify the retention time to change how long this data is retained by specifying a time value in the `user-workload-monitoring-config` config map in the `openshift-user-workload-monitoring` namespace.

.Prerequisites

* You have access to the cluster as a user with the `cluster-admin` cluster role or as a user with the `user-workload-monitoring-config-edit` role in the `openshift-user-workload-monitoring` project.
* A cluster administrator has enabled monitoring for user-defined projects.

* You have access to the cluster as a user with the `dedicated-admin` role.
* The `user-workload-monitoring-config` `ConfigMap` object exists. This object is created by default when the cluster is created.

* You have installed the OpenShift CLI (`oc`).

.Procedure

. Edit the `user-workload-monitoring-config` `ConfigMap` object in the `openshift-user-workload-monitoring` project:
+

```terminal
$ oc -n openshift-user-workload-monitoring edit configmap user-workload-monitoring-config

```

. Add the retention time configuration under `data/config.yaml`:
+

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: user-workload-monitoring-config
  namespace: openshift-user-workload-monitoring
data:
  config.yaml: |
    thanosRuler:
      retention: <time_specification> <1>

```
+
<1> Specify the retention time in the following format: a number directly followed by `ms` (milliseconds), `s` (seconds), `m` (minutes), `h` (hours), `d` (days), `w` (weeks), or `y` (years).
You can also combine time values for specific times, such as `1h30m15s`.
The default is `24h`.
+
The following example sets the retention time to 10 days for Thanos Ruler data:
+

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: user-workload-monitoring-config
  namespace: openshift-user-workload-monitoring
data:
  config.yaml: |
    thanosRuler:
      retention: 10d

```

. Save the file to apply the changes. The pods affected by the new configuration are automatically redeployed.
