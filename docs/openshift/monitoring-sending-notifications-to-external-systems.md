:_mod-docs-content-type: CONCEPT
[id="sending-notifications-to-external-systems_{context}"]
# Sending notifications to external systems

In OpenShift , firing alerts can be viewed in the Alerting UI. Alerts are not configured by default to be sent to any notification systems. You can configure OpenShift to send alerts to the following receiver types:

* PagerDuty
* Webhook
* Email
* Slack
* Microsoft Teams

Routing alerts to receivers enables you to send timely notifications to the appropriate teams when failures occur. For example, critical alerts require immediate attention and are typically paged to an individual or a critical response team. Alerts that provide non-critical warning notifications might instead be routed to a ticketing system for non-immediate review.

.Checking that alerting is operational by using the watchdog alert

OpenShift monitoring includes a watchdog alert that fires continuously. Alertmanager repeatedly sends watchdog alert notifications to configured notification providers. The provider is usually configured to notify an administrator when it stops receiving the watchdog alert. This mechanism helps you quickly identify any communication issues between Alertmanager and the notification provider.
