:_mod-docs-content-type: PROCEDURE
[id="configuring-alert-routing-for-user-defined-projects_{context}"]
# Configuring alert routing for user-defined projects

If you are a non-administrator user who has been given the `alert-routing-edit` cluster role, you can create or edit alert routing for user-defined projects.

.Prerequisites

* A cluster administrator has enabled monitoring for user-defined projects.
* A cluster administrator has enabled alert routing for user-defined projects.

* Alert routing has been enabled for user-defined projects.

* You are logged in as a user that has the `alert-routing-edit` cluster role for the project for which you want to create alert routing.
* You have installed the OpenShift CLI (`oc`).

.Procedure

. Create a YAML file for alert routing. The example in this procedure uses a file called `example-app-alert-routing.yaml`.

. Add an `AlertmanagerConfig` YAML definition to the file. For example:
+

```yaml
apiVersion: monitoring.coreos.com/v1beta1
kind: AlertmanagerConfig
metadata:
  name: example-routing
  namespace: ns1
spec:
  route:
    receiver: default
    groupBy: [job]
  receivers:
  - name: default
    webhookConfigs:
    - url: https://example.org/post

```

. Save the file.

. Apply the resource to the cluster:
+

```terminal
$ oc apply -f example-app-alert-routing.yaml

```
+
The configuration is automatically applied to the Alertmanager pods.
