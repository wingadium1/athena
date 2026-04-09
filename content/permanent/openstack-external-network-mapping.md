---
title: "OpenStack External Network Mapping: Provider Networks to Tenant Overlays"
aliases: ["provider network Neutron", "external network OVS", "br-ex bridge mapping"]
tags: [infrastructure, openstack, networking, provider-network]
created: 2026-04-09
updated: 2026-04-09
---

## The Two-Network World in Neutron

Neutron manages two fundamentally different network types:

**Provider Networks** (admin-created): mapped directly to physical network infrastructure. They can be:
- `flat` — untagged, one-to-one bridge mapping to a physical interface
- `vlan` — 802.1q tagged, up to 4094 per physical network
- `vxlan`/`gre`/`geneve` — overlay (rare for provider; usually used for self-service)

**Self-service (Tenant) Networks** (tenant-created): fully virtual overlays (VXLAN, GRE, GENEVE) with no direct physical mapping — exist only inside the hypervisor fabric.

The **critical challenge**: how does a VM on a VXLAN tenant network reach the external world (a flat or VLAN provider network)?

## The Bridge Mapping Architecture

In ML2/OVS, Neutron uses two OVS bridges connected via patch ports:

```
[Physical NIC] ── [br-provider (OVS)] ── patch ── [br-int (OVS integration bridge)]
                     (maps VLAN tags)                  (internal VLAN tags for all networks)
```

Configuration in `openvswitch_agent.ini`:
```
[ovs]
bridge_mappings = provider:br-provider
```

- `br-provider` carries actual VLAN tags seen by the physical network
- `br-int` uses **internal VLAN tags** (locally significant on each host) for all networks — both provider and overlay
- `br-tun` (tunnel bridge) handles VXLAN encap/decap and VLAN ↔ VNI translation

## How OVS Routes Between Overlay and Provider

When a VM on VXLAN VNI 101 sends traffic toward the external network:

1. Frame exits VM → `br-int` assigns internal VLAN tag
2. `br-int` → `br-tun`: exchanges VLAN tag for VXLAN VNI 101, encapsulates in UDP
3. Traffic travels over underlay IP to network node
4. Network node `br-tun` unwraps VXLAN, assigns internal VLAN
5. `br-int` on network node delivers to `qrouter-<uuid>` namespace (the L3 router)
6. Router namespace routes packet out via `qg-<uuid>` (gateway port) to provider network
7. `br-int` → `phy-br-provider` patch: swaps internal VLAN for actual provider VLAN tag 101
8. Exits physical interface to external network

**Key insight**: The L3 router namespace is the critical translation point. Each Neutron router has:
- One or more `qr-<uuid>` interfaces (one per connected tenant subnet)
- One `qg-<uuid>` interface (gateway, on the provider/external network)

## The Router Namespace in Detail

```bash
# On network node, list namespaces
ip netns
# → qrouter-78d2f628-137c-4f26-a257-25fc20f203c1
# → qdhcp-8b868082-e312-4110-8627-298109d4401c

# Inside the router namespace
ip netns exec qrouter-<uuid> ip route
# default via 203.0.113.1 dev qg-<uuid>    ← gateway to external
# 192.0.2.0/24 dev qr-<uuid>               ← tenant subnet 1
# 198.51.100.0/24 dev qr-<uuid2>           ← tenant subnet 2
```

The router also holds **iptables rules** for NAT (SNAT/DNAT) applied on the `qg-` interface. This is how the overlay-to-external address translation happens.

## Multi-External-Network Support (Provider Network Model)

In the modern provider network model (as opposed to the legacy `external_network_bridge`):
- All `qg-` and `qr-` interfaces attach to `br-int` directly
- Each external network maps to a different VLAN on `br-provider` (or a different `br-provider` bridge)
- Enables multiple external networks per L3 agent (critical for multi-tenant with separate external pools)

```
[External Net 1: VLAN 101] ── br-provider ── br-int ── qrouter-A (qg on VLAN 101)
[External Net 2: VLAN 102] ── br-provider ── br-int ── qrouter-B (qg on VLAN 102)
```

Both routers coexist on the same `br-int`, isolated by internal VLAN tags.

## Connections

- [[permanent/openstack-overlay-networks]] — VNI/GRE tunnel segment IDs
- [[permanent/openstack-dvr-architecture]] — how DVR distributes this router namespace to compute nodes
- [[permanent/openstack-floating-ip-nat]] — NAT rules in the router namespace

## Sources

- [[literature/openstack-neutron-external-connectivity]]
- [Open vSwitch: Self-service Networks](https://docs.openstack.org/neutron/2024.2/admin/deploy-ovs-selfservice.html)
- [Provider Networks Architecture](https://blog.oddbit.com/post/2015-08-13-provider-external-networks-det/)
