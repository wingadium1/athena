---
title: "RHOSO — Red Hat OpenStack Services on OpenShift"
aliases: ["rhoso deployment", "rhoso 18", "openstack on openshift", "edpm data plane"]
tags: [openstack, rhoso, deployment, architecture, infrastructure]
created: 2026-05-22
updated: 2026-05-22
---

RHOSO (Red Hat OpenStack Services on OpenShift) 18 là deployment model mới cho OpenStack — control plane chạy như OpenShift pods thay vì dedicated controller VMs, trong khi data plane (compute nodes) vẫn là bare-metal RHEL với Libvirt/KVM và OVN. Đây là architectural shift lớn nhất của Red Hat OpenStack Platform kể từ khi bỏ TripleO.

## Kiến trúc

```
Control Plane (OpenShift Pods)             Data Plane (Bare-Metal RHEL)
┌─────────────────────────┐          ┌──────────────────────────┐
│ Keystone, Nova API,     │          │ Nova Compute + OVN agent │
│ Neutron, Glance, Cinder │          │ Libvirt/KVM hypervisor   │
│ (OpenShift pods)        │ ◄──────► │ (EDPM nodes)             │
│                         │ Ansible  │                          │
│ Galera + RabbitMQ       │ Operator │ Bare-metal performance   │
│ (K8s StatefulSets)      │          │ Native L2 networking     │
└─────────────────────────┘          └──────────────────────────┘
```

- **Control plane**: Chạy trên RHOCP 4.18+ qua Kubernetes Operators và Custom Resources (CRDs)
- **Data plane**: Quản lý bởi `OpenStackDataPlane Operator` dùng Ansible để provision bare-metal nodes
- **HA**: Kế thừa từ OpenShift scheduling/replicas + Galera/RabbitMQ Operators + OpenShift routing
- **Network**: OVN-native (không configurable sang OVS như Kolla-Ansible)
- **Upgrade**: Declarative qua OLM (Operator Lifecycle Manager) — CR update thay vì imperative playbooks

## So sánh với Kolla-Ansible

| Feature | Kolla-Ansible | RHOSO 18 |
|---------|--------------|----------|
| Control plane | Docker containers trên bare-metal/VMs | OpenShift pods |
| Orchestration | Ansible + Docker/Podman | Kubernetes Operators |
| Network backend | OVS hoặc OVN | OVN-only |
| Configuration | YAML `globals.yml` | Custom Resources (CRDs) |
| HA | Keepalived + HAProxy | OpenShift self-healing |
| Upgrade | `kolla-ansible upgrade` | OLM + CR updates |
| Kubernetes required | Không | RHOCP 4.18+ |
| Vendor support | Community + tự support | Red Hat subscription |

## NIC Assignment Patterns

RHOSO 18 yêu cầu tách traffic thành 2 nhóm logical:

**Control Group** (bond0 — LACP 802.3ad):
- ctlplane (native/PXE), storage (VLAN 20, MTU 9000), internalapi (VLAN 30)

**Data Group** (OVS bridge `br-ex`):
- tenant (VLAN 40, MTU 9000 — GENEVE encapsulation), provider (VLAN 100)
- Physical NIC enslaved to `br-ex` — **must have NO IP** (nếu không → asymmetric routing)

## VLAN Design cho RHOSO Lab

| Network | VLAN | MTU | Purpose |
|---------|------|-----|---------|
| ctlplane | native | 1500 | PXE boot, OCP cluster provisioning |
| storage | 20 | 9000 | Cinder iSCSI / Ceph OSD replication |
| internalapi | 30 | 1500 | OpenStack service API calls (internal) |
| tenant | 40 | 9000 | Tenant VM overlay (GENEVE encapsulation) |
| provider | 100 | 1500 | External/provider network (direct L2) |

MTU 9000 trên storage và tenant là **mandatory** — GENEVE header overhead (50 bytes) + Ceph/storage throughput yêu cầu jumbo frames end-to-end.

## Production Readiness Checklist

| Severity | Check |
|----------|-------|
| 🔴 Critical | LACP bonding trên tất cả production nodes (single NIC = SPOF) |
| 🔴 Critical | Jumbo frames (MTU 9000) end-to-end trên physical switches |
| 🔴 Critical | OVS bridge slave NIC must have no IP |
| 🔴 Critical | Hard anti-affinity requires ≥3 compute nodes cho 3-node DB clusters |
| 🟡 Important | etcd cluster cho Patroni DCS must run on ≥3 OCP nodes |
| 🟡 Important | Barbican must be deployed HA (not single-node) |
| 🟡 Important | Masakari requires IPMI credentials for host fencing (không chỉ ping) |
| 🔵 Design | Separate storage nodes nếu adopt Ceph (Phase 2) |
| 🔵 Design | OCP cluster sizing: 3 nodes minimum; 5+ nodes for production HA |

## Connections

- [[permanent/kolla-ansible-deployment-patterns]] — Kolla-Ansible alternative
- [[permanent/openstack-neutron-overlay-protocols]] — GENEVE in OVN context
- [[permanent/ovs-bridge-management-nic-pitfall]] — NIC pattern B constraint
- [[permanent/patroni]] — etcd HA dependency

## Sources

- [[literature/openstack-dbaas-work-wiki]]
