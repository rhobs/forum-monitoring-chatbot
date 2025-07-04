:_mod-docs-content-type: PROCEDURE
[id="creating-cluster-id-labels-for-metrics_{context}"]
# Creating cluster ID labels for metrics

:configmap-name: cluster-monitoring-config
:namespace-name: openshift-monitoring
:component: prometheusK8s

:configmap-name: user-workload-monitoring-config
:namespace-name: openshift-user-workload-monitoring
:component: prometheus

You can create cluster ID labels for metrics by adding the `write_relabel` settings for remote write storage in the `{configmap-name}` config map in the `{namespace-name}` namespace.

# [NOTE]
When Prometheus scrapes user workload targets that expose a `namespace` label, the system stores this label as `exported_namespace`. 
This behavior ensures that the final namespace label value is equal to the namespace of the target pod.
# You cannot override this default configuration by setting the value of the `honorLabels` field to `true` for `PodMonitor` or `ServiceMonitor` objects.

.Prerequisites

* You have access to the cluster as a user with the `cluster-admin` cluster role.
* You have created the `cluster-monitoring-config` `ConfigMap` object.

* You have access to the cluster as a user with the `cluster-admin` cluster role, or as a user with the `user-workload-monitoring-config-edit` role in the `openshift-user-workload-monitoring` project.
* A cluster administrator has enabled monitoring for user-defined projects.

* You have access to the cluster as a user with the `dedicated-admin` role.
* The `user-workload-monitoring-config` ConfigMap object exists. This object is created by default when the cluster is created.

* You have installed the OpenShift CLI (`oc`).
* You have configured remote write storage.

.Procedure

. Edit the `{configmap-name}` config map in the `{namespace-name}` project:
+
[source,terminal,subs="attributes+"]

```
$ oc -n {namespace-name} edit configmap {configmap-name}

```

. In the `writeRelabelConfigs:` section under `data/config.yaml/{component}/remoteWrite`, add cluster ID relabel configuration values:
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
      remoteWrite:
      - url: "https://remote-write-endpoint.example.com"
        <endpoint_authentication_credentials>
        writeRelabelConfigs: # <1>
          - <relabel_config> # <2>

```
<1> Add a list of write relabel configurations for metrics that you want to send to the remote endpoint.
<2> Substitute the label configuration for the metrics sent to the remote write endpoint.
+
The following sample shows how to forward a metric with the cluster ID label `cluster_id`:
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
      remoteWrite:
      - url: "https://remote-write-endpoint.example.com"
        writeRelabelConfigs:
        - sourceLabels:
          - __tmp_openshift_cluster_id__ # <1>
          targetLabel: cluster_id # <2>
          action: replace # <3>

```
<1> The system initially applies a temporary cluster ID source label named `+++__tmp_openshift_cluster_id__+++`. This temporary label gets replaced by the cluster ID label name that you specify.
<2> Specify the name of the cluster ID label for metrics sent to remote write storage.
If you use a label name that already exists for a metric, that value is overwritten with the name of this cluster ID label.
For the label name, do not use `+++__tmp_openshift_cluster_id__+++`. The final relabeling step removes labels that use this name.
<3> The `replace` write relabel action replaces the temporary label with the target label for outgoing metrics.
This action is the default and is applied if no action is specified.

. Save the file to apply the changes. The new configuration is applied automatically.

:!configmap-name:
:!namespace-name:
:!component:
