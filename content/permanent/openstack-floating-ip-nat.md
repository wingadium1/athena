---
title: "Floating IP NAT Mechanism in OpenStack Neutron"
aliases:
  ["floating IP Neutron", "DNAT SNAT Neutron", "one-to-one NAT OpenStack", "qrouter iptables"]
tags: [infrastructure, openstack, networking, nat, floating-ip]
created: 2026-04-09
updated: 2026-04-09
---

## What Floating IPs Are

Floating IPs implement **one-to-one NAT** (1:1 NAT, not NAPT): a single public IP is permanently and exclusively mapped to a single VM's private IP. Unlike SNAT (many-to-one), floating IPs give external hosts a stable, direct way to initiate connections to a specific VM.

Neutron uses **iptables in the router namespace** (or the FIP namespace in DVR) to implement this — not hardware. The Linux server running `neutron-l3-agent` is the NAT device.

## Lifecycle of a Floating IP

1. Admin marks a provider network as `router:external = true`
2. Admin creates subnet with public IP range on that network
3. Tenant creates floating IP from the pool: `openstack floating ip create provider1`
4. Tenant associates it with a VM port: `openstack server add floating ip <instance> <fip>`
5. Neutron L3 agent adds iptables rules in the router namespace

## iptables Rules in the Router Namespace

In the **centralized (non-DVR)** model, inside `qrouter-<uuid>`:

```bash
# DNAT rule: incoming traffic to floating IP → VM private IP
-A neutron-l3-agent-PREROUTING -d 203.0.113.17 -j DNAT --to-destination 192.0.2.4

# SNAT rule: outgoing traffic from VM private IP → floating IP
-A neutron-l3-agent-float-snat -s 192.0.2.4 -j SNAT --to-source 203.0.113.17
```

For SNAT on fixed-IP instances (no floating IP):

```bash
# SNAT to router's external IP (masquerade)
-A neutron-l3-agent-snat -o qg-<uuid> -j SNAT --to-source 203.0.113.11
```

The gateway interface `qg-<uuid>` is where the public IP is assigned. The `qr-<uuid>` interfaces carry tenant subnet IPs.

## Traffic Flow: Floating IP Ingress (Centralized)

```
Internet → Physical NIC → br-provider (strip VLAN) → br-int → qrouter namespace
  qg-<uuid> receives packet (dest: 203.0.113.17)
  DNAT → dest becomes 192.0.2.4
  Route lookup → out via qr-<uuid> to VXLAN tenant network
  br-int → br-tun → VXLAN encap → compute node → VM
```

## Traffic Flow: Floating IP with DVR (Distributed)

With DVR enabled, the DNAT/SNAT for floating IPs happens **on the compute node** itself. The `fip-<provider-net-uuid>` namespace is created on the compute node:

```
Internet → Physical NIC → br-provider → br-int → fip namespace (fg- port does Proxy ARP)
  fip namespace routes via rfp- ↔ fpr- veth (169.254.x.x/31) to qrouter namespace
  qrouter namespace: DNAT (203.0.113.17 → 192.0.2.4) + route to VM
  br-int → security group bridge → VM vNIC
```

Egress (VM → Internet with floating IP, DVR):

```
VM → br-int → qrouter namespace: SNAT (192.0.2.4 → 203.0.113.17)
  qrouter → fpr- veth → fip namespace
  fip namespace fg- port → br-int → br-provider → Physical NIC → Internet
```

## Floating IP vs SNAT: When to Use Which

| Scenario                                            | Mechanism                       | Who initiates    |
| --------------------------------------------------- | ------------------------------- | ---------------- |
| VM needs outbound internet                          | SNAT (shared router IP)         | VM only          |
| External service needs to reach VM directly         | Floating IP (DNAT)              | External host    |
| Services not yet in tenant subnet needing VM access | Floating IP on external network | External service |

> [!important] Your Use Case
> For the multi-tenant deployment context (external services needing to connect **into** tenant VMs before those services are ready to move into the tenant subnet), **floating IPs are the right answer**. Assign a floating IP from the external network pool → external services reach VMs directly via DNAT without changing network topology.

## ARP and Proxy ARP Behavior

- The `qg-<uuid>` interface in the router namespace holds the floating IP addresses (or in DVR, the `fg-` port in `fip-` namespace)
- It responds to ARP requests for those floating IPs on the provider/external network
- In DVR, the `fg-` port uses **Proxy ARP** — it responds on behalf of all floating IPs hosted on that compute node

## Connections

- [[permanent/openstack-dvr-architecture]] — DVR changes which namespace does the NAT
- [[permanent/openstack-external-network-mapping]] — provider network where floating IPs live
- [[permanent/openstack-neutron-overlay-protocols]] — VXLAN carries the post-DNAT traffic to the VM

## Sources

- [[literature/openstack-neutron-external-connectivity]]
- [NAT in Neutron](https://docs.openstack.org/neutron/2024.2/admin/intro-nat.html)
- [Eran Gampel — DVR Floating IPs Part 2](https://blog.gampel.net/2014/12/openstack-dvr2-floating-ips.html)
- [networkop.co.uk — Neutron SDN Deep Dive](https://networkop.co.uk/blog/2016/04/22/neutron-native/)
