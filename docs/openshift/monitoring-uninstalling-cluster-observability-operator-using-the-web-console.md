:_mod-docs-content-type: PROCEDURE
[id="uninstalling-the-cluster-observability-operator-using-the-web-console_{context}"]
# Uninstalling the {coo-full} using the web console
If you have installed the {coo-first} by using OperatorHub, you can uninstall it in the OpenShift web console.

.Prerequisites

* You have access to the cluster as a user with the `cluster-admin` cluster role.
* You have logged in to the OpenShift web console.

.Procedure

. Go to *Operators* -> *Installed Operators*.

. Locate the *{coo-full}* entry in the list.

. Click {kebab} for this entry and select *Uninstall Operator*.

.Verification

* Go to *Operators* -> *Installed Operators*, and verify that the *{coo-full}* entry no longer appears in the list.
