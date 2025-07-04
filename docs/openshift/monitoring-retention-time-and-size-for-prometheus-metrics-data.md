:_mod-docs-content-type: CONCEPT
[id="retention-time-and-size-for-prometheus-metrics-data_{context}"]
# Retention time and size for Prometheus metrics

By default, Prometheus retains metrics data for the following durations:

* *Core platform monitoring*: 15 days
* *Monitoring for user-defined projects*: 24 hours

You can modify the retention time for the Prometheus instance to change how soon the data is deleted. You can also set the maximum amount of disk space the retained metrics data uses. If the data reaches this size limit, Prometheus deletes the oldest data first until the disk space used is again below the limit.

Note the following behaviors of these data retention settings:

* The size-based retention policy applies to all data block directories in the `/prometheus` directory, including persistent blocks, write-ahead log (WAL) data, and m-mapped chunks.
* Data in the `/wal` and `/head_chunks` directories counts toward the retention size limit, but Prometheus never purges data from these directories based on size- or time-based retention policies.
Thus, if you set a retention size limit lower than the maximum size set for the `/wal` and `/head_chunks` directories, you have configured the system not to retain any data blocks in the `/prometheus` data directories.
* The size-based retention policy is applied only when Prometheus cuts a new data block, which occurs every two hours after the WAL contains at least three hours of data.
* If you do not explicitly define values for either `retention` or `retentionSize`, retention time defaults to 15 days for core platform monitoring and 24 hours for user-defined project monitoring. Retention size is not set.
* If you define values for both `retention` and `retentionSize`, both values apply.
If any data blocks exceed the defined retention time or the defined size limit, Prometheus purges these data blocks.
* If you define a value for `retentionSize` and do not define `retention`, only the `retentionSize` value applies.
* If you do not define a value for `retentionSize` and only define a value for `retention`, only the `retention` value applies.
* If you set the `retentionSize` or `retention` value to `0`, the default settings apply. The default settings set retention time to 15 days for core platform monitoring and 24 hours for user-defined project monitoring. By default, retention size is not set.

# [NOTE]
# Data compaction occurs every two hours. Therefore, a persistent volume (PV) might fill up before compaction, potentially exceeding the `retentionSize` limit. In such cases, the `KubePersistentVolumeFillingUp` alert fires until the space on a PV is lower than the `retentionSize` limit.
