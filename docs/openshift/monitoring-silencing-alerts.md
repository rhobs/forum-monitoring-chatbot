:_mod-docs-content-type: PROCEDURE

[id="silencing-alerts-adm_{context}"]
# Silencing alerts from the Administrator perspective

[id="silencing-alerts-dev_{context}"]
# Silencing alerts from the Developer perspective

You can silence a specific alert or silence alerts that match a specification that you define.

.Prerequisites

* You have access to the cluster as a user with the `cluster-admin` role.

* If you are a cluster administrator, you have access to the cluster as a user with the `cluster-admin` role.

* If you are a cluster administrator, you have access to the cluster as a user with the `dedicated-admin` role.

* If you are a non-administrator user, you have access to the cluster as a user with the following user roles:
__ The `cluster-monitoring-view` cluster role, which allows you to access Alertmanager.
__ The `monitoring-alertmanager-edit` role, which permits you to create and silence alerts in the *Administrator* perspective in the web console.
** The `monitoring-rules-edit` cluster role, which permits you to create and silence alerts in the *Developer* perspective in the web console.

.Procedure

To silence a specific alert:

. From the *Administrator* perspective of the OpenShift web console, go to *Observe* -> *Alerting* -> *Alerts*.

. For the alert that you want to silence, click {kebab} and select *Silence alert* to open the *Silence alert* page with a default configuration for the chosen alert.

. Optional: Change the default configuration details for the silence.
+
# [NOTE]
# You must add a comment before saving a silence.

. To save the silence, click *Silence*.

To silence a set of alerts:

. From the *Administrator* perspective of the OpenShift web console, go to *Observe* -> *Alerting* -> *Silences*.

. Click *Create silence*.

. On the *Create silence* page, set the schedule, duration, and label details for an alert.
+
# [NOTE]
# You must add a comment before saving a silence.

. To create silences for alerts that match the labels that you entered, click *Silence*.

To silence a specific alert:

. From the *Developer* perspective of the OpenShift web console, go to *Observe* and go to the *Alerts* tab.

. Select the project that you want to silence an alert for from the *Project:* list. 

. If necessary, expand the details for the alert by clicking a greater than symbol (*>*) next to the alert name.

. Click the alert message in the expanded view to open the *Alert details* page for the alert.

. Click *Silence alert* to open the *Silence alert* page with a default configuration for the alert.

. Optional: Change the default configuration details for the silence.
+
# [NOTE]
# You must add a comment before saving a silence.

. To save the silence, click *Silence*.

To silence a set of alerts:

. From the *Developer* perspective of the OpenShift web console, go to *Observe* and go to the *Silences* tab.

. Select the project that you want to silence alerts for from the *Project:* list. 

. Click *Create silence*.

. On the *Create silence* page, set the duration and label details for an alert.
+
# [NOTE]
# You must add a comment before saving a silence.

. To create silences for alerts that match the labels that you entered, click *Silence*.
