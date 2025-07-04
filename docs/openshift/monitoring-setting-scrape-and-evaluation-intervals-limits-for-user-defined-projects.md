:_mod-docs-content-type: PROCEDURE
[id="setting-scrape-and-evaluation-intervals-limits-for-user-defined-projects_{context}"]
# Setting scrape intervals, evaluation intervals, and enforced limits for user-defined projects

You can set the following scrape and label limits for user-defined projects:

* Limit the number of samples that can be accepted per target scrape
* Limit the number of scraped labels
* Limit the length of label names and label values

You can also set an interval between consecutive scrapes and between Prometheus rule evaluations.

# [WARNING]
# If you set sample or label limits, no further sample data is ingested for that target scrape after the limit is reached.

.Prerequisites

* You have access to the cluster as a user with the `cluster-admin` cluster role, or as a user with the `user-workload-monitoring-config-edit` role in the `openshift-user-workload-monitoring` project.
* A cluster administrator has enabled monitoring for user-defined projects.

* You have access to the cluster as a user with the `dedicated-admin` role.
* The `user-workload-monitoring-config` `ConfigMap` object exists. This object is created by default when the cluster is created.

* You have installed the OpenShift CLI (`oc`).

.Procedure

. Edit the `user-workload-monitoring-config` `ConfigMap` object in the `openshift-user-workload-monitoring` project:
+

```terminal
$ oc -n openshift-user-workload-monitoring edit configmap user-workload-monitoring-config

```

. Add the enforced limit and time interval configurations to `data/config.yaml`:
+

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: user-workload-monitoring-config
  namespace: openshift-user-workload-monitoring
data:
  config.yaml: |
    prometheus:
      enforcedSampleLimit: 50000 # <1>
      enforcedLabelLimit: 500 # <2>
      enforcedLabelNameLengthLimit: 50 # <3>
      enforcedLabelValueLengthLimit: 600 # <4>
      scrapeInterval: 1m30s # <5>
      evaluationInterval: 1m15s # <6>

```
<1> A value is required if this parameter is specified. This `enforcedSampleLimit` example limits the number of samples that can be accepted per target scrape in user-defined projects to 50,000.
<2> Specifies the maximum number of labels per scrape.
The default value is `0`, which specifies no limit.
<3> Specifies the maximum character length for a label name.
The default value is `0`, which specifies no limit.
<4> Specifies the maximum character length for a label value.
The default value is `0`, which specifies no limit.
<5> Specifies the interval between consecutive scrapes. The interval must be set between 5 seconds and 5 minutes.
The default value is `30s`.
<6> Specifies the interval between Prometheus rule evaluations. The interval must be set between 5 seconds and 5 minutes.
The default value for Prometheus is `30s`.
+
# [NOTE]
# You can also configure the `evaluationInterval` property for Thanos Ruler through the `data/config.yaml/thanosRuler` field. The default value for Thanos Ruler is `15s`.

. Save the file to apply the changes. The limits are applied automatically.
