:_mod-docs-content-type: PROCEDURE
[id="accessing-a-monitoring-web-service-api_{context}"]
# Accessing a monitoring web service API

The following example shows how to query the service API receivers for the Alertmanager service used in core platform monitoring.
You can use a similar method to access the `prometheus-k8s` service for core platform Prometheus and the `thanos-ruler` service for Thanos Ruler.

.Prerequisites

* You are logged in to an account that is bound against the `monitoring-alertmanager-edit` role in the `openshift-monitoring` namespace.
* You are logged in to an account that has permission to get the Alertmanager API route.
+
# [NOTE]
# If your account does not have permission to get the Alertmanager API route, a cluster administrator can provide the URL for the route.

.Procedure

. Extract an authentication token by running the following command:
+

```terminal
$ TOKEN=$(oc whoami -t)

```

. Extract the `alertmanager-main` API route URL by running the following command:
+

```terminal
$ HOST=$(oc -n openshift-monitoring get route alertmanager-main -ojsonpath='{.status.ingress[].host}')

```

. Query the service API receivers for Alertmanager by running the following command:
+

```terminal
$ curl -H "Authorization: Bearer $TOKEN" -k "https://$HOST/api/v2/receivers"

```
