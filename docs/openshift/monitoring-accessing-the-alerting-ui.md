:_mod-docs-content-type: PROCEDURE

[id="monitoring-accessing-the-alerting-ui-adm_{context}"]
# Accessing the Alerting UI from the Administrator perspective

[id="monitoring-accessing-the-alerting-ui-dev_{context}"]
# Accessing the Alerting UI from the Developer perspective

:perspective: Administrator

:perspective: Developer

The Alerting UI is accessible through the *{perspective}* perspective of the OpenShift web console.

* From the *Administrator* perspective, go to *Observe* -> *Alerting*. The three main pages in the Alerting UI in this perspective are the *Alerts*, *Silences*, and *Alerting rules* pages.

* From the *Developer* perspective, go to *Observe* and go to the *Alerts* tab.
* Select the project that you want to manage alerts for from the *Project:* list. 

In this perspective, alerts, silences, and alerting rules are all managed from the *Alerts* tab. The results shown in the *Alerts* tab are specific to the selected project.

# [NOTE]
# In the *Developer* perspective, you can select from core OpenShift and user-defined projects that you have access to in the *Project: <project_name>* list. However, alerts, silences, and alerting rules relating to core OpenShift projects are not displayed if you are not logged in as a cluster administrator.

:!perspective:
