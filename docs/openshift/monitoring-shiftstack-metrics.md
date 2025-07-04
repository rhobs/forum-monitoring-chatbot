:_mod-docs-content-type: CONCEPT
[id="monitoring-shiftstack-metrics.adoc_{context}"]
# Available metrics for clusters that run on RHOSO

To query metrics and identifying resources across the stack, there are helper metrics that establish a correlation between {rhoso-first} infrastructure resources and their representations in the tenant OpenShift cluster.

To map nodes with {rhoso} compute instances, in the metric `kube_node_info`:

* `node` is the Kubernetes node name.

* `provider_id` contains the identifier of the corresponding compute service instance.

To map persistent volumes with {rhoso} block storage or shared filesystems shares, in the metric `kube_persistentvolume_info`:

* `persistentvolume` is the volume name.

* `csi_volume_handle` is the block storage volume or share identifier.

By default, the compute machines that back the cluster control plane nodes are created in a server group with a soft anti-affinity policy. As a result, the compute service creates them on separate hypervisors on a best-effort basis. However, if the state of the {rhoso} cluster is not appropriate for this distribution, the machines are created anyway.

In combination with the default soft anti-affinity policy, you can configure an alert that activates when a hypervisor hosts more than one control plane node of a given cluster to highlight the degraded level of high availability.

As an example, this PromQL query returns the number of OpenShift master nodes per {rh-openstack} host:

```promql
sum by (vm_instance) (
  group by (vm_instance, resource) (ceilometer_cpu)
    / on (resource) group_right(vm_instance) (
      group by (node, resource) (
        label_replace(kube_node_info, "resource", "$1", "system_uuid", "(.+)")
      )
    / on (node) group_left group by (node) (
      cluster:master_nodes
    )
  )
## )
