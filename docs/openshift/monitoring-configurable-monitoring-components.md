:_mod-docs-content-type: REFERENCE
[id="configurable-monitoring-components_{context}"]
# Configurable monitoring components



:configmap-name: cluster-monitoring-config
:alertmanager: alertmanagerMain
:prometheus: prometheusK8s
:thanosname: Thanos Querier
:thanos: thanosQuerier


:configmap-name: user-workload-monitoring-config
:alertmanager: alertmanager
:prometheus: prometheus
:thanosname: Thanos Ruler
:thanos: thanosRuler


This table shows the monitoring components you can configure and the keys used to specify the components in the `{configmap-name}` config map.



[WARNING]
====
Do not modify the monitoring components in the `cluster-monitoring-config` `ConfigMap` object. Red{nbsp}Hat Site Reliability Engineers (SRE) use these components to monitor the core cluster components and Kubernetes services.
====




.Configurable core platform monitoring components


.Configurable monitoring components for user-defined projects

[options="header"]
|====
|Component |{configmap-name} config map key
|Prometheus Operator |`prometheusOperator`
|Prometheus |`{prometheus}`
|Alertmanager |`{alertmanager}`
|{thanosname} | `{thanos}`

|kube-state-metrics |`kubeStateMetrics`
|monitoring-plugin | `monitoringPlugin`
|openshift-state-metrics |`openshiftStateMetrics`
|Telemeter Client |`telemeterClient`
|Metrics Server |`metricsServer`

|====


[WARNING]
====
Different configuration changes to the `ConfigMap` object result in different outcomes:

* The pods are not redeployed. Therefore, there is no service outage.

* The affected pods are redeployed:

** For single-node clusters, this results in temporary service outage.

** For multi-node clusters, because of high-availability, the affected pods are gradually rolled out and the monitoring stack remains available.

** Configuring and resizing a persistent volume always results in a service outage, regardless of high availability.

Each procedure that requires a change in the config map includes its expected outcome.
====



:!configmap-name:
:!alertmanager:
:!prometheus:
:!thanosname:
:!thanos:
