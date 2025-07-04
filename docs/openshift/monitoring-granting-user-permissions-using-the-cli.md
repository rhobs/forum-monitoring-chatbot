:_mod-docs-content-type: PROCEDURE
[id="granting-user-permissions-using-the-cli_{context}"]
# Granting user permissions by using the CLI

You can grant users permissions

for the `openshift-monitoring` project or

to monitor

their own projects, by using the {oc-first}.

# [IMPORTANT]
# Whichever role or cluster role you choose, you must bind it against a specific project as a cluster administrator.

.Prerequisites

* You have access to the cluster as a user with the `cluster-admin` cluster role.
* The user account that you are assigning the role to already exists.
* You have installed the {oc-first}.

.Procedure

* To assign a monitoring role to a user for a project, enter the following command:
+

```terminal
$ oc adm policy add-role-to-user <role> <user> -n <namespace> --role-namespace <namespace> <1>

```
<1> Substitute `<role>` with the wanted monitoring role, `<user>` with the user to whom you want to assign the role, and `<namespace>` with the project where you want to grant the access.

* To assign a monitoring cluster role to a user for a project, enter the following command:
+

```terminal
$ oc adm policy add-cluster-role-to-user <cluster-role> <user> -n <namespace> <1>

```
<1> Substitute `<cluster-role>` with the wanted monitoring cluster role, `<user>` with the user to whom you want to assign the cluster role, and `<namespace>` with the project where you want to grant the access.
