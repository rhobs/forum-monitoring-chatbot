:_mod-docs-content-type: PROCEDURE
[id="monitoring-configuring-shiftstack-remotewrite_{context}"]
# Remote writing to an external Prometheus instance

Use remote write with both {rhoso-first} and OpenShift to push their metrics to an external Prometheus instance.

.Prerequisites

- You have access to an external Prometheus instance.
- You have administrative access to {rhoso} and your cluster.
- You have certificates for secure communication with mTLS.
- Your Prometheus instance is configured for client TLS certificates and has been set up as a remote write receiver.
- The Cluster Observability Operator is installed on your {rhoso} cluster.
- The monitoring stack for your {rhoso} cluster is configured to collect the metrics that you are interested in.
- Telemetry is enabled in the {rhoso} environment.
+
# [NOTE]
To verify that the telemetry service is operating normally, entering the following command:

```shell
$ oc -n openstack get monitoringstacks metric-storage -o yaml

```
# The `monitoringstacks` CRD indicates whether telemetry is enabled correctly.

.Procedure

. Configure your {rhoso} management cluster to send metrics to Prometheus:

.. Create a secret that is named `mtls-bundle` in the `openstack` namespace that contains HTTPS client certificates for authentication to Prometheus by entering the following command:
+

```shell
$ oc --namespace openstack \
    create secret generic mtls-bundle \
        --from-file=./ca.crt \
        --from-file=osp-client.crt \
        --from-file=osp-client.key

```

.. Open the `controlplane` configuration for editing by running the following command:
+

```shell
$ oc -n openstack edit openstackcontrolplane/controlplane

```

.. With the configuration open, replace the `.spec.telemetry.template.metricStorage` section so that {rhoso} sends metrics to Prometheus. As an example:
+

```yaml
      metricStorage:
        customMonitoringStack:
          alertmanagerConfig:
            disabled: false
          logLevel: info
          prometheusConfig:
            scrapeInterval: 30s
            remoteWrite:
            - url: https://external-prometheus.example.com/api/v1/write # <1>
              tlsConfig:
                ca:
                  secret:
                    name: mtls-bundle
                    key: ca.crt
                cert:
                  secret:
                    name: mtls-bundle
                    key: ocp-client.crt
                keySecret:
                  name: mtls-bundle
                  key: ocp-client.key
            replicas: 2
          resourceSelector:
            matchLabels:
              service: metricStorage
          resources:
            limits:
              cpu: 500m
              memory: 512Mi
            requests:
              cpu: 100m
              memory: 256Mi
          retention: 1d # <2>
        dashboardsEnabled: false
        dataplaneNetwork: ctlplane
        enabled: true
        prometheusTls: {}

```
<1> Replace this URL with the URL of your Prometheus instance.
<2> Set a retention period. Optionally, you can reduce retention for local metrics because of external collection.

. Configure the tenant cluster on which your workloads run to send metrics to Prometheus:

.. Create a cluster monitoring config map as a YAML file. The map must include a remote write configuration and cluster identifiers. As an example:
+

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: cluster-monitoring-config
  namespace: openshift-monitoring
data:
  config.yaml: |
    prometheusK8s:
      retention: 1d # <1>
      remoteWrite:
      - url: "https://external-prometheus.example.com/api/v1/write"
        writeRelabelConfigs:
        - sourceLabels:
          - __tmp_openshift_cluster_id__
          targetLabel: cluster_id
          action: replace
        tlsConfig:
          ca:
            secret:
              name: mtls-bundle
              key: ca.crt
          cert:
            secret:
              name: mtls-bundle
              key: ocp-client.crt
          keySecret:
            name: mtls-bundle
            key: ocp-client.key

```
<1> Set a retention period. Optionally, you can reduce retention for local metrics because of external collection.

.. Save the config map as a file called `cluster-monitoring-config.yaml`.

.. Create a secret that is named `mtls-bundle` in the `openshift-monitoring` namespace that contains HTTPS client certificates for authentication to Prometheus by entering the following command:
+

```terminal
$ oc --namespace openshift-monitoring \
    create secret generic mtls-bundle \
        --from-file=./ca.crt \
        --from-file=ocp-client.crt \
        --from-file=ocp-client.key

```

.. Apply the cluster monitoring configuration by running the following command:
+

```terminal
$ oc apply -f cluster-monitoring-config.yaml

```

After the changes propagate, you can see aggregated metrics in your external Prometheus instance.
