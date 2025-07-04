:_mod-docs-content-type: PROCEDURE
[id="specifying-how-a-service-is-monitored_{context}"]
# Specifying how a service is monitored

To use the metrics exposed by your service, you must configure OpenShift monitoring to scrape metrics from the `/metrics` endpoint. You can do this using a `ServiceMonitor` custom resource definition (CRD) that specifies how a service should be monitored, or a `PodMonitor` CRD that specifies how a pod should be monitored. The former requires a `Service` object, while the latter does not, allowing Prometheus to directly scrape metrics from the metrics endpoint exposed by a pod.

This procedure shows you how to create a `ServiceMonitor` resource for a service in a user-defined project.

.Prerequisites

* You have access to the cluster as a user with the `cluster-admin` cluster role or the `monitoring-edit` cluster role.
* You have enabled monitoring for user-defined projects.

* You have access to the cluster as a user with the `dedicated-admin` role or the `monitoring-edit` role.

* For this example, you have deployed the `prometheus-example-app` sample service in the `ns1` project.
+
# [NOTE]
# The `prometheus-example-app` sample service does not support TLS authentication.

.Procedure

. Create a new YAML configuration file named `example-app-service-monitor.yaml`.

. Add a `ServiceMonitor` resource to the YAML file. The following example creates a service monitor named `prometheus-example-monitor` to scrape metrics exposed by the `prometheus-example-app` service in the `ns1` namespace:
+

```yaml
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: prometheus-example-monitor
  namespace: ns1 #<1>
spec:
  endpoints:
  - interval: 30s
    port: web #<2>
    scheme: http
  selector: #<3>
    matchLabels:
      app: prometheus-example-app

```
<1> Specify a user-defined namespace where your service runs.
<2> Specify endpoint ports to be scraped by Prometheus.
<3> Configure a selector to match your service based on its metadata labels.
+
# [NOTE]
# A `ServiceMonitor` resource in a user-defined namespace can only discover services in the same namespace. That is, the `namespaceSelector` field of the `ServiceMonitor` resource is always ignored.

. Apply the configuration to the cluster:
+

```terminal
$ oc apply -f example-app-service-monitor.yaml

```
+
It takes some time to deploy the `ServiceMonitor` resource.

. Verify that the `ServiceMonitor` resource is running:
+

```terminal
$ oc -n <namespace> get servicemonitor

```
+
.Example output

```terminal
NAME                         AGE
prometheus-example-monitor   81m

```
