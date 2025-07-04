:_mod-docs-content-type: PROCEDURE
[id="monitoring-configuring-external-alertmanagers_{context}"]
# Configuring external Alertmanager instances

:configmap-name: cluster-monitoring-config
:namespace-name: openshift-monitoring
:component: prometheusK8s
:component-name: Prometheus

:configmap-name: user-workload-monitoring-config
:namespace-name: openshift-user-workload-monitoring
:component: thanosRuler
:component-name: Thanos Ruler

The OpenShift monitoring stack includes a local Alertmanager instance that routes alerts from Prometheus.

You can add external Alertmanager instances to route alerts for core OpenShift projects.

You can add external Alertmanager instances to route alerts for user-defined projects.

If you add the same external Alertmanager configuration for multiple clusters and disable the local instance for each cluster, you can then manage alert routing for multiple clusters by using a single external Alertmanager instance.

.Prerequisites

* You have access to the cluster as a user with the `cluster-admin` cluster role.
* You have created the `cluster-monitoring-config` `ConfigMap` object.

* You have access to the cluster as a user with the `cluster-admin` cluster role or as a user with the `user-workload-monitoring-config-edit` role in the `openshift-user-workload-monitoring` project.
* A cluster administrator has enabled monitoring for user-defined projects.

* You have access to the cluster as a user with the `dedicated-admin` role.
* The `user-workload-monitoring-config` `ConfigMap` object exists. This object is created by default when the cluster is created.

* You have installed the OpenShift CLI (`oc`).

.Procedure

. Edit the `{configmap-name}` config map in the `{namespace-name}` project:
+
[source,terminal,subs="attributes+"]

```
$ oc -n {namespace-name} edit configmap {configmap-name}

```

. Add an `additionalAlertmanagerConfigs` section with configuration details under 

`data/config.yaml/prometheusK8s`:

`data/config.yaml/<component>`:

+
[source,yaml,subs="attributes+"]

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: {configmap-name}
  namespace: {namespace-name}
data:
  config.yaml: |
# tag::CPM[]
    prometheusK8s:
# end::CPM[]
# tag::UWM[]
    <component>: # <2>
# end::UWM[]
      additionalAlertmanagerConfigs:
      - <alertmanager_specification> # <1>

```
<1> Substitute `<alertmanager_specification>` with authentication and other configuration details for additional Alertmanager instances.
Currently supported authentication methods are bearer token (`bearerToken`) and client TLS (`tlsConfig`).

<2> Substitute `<component>` for one of two supported external Alertmanager components: `prometheus` or `thanosRuler`.

+
The following sample config map configures an additional Alertmanager for {component-name} by using a bearer token with client TLS authentication:
+
[source,yaml,subs="attributes+"]

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: {configmap-name}
  namespace: {namespace-name}
data:
  config.yaml: |
    {component}:
      additionalAlertmanagerConfigs:
      - scheme: https
        pathPrefix: /
        timeout: "30s"
        apiVersion: v1
        bearerToken:
          name: alertmanager-bearer-token
          key: token
        tlsConfig:
          key:
            name: alertmanager-tls
            key: tls.key
          cert:
            name: alertmanager-tls
            key: tls.crt
          ca:
            name: alertmanager-tls
            key: tls.ca
        staticConfigs:
        - external-alertmanager1-remote.com
        - external-alertmanager1-remote2.com

```

. Save the file to apply the changes. The pods affected by the new configuration are automatically redeployed.

:!configmap-name:
:!namespace-name:
:!component:
:!component-name:
