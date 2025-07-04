:_mod-docs-content-type: CONCEPT
[id="configuring-different-alert-receivers-for-default-platform-alerts-and-user-defined-alerts_{context}"]
# Configuring different alert receivers for default platform alerts and user-defined alerts

You can configure different alert receivers for default platform alerts and user-defined alerts to ensure the following results:

* All default platform alerts are sent to a receiver owned by the team in charge of these alerts.
* All user-defined alerts are sent to another receiver so that the team can focus only on platform alerts.

You can achieve this by using the `openshift_io_alert_source="platform"` label that is added by the {cmo-full} to all platform alerts:

* Use the `openshift_io_alert_source="platform"` matcher to match default platform alerts.
* Use the `openshift_io_alert_source!="platform"` or `'openshift_io_alert_source=""'` matcher to match user-defined alerts.

# [NOTE]
# This configuration does not apply if you have enabled a separate instance of Alertmanager dedicated to user-defined alerts.
