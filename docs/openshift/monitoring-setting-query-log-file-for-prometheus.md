:_mod-docs-content-type: PROCEDURE
[id="setting-query-log-file-for-prometheus_{context}"]
# Enabling the query log file for Prometheus

:configmap-name: cluster-monitoring-config
:namespace-name: openshift-monitoring
:component: prometheusK8s
:pod: prometheus-k8s-0

:configmap-name: user-workload-monitoring-config
:namespace-name: openshift-user-workload-monitoring
:component: prometheus
:pod: prometheus-user-workload-0

You can configure Prometheus to write all queries that have been run by the engine to a log file.

# [IMPORTANT]
# Because log rotation is not supported, only enable this feature temporarily when you need to troubleshoot an issue. After you finish troubleshooting, disable query logging by reverting the changes you made to the `ConfigMap` object to enable the feature.

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

. Add the `queryLogFile` parameter for Prometheus under `data/config.yaml`:
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
      queryLogFile: <path> # <1>

```
<1> Add the full path to the file in which queries will be logged.

. Save the file to apply the changes. The pods affected by the new configuration are automatically redeployed.

. Verify that the pods for the component are running. The following sample command lists the status of pods:
+
[source,terminal,subs="attributes+"]

```
$ oc -n {namespace-name} get pods

```
+

.Example output

```terminal
...
prometheus-operator-567c9bc75c-96wkj   2/2     Running   0          62m
prometheus-k8s-0                       6/6     Running   1          57m
prometheus-k8s-1                       6/6     Running   1          57m
thanos-querier-56c76d7df4-2xkpc        6/6     Running   0          57m
thanos-querier-56c76d7df4-j5p29        6/6     Running   0          57m
...

```

.Example output

```terminal
...
prometheus-operator-776fcbbd56-2nbfm   2/2     Running   0          132m
prometheus-user-workload-0             5/5     Running   1          132m
prometheus-user-workload-1             5/5     Running   1          132m
thanos-ruler-user-workload-0           3/3     Running   0          132m
thanos-ruler-user-workload-1           3/3     Running   0          132m
...

```

. Read the query log:
+
[source,terminal,subs="attributes+"]

```
$ oc -n {namespace-name} exec {pod} -- cat <path>

```
+
# [IMPORTANT]
# Revert the setting in the config map after you have examined the logged query information.

:!configmap-name:
:!namespace-name:
:!component:
:!pod:
