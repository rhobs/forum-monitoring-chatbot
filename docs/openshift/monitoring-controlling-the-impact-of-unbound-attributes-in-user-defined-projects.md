:_mod-docs-content-type: CONCEPT
[id="controlling-the-impact-of-unbound-attributes-in-user-defined-projects_{context}"]
# Controlling the impact of unbound metrics attributes in user-defined projects

Developers can create labels to define attributes for metrics in the form of key-value pairs. The number of potential key-value pairs corresponds to the number of possible values for an attribute. An attribute that has an unlimited number of potential values is called an unbound attribute. For example, a `customer_id` attribute is unbound because it has an infinite number of possible values.

Every assigned key-value pair has a unique time series. The use of many unbound attributes in labels can result in an exponential increase in the number of time series created. This can impact Prometheus performance and can consume a lot of disk space.

Cluster administrators

A `dedicated-admin`

can use the following measures to control the impact of unbound metrics attributes in user-defined projects:

* Limit the number of samples that can be accepted per target scrape in user-defined projects
* Limit the number of scraped labels, the length of label names, and the length of label values
* Configure the intervals between consecutive scrapes and between Prometheus rule evaluations
* Create alerts that fire when a scrape sample threshold is reached or when the target cannot be scraped

# [NOTE]
# Limiting scrape samples can help prevent the issues caused by adding many unbound attributes to labels. Developers can also prevent the underlying cause by limiting the number of unbound attributes that they define for metrics. Using attributes that are bound to a limited set of possible values reduces the number of potential key-value pair combinations.
