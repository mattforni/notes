# Chapter 10: Practical Alerting
## Overview
Monitoring is the foundation upon which the reliable operation of services and systems is built. Monitoring provides the insight necessary to understand how a system is operating and whether anything is out of alignment with business goals. Of course, monitoring systems operating at scale can be overwhelming, so a good rule of thumb is to instrument observability in such a way that metrics for individual machines can be inspected, but where signals are derived and alerted on in the aggregate.

## Alerting
Alerting should occur on aggregates, which should be queryable from a centralized data source. There are many monitoring tools that adhere to this style of storage and querying. This standardizes the ability for engineers and operators to publish and consume metrics without having to reinvent the wheel.

For example, if the desire is to alert when the error rate exceeds a certain ratio, this would be represented as the sum of all non-200 requests divided by the sum of all requests.