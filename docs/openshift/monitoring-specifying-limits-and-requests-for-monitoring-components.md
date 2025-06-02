:_mod-docs-content-type: PROCEDURE
[id="specifying-limits-and-resource-requests-for-monitoring-components_{context}"]
# Specifying limits and requests



:configmap-name: cluster-monitoring-config
:namespace-name: openshift-monitoring
:alertmanager: alertmanagerMain
:prometheus: prometheusK8s
:thanos: thanosQuerier


:configmap-name: user-workload-monitoring-config
:namespace-name: openshift-user-workload-monitoring
:alertmanager: alertmanager
:prometheus: prometheus
:thanos: thanosRuler


To configure CPU and memory resources, specify values for resource limits and requests in the `{configmap-name}` `ConfigMap` object in the `{namespace-name}` namespace.

.Prerequisites


* You have access to the cluster as a user with the `cluster-admin` cluster role.
* You have created the `ConfigMap` object named `cluster-monitoring-config`.



* You have access to the cluster as a user with the `cluster-admin` cluster role, or as a user with the `user-workload-monitoring-config-edit` role in the `openshift-user-workload-monitoring` project.

* You have installed the OpenShift CLI (`oc`).

.Procedure

. Edit the `{configmap-name}` config map in the `{namespace-name}` project:
+
[source,terminal,subs="attributes+"]
```
$ oc -n {namespace-name} edit configmap {configmap-name}
```

. Add values to define resource limits and requests for each component you want to configure.
+
[IMPORTANT]
====
Ensure that the value set for a limit is always higher than the value set for a request.
Otherwise, an error will occur, and the container will not run.
====
+
.Example of setting resource limits and requests
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
    {alertmanager}:
      resources:
        limits:
          cpu: 500m
          memory: 1Gi
        requests:
          cpu: 200m
          memory: 500Mi
    {prometheus}:
      resources:
        limits:
          cpu: 500m
          memory: 3Gi
        requests:
          cpu: 200m
          memory: 500Mi
    {thanos}:
      resources:
        limits:
          cpu: 500m
          memory: 1Gi
        requests:
          cpu: 200m
          memory: 500Mi
# tag::CPM[]
    prometheusOperator:
      resources:
        limits:
          cpu: 500m
          memory: 1Gi
        requests:
          cpu: 200m
          memory: 500Mi
    metricsServer:
      resources:
        requests:
          cpu: 10m
          memory: 50Mi
        limits:
          cpu: 50m
          memory: 500Mi
    kubeStateMetrics:
      resources:
        limits:
          cpu: 500m
          memory: 1Gi
        requests:
          cpu: 200m
          memory: 500Mi
    telemeterClient:
      resources:
        limits:
          cpu: 500m
          memory: 1Gi
        requests:
          cpu: 200m
          memory: 500Mi
    openshiftStateMetrics:
      resources:
        limits:
          cpu: 500m
          memory: 1Gi
        requests:
          cpu: 200m
          memory: 500Mi
    nodeExporter:
      resources:
        limits:
          cpu: 50m
          memory: 150Mi
        requests:
          cpu: 20m
          memory: 50Mi
    monitoringPlugin:
      resources:
        limits:
          cpu: 500m
          memory: 1Gi
        requests:
          cpu: 200m
          memory: 500Mi
    prometheusOperatorAdmissionWebhook:
      resources:
        limits:
          cpu: 50m
          memory: 100Mi
        requests:
          cpu: 20m
          memory: 50Mi
# end::CPM[]
```

. Save the file to apply the changes. The pods affected by the new configuration are automatically redeployed.


:!configmap-name:
:!namespace-name:
:!alertmanager:
:!prometheus:
:!thanos: