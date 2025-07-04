:_mod-docs-content-type: PROCEDURE
[id="choosing-a-metrics-collection-profile_{context}"]
# Choosing a metrics collection profile

:FeatureName: Metrics collection profile
include::snippets/technology-preview.adoc[]

To choose a metrics collection profile for core OpenShift monitoring components, edit the `cluster-monitoring-config` `ConfigMap` object.

.Prerequisites

* You have installed the OpenShift CLI (`oc`).
* You have enabled Technology Preview features by using the `FeatureGate` custom resource (CR).
* You have created the `cluster-monitoring-config` `ConfigMap` object.
* You have access to the cluster as a user with the `cluster-admin` cluster role.

.Procedure

. Edit the `cluster-monitoring-config` `ConfigMap` object in the `openshift-monitoring` project:
+

```terminal
$ oc -n openshift-monitoring edit configmap cluster-monitoring-config

```

. Add the metrics collection profile setting under `data/config.yaml/prometheusK8s`:
+

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: cluster-monitoring-config
  namespace: openshift-monitoring
data:
  config.yaml: |
    prometheusK8s:
      collectionProfile: <metrics_collection_profile_name> <1>

```
+
<1> The name of the metrics collection profile.
The available values are `full` or `minimal`.
If you do not specify a value or if the `collectionProfile` key name does not exist in the config map, the default setting of `full` is used.
+
The following example sets the metrics collection profile to `minimal` for the core platform instance of Prometheus:
+
[source,yaml,subs=quotes]

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: cluster-monitoring-config
  namespace: openshift-monitoring
data:
  config.yaml: |
    prometheusK8s:
      collectionProfile: *minimal*

```

. Save the file to apply the changes. The new configuration is applied automatically.
