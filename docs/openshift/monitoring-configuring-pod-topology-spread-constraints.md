:_mod-docs-content-type: PROCEDURE
[id="configuring-pod-topology-spread-constraints_{context}"]
# Configuring pod topology spread constraints

:configmap-name: cluster-monitoring-config
:namespace-name: openshift-monitoring
:component: prometheusK8s
:component-name: Prometheus
:label: prometheus

:configmap-name: user-workload-monitoring-config
:namespace-name: openshift-user-workload-monitoring
:component: thanosRuler
:component-name: Thanos Ruler
:label: thanos-ruler

You can configure pod topology spread constraints for 

all the pods deployed by the {cmo-full}

all the pods for user-defined monitoring

to control how pod replicas are scheduled to nodes across zones.
This ensures that the pods are highly available and run more efficiently, because workloads are spread across nodes in different data centers or hierarchical infrastructure zones.

You can configure pod topology spread constraints for monitoring pods by using the `{configmap-name}` config map.

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

. Add the following settings under the `data/config.yaml` field to configure pod topology spread constraints:
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
      topologySpreadConstraints:
      - maxSkew: <n> # <2>
        topologyKey: <key> # <3>
        whenUnsatisfiable: <value> # <4>
        labelSelector: # <5>
          <match_option>

```
<1> Specify a name of the component for which you want to set up pod topology spread constraints.
<2> Specify a numeric value for `maxSkew`, which defines the degree to which pods are allowed to be unevenly distributed.
<3> Specify a key of node labels for `topologyKey`.
Nodes that have a label with this key and identical values are considered to be in the same topology.
The scheduler tries to put a balanced number of pods into each domain.
<4> Specify a value for `whenUnsatisfiable`.
Available options are `DoNotSchedule` and `ScheduleAnyway`.
Specify `DoNotSchedule` if you want the `maxSkew` value to define the maximum difference allowed between the number of matching pods in the target topology and the global minimum.
Specify `ScheduleAnyway` if you want the scheduler to still schedule the pod but to give higher priority to nodes that might reduce the skew.
<5> Specify `labelSelector` to find matching pods. 
Pods that match this label selector are counted to determine the number of pods in their corresponding topology domain.
+
.Example configuration for {component-name}
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
      topologySpreadConstraints:
      - maxSkew: 1
        topologyKey: monitoring
# tag::CPM[]
        whenUnsatisfiable: DoNotSchedule
# end::CPM[]
# tag::UWM[]
        whenUnsatisfiable: ScheduleAnyway
# end::UWM[]
        labelSelector:
          matchLabels:
            app.kubernetes.io/name: {label}

```

. Save the file to apply the changes. The pods affected by the new configuration are automatically redeployed.

:!configmap-name:
:!namespace-name:
:!component:
:!component-name:
:!label:
