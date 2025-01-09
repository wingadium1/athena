---
title: "The DORA KPI metrics"
tags: [ DevOps ]
---

> [!info]
> The DORA metrics measure the performance of a DevOps organisation by providing essential insights and informing DevOps key performance indicators (KPIs). Here are four DORA metrics:
> * Deployment Frequency (org's velocity)
> * Lead Time for Changes (org's velocity)
> * Mean Time to Restore (org's stability)
> * Change Failure Rate (org's stability)


### Lead time
length of time from a commit to  the version control repository to the deployment of that code into a production environment.

### Deployment frequency
How frequently organizations can successfully deploy code to production

### Change failure rate
$$
change failure rate = \frac{failures}{deployments}
$$

### Recovery time
instances where service incidents occurred in production environments.

the metric for recovery time is expressed as the Mean  Time to Recovery ([[refs/mean_time_to_recovery|MTTR]])


## DORA metric performance levels
* Elite
* High
* Medium
* Low