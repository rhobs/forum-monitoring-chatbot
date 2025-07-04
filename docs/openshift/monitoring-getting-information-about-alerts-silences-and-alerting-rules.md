:_mod-docs-content-type: PROCEDURE

[id="getting-information-about-alerts-silences-and-alerting-rules-adm_{context}"]
# Getting information about alerts, silences, and alerting rules from the Administrator perspective

[id="getting-information-about-alerts-silences-and-alerting-rules-dev_{context}"]
# Getting information about alerts, silences, and alerting rules from the Developer perspective

The Alerting UI provides detailed information about alerts and their governing alerting rules and silences.

.Prerequisites

* You have access to the cluster as a user with view permissions for the project that you are viewing alerts for.

.Procedure

To obtain information about alerts:

. From the *Administrator* perspective of the OpenShift web console, go to the *Observe* -> *Alerting* -> *Alerts* page.

. Optional: Search for alerts by name by using the *Name* field in the search list.

. Optional: Filter alerts by state, severity, and source by selecting filters in the *Filter* list.

. Optional: Sort the alerts by clicking one or more of the *Name*, *Severity*, *State*, and *Source* column headers.

. Click the name of an alert to view its *Alert details* page. The page includes a graph that illustrates alert time series data. It also provides the following information about the alert:

* A description of the alert
* Messages associated with the alert
* A link to the runbook page on GitHub for the alert, if the page exists
* Labels attached to the alert
* A link to its governing alerting rule
* Silences for the alert, if any exist

To obtain information about silences:

. From the *Administrator* perspective of the OpenShift web console, go to the *Observe* -> *Alerting* -> *Silences* page.

. Optional: Filter the silences by name using the *Search by name* field.

. Optional: Filter silences by state by selecting filters in the *Filter* list. By default, *Active* and *Pending* filters are applied.

. Optional: Sort the silences by clicking one or more of the *Name*, *Firing alerts*, *State*, and *Creator* column headers.

. Select the name of a silence to view its *Silence details* page. The page includes the following details:

* Alert specification
* Start time
* End time
* Silence state
* Number and list of firing alerts

To obtain information about alerting rules:

. From the *Administrator* perspective of the OpenShift web console, go to the *Observe* -> *Alerting* -> *Alerting rules* page.

. Optional: Filter alerting rules by state, severity, and source by selecting filters in the *Filter* list.

. Optional: Sort the alerting rules by clicking one or more of the *Name*, *Severity*, *Alert state*, and *Source* column headers.

. Select the name of an alerting rule to view its *Alerting rule details* page. The page provides the following details about the alerting rule:

* Alerting rule name, severity, and description.
* The expression that defines the condition for firing the alert.
* The time for which the condition should be true for an alert to fire.
* A graph for each alert governed by the alerting rule, showing the value with which the alert is firing.
* A table of all alerts governed by the alerting rule.

To obtain information about alerts, silences, and alerting rules:

. From the *Developer* perspective of the OpenShift web console, go to the *Observe* -> *<project_name>* -> *Alerts* page.

. View details for an alert, silence, or an alerting rule:

* *Alert details* can be viewed by clicking a greater than symbol (*>*) next to an alert name and then selecting the alert from the list.

* *Silence details* can be viewed by clicking a silence in the *Silenced by* section of the *Alert details* page. The *Silence details* page includes the following information:

__ Alert specification
__ Start time
__ End time
__ Silence state
** Number and list of firing alerts

* *Alerting rule details* can be viewed by clicking the {kebab} menu next to an alert in the *Alerts* page and then clicking *View Alerting Rule*.

# [NOTE]
# Only alerts, silences, and alerting rules relating to the selected project are displayed in the *Developer* perspective.
