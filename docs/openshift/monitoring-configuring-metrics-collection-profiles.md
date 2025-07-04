:_mod-docs-content-type: CONCEPT
[id="configuring-metrics-collection-profiles_{context}"]
# About metrics collection profiles

:FeatureName: Metrics collection profile
include::snippets/technology-preview.adoc[]

By default, Prometheus collects metrics exposed by all default metrics targets in OpenShift components.
However, you might want Prometheus to collect fewer metrics from a cluster in certain scenarios:

* If cluster administrators require only alert, telemetry, and console metrics and do not require other metrics to be available.
* If a cluster increases in size, and the increased size of the default metrics data collected now requires a significant increase in CPU and memory resources.

You can use a metrics collection profile to collect either the default amount of metrics data or a minimal amount of metrics data.
When you collect minimal metrics data, basic monitoring features such as alerting continue to work.
At the same time, the CPU and memory resources required by Prometheus decrease.

You can enable one of two metrics collection profiles:

* *full*: Prometheus collects metrics data exposed by all platform components. This setting is the default.
* *minimal*: Prometheus collects only the metrics data required for platform alerts, recording rules, telemetry, and console dashboards.
