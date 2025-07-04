:_mod-docs-content-type: PROCEDURE
[id="creating-cross-project-alerting-rules-for-user-defined-projects_{context}"]
# Creating cross-project alerting rules for user-defined projects

You can create alerting rules that are not bound to their project of origin by configuring a project in the `user-workload-monitoring-config` config map. The `PrometheusRule` objects created in these projects are then applicable to all projects.

Therefore, you can have generic alerting rules that apply to multiple user-defined projects instead of having individual `PrometheusRule` objects in each user project. You can filter which projects are included or excluded from the alerting rule by using PromQL queries in the `PrometheusRule` object.

.Prerequisites

* If you are a cluster administrator, you have access to the cluster as a user with the `cluster-admin` cluster role.
* If you are a non-administrator user, you have access to the cluster as a user with the following user roles:
__ The `user-workload-monitoring-config-edit` role in the `openshift-user-workload-monitoring` project to edit the `user-workload-monitoring-config` config map.
__ The `monitoring-rules-edit` cluster role for the project where you want to create an alerting rule.
* A cluster administrator has enabled monitoring for user-defined projects.

* You have access to the cluster as a user with the `dedicated-admin` role.
+
# [NOTE]
# If you are a non-administrator user, you can still create cross-project alerting rules if you have the `monitoring-rules-edit` cluster role for the project where you want to create an alerting rule. However, that project needs to be configured in the `user-workload-monitoring-config` config map under the `namespacesWithoutLabelEnforcement` property, which can be done only by cluster administrators.
* The `user-workload-monitoring-config` `ConfigMap` object exists. This object is created by default when the cluster is created.

* You have installed the OpenShift CLI (`oc`).

.Procedure

. Edit the `user-workload-monitoring-config` config map in the `openshift-user-workload-monitoring` project:
+

```terminal
$ oc -n openshift-user-workload-monitoring edit configmap user-workload-monitoring-config

```

. Configure projects in which you want to create alerting rules that are not bound to a specific project:
+

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: user-workload-monitoring-config
  namespace: openshift-user-workload-monitoring
data:
  config.yaml: |
    namespacesWithoutLabelEnforcement: [ <namespace1>, <namespace2> ] # <1>
    # ...

```
<1> Specify one or more projects in which you want to create cross-project alerting rules. Prometheus and Thanos Ruler for user-defined monitoring do not enforce the `namespace` label in `PrometheusRule` objects created in these projects, making the `PrometheusRule` objects applicable to all projects.

. Create a YAML file for alerting rules. In this example, it is called `example-cross-project-alerting-rule.yaml`.

. Add an alerting rule configuration to the YAML file.
The following example creates a new cross-project alerting rule called `example-security`. The alerting rule fires when a user project does not enforce the restricted pod security policy:
+
.Example cross-project alerting rule

```yaml
apiVersion: monitoring.coreos.com/v1
kind: PrometheusRule
metadata:
  name: example-security
  namespace: ns1 #<1>
spec:
  groups:
    - name: pod-security-policy
      rules:
        - alert: "ProjectNotEnforcingRestrictedPolicy" # <2>
          for: 5m # <3>
          expr: kube_namespace_labels{namespace!~"(openshift|kube).*|default",label_pod_security_kubernetes_io_enforce!="restricted"} # <4>
          annotations:
            message: "Restricted policy not enforced. Project {{ $labels.namespace }} does not enforce the restricted pod security policy." #<5>
          labels:
            severity: warning # <6>

```
<1> Ensure that you specify the project that you defined in the `namespacesWithoutLabelEnforcement` field.
<2> The name of the alerting rule you want to create.
<3> The duration for which the condition should be true before an alert is fired.
<4> The PromQL query expression that defines the new rule. You can use label matchers on the `namespace` label to filter which projects are included or excluded from the alerting rule.
<5> The message associated with the alert.
<6> The severity that alerting rule assigns to the alert.
+
# [IMPORTANT]
Ensure that you create a specific cross-project alerting rule in only one of the projects that you specified in the `namespacesWithoutLabelEnforcement` field.
# If you create the same cross-project alerting rule in multiple projects, it results in repeated alerts.

. Apply the configuration file to the cluster:
+

```terminal
$ oc apply -f example-cross-project-alerting-rule.yaml

```
