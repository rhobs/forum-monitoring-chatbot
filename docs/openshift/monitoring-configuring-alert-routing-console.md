:_mod-docs-content-type: PROCEDURE
[id="configuring-alert-routing-console_{context}"]
# Configuring alert routing with the OpenShift web console

You can configure alert routing through the OpenShift web console to ensure that you learn about important issues with your cluster. 

# [NOTE]
# The OpenShift web console provides fewer settings to configure alert routing than the `alertmanager-main` secret. To configure alert routing with the access to more configuration settings, see "Configuring alert routing for default platform alerts".

.Prerequisites

* You have access to the cluster as a user with the `cluster-admin` cluster role.

.Procedure

. In the *Administrator* perspective, go to *Administration* -> *Cluster Settings* -> *Configuration* -> *Alertmanager*.
+
# [NOTE]
# Alternatively, you can go to the same page through the notification drawer. Select the bell icon at the top right of the OpenShift web console and choose *Configure* in the *AlertmanagerReceiverNotConfigured* alert.

. Click *Create Receiver* in the *Receivers* section of the page.

. In the *Create Receiver* form, add a *Receiver name* and choose a *Receiver type* from the list.

. Edit the receiver configuration:
+
* For PagerDuty receivers:
+
.. Choose an integration type and add a PagerDuty integration key.
+
.. Add the URL of your PagerDuty installation.
+
.. Click *Show advanced configuration* if you want to edit the client and incident details or the severity specification.
+
* For webhook receivers:
+
.. Add the endpoint to send HTTP POST requests to.
+
.. Click *Show advanced configuration* if you want to edit the default option to send resolved alerts to the receiver.
+
* For email receivers:
+
.. Add the email address to send notifications to.
+
.. Add SMTP configuration details, including the address to send notifications from, the smarthost and port number used for sending emails, the hostname of the SMTP server, and authentication details.
+
# [IMPORTANT]
# Alertmanager requires an external SMTP server to send email alerts. To configure email alert receivers, ensure you have the necessary connection details for an external SMTP server.
+
.. Select whether TLS is required.
+
.. Click *Show advanced configuration* if you want to edit the default option not to send resolved alerts to the receiver or edit the body of email notifications configuration.
+
* For Slack receivers:
+
.. Add the URL of the Slack webhook.
+
.. Add the Slack channel or user name to send notifications to.
+
.. Select *Show advanced configuration* if you want to edit the default option not to send resolved alerts to the receiver or edit the icon and username configuration. You can also choose whether to find and link channel names and usernames.

. By default, firing alerts with labels that match all of the selectors are sent to the receiver. If you want label values for firing alerts to be matched exactly before they are sent to the receiver, perform the following steps:
.. Add routing label names and values in the *Routing labels* section of the form.

.. Click *Add label* to add further routing labels.

. Click *Create* to create the receiver.
