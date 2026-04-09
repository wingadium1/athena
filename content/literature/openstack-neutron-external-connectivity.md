---
title: "OpenStack Neutron External-to-Tenant Network Connectivity"
type: literature
source: "https://docs.openstack.org/neutron/latest/admin/"
author: "OpenStack Neutron contributors"
date-read: 2026-04-09
tags: [infrastructure, openstack, networking]
---

## Summary

OpenStack Neutron solves the VLAN-limit problem by using overlay protocols (VXLAN/GRE/GENEVE) for tenant isolation, while provider networks (flat/VLAN) serve as the external connectivity boundary. L3 routing between the two happens inside Linux network namespaces (`qrouter-*`) managed by the L3 agent, using iptables for NAT. DVR distributes this routing to compute nodes to eliminate the network node bottleneck. BGP extensions enable large-scale dynamic routing between the cloud and external networks.

## Key Ideas

- **Overlay tunnels solve VLAN exhaustion**: VXLAN VNIs are 24-bit (16M segments vs. 4094 VLANs). GENEVE adds extensible metadata headers. OVN uses GENEVE by default.

- **OVS bridge hierarchy**: `br-int` (integration) ↔ `br-tun` (tunnel/VXLAN) ↔ `br-provider` (physical VLAN). The L3 router namespace spans `br-int`, routing between tenant ports (`qr-`) and external gateway port (`qg-`).

- **L3 agent creates Linux namespaces**: one `qrouter-<uuid>` per virtual router. Contains iptables SNAT/DNAT rules + route table. In centralized mode, lives on network node only. In DVR mode, replicated to all compute nodes hosting VMs on that router.

- **DVR distributes east-west completely**: VM-to-VM across subnets never hits the network node with DVR. But SNAT for fixed-IP instances remains centralized on the network node (`snat-<uuid>` namespace, `dvr_snat` agent mode).

- **Floating IP = 1:1 NAT via iptables**: DNAT rule maps external IP to VM private IP in the router namespace. With DVR, this moves to the `fip-<uuid>` namespace on the compute node. Consumes one external IP per floating IP, plus one per compute node per distributed router for the DVR gateway port.

- **BGP dynamic routing** (`neutron-dynamic-routing`): advertises self-service network prefixes and floating IP host routes to upstream physical routers via eBGP. Enables dynamic routing instead of static routes.

- **networking-bgpvpn**: interconnects Neutron resources with external BGP L3VPN/E-VPN (RFC 4364 / RFC 7432). Useful when enterprise MPLS VPNs need seamless extension into the cloud.

- **ovn-bgp-agent** (modern ML2/OVN): uses FRR + Linux VRFs + VXLAN to expose VMs via EVPN. Creates one VRF per provider network with a VNI. OVN 25.03 adds native BGP; Neutron RFE approved to integrate natively.

## Quotes

> "For instances with a floating IPv4 address using a self-service network on a distributed router, the compute node containing the instance performs SNAT on north-south traffic passing from the instance to external networks such as the Internet and DNAT on north-south traffic passing from external networks to the instance."
> — [OpenStack Neutron DVR Docs](https://docs.openstack.org/neutron/latest/admin/deploy-ovs-ha-dvr.html)

> "Routing also resides completely on the compute nodes for instances with a fixed or floating IPv4 address using self-service networks on the same distributed virtual router. However, instances with a fixed IP address still rely on the network node for routing and SNAT services."
> — Same source

> "BGP dynamic routing enables advertisement of self-service (private) network prefixes to physical network devices that support BGP, thus removing the conventional dependency on static routes."
> — [BGP Dynamic Routing Docs](https://docs.openstack.org/neutron/2024.2/admin/config-bgp-dynamic-routing.html)

## Links

- [[permanent/openstack-neutron-overlay-protocols]]
- [[permanent/openstack-external-network-mapping]]
- [[permanent/openstack-dvr-architecture]]
- [[permanent/openstack-floating-ip-nat]]
- [[permanent/openstack-bgp-evpn-external]]
