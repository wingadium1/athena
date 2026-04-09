---
title: "OpenStack Overlay Networks: VXLAN, GRE, GENEVE"
aliases: ["VXLAN tenant network", "overlay tunnel Neutron", "Neutron tunnel types"]
tags: [infrastructure, openstack, networking, overlay]
created: 2026-04-09
updated: 2026-04-09
---

## The Problem: VLAN Exhaustion in Multi-Tenant Clouds

VLAN tagging (802.1q) supports at most 4094 IDs. In a multi-tenant cloud with thousands of projects, each needing network isolation, this ceiling is hit quickly. Overlay (tunnel) protocols solve this by encapsulating tenant L2 frames inside underlay L3 packets — decoupling tenant isolation identifiers from physical VLAN tags.

## Three Overlay Protocols Supported by Neutron

OpenStack Neutron ML2 plugin supports three tunnel types, configured via `tunnel_types` in `openvswitch_agent.ini`:

### GRE (Generic Routing Encapsulation)

- Encapsulates any network-layer protocol inside IP
- Creates point-to-point tunnels between hypervisors
- Each tenant network gets a unique **GRE key** (32-bit)
- Limitation: does not scale well for large deployments (full mesh of tunnels between all compute nodes)
- Configured: `tunnel_types = gre`

### VXLAN (Virtual Extensible LAN)

- L2-over-L3 overlay using UDP (default port 4789)
- 24-bit **VNI** (VXLAN Network Identifier): supports **16 million** isolated segments — 4000× VLAN's 4094
- Each tenant's self-service network gets a VNI from a configured range (`vni_ranges = VNI_START:VNI_END`)
- Uses IP multicast (or l2population for unicast) for BUM (Broadcast/Unknown Unicast/Multicast) traffic
- Encapsulation: `[Outer Ethernet][Outer IP][UDP][VXLAN header (8B, VNI)][Inner L2 frame]`
- Requires Linux kernel ≥ 3.13
- **Current default for self-service networks** in ML2/OVS deployments

### GENEVE (Generic Network Virtualization Encapsulation)

- Designed as a flexible successor to both VXLAN and STT
- Variable-length header with extensible TLV options (allows rich metadata)
- Uses UDP transport; VNI is 24-bit (same scale as VXLAN)
- OVN (Open Virtual Network) uses GENEVE as its default encapsulation, replacing VXLAN/GRE in modern deployments
- Better suited for future protocol evolution — metadata can carry flow context, service hints, etc.

## How Neutron Assigns Tunnel IDs

```
[ml2]
type_drivers = flat,vlan,vxlan
tenant_network_types = vxlan

[ml2_type_vxlan]
vni_ranges = 1:1000
```

When a tenant creates a self-service network, Neutron allocates the next available VNI from the configured range. This VNI is stored in Neutron's DB and distributed to all OVS agents via RPC. OVS then installs flow rules mapping the VNI ↔ internal VLAN tag used within the integration bridge (`br-int`).

## Encapsulation Path (OVS ML2/VXLAN)

```
VM vNIC
  → tap interface (qbr bridge / security group rules)
  → OVS br-int (internal VLAN tag assigned per network)
  → OVS br-tun (VLAN ↔ VXLAN VNI translation via OpenFlow rules)
  → Physical NIC (VXLAN UDP packet sent over underlay IP network)
  → Remote compute node br-tun (unwrap, add internal tunnel ID → VLAN)
  → Remote br-int → target VM
```

The `l2population` mechanism driver pre-populates forwarding tables (eliminating multicast flood), improving scale significantly for VXLAN.

## Why Connections

- [[permanent/openstack-external-network-mapping]] — how the external provider network bridges into the overlay
- [[permanent/openstack-dvr-architecture]] — how L3 routing happens between overlay and provider networks
- [[permanent/openstack-floating-ip-nat]] — NAT translation at the overlay/external boundary

## Sources

- [[literature/openstack-neutron-external-connectivity]]
- [OpenStack Networking Guide — Overlay Protocols](https://docs.openstack.org/neutron/2024.2/admin/intro-overlay-protocols.html)
- [ML2 Self-Service Networks with OVS](https://docs.openstack.org/neutron/2024.2/admin/deploy-ovs-selfservice.html)
