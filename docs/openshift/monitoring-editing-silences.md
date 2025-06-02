:_mod-docs-content-type: PROCEDURE




[id="editing-silences-adm_{context}"]
# Editing silences from the Administrator perspective



[id="editing-silences-dev_{context}"]
# Editing silences from the Developer perspective


You can edit a silence, which expires the existing silence and creates a new one with the changed configuration.

.Prerequisites


* If you are a cluster administrator, you have access to the cluster as a user with the `cluster-admin` role.


* If you are a cluster administrator, you have access to the cluster as a user with the `dedicated-admin` role.

* If you are a non-administrator user, you have access to the cluster as a user with the following user roles:
** The `cluster-monitoring-view` cluster role, which allows you to access Alertmanager.

** The `monitoring-alertmanager-edit` role, which permits you to create and silence alerts in the *Administrator* perspective in the web console.


** The `monitoring-rules-edit` cluster role, which permits you to create and silence alerts in the *Developer* perspective in the web console.


.Procedure


. From the *Administrator* perspective of the OpenShift web console, go to *Observe* -> *Alerting* -> *Silences*.



. From the *Developer* perspective of the OpenShift web console, go to *Observe* and go to the *Silences* tab.
. Select the project that you want to edit silences for from the *Project:* list. 


. For the silence you want to modify, click {kebab} and select *Edit silence*.
+
Alternatively, you can click *Actions* and select *Edit silence* on the *Silence details* page for a silence.

. On the *Edit silence* page, make changes and click *Silence*. Doing so expires the existing silence and creates one with the updated configuration.





