---
title: "OpenStack Provider Network Routing (No Floating IP)"
aliases: ["provider net static route", "openstack no floating ip", "br-ex gateway pattern"]
tags: [openstack, neutron, networking, routing, infrastructure]
created: 2026-04-10
updated: 2026-04-10
---

Trong môi trường lab hoặc private cloud, floating IP thường không cần thiết cho infra tools (Packer, control plane services). Một static route đơn giản từ jump host qua gateway node đến provider network đủ để đạt được reachability — và tránh hoàn toàn complexity của floating IP NAT.

## Topology

```
tools-1 (10.16.90.x)
    │
    │  static route: 10.10.1.0/24 via ctrl-1
    ▼
ctrl-1 (10.16.90.66 / br-ex: 10.10.1.66)
    │
    │  br-ex bridges to provider network
    ▼
Provider Network: 10.10.1.0/24
    │
    ├── VM instances
    └── Trove guest VMs
```

## Static Route Setup

Trên `tools-1` (jump host / control plane node):

```bash
# Persistent route (Ubuntu 24.04 — netplan)
# /etc/netplan/99-routes.yaml
network:
  version: 2
  ethernets:
    eth0:
      routes:
        - to: 10.10.1.0/24
          via: 10.16.90.66
```

Hoặc tạm thời:

```bash
ip route add 10.10.1.0/24 via 10.16.90.66
```

Trên `ctrl-1`, enable IP forwarding:

```bash
echo 'net.ipv4.ip_forward = 1' >> /etc/sysctl.conf
sysctl -p
```

## Gateway IP phải ở trên br-ex

> **Rule**: Gateway IP cho provider network phải được gán cho `br-ex`, không phải raw `enp6s19`.

Lý do: OVS owns `enp6s19` sau khi add vào bridge. IP trên raw NIC sẽ không reachable. Chỉ IP trên `br-ex` (OVS internal port) mới được Linux routing stack xử lý.

```bash
# ĐÚNG: IP trên br-ex
ip addr add 10.10.1.66/24 dev br-ex

# SAI: IP trên raw NIC (unreachable sau khi add vào OVS)
ip addr add 10.10.1.66/24 dev enp6s19
```

Để persistent, thêm vào Netplan hoặc `/etc/network/interfaces` trỏ vào `br-ex`.

## Tại Sao Không Dùng Floating IP

Floating IP cần:

1. External network với allocated IP range
2. Router kết nối tenant network với external network
3. `neutron floatingip-create` + `floatingip-associate` workflow

Với static route approach:

- Không cần external network allocation
- `tools-1` → `ctrl-1` → `br-ex` → provider subnet — direct routing
- Packer, Trove guest agents, và các infra tools reach VMs trực tiếp qua provider IP

Trade-off: chỉ phù hợp khi `tools-1` và VMs đều trong cùng administrative domain (không cần multi-tenant isolation).

## Connections

- [[permanent/ovs-bridge-management-nic-pitfall]] — incident dẫn đến decision này
- [[permanent/openstack-trove-guest-agent-connectivity]] — Trove cần br-ex IP `10.10.1.66` cho SNAT và RabbitMQ
- [[permanent/openstack-floating-ip-nat]] — floating IP mechanism nếu cần cho production
- [[permanent/openstack-external-network-mapping]] — br-ex architecture tổng quan

## Sources

- [[literature/proxmox-dbaas-lab-day1-to-day4.5]]
