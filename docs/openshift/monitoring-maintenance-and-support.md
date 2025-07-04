[id="maintenance-and-support_{context}"]
# Maintenance and support for monitoring

Not all configuration options for the monitoring stack are exposed. The only supported way of configuring OpenShift monitoring is by configuring the {cmo-first} using the options described in the "Config map reference for the {cmo-short}". _Do not use other configurations, as they are unsupported._

Configuration paradigms might change across Prometheus releases, and such cases can only be handled gracefully if all configuration possibilities are controlled. If you use configurations other than those described in the "Config map reference for the {cmo-full}", your changes will disappear because the {cmo-short} automatically reconciles any differences and resets any unsupported changes back to the originally defined state by default and by design.

# [IMPORTANT]
# Installing another Prometheus instance is not supported by the Red Hat Site Reliability Engineers (SRE).
