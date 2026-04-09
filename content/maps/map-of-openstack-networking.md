---
title: "Map of Content: OpenStack Networking"
tags: [infrastructure, openstack, networking, map]
created: 2026-04-09
updated: 2026-04-09
---

# OpenStack Networking

Overview map for OpenStack Neutron architecture notes. Focus: external-to-tenant network connectivity in multi-tenant deployments.

## Core Concepts

| Note                                                                                    | Summary                                                                       |
| --------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| [[permanent/openstack-neutron-overlay-protocols\|Overlay Protocols (VXLAN/GRE/GENEVE)]] | How overlay tunnels solve the 4094 VLAN limit; VNI allocation                 |
| [[permanent/openstack-external-network-mapping\|External Network Mapping]]              | How provider networks bridge to tenant overlays via OVS + router namespace    |
| [[permanent/openstack-dvr-architecture\|DVR — Distributed Virtual Router]]              | Distributing L3 routing to compute nodes; east-west + floating IP north-south |
| [[permanent/openstack-floating-ip-nat\|Floating IP NAT Mechanism]]                      | DNAT/SNAT iptables rules; centralized vs. DVR models                          |
| [[permanent/openstack-bgp-evpn-external\|BGP/EVPN External Connectivity]]               | Dynamic routing at scale: neutron-dynamic-routing, BGPVPN, ovn-bgp-agent      |

## Traffic Flow Cheat Sheet

```
External Host → Floating IP (DNAT) → Tenant VM
  ↓
Physical NIC → br-provider → br-int → qrouter/fip namespace (DNAT)
  → VXLAN encap (br-tun) → compute node → VM

Tenant VM → Internet (SNAT) → External
  ↓ (centralized SNAT)
VXLAN → network node → snat namespace → br-provider → Physical NIC

  ↓ (DVR SNAT with floating IP)
br-int → qrouter namespace (SNAT to FIP) → fip namespace → br-provider → Physical NIC
```

## Overlay Protocol Comparison

| Protocol | ID Size    | Transport | Notes                             |
| -------- | ---------- | --------- | --------------------------------- |
| GRE      | 32-bit key | IP        | Point-to-point; weak auth         |
| VXLAN    | 24-bit VNI | UDP/4789  | Default for ML2/OVS; 16M segments |
| GENEVE   | 24-bit VNI | UDP       | Extensible headers; OVN default   |

## DVR Mode Quick Reference

| Agent Mode | Location      | Role                                |
| ---------- | ------------- | ----------------------------------- |
| `legacy`   | Network node  | All routing centralized             |
| `dvr_snat` | Network node  | SNAT only (fixed-IP VMs)            |
| `dvr`      | Compute nodes | East-west + Floating IP north-south |

## BGP Options

| Tool                      | Use Case                                              |
| ------------------------- | ----------------------------------------------------- |
| `neutron-dynamic-routing` | Advertise prefixes/FIPs to upstream routers (ML2/OVS) |
| `networking-bgpvpn`       | Interconnect with existing enterprise L3VPN/E-VPN     |
| `ovn-bgp-agent` + FRR     | EVPN fabric (ML2/OVN, modern)                         |

## Literature

- [[literature/openstack-neutron-external-connectivity]]
