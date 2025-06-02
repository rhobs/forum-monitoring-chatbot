:_mod-docs-content-type: PROCEDURE
[id="moving-monitoring-components-to-different-nodes_{context}"]
# Moving monitoring components to different nodes



:configmap-name: cluster-monitoring-config
:namespace-name: openshift-monitoring


:configmap-name: user-workload-monitoring-config
:namespace-name: openshift-user-workload-monitoring



To specify the nodes in your cluster on which monitoring stack components will run, configure the `nodeSelector` constraint for the components in the `cluster-monitoring-config` config map to match labels assigned to the nodes.

[NOTE]
====
You cannot add a node selector constraint directly to an existing scheduled pod.
====



You can move any of the components that monitor workloads for user-defined projects to specific worker nodes. 

[WARNING]
====
It is not permitted to move components to control plane or infrastructure nodes.
====


.Prerequisites


* You have access to the cluster as a user with the `cluster-admin` cluster role.
* You have created the `cluster-monitoring-config` `ConfigMap` object.
* You have installed the OpenShift CLI (`oc`).




* You have access to the cluster as a user with the `cluster-admin` cluster role or as a user with the `user-workload-monitoring-config-edit` role in the `openshift-user-workload-monitoring` project.
* A cluster administrator has enabled monitoring for user-defined projects.


* You have access to the cluster as a user with the `dedicated-admin` role.
* The `user-workload-monitoring-config` `ConfigMap` object exists. This object is created by default when the cluster is created.

* You have installed the OpenShift CLI (`oc`).


.Procedure

. If you have not done so yet, add a label to the nodes on which you want to run the monitoring components:
+
```terminal
$ oc label nodes <node_name> <node_label> <1>
```
<1> Replace `<node_name>` with the name of the node where you want to add the label. 
Replace `<node_label>` with the name of the wanted label.

. Edit the `{configmap-name}` `ConfigMap` object in the `{namespace-name}` project:
+
[source,terminal,subs="attributes+"]
```
$ oc -n {namespace-name} edit configmap {configmap-name}
```

. Specify the node labels for the `nodeSelector` constraint for the component under `data/config.yaml`:
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
    # ...
    <component>: #<1>
      nodeSelector:
        <node_label_1> #<2>
        <node_label_2> #<3>
    # ...
```
<1> Substitute `<component>` with the appropriate monitoring stack component name.
<2> Substitute `<node_label_1>` with the label you added to the node.
<3> Optional: Specify additional labels.
If you specify additional labels, the pods for the component are only scheduled on the nodes that contain all of the specified labels.
+
[NOTE]
====
If monitoring components remain in a `Pending` state after configuring the `nodeSelector` constraint, check the pod events for errors relating to taints and tolerations.
====

. Save the file to apply the changes. The components specified in the new configuration are automatically moved to the new nodes, and the pods affected by the new configuration are redeployed.


:!configmap-name:
:!namespace-name:
