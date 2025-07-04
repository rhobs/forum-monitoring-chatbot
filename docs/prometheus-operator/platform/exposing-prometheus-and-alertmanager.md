---
weight: 202
toc: true
title: Exposing Prometheus and Alertmanager
menu:
    docs:
        parent: user-guides
lead: ""
images: []
draft: false
## description: Guide to exposing Prometheus and Alertmanager in Kubernetes using NodePort, ClusterIP, or Ingress for external access.

<br>
<div class="alert alert-info" role="alert">
    <i class="fa fa-exclamation-triangle"></i><b> Note:</b> Starting with v0.39.0, Prometheus Operator requires use of Kubernetes v1.16.x and up.
</div>

# Exposing Prometheus and Alertmanager

The Prometheus Operator takes care of operating Prometheus and Alertmanagers clusters. Kubernetes provides several ways to expose these clusters to the outside world. This document outlines best practices and caveats for exposing Prometheus and Alertmanager clusters.

## NodePort

The easiest way to expose Prometheus or Alertmanager is to use a Service of type `NodePort`.

Create a simple Prometheus object with one replica:

```yaml
apiVersion: monitoring.coreos.com/v1
kind: Prometheus
metadata:
  name: main
spec:
  resources:
    requests:
      memory: 400Mi

```

All Prometheus Pods are labeled with `prometheus: <prometheus-name>`. If the Prometheus object's name is `main`, the selector is `prometheus: main`. This means that the respective manifest for the `Service` must define the selector as `prometheus: main`.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: prometheus-main
spec:
  type: NodePort
  ports:
  - name: web
    nodePort: 30900
    port: 9090
    protocol: TCP
    targetPort: web
  selector:
    prometheus: main

```

After creating a Service with the above manifest, the web UI of Prometheus will be accessible by browsing to any of the worker nodes using `http://<node-ip>:30900/`.

Exposing the Alertmanager works in the same fashion, with the selector `alertmanager: <alertmanager-name>`.

```yaml
apiVersion: monitoring.coreos.com/v1
kind: Alertmanager
metadata:
  name: main
spec:
  replicas: 3
  resources:
    requests:
      memory: 400Mi

```

And the `Service`.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: alertmanager-main
spec:
  type: NodePort
  ports:
  - name: web
    nodePort: 30903
    port: 9093
    protocol: TCP
    targetPort: web
  selector:
    alertmanager: main

```

The Alertmanager web UI will be available at `http://<node-ip>:30903/`.

## Kubernetes API

The Kubernetes API has a feature of forwarding requests from the API to a cluster internal Service. The general URL scheme to access these is:



```ttp(s)://control-plane-host/api/v1/proxy/namespaces/<namespace>/services/<service-name>:<port-name-or-number>/

```

> For ease of use, use `kubectl proxy`. It proxies requests from a local address to the Kubernetes API server and handles authentication.

To be able to do so, create a Service of type `ClusterIP`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: prometheus-main
spec:
  type: ClusterIP
  ports:
  - name: web
    port: 9090
    protocol: TCP
    targetPort: web
  selector:
    prometheus: main

```

Prometheus and Alertmanager must be configured with the full URL at which they will be exposed. Therefore the Prometheus manifest requires an entry for `externalUrl`:

```yaml
apiVersion: monitoring.coreos.com/v1
kind: Prometheus
metadata:
  name: main
spec:
  externalUrl: http://127.0.0.1:8001/api/v1/proxy/namespaces/default/services/prometheus-main:web/
  resources:
    requests:
      memory: 400Mi

```

> Note the `externalUrl` uses the host `127.0.0.1:8001`, which is how `kubectl proxy` exposes the Kubernetes API by default.

Once the Prometheus Pods are running they are reachable under the specified `externalUrl`.

The Alertmanager object's manifest follows the same rules:

```yaml
apiVersion: monitoring.coreos.com/v1
kind: Alertmanager
metadata:
  name: main
spec:
  replicas: 3
  externalUrl: http://127.0.0.1:8001/api/v1/proxy/namespaces/default/services/alertmanager-main:web/
  resources:
    requests:
      memory: 400Mi

```

And the respective Service:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: alertmanager-main
spec:
  type: ClusterIP
  ports:
  - name: web
    port: 9093
    protocol: TCP
    targetPort: web
  selector:
    alertmanager: main

```

Then it will be available under http://127.0.0.1:8001/api/v1/proxy/namespaces/default/services/alertmanager-main:web/.

> Note the URL for the Service uses the host `127.0.0.1:8001`, which is how `kubectl proxy` exposes the Kubernetes API by default.

## Ingress

Exposing the Prometheus or Alertmanager web UI through an Ingress object requires a running Ingress controller. For more information, see the Kubernetes documentation on [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/).

This example was tested with the [NGINX Ingress Controller](https://github.com/kubernetes/ingress-nginx). For a quick-start for running the NGINX Ingress Controller follow the instructions in the [NGINX Ingress installation guide](https://github.com/kubernetes/ingress-nginx/blob/main/docs/deploy/index.md).

> Be certain to evaluate available Ingress controllers for the specifics of your production environment. The NGINX Ingress Controller may or may not be suitable. Consider other solutions, like HA Proxy, Traefik, GCE, or AWS.

> WARNING: If your Ingress is exposed to the internet, everyone can have full access on your resources. It's strongly recommend to enable an [external authentication](https://github.com/kubernetes/ingress-nginx/blob/351ce5394a5ec37153344135c8112f71539b5af4/docs/examples/auth/external-auth/README.md) or [whitelisting IP address](https://github.com/kubernetes/ingress-nginx/blob/351ce5394a5ec37153344135c8112f71539b5af4/docs/user-guide/nginx-configuration/annotations.md#whitelist-source-range).

An Ingress object also requires a Service to be set up, as the requests are routed from the Ingress endpoint to the internal Service.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: prometheus-main
spec:
  type: ClusterIP
  ports:
  - name: web
    port: 9090
    protocol: TCP
    targetPort: web
  selector:
##     prometheus: main
apiVersion: v1
kind: Service
metadata:
  name: alertmanager-main
spec:
  type: ClusterIP
  ports:
  - name: web
    port: 9093
    protocol: TCP
    targetPort: web
  selector:
    alertmanager: main

```

A corresponding Ingress object would be:

```yaml
apiVersion: extensions/v1
kind: Ingress
metadata:
  name: monitoring
  annotations:
    ingress.kubernetes.io/whitelist-source-range: 10.0.0.0/16 # change this range to admin IPs
    nginx.ingress.kubernetes.io/rewrite-target: /$2
spec:
  rules:
  - http:
      paths:
      - backend:
          serviceName: prometheus-main
          servicePort: 9090
        path: /prometheus(/|$)(.*)
      - backend:
          serviceName: alertmanager-main
          servicePort: 9093
        path: /alertmanager(/|$)(.*)

```

Finally, the Prometheus and `Alertmanager` objects must be created, specifying the `externalUrl` at which they will be found.

```yaml
apiVersion: monitoring.coreos.com/v1
kind: Prometheus
metadata:
  name: main
spec:
  externalUrl: http://monitoring.my.systems/prometheus
  resources:
    requests:
##       memory: 400Mi
apiVersion: monitoring.coreos.com/v1
kind: Alertmanager
metadata:
  name: main
spec:
  replicas: 3
  externalUrl: http://monitoring.my.systems/alertmanager
  resources:
    requests:
      memory: 400Mi

```

> Note the path `/prometheus` at the end of the `externalUrl`, as specified in the `Ingress` object.
