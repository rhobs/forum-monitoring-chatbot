:_mod-docs-content-type: CONCEPT
[id="about-openshift-monitoring_{context}"]
ifeval::["{context}" == "understanding-the-monitoring-stack"]
:ocp-monitoring:

# About OpenShift monitoring

:ocp-monitoring!:

OpenShift includes a preconfigured, preinstalled, and self-updating monitoring stack that provides *monitoring for core platform components*. OpenShift delivers monitoring best practices out of the box. A set of alerts are included by default that immediately notify cluster administrators about issues with a cluster. Default dashboards in the OpenShift web console include visual representations of cluster metrics to help you to quickly understand the state of your cluster.

After installing OpenShift , cluster administrators can optionally enable *monitoring for user-defined projects*. By using this feature, cluster administrators, developers, and other users can specify how services and pods are monitored in their own projects. You can then query metrics, review dashboards, and manage alerting rules and silences for your own projects in the OpenShift web console.

# [NOTE]
# Cluster administrators can grant developers and other users permission to monitor their own projects. Privileges are granted by assigning one of the predefined monitoring roles.

[id="about-openshift-monitoring_{context}"]
ifeval::["{context}" == "understanding-the-monitoring-stack"]
:!ocp-monitoring:
