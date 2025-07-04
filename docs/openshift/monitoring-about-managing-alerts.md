:_mod-docs-content-type: CONCEPT
[id="about-managing-alerts_{context}"]
# Managing alerts

In the OpenShift, the Alerting UI enables you to manage alerts, silences, and alerting rules.

* *Alerting rules*. Alerting rules contain a set of conditions that outline a particular state within a cluster. Alerts are triggered when those conditions are true. An alerting rule can be assigned a severity that defines how the alerts are routed.
* *Alerts*. An alert is fired when the conditions defined in an alerting rule are true. Alerts provide a notification that a set of circumstances are apparent within an OpenShift cluster.
* *Silences*. A silence can be applied to an alert to prevent notifications from being sent when the conditions for an alert are true. You can mute an alert after the initial notification, while you work on resolving the issue.

# [NOTE]
# The alerts, silences, and alerting rules that are available in the Alerting UI relate to the projects that you have access to. For example, if you are logged in as a user with the `cluster-admin` role, you can access all alerts, silences, and alerting rules.
