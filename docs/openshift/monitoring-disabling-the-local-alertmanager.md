:_mod-docs-content-type: PROCEDURE
[id="monitoring-disabling-the-local-alertmanager_{context}"]
# Disabling the local Alertmanager

A local Alertmanager that routes alerts from Prometheus instances is enabled by default in the `openshift-monitoring` project of the OpenShift monitoring stack.

If you do not need the local Alertmanager, you can disable it by configuring the `cluster-monitoring-config` config map in the `openshift-monitoring` project.

.Prerequisites

* You have access to the cluster as a user with the `cluster-admin` cluster role.
* You have created the `cluster-monitoring-config` config map.
* You have installed the OpenShift CLI (`oc`).

.Procedure

. Edit the `cluster-monitoring-config` config map in the `openshift-monitoring` project:
+

```terminal
$ oc -n openshift-monitoring edit configmap cluster-monitoring-config

```

. Add `enabled: false` for the `alertmanagerMain` component under `data/config.yaml`:
+

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: cluster-monitoring-config
  namespace: openshift-monitoring
data:
  config.yaml: |
    alertmanagerMain:
      enabled: false

```

. Save the file to apply the changes. The Alertmanager instance is disabled automatically when you apply the change.
