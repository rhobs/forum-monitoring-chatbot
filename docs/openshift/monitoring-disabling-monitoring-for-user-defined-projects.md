:_mod-docs-content-type: PROCEDURE
[id="disabling-monitoring-for-user-defined-projects_{context}"]
# Disabling monitoring for user-defined projects

After enabling monitoring for user-defined projects, you can disable it again by setting `enableUserWorkload: false` in the cluster monitoring `ConfigMap` object.

# [NOTE]
# Alternatively, you can remove `enableUserWorkload: true` to disable monitoring for user-defined projects.

.Procedure

. Edit the `cluster-monitoring-config` `ConfigMap` object:
+

```terminal
$ oc -n openshift-monitoring edit configmap cluster-monitoring-config

```
+
.. Set `enableUserWorkload:` to `false` under `data/config.yaml`:
+

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: cluster-monitoring-config
  namespace: openshift-monitoring
data:
  config.yaml: |
    enableUserWorkload: false

```

. Save the file to apply the changes. Monitoring for user-defined projects is then disabled automatically.

. Check that the `prometheus-operator`, `prometheus-user-workload` and `thanos-ruler-user-workload` pods are terminated in the `openshift-user-workload-monitoring` project. This might take a short while:
+

```terminal
$ oc -n openshift-user-workload-monitoring get pod

```
+
.Example output

```terminal
No resources found in openshift-user-workload-monitoring project.

```

# [NOTE]
# The `user-workload-monitoring-config` `ConfigMap` object in the `openshift-user-workload-monitoring` project is not automatically deleted when monitoring for user-defined projects is disabled. This is to preserve any custom configurations that you may have created in the `ConfigMap` object.
