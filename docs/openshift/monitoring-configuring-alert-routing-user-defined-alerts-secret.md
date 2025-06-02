:_mod-docs-content-type: PROCEDURE
[id="configuring-alert-routing-user-defined-alerts-secret_{context}"]
# Configuring alert routing for user-defined projects with the Alertmanager secret

If you have enabled a separate instance of Alertmanager that is dedicated to user-defined alert routing, you can customize where and how the instance sends notifications by editing the `alertmanager-user-workload` secret in the `openshift-user-workload-monitoring` namespace.

[NOTE]
====
All features of a supported version of upstream Alertmanager are also supported in an OpenShift Alertmanager configuration. To check all the configuration options of a supported version of upstream Alertmanager, see link:https://prometheus.io/docs/alerting/0.27/configuration/[Alertmanager configuration] (Prometheus documentation).
====

.Prerequisites


* You have access to the cluster as a user with the `cluster-admin` cluster role.
* You have enabled a separate instance of Alertmanager for user-defined alert routing.


* You have access to the cluster as a user with the `dedicated-admin` role.

* You have installed the OpenShift CLI (`oc`).

.Procedure

. Print the currently active Alertmanager configuration into the file `alertmanager.yaml`:
+
```terminal
$ oc -n openshift-user-workload-monitoring get secret alertmanager-user-workload --template='{{ index .data "alertmanager.yaml" }}' | base64 --decode > alertmanager.yaml
```
+
. Edit the configuration in `alertmanager.yaml`:
+
```yaml
global:
  http_config:
    proxy_from_environment: true # <1>
route:
  receiver: Default
  group_by:
  - name: Default
  routes:
  - matchers:
    - "service = prometheus-example-monitor" # <2>
    receiver: <receiver> # <3>
receivers:
- name: Default
- name: <receiver>
  <receiver_configuration> # <4>
```
<1> If you configured an HTTP cluster-wide proxy, set the `proxy_from_environment` parameter to `true` to enable proxying for all alert receivers.
<2> Specify labels to match your alerts. This example targets all alerts that have the `service="prometheus-example-monitor"` label.
<3> Specify the name of the receiver to use for the alerts group.
<4> Specify the receiver configuration.
+
. Apply the new configuration in the file:
+
```terminal
$ oc -n openshift-user-workload-monitoring create secret generic alertmanager-user-workload --from-file=alertmanager.yaml --dry-run=client -o=yaml |  oc -n openshift-user-workload-monitoring replace secret --filename=-
```
