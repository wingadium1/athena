---
title: "BGP/EVPN External Connectivity in OpenStack Neutron"
aliases: ["BGP dynamic routing Neutron", "BGPVPN OpenStack", "neutron-dynamic-routing", "ovn-bgp-agent", "networking-bgpvpn"]
tags: [infrastructure, openstack, networking, bgp, evpn, large-scale]
created: 2026-04-09
updated: 2026-04-09
---

## Why BGP for External Connectivity?

Floating IPs and SNAT work well at moderate scale but have limitations for large deployments:
- Floating IP pools are finite; each DVR compute node also consumes an external IP for its gateway port
- Static routes on physical routers don't scale
- No dynamic advertisement of tenant network prefixes to upstream routers

**BGP dynamic routing** enables Neutron to advertise self-service network prefixes to physical routers, removing the dependency on static routes and enabling dynamic, scalable routing between OpenStack and external networks.

## Two Distinct BGP Mechanisms in OpenStack

### 1. `neutron-dynamic-routing` (BGP Speaker)

The traditional BGP integration for ML2/OVS. Advertises Neutron routes (floating IPs, tenant prefixes) to external BGP peers.

```ini
# neutron.conf
service_plugins = neutron_dynamic_routing.services.bgp.bgp_plugin.BgpPlugin,
                  neutron.services.l3_router.l3_router_plugin.L3RouterPlugin

# dragent.ini
[BGP]
bgp_speaker_driver = neutron_dynamic_routing.services.bgp.agent.driver.os_ken.driver.OsKenBgpDriver
bgp_router_id = 10.0.0.1
```

Routes advertised:
- **Host routes** for floating IPs (nexthop = centralized router's external IP, or DVR compute node IP)
- **Prefix routes** for tenant networks that share the same address scope as the external network

Example advertised routes:
```
Destination: 192.0.2.0/25   Nexthop: 203.0.113.11  ← tenant prefix
Destination: 203.0.113.17   Nexthop: 203.0.113.11  ← floating IP host route
```

**DVR consideration**: with DVR, floating IP routes advertise the **compute node's external IP** as the next-hop, enabling direct external → compute routing without hitting the network node.

### 2. `networking-bgpvpn` (L3VPN / E-VPN Interconnection)

A separate OpenStack project enabling RFC 4364 L3VPN and RFC 7432 E-VPN interconnection between Neutron resources and existing enterprise BGP VPNs.

Use case: a tenant already has MPLS L3VPN/E-VPN sites outside the datacenter; they want VMs to seamlessly join that VPN.

```bash
# Create a BGP VPN resource
openstack bgpvpn create --type l3 --route-target 64512:100 --name tenant-vpn

# Associate with a Neutron router
openstack bgpvpn router association create tenant-vpn router1

# Or directly with a network
openstack bgpvpn network association create tenant-vpn tenant-network1
```

Drivers available:
- **BaGPipe** — reference driver for ML2/OVS using `bagpipe-bgp` daemon
- **OpenDaylight** — SDN controller driver
- **OVN** — via `ovn-bgp-agent` with EVPN VRF mode

VXLAN VNI can be explicitly set for EVPN:
```bash
openstack bgpvpn create --vni 1001 --type l3 --route-target 64512:100 my-evpn
```

### 3. `ovn-bgp-agent` + EVPN (Modern OVN/FRR Integration)

For ML2/OVN deployments (the modern path), `ovn-bgp-agent` is a Python daemon running on each node that:
1. Watches OVN Northbound DB for changes
2. Configures Linux VRFs + VXLAN interfaces
3. Uses FRR (Free Range Routing) as BGP speaker to advertise routes via EVPN

EVPN L3VNI mode creates one VRF per provider network:
```yaml
# ovn-bgp-agent config (EVPN VRF mode)
exposing_method: vrf
evpn_local_ip: 10.0.0.1  # VTEP IP
```

For each provider network:
- A VRF device is created (e.g., `vrf-1001`)
- A VXLAN interface (e.g., `vxlan-1001`) bridges the VRF
- FRR advertises VM IPs via BGP EVPN Type-5 (IP Prefix) routes with the VNI as L3VNI

```
ovn-nbctl set logical-switch <uuid> external_ids:"neutron_bgpvpn\:type"="l3"
ovn-nbctl set logical-switch <uuid> external_ids:"neutron_bgpvpn\:vni"="1001"
```

**Status (2026)**: OVN 25.03 introduced native dynamic routing support, with an approved RFE to integrate OVN BGP capabilities directly into Neutron (replacing the need for the external `ovn-bgp-agent` daemon in future releases).

## Choosing the Right BGP Approach

| Scenario | Recommendation |
|----------|----------------|
| ML2/OVS, advertising prefixes to upstream router | `neutron-dynamic-routing` |
| Existing enterprise L3VPN/E-VPN, VMs need to join | `networking-bgpvpn` |
| ML2/OVN, modern deployment, EVPN fabric | `ovn-bgp-agent` with FRR |
| Large telco/NFV requiring MPLS handoff | `networking-bgpvpn` with hardware driver |

## Connections

- [[permanent/openstack-overlay-networks]] — VXLAN VNIs used by EVPN
- [[permanent/openstack-dvr-architecture]] — DVR changes floating IP next-hop advertisement
- [[permanent/openstack-external-network-mapping]] — provider networks that BGP speakers advertise from

## Sources

- [[literature/openstack-neutron-external-connectivity]]
- [BGP Dynamic Routing — Neutron Docs](https://docs.openstack.org/neutron/2024.2/admin/config-bgp-dynamic-routing.html)
- [networking-bgpvpn Overview](https://docs.openstack.org/networking-bgpvpn/2025.1/user/overview.html)
- [ovn-bgp-agent EVPN L3VNI Example](https://docs.openstack.org/ovn-bgp-agent/2024.1/examples/nb_evpn_vrf.html)
- [Red Hat: EVPN in OpenStack Services on OpenShift](https://developers.redhat.com/articles/2025/07/09/how-deploy-evpn-openstack-services-openshift)
- [Neutron RFE: Integrate OVN BGP into Neutron](https://bugs.launchpad.net/bugs/2111276)
