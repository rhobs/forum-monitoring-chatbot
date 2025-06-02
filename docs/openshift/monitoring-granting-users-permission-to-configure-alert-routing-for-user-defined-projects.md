:_mod-docs-content-type: PROCEDURE
[id="granting-users-permission-to-configure-alert-routing-for-user-defined-projects_{context}"]
# Granting users permission to configure alert routing for user-defined projects

You can grant users permission to configure alert routing for user-defined projects.

.Prerequisites


* You have access to the cluster as a user with the `dedicated-admin` role.
* The `user-workload-monitoring-config` `ConfigMap` object exists. This object is created by default when the cluster is created.


* You have access to the cluster as a user with the `cluster-admin` cluster role.
* You have enabled monitoring for user-defined projects.

* The user account that you are assigning the role to already exists.
* You have installed the OpenShift CLI (`oc`).

.Procedure

* Assign the `alert-routing-edit` cluster role to a user in the user-defined project:
+
```terminal
$ oc -n <namespace> adm policy add-role-to-user alert-routing-edit <user> <1>
```
<1> For `<namespace>`, substitute the namespace for the user-defined project, such as `ns1`. For `<user>`, substitute the username for the account to which you want to assign the role.
