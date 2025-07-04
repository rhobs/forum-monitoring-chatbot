:_mod-docs-content-type: CONCEPT
[id="configuring-audit-logs-for-metrics-server_{context}"]
# Configuring audit logs for Metrics Server

You can configure audit logs for Metrics Server to help you troubleshoot issues with the server. 
Audit logs record the sequence of actions in a cluster. It can record user, application, or control plane activities.

You can set audit log rules, which determine what events are recorded and what data they should include. This can be achieved with the following audit profiles:

* *Metadata (default)*: This profile enables the logging of event metadata including user, timestamps, resource, and verb. It does not record request and response bodies.
* *Request*: This enables the logging of event metadata and request body, but it does not record response body. This configuration does not apply for non-resource requests.
* *RequestResponse*: This enables the logging of event metadata, and request and response bodies. This configuration does not apply for non-resource requests.
* *None*: None of the previously described events are recorded.

You can configure the audit profiles by modifying the `cluster-monitoring-config` config map.
The following example sets the profile to `Request`, allowing the logging of event metadata and request body for Metrics Server:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: cluster-monitoring-config
  namespace: openshift-monitoring
data:
  config.yaml: |
    metricsServer:
      audit:
        profile: Request

```
