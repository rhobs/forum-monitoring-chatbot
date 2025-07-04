[id="running-metrics-queries_{context}"]
# Running metrics queries

You begin working with metrics by entering one or several Prometheus Query Language (PromQL) queries.

.Procedure

. Open the OpenShift web console and navigate to the *Observe* -> *Metrics* page.

. In the query field, enter your PromQL query.
* To show all available metrics and PromQL functions, click *Insert Metric at Cursor*.
. For multiple queries, click *Add Query*.
. For deleting queries, click {kebab} for the query, then select *Delete query*.
. For keeping but not running a query, click *Disable query*.
. After you finish creating queries, click *Run Queries*. The metrics from the queries are visualized on the plot. If a query is invalid, the UI shows an error message.
+
# [NOTE]
# Queries that operate on large amounts of data might timeout or overload the browser when drawing timeseries graphs. To avoid this, hide the graph and calibrate your query using only the metrics table. Then, after finding a feasible query, enable the plot to draw the graphs.
+
. Optional: The page URL now contains the queries you ran. To use this set of queries again in the future, save this URL.

[role="_additional-resources"]
.Additional resources

* See the link:https://prometheus.io/docs/prometheus/latest/querying/basics/[Prometheus Query Language documentation].
