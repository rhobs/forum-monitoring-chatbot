:_mod-docs-content-type: PROCEDURE
[id="configuring-monitoring-for-an-application_{context}"]
# Configuring monitoring for an application

This procedure shows, on an example, how an application developer can deploy an application and configure monitoring for it.

.Prerequisites

* Make sure you configured the cluster for application monitoring. In this example, it is presumed that Prometheus and Alertmanager instances were installed in the `default` project.

.Procedure

. Create a YAML file for your configuration. In this example, it is called `deploy.yaml`.

. Add configuration for deploying a sample application:
+

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: example-app
  namespace: default
spec:
  selector:
    matchLabels:
      app: example-app
  replicas: 1
  template:
    metadata:
      labels:
        app: example-app
    spec:
      containers:
      - name: example-app
        image: ghcr.io/rhobs/prometheus-example-app:0.4.2
        ports:
        - name: web
##           containerPort: 8080

```

. Add configuration for exposing the sample application as a service:
+

```yaml
kind: Service
apiVersion: v1
metadata:
  name: example-app
  namespace: default
  labels:
    tier: frontend
spec:
  selector:
    app: example-app
  ports:
  - name: web
##     port: 8080

```

. Add configuration for creating a service monitor for the sample application. This will add your application as a target for monitoring:
+

```yaml
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: example-app
  namespace: default
  labels:
    k8s-app: example-app
spec:
  selector:
    matchLabels:
      tier: frontend
  endpoints:
  - port: web

```

. Apply the configuration file to the cluster:
+

```terminal
$ oc apply -f deploy.yaml

```

. Forward a port to the Prometheus UI. In this example, port 9090 is used:
+

```terminal
$ oc port-forward -n openshift-user-workload-monitoring svc/prometheus-operated 9090

```

. Navigate to the Prometheus UI at http://localhost:9090/targets to see the sample application being monitored.
