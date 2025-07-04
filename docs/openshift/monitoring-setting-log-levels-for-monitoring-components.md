:_mod-docs-content-type: PROCEDURE
[id="setting-log-levels-for-monitoring-components_{context}"]
# Setting log levels for monitoring components

:configmap-name: cluster-monitoring-config
:namespace-name: openshift-monitoring
:prometheus: prometheusK8s
:alertmanager: alertmanagerMain
:thanos: thanosQuerier
:component-name: Thanos Querier

:configmap-name: user-workload-monitoring-config
:namespace-name: openshift-user-workload-monitoring
:prometheus: prometheus
:alertmanager: alertmanager
:thanos: thanosRuler
:component-name: Thanos Ruler

You can configure the log level for Alertmanager, Prometheus Operator, Prometheus, and {component-name}.

The following log levels can be applied to the relevant component in the `{configmap-name}` `ConfigMap` object:

* `debug`. Log debug, informational, warning, and error messages.
* `info`. Log informational, warning, and error messages.
* `warn`. Log warning and error messages only.
* `error`. Log error messages only.

The default log level is `info`.

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

. Add `logLevel: <log_level>` for a component under `data/config.yaml`:
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
    <component>: # <1>
      logLevel: <log_level> # <2>

```
<1> The monitoring stack component for which you are setting a log level.
Available component values are `{prometheus}`, `{alertmanager}`, `prometheusOperator`, and `{thanos}`.
<2> The log level to set for the component.
The available values are `error`, `warn`, `info`, and `debug`.
The default value is `info`.

. Save the file to apply the changes. The pods affected by the new configuration are automatically redeployed.

. Confirm that the log level has been applied by reviewing the deployment or pod configuration in the related project. 
The following example checks the log level for the `prometheus-operator` deployment:
+
[source,terminal,subs="attributes+"]

```
$ oc -n {namespace-name} get deploy prometheus-operator -o yaml | grep "log-level"

```
+
.Example output
[source,terminal,subs="attributes+"]

```
        - --log-level=debug

```

. Check that the pods for the component are running. The following example lists the status of pods:
+
[source,terminal,subs="attributes+"]

```
$ oc -n {namespace-name} get pods

```
+
# [NOTE]
# If an unrecognized `logLevel` value is included in the `ConfigMap` object, the pods for the component might not restart successfully.

:!configmap-name:
:!namespace-name:
:!prometheus:
:!alertmanager:
:!thanos:
:!component-name:
