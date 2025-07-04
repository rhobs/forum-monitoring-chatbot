:_mod-docs-content-type: REFERENCE
[id="components-for-monitoring-user-defined-projects_{context}"]
# Components for monitoring user-defined projects

OpenShift

includes an optional enhancement to the monitoring stack that enables you to monitor services and pods in user-defined projects. This feature includes the following components:

.Components for monitoring user-defined projects
[options="header"]
|===

|Component|Description

|Prometheus Operator
|The Prometheus Operator (PO) in the `openshift-user-workload-monitoring` project creates, configures, and manages Prometheus and Thanos Ruler instances in the same project.

|Prometheus
|Prometheus is the monitoring system through which monitoring is provided for user-defined projects. Prometheus sends alerts to Alertmanager for processing.

|Thanos Ruler
|The Thanos Ruler is a rule evaluation engine for Prometheus that is deployed as a separate process. In OpenShift

, Thanos Ruler provides rule and alerting evaluation for the monitoring of user-defined projects.

|Alertmanager
|The Alertmanager service handles alerts received from Prometheus and Thanos Ruler. Alertmanager is also responsible for sending user-defined alerts to external notification systems. Deploying this service is optional.

|===

# [NOTE]
# The components in the preceding table are deployed after monitoring is enabled for user-defined projects.

All of these components are monitored by the stack and are automatically updated when OpenShift is updated.
