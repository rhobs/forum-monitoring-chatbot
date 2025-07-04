:_mod-docs-content-type: PROCEDURE
[id="deploying-a-sample-service_{context}"]
# Deploying a sample service

To test monitoring of a service in a user-defined project, you can deploy a sample service.

.Prerequisites

* You have access to the cluster as a user with the `cluster-admin` cluster role or as a user with administrative permissions for the namespace.

.Procedure

. Create a YAML file for the service configuration. In this example, it is called `prometheus-example-app.yaml`.

. Add the following deployment and service configuration details to the file:
+

```yaml
apiVersion: v1
kind: Namespace
metadata:
##   name: ns1
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: prometheus-example-app
  name: prometheus-example-app
  namespace: ns1
spec:
  replicas: 1
  selector:
    matchLabels:
      app: prometheus-example-app
  template:
    metadata:
      labels:
        app: prometheus-example-app
    spec:
      containers:
      - image: ghcr.io/rhobs/prometheus-example-app:0.4.2
        imagePullPolicy: IfNotPresent
##         name: prometheus-example-app
apiVersion: v1
kind: Service
metadata:
  labels:
    app: prometheus-example-app
  name: prometheus-example-app
  namespace: ns1
spec:
  ports:
  - port: 8080
    protocol: TCP
    targetPort: 8080
    name: web
  selector:
    app: prometheus-example-app
  type: ClusterIP

```
+
This configuration deploys a service named `prometheus-example-app` in the user-defined `ns1` project. This service exposes the custom `version` metric.

. Apply the configuration to the cluster:
+

```terminal
$ oc apply -f prometheus-example-app.yaml

```
+
It takes some time to deploy the service.

. You can check that the pod is running:
+

```terminal
$ oc -n ns1 get pod

```
+
.Example output

```terminal
NAME                                      READY     STATUS    RESTARTS   AGE
prometheus-example-app-7857545cb7-sbgwq   1/1       Running   0          81m

```
