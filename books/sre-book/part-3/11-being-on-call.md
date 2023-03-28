# Chapter 11: Being On-Call`
## Overview
Being on-call is a critical component of keeping services available and running reliably, but comes with a cost, especially if on-call rotations are not appropriately designed. Being "on-call" for a service means that should an issue arise that requires manual attention, and often intervention, it is the responsibility of the person carrying the "pager" to engage and diagnose the issue.

## A Day in the Life
The availability of an on-call engineer is determined by the team and systems they support, but it is common to expect an on-call engineer to engage with a situation within 5 minutes if it is customer facing, and 30 minutes if it is not. Depending on the SLO, response times may need to be near real time, which is often indicative of a need for more aggressive alerting threshold. Non-paging production events can and should be triaged by the on-call engineering during working hours. It is also a common practice for teams to have a primary and secondary on-call rotation, though different teams define the responsibilities of these roles differently. In some cases the secondary acts exclusively as a catchall for pages missed by the primary, while in others the secondary deal with all non-paging issues, freeing the primary up to focus on paging incidents.

Google preaches a 50-25-25 percent rules where at least 50% of an SREs time is spent on engineering tasks, at most 25% is spent on-call, and the final 25% can be spent on operational, non-production work. This may not be possible for all organizations, but it is a reasonable rule of thumb. Using this rule of thumb, the minimum number of engineers required to sustain a 24/7 on-call rotation with week long rotations is **eight**.

On-call rotations can also be split based on daytime boundaries if a team is fortunate enough to staff a multi-site, also known as "follow the sun" on-call rotation. In a "follow the sun" on-call rotation, each shift is split in such a way that engineers are on-call based on their daytime hours. This does not always line up perfectly, but more often than not it alleviates the need to remain on-call through the night. In these cases the ideal size for teams is **six**.

Being on-call can be a very stressful experience. It is important to provide on-call engineers with support and resources to alleviate some of this stress. The most immediately ready resources are:
- Clear escalation paths
- Well-defined incident-management procedures
- A blameless postmortem culture

## Operational Overload
When a team or system becomes overly burdened with operational tasks it is important to identify the issue and takes steps toward rectifying it as soon as possible.

One of the most common causes of operational overload is eager alerts that are configured to fire to aggressively, causing an overwhelming amount of noise. Luckily, this situation is easy to rectify by aligning paging alerts with issues that threaten SLOs and are actionable. Otherwise, these pages will eventually being to contribute to pager pain and alert fatigue.

Similarly, a single incident can cause a multitude of alerts to be routed to the on-call engineer, resulting in being overwhelmed with a deluge of alerts that all indicate the same issue. Pruning alerts such that they approach more of a 1:1 ratio with 