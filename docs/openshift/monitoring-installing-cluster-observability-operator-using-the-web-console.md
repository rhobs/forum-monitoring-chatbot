:_mod-docs-content-type: PROCEDURE
[id="installing-the-cluster-observability-operator-in-the-web-console-_{context}"]
# Installing the {coo-full} in the web console
Install the {coo-first} from OperatorHub by using the OpenShift web console.

.Prerequisites

* You have access to the cluster as a user with the `cluster-admin` cluster role.
* You have logged in to the OpenShift web console.

.Procedure

. In the OpenShift web console, click *Operators* -> *OperatorHub*.
. Type `cluster observability operator` in the *Filter by keyword* box.
. Click  *{coo-full}* in the list of results.
. Read the information about the Operator, and configure the following installation settings:
+
* *Update channel* -> *stable*
* *Version* -> *1.0.0* or later
* *Installation mode* -> *All namespaces on the cluster (default)*
* *Installed Namespace* -> *Operator recommended Namespace: openshift-cluster-observability-operator*
* Select *Enable Operator recommended cluster monitoring on this Namespace*
* *Update approval* -> *Automatic*

. Optional: You can change the installation settings to suit your requirements.
For example, you can select to subscribe to a different update channel, to install an older released version of the Operator, or to require manual approval for updates to new versions of the Operator.
. Click *Install*.

.Verification

* Go to *Operators* -> *Installed Operators*, and verify that the *{coo-full}* entry appears in the list.
