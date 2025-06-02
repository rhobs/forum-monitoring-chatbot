:_mod-docs-content-type: PROCEDURE
[id="configuring-the-monitoring-stack_{context}"]
# Configuring the monitoring stack

In OpenShift , you can configure the monitoring stack using the `cluster-monitoring-config` or `user-workload-monitoring-config` `ConfigMap` objects. Config maps configure the {cmo-first}, which in turn configures the components of the stack.

In OpenShift, you can configure the stack that monitors workloads for user-defined projects by using the `user-workload-monitoring-config` `ConfigMap` object. Config maps configure the {cmo-first}, which in turn configures the components of the stack.

.Prerequisites

* *If you are configuring core OpenShift monitoring components*:
__ You have access to the cluster as a user with the `cluster-admin` cluster role.
__ You have created the `cluster-monitoring-config` `ConfigMap` object.
* *If you are configuring components that monitor user-defined projects*:
__ You have access to the cluster as a user with the `cluster-admin` cluster role, or as a user with the `user-workload-monitoring-config-edit` role in the `openshift-user-workload-monitoring` project.
__ A cluster administrator has enabled monitoring for user-defined projects.

* You have access to the cluster as a user with the `dedicated-admin` role.
* The `user-workload-monitoring-config` `ConfigMap` object exists. This object is created by default when the cluster is created.

* You have installed the OpenShift CLI (`oc`).

.Procedure

. Edit the `ConfigMap` object.

** *To configure core OpenShift monitoring components*:
.. Edit the `cluster-monitoring-config` `ConfigMap` object in the `openshift-monitoring` project:
+

```terminal
$ oc -n openshift-monitoring edit configmap cluster-monitoring-config

```

.. Add your configuration under `data/config.yaml` as a key-value pair `<component_name>:{nbsp}<component_configuration>`:
+

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: cluster-monitoring-config
  namespace: openshift-monitoring
data:
  config.yaml: |
    <component>:
      <configuration_for_the_component>

```
+
Substitute `<component>` and `<configuration_for_the_component>` accordingly.
+
The following example `ConfigMap` object configures a persistent volume claim (PVC) for Prometheus. This relates to the Prometheus instance that monitors core OpenShift components only:
+

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: cluster-monitoring-config
  namespace: openshift-monitoring
data:
  config.yaml: |
    prometheusK8s: <1>
      volumeClaimTemplate:
        spec:
          storageClassName: fast
          volumeMode: Filesystem
          resources:
            requests:
              storage: 40Gi

```
<1> Defines the Prometheus component and the subsequent lines define its configuration.

** *To configure components that monitor user-defined projects*:

.. Edit the `user-workload-monitoring-config` `ConfigMap` object in the `openshift-user-workload-monitoring` project:
+

```terminal
$ oc -n openshift-user-workload-monitoring edit configmap user-workload-monitoring-config

```

.. Add your configuration under `data/config.yaml` as a key-value pair `<component_name>:{nbsp}<component_configuration>`:
+

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: user-workload-monitoring-config
  namespace: openshift-user-workload-monitoring
data:
  config.yaml: |
    <component>:
      <configuration_for_the_component>

```
+
Substitute `<component>` and `<configuration_for_the_component>` accordingly.
+
The following example `ConfigMap` object configures a data retention period and minimum container resource requests for Prometheus. This relates to the Prometheus instance that monitors user-defined projects only:
+

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: user-workload-monitoring-config
  namespace: openshift-user-workload-monitoring
data:
  config.yaml: |
    prometheus: <1>
      retention: 24h <2>
      resources:
        requests:
          cpu: 200m <3>
          memory: 2Gi <4>

```
<1> Defines the Prometheus component and the subsequent lines define its configuration.
<2> Configures a twenty-four hour data retention period for the Prometheus instance that monitors user-defined projects.
<3> Defines a minimum resource request of 200 millicores for the Prometheus container.
<4> Defines a minimum pod resource request of 2 GiB of memory for the Prometheus container.

+
# [NOTE]
# The Prometheus config map component is called `prometheusK8s` in the `cluster-monitoring-config` `ConfigMap` object and `prometheus` in the `user-workload-monitoring-config` `ConfigMap` object.

. Save the file to apply the changes to the `ConfigMap` object.
+
# [WARNING]
Different configuration changes to the `ConfigMap` object result in different outcomes:

* The pods are not redeployed. Therefore, there is no service outage.

* The affected pods are redeployed:

__ For single-node clusters, this results in temporary service outage.

__ For multi-node clusters, because of high-availability, the affected pods are gradually rolled out and the monitoring stack remains available.

** Configuring and resizing a persistent volume always results in a service outage, regardless of high availability.

# Each procedure that requires a change in the config map includes its expected outcome.
