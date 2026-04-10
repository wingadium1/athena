---
title: "Cassandra High Availability on Kubernetes with Spot Instances"
aliases: ["cassandra-k8s-ha", "cassandra-eks-spot"]
tags: [Kubernetes, Cassandra, AWS, HighAvailability, Spot, Infrastructure]
created: 2026-04-10
updated: 2026-04-10
---

Cassandra's own HA model — no single point of failure, peer-to-peer replication — maps cleanly onto Kubernetes and AWS primitives, making it viable to run even on Spot instances without sacrificing availability.

**The key mapping**: each Cassandra _Data Center_ becomes one AWS Availability Zone. You deploy one `StatefulSet` per AZ, pinned via `nodeAffinity` using the `topology.kubernetes.io/zone` label. Each StatefulSet gets its own `Service` to expose its nodes. When one AZ goes down, the remaining DCs absorb traffic seamlessly.

**Seed nodes** bootstrap cluster membership. The first pod of each StatefulSet (`cassandra{a,b,c}-0`) serves as that DC's seed. All three cross-DC seeds are listed in `CASSANDRA_SEEDS`. Seeds don't all need to be up simultaneously — the cluster tolerates partial seed availability, which is what makes Spot reclamation survivable.

**Replication strategy**: use `NetworkTopologyStrategy`, not `SimpleStrategy`, when deploying across DCs. It lets you set a replication factor per DC, so data is copied across AZs asynchronously, and any single DC loss doesn't mean data loss.

**Application side**: each app replica should set `CASSANDRA_LOCAL_DATACENTER` to its own AZ. If you point an app node at nodes in a different DC, the DataStax driver will log a warning — route locally to avoid cross-AZ latency and driver noise.

**Why Spot works here**: Cassandra's HA design already assumes node failure is normal. A Spot reclamation is just a node restart from the cluster's perspective. The StatefulSet will reschedule the pod in the same AZ once capacity is available, and other DCs continue serving traffic. This makes Spot a genuinely cost-effective choice — not a risk — for Cassandra on K8s.

## Connections

- [[permanent/kubernetes-node-pool-design]] — Taint/toleration + affinity patterns used to pin Cassandra pods per AZ
- [[permanent/kubernetes-node-consolidation]] — Trade-offs of Spot + stateful workloads

## Sources

- [[journal/cassandra_on_aws_eks_spot]] — Original blog post with YAML examples (2021)
