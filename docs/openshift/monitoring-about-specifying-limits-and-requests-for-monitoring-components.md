:_mod-docs-content-type: CONCEPT
[id="about-specifying-limits-and-requests-for-monitoring-components_{context}"]
# About specifying limits and requests for monitoring components

You can configure resource limits and requests for the following core platform monitoring components:

* Alertmanager
* kube-state-metrics
* monitoring-plugin
* node-exporter
* openshift-state-metrics
* Prometheus
* Metrics Server
* Prometheus Operator and its admission webhook service
* Telemeter Client
* Thanos Querier

You can configure resource limits and requests for the following components that monitor user-defined projects:

* Alertmanager
* Prometheus
* Thanos Ruler

By defining the resource limits, you limit a container's resource usage, which prevents the container from exceeding the specified maximum values for CPU and memory resources.

By defining the resource requests, you specify that a container can be scheduled only on a node that has enough CPU and memory resources available to match the requested resources.
