:_mod-docs-content-type: PROCEDURE
[id="attaching-additional-labels-to-your-time-series-and-alerts_{context}"]
# Attaching additional labels to your time series and alerts

:configmap-name: cluster-monitoring-config
:namespace-name: openshift-monitoring
:component: prometheusK8s

:configmap-name: user-workload-monitoring-config
:namespace-name: openshift-user-workload-monitoring
:component: prometheus

You can attach custom labels to all time series and alerts leaving Prometheus by using the external labels feature of Prometheus.

.Prerequisites

* You have access to the cluster as a user with the `cluster-admin` cluster role.
* You have created the `cluster-monitoring-config` `ConfigMap` object.

* You have access to the cluster as a user with the `cluster-admin` cluster role or as a user with the `user-workload-monitoring-config-edit` role in the `openshift-user-workload-monitoring` project.
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

. Define labels you want to add for every metric under `data/config.yaml`:
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
      externalLabels:
        <key>: <value> # <1>

```
<1> Substitute `<key>: <value>` with key-value pairs where `<key>` is a unique name for the new label and `<value>` is its value.
+
# [WARNING]
* Do not use `prometheus` or `prometheus_replica` as key names, because they are reserved and will be overwritten.

# * Do not use `cluster` or `managed_cluster` as key names. Using them can cause issues where you are unable to see data in the developer dashboards.

+
# [NOTE]
# In the `openshift-user-workload-monitoring` project, Prometheus handles metrics and Thanos Ruler handles alerting and recording rules. Setting `externalLabels` for `prometheus` in the `user-workload-monitoring-config` `ConfigMap` object will only configure external labels for metrics and not for any rules.

+
For example, to add metadata about the region and environment to all time series and alerts, use the following example:
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
      externalLabels:
        region: eu
        environment: prod

```

. Save the file to apply the changes. The pods affected by the new configuration are automatically redeployed.

:!configmap-name:
:!namespace-name:
:!component:
