:_mod-docs-content-type: CONCEPT
[id="understanding-alert-routing-for-user-defined-projects_{context}"]
# Understanding alert routing for user-defined projects

As a cluster administrator, you can enable alert routing for user-defined projects.

As a `dedicated-admin`, you can enable alert routing for user-defined projects.

With this feature, you can allow users with the `alert-routing-edit` cluster role to configure alert notification routing and receivers for user-defined projects.

These notifications are routed by the default Alertmanager instance or, if enabled, an optional Alertmanager instance dedicated to user-defined monitoring.

These notifications are routed by an Alertmanager instance dedicated to user-defined monitoring.

Users can then create and configure user-defined alert routing by creating or editing the `AlertmanagerConfig` objects for their user-defined projects without the help of an administrator.

After a user has defined alert routing for a user-defined project, user-defined alert notifications are routed as follows:

* To the `alertmanager-main` pods in the `openshift-monitoring` namespace if using the default platform Alertmanager instance.

* To the `alertmanager-user-workload` pods in the `openshift-user-workload-monitoring` namespace if you have enabled a separate instance of Alertmanager for user-defined projects.

After a user has defined alert routing for a user-defined project, user-defined alert notifications are routed to the `alertmanager-user-workload` pods in the `openshift-user-workload-monitoring` namespace.

# [NOTE]
Review the following limitations of alert routing for user-defined projects:

* For user-defined alerting rules, user-defined routing is scoped to the namespace in which the resource is defined. For example, a routing configuration in namespace `ns1` only applies to `PrometheusRules` resources in the same namespace.

# * When a namespace is excluded from user-defined monitoring, `AlertmanagerConfig` resources in the namespace cease to be part of the Alertmanager configuration.
