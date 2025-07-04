:_mod-docs-content-type: PROCEDURE
[id="modifying-retention-time-and-size-for-prometheus-metrics-data_{context}"]
# Modifying retention time and size for Prometheus metrics data

:configmap-name: cluster-monitoring-config
:namespace-name: openshift-monitoring
:component: prometheusK8s

:configmap-name: user-workload-monitoring-config
:namespace-name: openshift-user-workload-monitoring
:component: prometheus

By default, Prometheus retains metrics data for 

15 days for core platform monitoring.

24 hours for monitoring for user-defined projects.

You can modify the retention time for the Prometheus instance to change when the data is deleted. You can also set the maximum amount of disk space the retained metrics data uses.

# [NOTE]
# Data compaction occurs every two hours. Therefore, a persistent volume (PV) might fill up before compaction, potentially exceeding the `retentionSize` limit. In such cases, the `KubePersistentVolumeFillingUp` alert fires until the space on a PV is lower than the `retentionSize` limit.

.Prerequisites

* You have access to the cluster as a user with the `cluster-admin` cluster role.
* You have created the `cluster-monitoring-config` `ConfigMap` object.

* You have access to the cluster as a user with the `cluster-admin` cluster role, or as a user with the `user-workload-monitoring-config-edit` role in the `openshift-user-workload-monitoring` project.
* A cluster administrator has enabled monitoring for user-defined projects.

* You have access to the cluster as a user with the `dedicated-admin` role.
* The `user-workload-monitoring-config` `ConfigMap` object exists. This object is created by default when the cluster is created.

* You have installed the OpenShift CLI (`oc`).

.Procedure

. Edit the `{configmap-name}` config map in the `{namespace-name}` project:
+
[source,terminal,subs="attributes+"]

```
$ oc -n {namespace-name} edit configmap {configmap-name}

```

. Add the retention time and size configuration under `data/config.yaml`:
+
[source,yaml,subs="attributes+"]

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: {configmap-name}
  namespace: {namespace-name}
data:
  config.yaml: |
    {component}:
      retention: <time_specification> # <1>
      retentionSize: <size_specification> # <2>

```
<1> The retention time: a number directly followed by `ms` (milliseconds), `s` (seconds), `m` (minutes), `h` (hours), `d` (days), `w` (weeks), or `y` (years). You can also combine time values for specific times, such as `1h30m15s`.
<2> The retention size: a number directly followed by `B` (bytes), `KB` (kilobytes), `MB` (megabytes), `GB` (gigabytes), `TB` (terabytes), `PB` (petabytes), and `EB` (exabytes).
+
The following example sets the retention time to 24 hours and the retention size to 10 gigabytes for the Prometheus instance:
+
.Example of setting retention time for Prometheus
[source,yaml,subs="attributes+"]

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: {configmap-name}
  namespace: {namespace-name}
data:
  config.yaml: |
    {component}:
      retention: 24h
      retentionSize: 10GB

```

. Save the file to apply the changes. The pods affected by the new configuration are automatically redeployed.

:!configmap-name:
:!namespace-name:
:!component:
