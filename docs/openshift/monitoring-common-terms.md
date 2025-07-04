:_mod-docs-content-type: REFERENCE
[id="monitoring-common-terms_{context}"]
# Glossary of common terms for OpenShift monitoring

This glossary defines common terms that are used in OpenShift architecture.

Alertmanager::
Alertmanager handles alerts received from Prometheus. Alertmanager is also responsible for sending the alerts to external notification systems.

Alerting rules::
Alerting rules contain a set of conditions that outline a particular state within a cluster. Alerts are triggered when those conditions are true. An alerting rule can be assigned a severity that defines how the alerts are routed.

{cmo-full}::
The {cmo-first} is a central component of the monitoring stack. It deploys and manages Prometheus instances such as, the Thanos Querier, the Telemeter Client, and metrics targets to ensure that they are up to date. The {cmo-short} is deployed by the Cluster Version Operator (CVO).

Cluster Version Operator::
The Cluster Version Operator (CVO) manages the lifecycle of cluster Operators, many of which are installed in OpenShift by default.

config map::
A config map provides a way to inject configuration data into pods. You can reference the data stored in a config map in a volume of type `ConfigMap`. Applications running in a pod can use this data.

Container::
A container is a lightweight and executable image that includes software and all its dependencies. Containers virtualize the operating system. As a result, you can run containers anywhere from a data center to a public or private cloud as well as a developer’s laptop.

custom resource (CR)::
A CR is an extension of the Kubernetes API. You can create custom resources.

etcd::
etcd is the key-value store for OpenShift, which stores the state of all resource objects.

Fluentd::
Fluentd is a log collector that resides on each OpenShift node. It gathers application, infrastructure, and audit logs and forwards them to different outputs.
## +
## include::snippets/logging-fluentd-dep-snip.adoc[]

Kubelets::
Runs on nodes and reads the container manifests. Ensures that the defined containers have started and are running.

Kubernetes API server::
Kubernetes API server validates and configures data for the API objects.

Kubernetes controller manager::
Kubernetes controller manager governs the state of the cluster.

Kubernetes scheduler::
Kubernetes scheduler allocates pods to nodes.

labels::
Labels are key-value pairs that you can use to organize and select subsets of objects such as a pod.

Metrics Server::
The Metrics Server monitoring component collects resource metrics and exposes them in the `metrics.k8s.io` Metrics API service for use by other tools and APIs, which frees the core platform Prometheus stack from handling this functionality.

node::
A worker machine in the OpenShift cluster. A node is either a virtual machine (VM) or a physical machine.

Operator::
The preferred method of packaging, deploying, and managing a Kubernetes application in an OpenShift cluster. An Operator takes human operational knowledge and encodes it into software that is packaged and shared with customers.

Operator Lifecycle Manager (OLM)::
OLM helps you install, update, and manage the lifecycle of Kubernetes native applications. OLM is an open source toolkit designed to manage Operators in an effective, automated, and scalable way.

Persistent storage::
Stores the data even after the device is shut down. Kubernetes uses persistent volumes to store the application data.

Persistent volume claim (PVC)::
You can use a PVC to mount a PersistentVolume into a Pod. You can access the storage without knowing the details of the cloud environment.

pod::
The pod is the smallest logical unit in Kubernetes. A pod is comprised of one or more containers to run in a worker node.

Prometheus::
Prometheus is the monitoring system on which the OpenShift monitoring stack is based. Prometheus is a time-series database and a rule evaluation engine for metrics. Prometheus sends alerts to Alertmanager for processing.

Prometheus Operator::
The Prometheus Operator (PO) in the `openshift-monitoring` project creates, configures, and manages platform Prometheus and Alertmanager instances. It also automatically generates monitoring target configurations based on Kubernetes label queries.

Silences::
A silence can be applied to an alert to prevent notifications from being sent when the conditions for an alert are true. You can mute an alert after the initial notification, while you work on resolving the underlying issue.

storage::

OpenShift supports many types of storage, both for on-premise and cloud providers.

OpenShift supports many types of storage on AWS and GCP.

OpenShift supports many types of storage on AWS.

You can manage container storage for persistent and non-persistent data in an OpenShift cluster.

Thanos Ruler::
The Thanos Ruler is a rule evaluation engine for Prometheus that is deployed as a separate process. In OpenShift, Thanos Ruler provides rule and alerting evaluation for the monitoring of user-defined projects.

Vector::
Vector is a log collector that deploys to each OpenShift node. It collects log data from each node, transforms the data, and forwards it to configured outputs.

web console::
A user interface (UI) to manage OpenShift.
