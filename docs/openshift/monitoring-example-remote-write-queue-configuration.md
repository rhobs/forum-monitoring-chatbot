:_mod-docs-content-type: REFERENCE
[id="example-remote-write-queue-configuration_{context}"]
# Example remote write queue configuration

:configmap-name: cluster-monitoring-config
:namespace-name: openshift-monitoring
:component: prometheusK8s

:configmap-name: user-workload-monitoring-config
:namespace-name: openshift-user-workload-monitoring
:component: prometheus

You can use the `queueConfig` object for remote write to tune the remote write queue parameters. The following example shows the queue parameters with their default values for 

default platform monitoring

monitoring for user-defined projects

in the `{namespace-name}` namespace.

.Example configuration of remote write parameters with default values
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
      remoteWrite:
      - url: "https://remote-write-endpoint.example.com" 
        <endpoint_authentication_credentials>
        queueConfig:
          capacity: 10000 #<1>
          minShards: 1 #<2>
          maxShards: 50 #<3>
          maxSamplesPerSend: 2000 #<4>
          batchSendDeadline: 5s #<5>
          minBackoff: 30ms #<6>
          maxBackoff: 5s #<7>
          retryOnRateLimit: false #<8>
          sampleAgeLimit: 0s #<9>

```
<1> The number of samples to buffer per shard before they are dropped from the queue.
<2> The minimum number of shards.
<3> The maximum number of shards.
<4> The maximum number of samples per send.
<5> The maximum time for a sample to wait in buffer.
<6> The initial time to wait before retrying a failed request. The time gets doubled for every retry up to the `maxbackoff` time.
<7> The maximum time to wait before retrying a failed request.
<8> Set this parameter to `true` to retry a request after receiving a 429 status code from the remote write storage.
<9> The samples that are older than the `sampleAgeLimit` limit are dropped from the queue. If the value is undefined or set to `0s`, the parameter is ignored.

:!configmap-name:
:!namespace-name:
:!component:
