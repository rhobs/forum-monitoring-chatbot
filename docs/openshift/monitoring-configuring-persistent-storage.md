:_mod-docs-content-type: CONCEPT
[id="configuring-persistent-storage_{context}"]
# Configuring persistent storage

Run cluster monitoring with persistent storage to gain the following benefits:

* Protect your metrics and alerting data from data loss by storing them in a persistent volume (PV). As a result, they can survive pods being restarted or recreated.
* Avoid getting duplicate notifications and losing silences for alerts when the Alertmanager pods are restarted.

For production environments, it is highly recommended to configure persistent storage. 

# [IMPORTANT]
# In multi-node clusters, you must configure persistent storage for Prometheus, Alertmanager, and Thanos Ruler to ensure high availability.

[id="persistent-storage-prerequisites_{context}"]
## Persistent storage prerequisites

* Use the block type of storage.

* Dedicate sufficient persistent storage to ensure that the disk does not become full.

* Use `Filesystem` as the storage type value for the `volumeMode` parameter when you configure the persistent volume.
+
# [IMPORTANT]
* Do not use a raw block volume, which is described with `volumeMode: Block` in the `PersistentVolume` resource. Prometheus cannot use raw block volumes.

* Prometheus does not support file systems that are not POSIX compliant.
For example, some NFS file system implementations are not POSIX compliant.
# If you want to use an NFS file system for storage, verify with the vendor that their NFS implementation is fully POSIX compliant.
