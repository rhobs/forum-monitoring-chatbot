:_mod-docs-content-type: CONCEPT
[id="understanding-metrics_{context}"]
# Understanding metrics

In OpenShift ,

In OpenShift,

cluster components are monitored by scraping metrics exposed through service endpoints. You can also configure metrics collection for user-defined projects. Metrics enable you to monitor how cluster components and your own workloads are performing.

You can define the metrics that you want to provide for your own workloads by using Prometheus client libraries at the application level.

In OpenShift, metrics are exposed through an HTTP service endpoint under the `/metrics` canonical name. You can list all available metrics for a service by running a `curl` query against `\http://<endpoint>/metrics`. For instance, you can expose a route to the `prometheus-example-app` example application and then run the following to view all of its available metrics:

```terminal
$ curl http://<example_app_endpoint>/metrics

```

.Example output

```terminal
# HELP http_requests_total Count of all HTTP requests
# TYPE http_requests_total counter
http_requests_total{code="200",method="get"} 4
http_requests_total{code="404",method="get"} 2
# HELP version Version information about this binary
# TYPE version gauge
version{version="v0.1.0"} 1

```
