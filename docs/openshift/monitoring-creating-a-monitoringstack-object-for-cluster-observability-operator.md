:_mod-docs-content-type: PROCEDURE
[id="creating-a-monitoringstack-object-for-cluster-observability-operator_{context}"]
# Creating a MonitoringStack object for the {coo-full}

To scrape the metrics data exposed by the target `prometheus-coo-example-app` service, create a `MonitoringStack` object that references the `ServiceMonitor` object you created in the "Specifying how a service is monitored for {coo-full}" section.
This `MonitoringStack` object can then discover the service and scrape the exposed metrics data from it.

.Prerequisites

* You have access to the cluster as a user with the `cluster-admin` cluster role or as a user with administrative permissions for the namespace.
* You have installed the {coo-full}.
* You have deployed the `prometheus-coo-example-app` sample service in the `ns1-coo` namespace.
* You have created a `ServiceMonitor` object named `prometheus-coo-example-monitor` in the `ns1-coo` namespace.

.Procedure

. Create a YAML file for the `MonitoringStack` object configuration. For this example, name the file `example-coo-monitoring-stack.yaml`.

. Add the following `MonitoringStack` object configuration details:
+
.Example `MonitoringStack` object
+

```yaml
apiVersion: monitoring.rhobs/v1alpha1
kind: MonitoringStack
metadata:
  name: example-coo-monitoring-stack
  namespace: ns1-coo
spec:
  logLevel: debug
  retention: 1d
  resourceSelector:
    matchLabels:
      k8s-app: prometheus-coo-example-monitor

```

. Apply the `MonitoringStack` object by running the following command:
+

```terminal
$ oc apply -f example-coo-monitoring-stack.yaml

```

. Verify that the `MonitoringStack` object is available by running the following command and inspecting the output:
+

```terminal
$ oc -n ns1-coo get monitoringstack

```
+
.Example output

```terminal
NAME                         AGE
example-coo-monitoring-stack   81m

```

. Run the following comand to retrieve information about the active targets from Prometheus and filter the output to list only targets labeled with `app=prometheus-coo-example-app`. This verifies which targets are discovered and actively monitored by Prometheus with this specific label.
+

```terminal
$ oc -n ns1-coo exec -c prometheus prometheus-example-coo-monitoring-stack-0 -- curl -s 'http://localhost:9090/api/v1/targets' | jq '.data.activeTargets[].discoveredLabels | select(.__meta_kubernetes_endpoints_label_app=="prometheus-coo-example-app")'

```
+
.Example output

```json
{
  "__address__": "10.129.2.25:8080",
  "__meta_kubernetes_endpoint_address_target_kind": "Pod",
  "__meta_kubernetes_endpoint_address_target_name": "prometheus-coo-example-app-5d8cd498c7-9j2gj",
  "__meta_kubernetes_endpoint_node_name": "ci-ln-8tt8vxb-72292-6cxjr-worker-a-wdfnz",
  "__meta_kubernetes_endpoint_port_name": "web",
  "__meta_kubernetes_endpoint_port_protocol": "TCP",
  "__meta_kubernetes_endpoint_ready": "true",
  "__meta_kubernetes_endpoints_annotation_endpoints_kubernetes_io_last_change_trigger_time": "2024-11-05T11:24:09Z",
  "__meta_kubernetes_endpoints_annotationpresent_endpoints_kubernetes_io_last_change_trigger_time": "true",
  "__meta_kubernetes_endpoints_label_app": "prometheus-coo-example-app",
  "__meta_kubernetes_endpoints_labelpresent_app": "true",
  "__meta_kubernetes_endpoints_name": "prometheus-coo-example-app",
  "__meta_kubernetes_namespace": "ns1-coo",
  "__meta_kubernetes_pod_annotation_k8s_ovn_org_pod_networks": "{\"default\":{\"ip_addresses\":[\"10.129.2.25/23\"],\"mac_address\":\"0a:58:0a:81:02:19\",\"gateway_ips\":[\"10.129.2.1\"],\"routes\":[{\"dest\":\"10.128.0.0/14\",\"nextHop\":\"10.129.2.1\"},{\"dest\":\"172.30.0.0/16\",\"nextHop\":\"10.129.2.1\"},{\"dest\":\"100.64.0.0/16\",\"nextHop\":\"10.129.2.1\"}],\"ip_address\":\"10.129.2.25/23\",\"gateway_ip\":\"10.129.2.1\",\"role\":\"primary\"}}",
  "__meta_kubernetes_pod_annotation_k8s_v1_cni_cncf_io_network_status": "[{\n    \"name\": \"ovn-kubernetes\",\n    \"interface\": \"eth0\",\n    \"ips\": [\n        \"10.129.2.25\"\n    ],\n    \"mac\": \"0a:58:0a:81:02:19\",\n    \"default\": true,\n    \"dns\": {}\n}]",
  "__meta_kubernetes_pod_annotation_openshift_io_scc": "restricted-v2",
  "__meta_kubernetes_pod_annotation_seccomp_security_alpha_kubernetes_io_pod": "runtime/default",
  "__meta_kubernetes_pod_annotationpresent_k8s_ovn_org_pod_networks": "true",
  "__meta_kubernetes_pod_annotationpresent_k8s_v1_cni_cncf_io_network_status": "true",
  "__meta_kubernetes_pod_annotationpresent_openshift_io_scc": "true",
  "__meta_kubernetes_pod_annotationpresent_seccomp_security_alpha_kubernetes_io_pod": "true",
  "__meta_kubernetes_pod_controller_kind": "ReplicaSet",
  "__meta_kubernetes_pod_controller_name": "prometheus-coo-example-app-5d8cd498c7",
  "__meta_kubernetes_pod_host_ip": "10.0.128.2",
  "__meta_kubernetes_pod_ip": "10.129.2.25",
  "__meta_kubernetes_pod_label_app": "prometheus-coo-example-app",
  "__meta_kubernetes_pod_label_pod_template_hash": "5d8cd498c7",
  "__meta_kubernetes_pod_labelpresent_app": "true",
  "__meta_kubernetes_pod_labelpresent_pod_template_hash": "true",
  "__meta_kubernetes_pod_name": "prometheus-coo-example-app-5d8cd498c7-9j2gj",
  "__meta_kubernetes_pod_node_name": "ci-ln-8tt8vxb-72292-6cxjr-worker-a-wdfnz",
  "__meta_kubernetes_pod_phase": "Running",
  "__meta_kubernetes_pod_ready": "true",
  "__meta_kubernetes_pod_uid": "054c11b6-9a76-4827-a860-47f3a4596871",
  "__meta_kubernetes_service_label_app": "prometheus-coo-example-app",
  "__meta_kubernetes_service_labelpresent_app": "true",
  "__meta_kubernetes_service_name": "prometheus-coo-example-app",
  "__metrics_path__": "/metrics",
  "__scheme__": "http",
  "__scrape_interval__": "30s",
  "__scrape_timeout__": "10s",
  "job": "serviceMonitor/ns1-coo/prometheus-coo-example-monitor/0"
}

```
+
# [NOTE]
# The above example uses link:https://jqlang.github.io/jq/[`jq` command-line JSON processor] to format the output for convenience.
