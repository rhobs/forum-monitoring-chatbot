:_mod-docs-content-type: CONCEPT
[id="support-policy-for-monitoring-operators_{context}"]
# Support policy for monitoring Operators

Monitoring Operators ensure that OpenShift monitoring resources function as designed and tested. If Cluster Version Operator (CVO) control of an Operator is overridden, the Operator does not respond to configuration changes, reconcile the intended state of cluster objects, or receive updates.

While overriding CVO control for an Operator can be helpful during debugging, this is  unsupported and the cluster administrator assumes full control of the individual component configurations and upgrades.

.Overriding the Cluster Version Operator

The `spec.overrides` parameter can be added to the configuration for the CVO to allow administrators to provide a list of overrides to the behavior of the CVO for a component. Setting the `spec.overrides[].unmanaged` parameter to `true` for a component blocks cluster upgrades and alerts the administrator after a CVO override has been set:

```terminal
Disabling ownership via cluster version overrides prevents upgrades. Please remove overrides before continuing.

```

# [WARNING]
# Setting a CVO override puts the entire cluster in an unsupported state and prevents the monitoring stack from being reconciled to its intended state. This impacts the reliability features built into Operators and prevents updates from being received. Reported issues must be reproduced after removing any overrides for support to proceed.
