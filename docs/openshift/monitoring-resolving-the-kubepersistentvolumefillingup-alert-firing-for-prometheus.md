:_mod-docs-content-type: PROCEDURE
[id="resolving-the-kubepersistentvolumefillingup-alert-firing-for-prometheus_{context}"]
# Resolving the KubePersistentVolumeFillingUp alert firing for Prometheus

As a cluster administrator, you can resolve the `KubePersistentVolumeFillingUp` alert being triggered for Prometheus. 

The critical alert fires when a persistent volume (PV) claimed by a `prometheus-k8s-*` pod in the `openshift-monitoring` project has less than 3% total space remaining. This can cause Prometheus to function abnormally.

# [NOTE]
There are two `KubePersistentVolumeFillingUp` alerts:

* *Critical alert*:  The alert with the `severity="critical"` label is triggered when the mounted PV has less than 3% total space remaining.
# * *Warning alert*: The alert with the `severity="warning"` label is triggered when the mounted PV has less than 15% total space remaining and is expected to fill up within four days.

To address this issue, you can remove Prometheus time-series database (TSDB) blocks to create more space for the PV.

.Prerequisites

* You have access to the cluster as a user with the `cluster-admin` cluster role.

* You have access to the cluster as a user with the `dedicated-admin` role.

* You have installed the OpenShift CLI (`oc`).

.Procedure

. List the size of all TSDB blocks, sorted from oldest to newest, by running the following command:
+

```terminal
$ oc debug <prometheus_k8s_pod_name> -n openshift-monitoring \// <1>
-c prometheus --image=$(oc get po -n openshift-monitoring <prometheus_k8s_pod_name> \// <1>
-o jsonpath='{.spec.containers[?(@.name=="prometheus")].image}') \
-- sh -c 'cd /prometheus/;du -hs $(ls -dtr */ | grep -Eo "[0-9|A-Z]{26}")'

```
<1> Replace `<prometheus_k8s_pod_name>` with the pod mentioned in the `KubePersistentVolumeFillingUp` alert description.
+
.Example output

```terminal
308M    01HVKMPKQWZYWS8WVDAYQHNMW6
52M     01HVK64DTDA81799TBR9QDECEZ
102M    01HVK64DS7TRZRWF2756KHST5X
140M    01HVJS59K11FBVAPVY57K88Z11
90M     01HVH2A5Z58SKT810EM6B9AT50
152M    01HV8ZDVQMX41MKCN84S32RRZ1
354M    01HV6Q2N26BK63G4RYTST71FBF
156M    01HV664H9J9Z1FTZD73RD1563E
216M    01HTHXB60A7F239HN7S2TENPNS
104M    01HTHMGRXGS0WXA3WATRXHR36B

```

. Identify which and how many blocks could be removed, then remove the blocks. The following example command removes the three oldest Prometheus TSDB blocks from the `prometheus-k8s-0` pod:
+

```terminal
$ oc debug prometheus-k8s-0 -n openshift-monitoring \
-c prometheus --image=$(oc get po -n openshift-monitoring prometheus-k8s-0 \
-o jsonpath='{.spec.containers[?(@.name=="prometheus")].image}') \
-- sh -c 'ls -latr /prometheus/ | egrep -o "[0-9|A-Z]{26}" | head -3 | \
while read BLOCK; do rm -r /prometheus/$BLOCK; done'

```

. Verify the usage of the mounted PV and ensure there is enough space available by running the following command:
+

```terminal
$ oc debug <prometheus_k8s_pod_name> -n openshift-monitoring \// <1>
--image=$(oc get po -n openshift-monitoring <prometheus_k8s_pod_name> \// <1>
-o jsonpath='{.spec.containers[?(@.name=="prometheus")].image}') -- df -h /prometheus/

```
<1> Replace `<prometheus_k8s_pod_name>` with the pod mentioned in the `KubePersistentVolumeFillingUp` alert description.
+
The following example output shows the mounted PV claimed by the `prometheus-k8s-0` pod that has 63% of space remaining:
+
.Example output

```terminal
Starting pod/prometheus-k8s-0-debug-j82w4 ...
Filesystem      Size  Used Avail Use% Mounted on
/dev/nvme0n1p4  40G   15G  40G  37% /prometheus

Removing debug pod ...

```
