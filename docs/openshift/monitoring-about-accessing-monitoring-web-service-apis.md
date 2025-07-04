:_mod-docs-content-type: CONCEPT
[id="about-accessing-monitoring-web-service-apis_{context}"]
# About accessing monitoring web service APIs

You can directly access web service API endpoints from the command line for the following monitoring stack components:

* Prometheus
* Alertmanager
* Thanos Ruler
* Thanos Querier

# [IMPORTANT]
# To access Thanos Ruler and Thanos Querier service APIs, the requesting account must have `get` permission on the namespaces resource, which can be granted by binding the `cluster-monitoring-view` cluster role to the account.

When you access web service API endpoints for monitoring components, be aware of the following limitations:

* You can only use bearer token authentication to access API endpoints.
* You can only access endpoints in the `/api` path for a route.
If you try to access an API endpoint in a web browser, an `Application is not available` error occurs.
To access monitoring features in a web browser, use the OpenShift web console to review monitoring dashboards.
