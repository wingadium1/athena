---
title: "OpenStack DBaaS — Work Wiki Knowledge Base"
type: literature
source: "Work wiki — OpenStack/DBaaS project"
author: "Work wiki — OpenStack project"
date-read: 2026-05-22
tags: [openstack, dbaas, infrastructure, architecture, operations]
---

## Summary

Comprehensive OpenStack/DBaaS knowledge base from the BA work wiki — covering production OpenStack architecture, deployment comparison (Kolla-Ansible vs RHOSO), PostgreSQL HA (Patroni + Pgpool-II hybrid), network topology decisions (Provider VLAN + GENEVE overlay), storage architecture, Day-2 operations, and lab validation patterns. Filtered to extract only general technical knowledge, excluding customer-specific decisions, estimation, deliverables, and validation evidence.

## Key Ideas

- **RHOSO 18 deploys OpenStack control plane as OpenShift pods** using Operators and Custom Resources, while the data plane remains bare-metal RHEL with Libvirt and OVN — a fundamentally different model from Kolla-Ansible's imperative Ansible containers
- **Patroni + Pgpool-II is a complementary hybrid** — Patroni handles leader election and automatic failover (10–30s via etcd Raft consensus), Pgpool-II handles connection pooling and read/write routing per-tenant. They don't overlap
- **GENEVE is the default OVN tunnel protocol** because VXLAN's 12-bit datapath limitation in OVN restricts to 4,096 logical networks, while GENEVE preserves full 24-bit VNI (16.7M networks) plus TLV-based extensible metadata
- **Floating IP was rejected for DBaaS at scale** due to public IP exhaustion, NAT latency (200–500 µs/packet), and network node CPU bottleneck at ~2,000 concurrent flows — Provider VLAN + GENEVE overlay hybrid chosen instead
- **Storage decision framework**: RAID 10 + Nova ephemeral as primary (immediate), Ceph RBD as future target (dedicated cluster), SAN as transitional path — RAID 5 rejected for bare-metal DB workloads
- **Day-2 operations philosophy**: monitoring severity tiers, dependency-gated runbooks, backup with retention verification, DR with RTO/RPO planning, and rolling upgrade with safety gates

## My Take

Đây là lần đầu tiên mình ingest tri thức từ wiki công việc qua — và thấy pattern rất rõ: những kiến thức kỹ thuật thuần túy (OpenStack architecture, HA patterns, network design) hoàn toàn tách rời khỏi context dự án. File nào có `## Key Knowledge Captured` là dễ extract nhất. Những insight đáng giá nhất thường nằm ở các quyết định kiến trúc (tại sao chọn cái này, bỏ cái kia) chứ không phải ở runbook chi tiết.

## Links

- [[permanent/postgresql-ha-patroni-pgpool-combo]]
- [[permanent/patroni]]
- [[permanent/pgpool-ii]]
- [[permanent/kolla-ansible-deployment-patterns]]
- [[permanent/rhoso-deployment-model]]
- [[permanent/openstack-neutron-overlay-protocols]]
- [[permanent/openstack-floating-ip-nat]]
- [[permanent/ovs-bridge-management-nic-pitfall]]
- [[permanent/packer-openstack-image-pipeline]]
- [[permanent/openstack-trove-postgresql-ha]]
