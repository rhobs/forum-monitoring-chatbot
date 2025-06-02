:_mod-docs-content-type: PROCEDURE
[id="listing-alerting-rules-for-all-projects-in-a-single-view_{context}"]
# Listing alerting rules for all projects in a single view


As a cluster administrator,


As a `dedicated-admin`,

you can list alerting rules for core OpenShift and user-defined projects together in a single view.

.Prerequisites


* You have access to the cluster as a user with the `dedicated-admin` role.


* You have access to the cluster as a user with the `cluster-admin` role.

* You have installed the OpenShift CLI (`oc`).

.Procedure

. From the *Administrator* perspective of the OpenShift web console, go to *Observe* -> *Alerting* -> *Alerting rules*.

. Select the *Platform* and *User* sources in the *Filter* drop-down menu.
+
[NOTE]
====
The *Platform* source is selected by default.
====
