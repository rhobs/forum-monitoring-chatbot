:_mod-docs-content-type: CONCEPT
[id="understanding-the-monitoring-stack_{context}"]
# Understanding the monitoring stack

The monitoring stack includes the following components:

* *Default platform monitoring components*.

A set of platform monitoring components are installed in the `openshift-monitoring` project by default during an OpenShift Container Platform installation. This provides monitoring for core cluster components including Kubernetes services. The default monitoring stack also enables remote health monitoring for clusters.

A set of platform monitoring components are installed in the `openshift-monitoring` project by default during a OpenShift installation. Red Hat Site Reliability Engineers (SRE) use these components to monitor core cluster components including Kubernetes services. This includes critical metrics, such as CPU and memory, collected from all of the workloads in every namespace.

+
These components are illustrated in the *Installed by default* section in the following diagram.

* *Components for monitoring user-defined projects*.

After optionally enabling monitoring for user-defined projects, additional monitoring components are installed in the `openshift-user-workload-monitoring` project. This provides monitoring for user-defined projects.

A set of user-defined project monitoring components are installed in the `openshift-user-workload-monitoring` project by default during a OpenShift installation. You can use these components to monitor services and pods in user-defined projects.

These components are illustrated in the *User* section in the following diagram.

image:monitoring-architecture.png[OpenShift monitoring architecture]
