---
title:  "Optimize Cost cho K8S"
date:   2024-06-16 12:00:00
tags: [Cloud, k8s, type/write]
---

# Cost Efficiency Arch
## Chọn đúng size clusters
  * Thách thức: over provisioned vs under provisioned, performance vs cost
  * Strategy: metrics và monitoring

## Multi-Cluster hay 1 Giant Cluster
## Managed K8S by Cloud Provider
* Pros: tối tưu tốt hơn
* Cons: upfront cost, complex

# Autoscaling Effective
## HPA VPA
## Cluster Autoscaling
# Sử dụng resouces quota, Limit Ranges
# Spot instance/Preemptible VM
# Optimizing Workload Placement and Scheduling
## Affinity and Anti-affinity Rules
* related microservices scheduled on the same host -> reduce network latency, hidden cost
## Taints and Tolerations for Specialized Node Usage
* GPU workload
* High CPU load or High Memory
## Pros
Enhance performance
## Cons
Detailed understanding
Complex Setup
# Storage Optimization
## Dynamic Volume Provisioning
## Efficient PVC
# Embracing a Cost-Aware Culture
