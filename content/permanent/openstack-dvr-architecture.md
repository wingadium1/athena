---
title: "DVR: Distributed Virtual Router in OpenStack Neutron"
aliases: ["DVR OpenStack", "distributed routing Neutron", "dvr_snat mode", "qrouter namespace compute"]
tags: [infrastructure, openstack, networking, dvr, l3]
created: 2026-04-09
updated: 2026-04-09
---

## The Problem DVR Solves

In the **centralized L3 model** (pre-Juno), all inter-subnet routing (east-west) and all north-south traffic passed through the **network node**:

```
VM1 (compute1) → overlay tunnel → Network Node (qrouter) → overlay tunnel → VM2 (compute2)
```

This created a single point of failure AND a bandwidth bottleneck: every packet traversed the network node twice even for two VMs on the same physical rack.

**DVR** (Distributed Virtual Router, introduced in Juno, 2014) solves this by instantiating router namespace replicas directly on compute nodes, using the same router UUID across all nodes.

## DVR Mode Configuration

Three distinct agent modes:

| Node | `agent_mode` | Role |
|------|-------------|------|
| Network node | `dvr_snat` | Handles centralized SNAT only |
| Compute nodes | `dvr` | Handles east-west + floating IP north-south |
| Legacy/Centralized | `legacy` | All routing centralized (non-DVR) |

```ini
# network node l3_agent.ini
[DEFAULT]
agent_mode = dvr_snat

# compute node l3_agent.ini
[DEFAULT]
agent_mode = dvr
```

Also requires in `neutron.conf`:
```ini
[DEFAULT]
router_distributed = True
```

And in `openvswitch_agent.ini` (both network and compute nodes):
```ini
[agent]
enable_distributed_routing = True
l2_population = True
```

## What DVR Distributes vs. What Remains Centralized

### ✅ Distributed to Compute Nodes

**East-West routing** (VM-to-VM across subnets on same router): the `qrouter-<uuid>` namespace is replicated on every compute node that hosts a VM attached to that router. Traffic never leaves the compute node for inter-subnet routing.

**North-South with Floating IP** (DNAT): a `fip-<provider-net-uuid>` namespace is created on each compute node that has VMs with floating IPs. The compute node performs DNAT/SNAT directly, bypassing the network node entirely for that traffic.

### ❌ Still Centralized (Network Node)

**SNAT (fixed-IP instances without floating IP)**: when a VM has no floating IP and needs to reach the internet, packets are still sent to the network node's `snat-<uuid>` namespace. The `dvr_snat` agent handles this.

Reason: SNAT requires maintaining a shared IP pool and connection tracking state that is difficult to distribute without consuming too many external IPs.

**IPv6 routing**: still centralized through the network node in ML2/OVS deployments (ML2/OVN distributes IPv6 too).

## Namespace Topology with DVR

### On each compute node (with a VM on distributed router):
```bash
ip netns list
# qrouter-78d2f628-137c-4f26-a257-25fc20f203c1   ← router replica
# fip-4bfa3075-b4b2-4f7d-b88e-df1113942d43       ← FIP namespace (per external net)
```

### On the network node:
```bash
ip netns list
# snat-78d2f628-137c-4f26-a257-25fc20f203c1      ← centralized SNAT only
# qrouter-78d2f628-137c-4f26-a257-25fc20f203c1   ← present but serves no routing purpose
```

## The FIP Namespace: DVR's Key Innovation

The `fip-<uuid>` namespace (one per external network per compute node) contains:
- A `fg-<uuid>` gateway port (with Proxy ARP) connected to the provider external network
- A `rfp-<uuid>` ↔ `fpr-<uuid>` point-to-point veth pair connecting to the `qrouter` namespace at 169.254.x.x/31 link-local address
- The DVR namespace consumes one IP from the external network per compute node

Floating IP DNAT flow (ingress):
```
External packet → br-provider → br-int → fip namespace (fg-) → rfp-rfp veth
  → qrouter namespace (DNAT: floating IP → VM private IP) → br-int → VM
```

This is why: **each compute node running DVR needs an additional external network interface** (or at least reachability to the external network) — the `fg-` gateway port must reach the physical external network.

## IP Address Consumption Warning

DVR requires **one external IP per compute node per distributed router**. For a deployment with:
- 10 compute nodes
- 5 distributed routers with floating IPs

You need 50 external IPs reserved just for DVR gateway ports. Plan your external IP allocation accordingly.

## DVR + HA (VRRP for SNAT)

DVR and HA can be combined (`--distributed --ha`) for high-availability SNAT:
- Multiple `snat-<uuid>` namespaces on different network nodes
- Keepalived VRRP provides failover of the SNAT service
- Max 3 agents per router controlled by `max_l3_agents_per_router`

```ini
[DEFAULT]
router_distributed = True
l3_ha = True
l3_ha_net_cidr = 169.254.192.0/18
max_l3_agents_per_router = 3
```

## Connections

- [[permanent/openstack-external-network-mapping]] — how provider networks connect to the DVR
- [[permanent/openstack-floating-ip-nat]] — the DNAT/SNAT mechanics inside DVR namespaces
- [[permanent/openstack-overlay-networks]] — VXLAN/GRE tunnels that carry east-west traffic between DVR instances

## Sources

- [[literature/openstack-neutron-external-connectivity]]
- [Official: Open vSwitch with DVR](https://docs.openstack.org/neutron/latest/admin/deploy-ovs-ha-dvr.html)
- [DVR Original Spec (Juno)](https://github.com/openstack/neutron-specs/blob/master/specs/juno/neutron-ovs-dvr.rst)
- [Eran Gampel DVR Blog Series](https://blog.gampel.net/2014/12/openstack-neutron-distributed-virtual.html)
- [Red Hat DVR Guide (RHOSP 16.2)](https://docs.redhat.com/en/documentation/red_hat_openstack_platform/16.2/html/networking_guide/config-dvr_rhosp-network)
