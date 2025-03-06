---
title:  "Overview Approach"
tags: [Software Architecture, developer, multi-cloud, hybrid-cloud, type/write]
---

## Current Situation Analysis
* Private Cloud
* Large K8s clusters (~1000 cores, ~1000 GB in Memory, TB-scale storage)
* Challenges:
    * Limited scaling capabilities
    * H/A limitations
    * Network stability issues
    * Difficult system upgrades
    * Higher storage costs (60% more than AWS)
    * But good compute costs (1/3 of AWS)

## Architecture Plan

### Distribution Strategy

Firstly, we start with distribution strategy, with simpliest approach: keep primary infrastructure on premise (private cloud) and AWS as a secondary choice

Purpose for private cloud
* Keep compute-intensive workloads
* Keep data that must remain in region due to compliance
* Maintain current advantages of lower compute costs

Purpose for Secondary Cloud (AWS)
* DR (Disaster Recovery) site
* Storage-heavy workloads
* Stateless applications that need auto-scaling
* Database replicas for HA

### Component

With common approach of infra structure on new adopted private cloud, we assume that: they do not have many managed service, just based on several common component: 
1. network
2. k8s cluster
3. some docker cluster to install database storage (sql, redis)
4. block storage

#### Network

Refer to [[writing/multi-cloud-arch/multi_cloud_network_setup|Network]]

#### Kubenetes
Refer to [[writing/multi-cloud-arch/multi_cloud_k8s_setup|Kubenetes Setup]]


#### Database

database on multi-cloud/multi-datacenter 