:_mod-docs-content-type: REFERENCE
[id="default-monitoring-components_{context}"]
# Default monitoring components

By default, the OpenShift  monitoring stack includes these components:

.Default monitoring stack components
[options="header"]
|===

|Component|Description

|{cmo-full}
|The {cmo-first} is a central component of the monitoring stack. It deploys, manages, and automatically updates Prometheus and Alertmanager instances, Thanos Querier, Telemeter Client, and metrics targets. The {cmo-short} is deployed by the Cluster Version Operator (CVO).

|Prometheus Operator
|The Prometheus Operator (PO) in the `openshift-monitoring` project creates, configures, and manages platform Prometheus instances and Alertmanager instances. It also automatically generates monitoring target configurations based on Kubernetes label queries.

|Prometheus
|Prometheus is the monitoring system on which the OpenShift monitoring stack is based. Prometheus is a time-series database and a rule evaluation engine for metrics. Prometheus sends alerts to Alertmanager for processing.

|Metrics Server
|The Metrics Server component (MS in the preceding diagram) collects resource metrics and exposes them in the `metrics.k8s.io` Metrics API service for use by other tools and APIs, which frees the core platform Prometheus stack from handling this functionality. Note that with the OpenShift 4.16 release, Metrics Server replaces Prometheus Adapter.

|Alertmanager
|The Alertmanager service handles alerts received from Prometheus. Alertmanager is also responsible for sending the alerts to external notification systems.

|kube-state-metrics agent
|The kube-state-metrics exporter agent (KSM in the preceding diagram) converts Kubernetes objects to metrics that Prometheus can use.

|monitoring-plugin
|The monitoring-plugin dynamic plugin component deploys the monitoring pages in the *Observe* section of the OpenShift web console.
You can use {cmo-full} config map settings to manage monitoring-plugin resources for the web console pages.

|openshift-state-metrics agent
|The openshift-state-metrics exporter (OSM in the preceding diagram) expands upon kube-state-metrics by adding metrics for OpenShift-specific resources.

|node-exporter agent
|The node-exporter agent (NE in the preceding diagram) collects metrics about every node in a cluster. The node-exporter agent is deployed on every node.

|Thanos Querier
|Thanos Querier aggregates and optionally deduplicates core OpenShift metrics and metrics for user-defined projects under a single, multi-tenant interface.

|Telemeter Client
|Telemeter Client sends a subsection of the data from platform Prometheus instances to Red Hat to facilitate Remote Health Monitoring for clusters.

|===

All of the components in the monitoring stack are monitored by the stack and are automatically updated when OpenShift is updated.

# [NOTE]
All components of the monitoring stack use the TLS security profile settings that are centrally configured by a cluster administrator.
# If you configure a monitoring stack component that uses TLS security settings, the component uses the TLS security profile settings that already exist in the `tlsSecurityProfile` field in the global OpenShift `apiservers.config.openshift.io/cluster` resource.
