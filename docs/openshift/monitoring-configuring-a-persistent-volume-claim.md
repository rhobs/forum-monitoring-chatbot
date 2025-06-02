:_mod-docs-content-type: PROCEDURE
[id="configuring-a-persistent-volume-claim_{context}"]
# Configuring a persistent volume claim

:configmap-name: cluster-monitoring-config
:namespace-name: openshift-monitoring
:component: prometheusK8s

:configmap-name: user-workload-monitoring-config
:namespace-name: openshift-user-workload-monitoring
:component: thanosRuler

To use a persistent volume (PV) for monitoring components, you must configure a persistent volume claim (PVC).

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

. Add your PVC configuration for the component under `data/config.yaml`:
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
      volumeClaimTemplate:
        spec:
          storageClassName: <storage_class> # <2>
          resources:
            requests:
              storage: <amount_of_storage> # <3>

```
<1> Specify the monitoring component for which you want to configure the PVC.
<2> Specify an existing storage class. If a storage class is not specified, the default storage class is used.
<3> Specify the amount of required storage.
+
The following example configures a PVC that claims persistent storage for 

Prometheus:

Thanos Ruler:

+
.Example PVC configuration
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
      volumeClaimTemplate:
        spec:
          storageClassName: my-storage-class
          resources:
            requests:
# tag::CPM[]
              storage: 40Gi
# end::CPM[]
# tag::UWM[]
              storage: 10Gi
# end::UWM[]

```

+
# [NOTE]
# Storage requirements for the `thanosRuler` component depend on the number of rules that are evaluated and how many samples each rule generates.

. Save the file to apply the changes. The pods affected by the new configuration are automatically redeployed and the new storage configuration is applied.
+
# [WARNING]
# When you update the config map with a PVC configuration, the affected `StatefulSet` object is recreated, resulting in a temporary service outage.

:!configmap-name:
:!namespace-name:
:!component:
