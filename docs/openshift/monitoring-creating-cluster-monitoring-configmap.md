:_mod-docs-content-type: PROCEDURE
[id="creating-cluster-monitoring-configmap_{context}"]
# Creating a cluster monitoring config map

You can configure the core OpenShift monitoring components by creating and updating the `cluster-monitoring-config` config map in the `openshift-monitoring` project. The {cmo-first} then configures the core components of the monitoring stack.

.Prerequisites

* You have access to the cluster as a user with the `cluster-admin` cluster role.
* You have installed the OpenShift CLI (`oc`).

.Procedure

. Check whether the `cluster-monitoring-config` `ConfigMap` object exists:
+

```terminal
$ oc -n openshift-monitoring get configmap cluster-monitoring-config

```

. If the `ConfigMap` object does not exist:
.. Create the following YAML manifest. In this example the file is called `cluster-monitoring-config.yaml`:
+

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: cluster-monitoring-config
  namespace: openshift-monitoring
data:
  config.yaml: |

```
+
.. Apply the configuration to create the `ConfigMap` object:
+

```terminal
$ oc apply -f cluster-monitoring-config.yaml

```
