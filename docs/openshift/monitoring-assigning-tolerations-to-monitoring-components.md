:_mod-docs-content-type: PROCEDURE
[id="assigning-tolerations-to-monitoring-components_{context}"]
# Assigning tolerations to monitoring components

:configmap-name: cluster-monitoring-config
:namespace-name: openshift-monitoring
:component: alertmanagerMain

:configmap-name: user-workload-monitoring-config
:namespace-name: openshift-user-workload-monitoring
:component: thanosRuler

You can assign tolerations to any of the monitoring stack components to enable moving them to tainted nodes.

You can assign tolerations to the components that monitor user-defined projects, to enable moving them to tainted worker nodes. Scheduling is not permitted on control plane or infrastructure nodes.

.Prerequisites

* You have access to the cluster as a user with the `cluster-admin` cluster role.
* You have created the `cluster-monitoring-config` `ConfigMap` object.

* You have access to the cluster as a user with the `cluster-admin` cluster role, or as a user with the `user-workload-monitoring-config-edit` role in the `openshift-user-workload-monitoring` project.
* A cluster administrator has enabled monitoring for user-defined projects.

* You have access to the cluster as a user with the `dedicated-admin` role.
* The `user-workload-monitoring-config` `ConfigMap` object exists in the `openshift-user-workload-monitoring` namespace. This object is created by default when the cluster is created.

* You have installed the OpenShift CLI (`oc`).

.Procedure

. Edit the `{configmap-name}` config map in the `{namespace-name}` project:
+
[source,terminal,subs="attributes+"]

```
$ oc -n {namespace-name} edit configmap {configmap-name}

```

. Specify `tolerations` for the component:
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
    <component>:
      tolerations:
        <toleration_specification>

```
+
Substitute `<component>` and `<toleration_specification>` accordingly.
+
For example, `oc adm taint nodes node1 key1=value1:NoSchedule` adds a taint to `node1` with the key `key1` and the value `value1`. This prevents monitoring components from deploying pods on `node1` unless a toleration is configured for that taint. The following example configures the `{component}` component to tolerate the example taint:
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
      tolerations:
      - key: "key1"
        operator: "Equal"
        value: "value1"
        effect: "NoSchedule"

```

. Save the file to apply the changes. The pods affected by the new configuration are automatically redeployed.

:!configmap-name:
:!namespace-name:
:!component:
