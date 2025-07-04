:_mod-docs-content-type: CONCEPT
[id="monitoring-stack-in-ha-clusters_{context}"]
# The monitoring stack in high-availability clusters

By default, in multi-node clusters, the following components run in high-availability (HA) mode to prevent data loss and service interruption:

* Prometheus
* Alertmanager
* Thanos Ruler

* Thanos Querier
* Metrics Server
* Monitoring plugin

The component is replicated across two pods, each running on a separate node. This means that the monitoring stack can tolerate the loss of one pod.

Prometheus in HA mode::

* Both replicas independently scrape the same targets and evaluate the same rules.
* The replicas do not communicate with each other. Therefore, data might differ between the pods. 

Alertmanager in HA mode::

* The two replicas synchronize notification and silence states with each other. This ensures that each notification is sent at least once.
* If the replicas fail to communicate or if there is an issue on the receiving side, notifications are still sent, but they might be duplicated.

# [IMPORTANT]
# Prometheus, Alertmanager, and Thanos Ruler are stateful components. To ensure high availability, you must configure them with persistent storage.
