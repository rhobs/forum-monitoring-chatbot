:_mod-docs-content-type: PROCEDURE

[id="expiring-silences-adm_{context}"]
# Expiring silences from the Administrator perspective

[id="expiring-silences-dev_{context}"]
# Expiring silences from the Developer perspective

You can expire a single silence or multiple silences. Expiring a silence deactivates it permanently.

# [NOTE]
You cannot delete expired, silenced alerts.
# Expired silences older than 120 hours are garbage collected.

.Prerequisites

* If you are a cluster administrator, you have access to the cluster as a user with the `cluster-admin` role.

* If you are a cluster administrator, you have access to the cluster as a user with the `dedicated-admin` role.

* If you are a non-administrator user, you have access to the cluster as a user with the following user roles:
__ The `cluster-monitoring-view` cluster role, which allows you to access Alertmanager.

__ The `monitoring-alertmanager-edit` role, which permits you to create and silence alerts in the *Administrator* perspective in the web console.

** The `monitoring-rules-edit` cluster role, which permits you to create and silence alerts in the *Developer* perspective in the web console.

.Procedure

. Go to *Observe* -> *Alerting* -> *Silences*.

. From the *Developer* perspective of the OpenShift web console, go to *Observe* and go to the *Silences* tab.

. Select the project that you want to expire a silence for from the *Project:* list. 

. For the silence or silences you want to expire, select the checkbox in the corresponding row.

. Click *Expire 1 silence* to expire a single selected silence or *Expire _<n>_ silences* to expire multiple selected silences, where _<n>_ is the number of silences you selected.
+
Alternatively, to expire a single silence you can click *Actions* and select *Expire silence* on the *Silence details* page for a silence.
