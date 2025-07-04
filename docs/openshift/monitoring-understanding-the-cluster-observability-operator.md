:_mod-docs-content-type: CONCEPT
[id="understanding-the-cluster-observability-operator_{context}"]
# Understanding the {coo-full}

A default monitoring stack created by the {coo-first} includes a highly available Prometheus instance capable of sending metrics to an external endpoint by using remote write.

Each {coo-short} stack also includes an optional Thanos Querier component, which you can use to query a highly available Prometheus instance from a central location, and an optional Alertmanager component, which you can use to set up alert configurations for different services.

[id="advantages-of-using-cluster-observability-operator_{context}"]
## Advantages of using the {coo-full}

The `MonitoringStack` CRD used by the {coo-short} offers an opinionated default monitoring configuration for {coo-short}-deployed monitoring components, but you can customize it to suit more complex requirements.

Deploying a {coo-short}-managed monitoring stack can help meet monitoring needs that are difficult or impossible to address by using the core platform monitoring stack deployed by the {cmo-first}.
A monitoring stack deployed using {coo-short} has the following advantages over core platform and user workload monitoring:

Extendability:: Users can add more metrics to a {coo-short}-deployed monitoring stack, which is not possible with core platform monitoring without losing support.
In addition, {coo-short}-managed stacks can receive certain cluster-specific metrics from core platform monitoring by using federation.
Multi-tenancy support:: The {coo-short} can create a monitoring stack per user namespace.
You can also deploy multiple stacks per namespace or a single stack for multiple namespaces.
For example, cluster administrators, SRE teams, and development teams can all deploy their own monitoring stacks on a single cluster, rather than having to use a single shared stack of monitoring components.
Users on different teams can then independently configure features such as separate alerts, alert routing, and alert receivers for their applications and services.
Scalability:: You can create {coo-short}-managed monitoring stacks as needed.
Multiple monitoring stacks can run on a single cluster, which can facilitate the monitoring of very large clusters by using manual sharding. This ability addresses cases where the number of metrics exceeds the monitoring capabilities of a single Prometheus instance.
Flexibility:: Deploying the {coo-short} with Operator Lifecycle Manager (OLM) decouples {coo-short} releases from OpenShift release cycles.
This method of deployment enables faster release iterations and the ability to respond rapidly to changing requirements and issues.
Additionally, by deploying a {coo-short}-managed monitoring stack, users can manage alerting rules independently of OpenShift release cycles.
Highly customizable:: The {coo-short} can delegate ownership of single configurable fields in custom resources to users by using Server-Side Apply (SSA), which enhances customization.
