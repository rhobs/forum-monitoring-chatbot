:_mod-docs-content-type: PROCEDURE
[id="monitoring-validating-a-monitoringstack-for-cluster-observability-operator_{context}"]
# Validating the monitoring stack

To validate that the monitoring stack is working correctly, access the example service and then view the gathered metrics.

.Prerequisites

* You have access to the cluster as a user with the `cluster-admin` cluster role or as a user with administrative permissions for the namespace.
* You have installed the {coo-full}.
* You have deployed the `prometheus-coo-example-app` sample service in the `ns1-coo` namespace.
* You have created a `ServiceMonitor` object named `prometheus-coo-example-monitor` in the `ns1-coo` namespace.
* You have created a `MonitoringStack` object named `example-coo-monitoring-stack` in the `ns1-coo` namespace.

.Procedure

. Create a route to expose the example `prometheus-coo-example-app` service. From your terminal, run the command:
+

```terminal
$ oc expose svc prometheus-coo-example-app -n ns1-coo

```
. Access the route from your browser, or command line, to generate metrics.

. Execute a query on the Prometheus pod to return the total HTTP requests metric:
+

```terminal
$ oc -n ns1-coo exec -c prometheus prometheus-example-coo-monitoring-stack-0 -- curl -s 'http://localhost:9090/api/v1/query?query=http_requests_total'

```
+
.Example output (formatted using `jq` for convenience)

```json
{
  "status": "success",
  "data": {
    "resultType": "vector",
    "result": [
      {
        "metric": {
          "__name__": "http_requests_total",
          "code": "200",
          "endpoint": "web",
          "instance": "10.129.2.25:8080",
          "job": "prometheus-coo-example-app",
          "method": "get",
          "namespace": "ns1-coo",
          "pod": "prometheus-coo-example-app-5d8cd498c7-9j2gj",
          "service": "prometheus-coo-example-app"
        },
        "value": [
          1730807483.632,
          "3"
        ]
      },
      {
        "metric": {
          "__name__": "http_requests_total",
          "code": "404",
          "endpoint": "web",
          "instance": "10.129.2.25:8080",
          "job": "prometheus-coo-example-app",
          "method": "get",
          "namespace": "ns1-coo",
          "pod": "prometheus-coo-example-app-5d8cd498c7-9j2gj",
          "service": "prometheus-coo-example-app"
        },
        "value": [
          1730807483.632,
          "0"
        ]
      }
    ]
  }
}

```
