:_mod-docs-content-type: CONCEPT
[id="support-considerations_{context}"]
# Support considerations for monitoring

# [NOTE]
# Backward compatibility for metrics, recording rules, or alerting rules is not guaranteed.

The following modifications are explicitly not supported:

* *Creating additional `ServiceMonitor`, `PodMonitor`, and `PrometheusRule` objects in the `openshift-&#42;` and `kube-&#42;` projects.*
* *Modifying any resources or objects deployed in the `openshift-monitoring` or `openshift-user-workload-monitoring` projects.* The resources created by the OpenShift monitoring stack are not meant to be used by any other resources, as there are no guarantees about their backward compatibility.
+
# [NOTE]
The Alertmanager configuration is deployed as the `alertmanager-main` secret resource in the `openshift-monitoring` namespace.
If you have enabled a separate Alertmanager instance for user-defined alert routing, an Alertmanager configuration is also deployed as the `alertmanager-user-workload` secret resource in the `openshift-user-workload-monitoring` namespace.
To configure additional routes for any instance of Alertmanager, you need to decode, modify, and then encode that secret.
# This procedure is a supported exception to the preceding statement.
+
* *Modifying resources of the stack.* The OpenShift monitoring stack ensures its resources are always in the state it expects them to be. If they are modified, the stack will reset them.
* *Deploying user-defined workloads to `openshift-&#42;`, and `kube-&#42;` projects.* These projects are reserved for Red Hat provided components and they should not be used for user-defined workloads.
* *Enabling symptom based monitoring by using the `Probe` custom resource definition (CRD) in Prometheus Operator.*
* *Manually deploying monitoring resources into namespaces that have the `openshift.io/cluster-monitoring: "true"` label.* 
* *Adding the `openshift.io/cluster-monitoring: "true"` label to namespaces.* This label is reserved only for the namespaces with core OpenShift components and Red{nbsp}Hat certified components.

* *Installing custom Prometheus instances on OpenShift.* A custom instance is a Prometheus custom resource (CR) managed by the Prometheus Operator.

* *Modifying the default platform monitoring components.* You should not modify any of the components defined in the `cluster-monitoring-config` config map. Red Hat SRE uses these components to monitor the core cluster components and Kubernetes services.
