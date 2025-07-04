:_mod-docs-content-type: PROCEDURE
[id="creating-user-defined-workload-monitoring-configmap_{context}"]
# Creating a user-defined workload monitoring config map

You can configure the user workload monitoring components with the `user-workload-monitoring-config` `ConfigMap` object in the `openshift-user-workload-monitoring` project. The {cmo-first} then configures the components that monitor user-defined projects.

# [NOTE]
* If you enable monitoring for user-defined projects, the `user-workload-monitoring-config` `ConfigMap` object is created by default.

# * When you save your changes to the `user-workload-monitoring-config` `ConfigMap` object, some or all of the pods in the `openshift-user-workload-monitoring` project might be redeployed. It can sometimes take a while for these components to redeploy.

.Prerequisites

* You have access to the cluster as a user with the `cluster-admin` cluster role.
* You have installed the OpenShift CLI (`oc`).

.Procedure

. Check whether the `user-workload-monitoring-config` `ConfigMap` object exists:
+

```terminal
$ oc -n openshift-user-workload-monitoring get configmap user-workload-monitoring-config

```

. If the `user-workload-monitoring-config` `ConfigMap` object does not exist:
.. Create the following YAML manifest. In this example the file is called `user-workload-monitoring-config.yaml`:
+

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: user-workload-monitoring-config
  namespace: openshift-user-workload-monitoring
data:
  config.yaml: |

```
+
.. Apply the configuration to create the `ConfigMap` object:
+

```terminal
$ oc apply -f user-workload-monitoring-config.yaml

```

+
# [NOTE]
# Configurations applied to the `user-workload-monitoring-config` `ConfigMap` object are not activated unless a cluster administrator has enabled monitoring for user-defined projects.
