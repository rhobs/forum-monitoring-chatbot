:_mod-docs-content-type: PROCEDURE
[id="disabling-cross-project-alerting-rules-for-user-defined-projects_{context}"]
# Disabling cross-project alerting rules for user-defined projects

Creating cross-project alerting rules for user-defined projects is enabled by default. Cluster administrators can disable the capability in the `cluster-monitoring-config` config map for the following reasons:

* To prevent user-defined monitoring from overloading the cluster monitoring stack. 
* To prevent buggy alerting rules from being applied to the cluster without having to identify the rule that causes the issue.

.Prerequisites


* You have access to the cluster as a user with the `cluster-admin` cluster role.


* You have access to the cluster as a user with the `dedicated-admin` role.

* You have installed the OpenShift CLI (`oc`).

.Procedure

. Edit the `cluster-monitoring-config` config map in the `openshift-monitoring` project:
+
```terminal
$ oc -n openshift-monitoring edit configmap cluster-monitoring-config
```

. In the `cluster-monitoring-config` config map, disable the option to create cross-project alerting rules by setting the `rulesWithoutLabelEnforcementAllowed` value under `data/config.yaml/userWorkload` to `false`:
+
```yaml
kind: ConfigMap
apiVersion: v1
metadata:
  name: cluster-monitoring-config
  namespace: openshift-monitoring
data:
  config.yaml: |
    userWorkload:
      rulesWithoutLabelEnforcementAllowed: false
    # ...
```

. Save the file to apply the changes.



