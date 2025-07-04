:_mod-docs-content-type: PROCEDURE
[id="determining-why-prometheus-is-consuming-disk-space_{context}"]
# Determining why Prometheus is consuming a lot of disk space

Developers can create labels to define attributes for metrics in the form of key-value pairs. The number of potential key-value pairs corresponds to the number of possible values for an attribute. An attribute that has an unlimited number of potential values is called an unbound attribute. For example, a `customer_id` attribute is unbound because it has an infinite number of possible values.

Every assigned key-value pair has a unique time series. The use of many unbound attributes in labels can result in an exponential increase in the number of time series created. This can impact Prometheus performance and can consume a lot of disk space.

You can use the following measures when Prometheus consumes a lot of disk:

* *Check the time series database (TSDB) status using the Prometheus HTTP API* for more information about which labels are creating the most time series data. Doing so requires cluster administrator privileges.

* *Check the number of scrape samples* that are being collected.

* *Reduce the number of unique time series that are created* by reducing the number of unbound attributes that are assigned to user-defined metrics.
+
# [NOTE]
# Using attributes that are bound to a limited set of possible values reduces the number of potential key-value pair combinations.
+
* *Enforce limits on the number of samples that can be scraped* across user-defined projects. This requires cluster administrator privileges.

.Prerequisites

* You have access to the cluster as a user with the `cluster-admin` cluster role.

* You have access to the cluster as a user with the `dedicated-admin` role.

* You have installed the OpenShift CLI (`oc`).

.Procedure

. In the *Administrator* perspective, navigate to *Observe* -> *Metrics*.

. Enter a Prometheus Query Language (PromQL) query in the *Expression* field.
The following example queries help to identify high cardinality metrics that might result in high disk space consumption:

* By running the following query, you can identify the ten jobs that have the highest number of scrape samples:
+

```text
topk(10, max by(namespace, job) (topk by(namespace, job) (1, scrape_samples_post_metric_relabeling)))

```
+
* By running the following query, you can pinpoint time series churn by identifying the ten jobs that have created the most time series data in the last hour:
+

```text
topk(10, sum by(namespace, job) (sum_over_time(scrape_series_added[1h])))

```

. Investigate the number of unbound label values assigned to metrics with higher than expected scrape sample counts:

* *If the metrics relate to a user-defined project*, review the metrics key-value pairs assigned to your workload. These are implemented through Prometheus client libraries at the application level. Try to limit the number of unbound attributes referenced in your labels.

* *If the metrics relate to a core OpenShift project*, create a Red Hat support case on the link:https://access.redhat.com/[Red Hat Customer Portal].

. Review the TSDB status using the Prometheus HTTP API by following these steps when logged in as a

cluster administrator:

`dedicated-admin`:

+
.. Get the Prometheus API route URL by running the following command:
+

```terminal
$ HOST=$(oc -n openshift-monitoring get route prometheus-k8s -ojsonpath='{.status.ingress[].host}')

```
+
.. Extract an authentication token by running the following command:
+

```terminal
$ TOKEN=$(oc whoami -t)

```
+
.. Query the TSDB status for Prometheus by running the following command:
+

```terminal
$ curl -H "Authorization: Bearer $TOKEN" -k "https://$HOST/api/v1/status/tsdb"

```
+
.Example output

```terminal
"status": "success","data":{"headStats":{"numSeries":507473,
"numLabelPairs":19832,"chunkCount":946298,"minTime":1712253600010,
"maxTime":1712257935346},"seriesCountByMetricName":
[{"name":"etcd_request_duration_seconds_bucket","value":51840},
{"name":"apiserver_request_sli_duration_seconds_bucket","value":47718},
...

```
