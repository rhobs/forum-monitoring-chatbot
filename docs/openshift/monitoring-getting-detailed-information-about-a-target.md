:_mod-docs-content-type: PROCEDURE
[id="getting-detailed-information-about-a-target_{context}"]
# Getting detailed information about a metrics target

You can use the OpenShift web console to view, search, and filter the endpoints that are currently targeted for scraping, which helps you to identify and troubleshoot problems. For example, you can view the current status of targeted endpoints to see when OpenShift monitoring is not able to scrape metrics from a targeted component.


The *Metrics targets* page shows targets for default OpenShift projects and for user-defined projects.


The *Metrics targets* page shows targets for user-defined projects.


.Prerequisites


* You have access to the cluster as an administrator for the project for which you want to view metrics targets.


* You have access to the cluster as a user with the `dedicated-admin` role.


.Procedure

. In the *Administrator* perspective of the OpenShift web console, go to *Observe* -> *Targets*. The *Metrics targets* page opens with a list of all service endpoint targets that are being scraped for metrics.
+
This page shows details about targets for default OpenShift and user-defined projects. This page lists the following information for each target:

** Service endpoint URL being scraped
** The `ServiceMonitor` resource being monitored
** The **up** or **down** status of the target
** Namespace
** Last scrape time
** Duration of the last scrape

. Optional: To find a specific target, perform any of the following actions:
+
|===
|Option |Description

|Filter the targets by status and source.
a|Choose filters in the *Filter* list.

The following filtering options are available:

* **Status** filters:
** **Up**. The target is currently up and being actively scraped for metrics.
** **Down**. The target is currently down and not being scraped for metrics.

* **Source** filters:
** **Platform**. Platform-level targets relate only to default {product-rosa} projects. These projects provide core {product-rosa} functionality.
** **User**. User targets relate to user-defined projects. These projects are user-created and can be customized.

|Search for a target by name or label. |Enter a search term in the **Text** or **Label** field next to the search box.

|Sort the targets. |Click one or more of the **Endpoint Status**, **Namespace**, **Last Scrape**, and **Scrape Duration** column headers.
|===

. Click the URL in the **Endpoint** column for a target to go to its **Target details** page. This page provides information about the target, including the following information:

** The endpoint URL being scraped for metrics
** The current *Up* or *Down* status of the target
** A link to the namespace
** A link to the `ServiceMonitor` resource details
** Labels attached to the target
** The most recent time that the target was scraped for metrics

