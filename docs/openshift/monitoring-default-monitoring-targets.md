:_mod-docs-content-type: REFERENCE
[id="default-monitoring-targets_{context}"]
# Default monitoring targets


In addition to the components of the stack itself, the default monitoring stack monitors additional platform components.

The following are examples of monitoring targets:



The following are examples of targets monitored by Red{nbsp}Hat Site Reliability Engineers (SRE) in your OpenShift cluster:


* CoreDNS
* etcd
* HAProxy
* Image registry
* Kubelets
* Kubernetes API server
* Kubernetes controller manager
* Kubernetes scheduler

* OpenShift API server
* OpenShift Controller Manager
* Operator Lifecycle Manager (OLM)



[NOTE]
====
The exact list of targets can vary depending on your cluster capabilities and installed components.
====



[NOTE]
====
* The exact list of targets can vary depending on your cluster capabilities and installed components.

* Each OpenShift component is responsible for its monitoring configuration. For problems with the monitoring of an OpenShift component, open a
link:https://issues.redhat.com/secure/CreateIssueDetails!init.jspa?pid=12332330&summary=Monitoring_issue&issuetype=1&priority=10200&versions=12385624[Jira issue] against that component, not against the general monitoring component.
====

Other OpenShift framework components might be exposing metrics as well. For details, see their respective documentation.

