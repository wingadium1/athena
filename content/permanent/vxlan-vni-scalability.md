---
title: "VXLAN và VNI Scalability"
aliases: ["VXLAN", "Virtual Extensible LAN", "VNI", "VXLAN Network Identifier", "RFC 7348"]
tags: [networking, overlay, VXLAN, cloud, multi-tenancy]
created: 2026-04-09
updated: 2026-04-09
---

VXLAN (Virtual Extensible LAN) — định nghĩa trong **RFC 7348** — là overlay protocol encapsulates Ethernet frames (L2) bên trong UDP/IP packets (L3). Điều này cho phép L2 networks "stretch" qua L3 boundaries của underlay network.

## 24-bit VNI: Giải pháp cho 4094 VLAN limit

Thay vì 12-bit VLAN ID, VXLAN dùng **24-bit VXLAN Network Identifier (VNI)**:

| | VLAN (802.1Q) | VXLAN |
|---|---|---|
| Identifier size | 12-bit | 24-bit |
| Max segments | 4,094 | **16,777,216** (~16 triệu) |
| Transport | L2 tagging | UDP/IP (port 4789) |
| L3 traversal | Không | Có |
| Encapsulation overhead | ~4 bytes | ~50 bytes |

16 triệu segments là đủ cho bất kỳ cloud deployment nào trong thực tế.

## Packet structure

```
Outer Ethernet header
└─ Outer IP header (src: VTEP IP, dst: VTEP IP)
   └─ Outer UDP header (dst port: 4789)
      └─ VXLAN header (8 bytes, gồm 24-bit VNI)
         └─ Inner Ethernet frame (original payload)
```

VTEP (VXLAN Tunnel Endpoint) là điểm encap/decap — thường là hypervisor (software VTEP) hoặc switch (hardware VTEP).

## Ưu điểm trong cloud networking

- **L3 underlay, L2 overlay**: Physical network chỉ cần route IP. Tenant networks tự do span across racks, pods, data centers.
- **ECMP-friendly**: Vì VXLAN dùng UDP, các underlay switches có thể load-balance bằng 5-tuple hash (src IP, dst IP, src port, dst port, protocol).
- **Workload mobility**: VM migration qua L3 boundaries trở nên dễ dàng vì identity (VNI) gắn với overlay, không với physical location.

## Giới hạn

- **Fixed 8-byte header**: Không có room cho metadata. Không thể truyền thêm thông tin (security policy, telemetry, service chaining) trong-band.
- **MTU overhead**: ~50 bytes overhead. Cần tăng physical MTU lên ít nhất 1550 bytes (hoặc dùng jumbo frames 9000 bytes).
- **Multicast dependency** (flood-and-learn mode): Nếu không dùng BGP EVPN hay L2 population, VXLAN dùng multicast để discover MACs — sinh ra BUM traffic.

> [!tip] OVN + VXLAN special case
> Trong OpenStack ML2/OVN, khi dùng VXLAN làm overlay, OVN bị giới hạn thêm: identifier space trong header bị reduce xuống **12-bit** để fit OVN internal metadata, giới hạn còn **4096 networks** và 4096 ports/network. Đây là lý do OVN ưu tiên GENEVE.

## Connections

- [[permanent/vlan-4094-limit]] — vấn đề mà VXLAN giải quyết
- [[permanent/openstack-neutron-overlay-protocols]] — so sánh VXLAN, GRE, GENEVE
- [[permanent/openstack-ml2-overlay-config]] — ml2_conf.ini configuration

## Sources

- [[literature/openstack-neutron-overlay-research-2026-04]]
