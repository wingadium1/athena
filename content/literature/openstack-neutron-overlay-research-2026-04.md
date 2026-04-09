---
title: "OpenStack Neutron Overlay Networks — Research Session"
type: literature
source: "https://docs.openstack.org/neutron/latest/admin/intro-overlay-protocols.html"
author: "OpenStack Docs / RFC 7348 / RFC 8926"
date-read: 2026-04-09
tags: [networking, OpenStack, Neutron, VXLAN, GENEVE, GRE, overlay, multi-tenancy]
---

## Summary

Research session tập trung vào cách OpenStack Neutron vượt qua giới hạn 4094 VLAN IDs của 802.1Q. Nguồn chính: Neutron docs (2024.1 và latest 28.x), RFC 7348 (VXLAN), RFC 8926 (GENEVE), ML2 configuration reference. Kết quả: ba overlay protocols (GRE, VXLAN, GENEVE) đều dùng 24-bit identifiers để support ~16M segments, nhưng khác biệt về metadata extensibility và backend support là yếu tố quyết định chọn GENEVE + OVN cho modern deployments.

## Key Ideas

- **12-bit VID → 24-bit VNI**: VLAN bị capped ở 4094 usable IDs. GRE, VXLAN, GENEVE đều dùng 24-bit identifiers → 16,777,216 isolated networks.
- **VXLAN fixed header = Achilles heel**: RFC 7348 định nghĩa 8-byte fixed header. Không có room cho metadata. Fixed format là nguồn gốc của nhiều VXLAN limitations so với GENEVE.
- **GENEVE TLV extensibility**: RFC 8926 cho phép variable-length TLV options trong header. Use cases: in-band telemetry, security policy tags, service chaining (NSH), transport security — tất cả không cần thay đổi base protocol.
- **OVN + GENEVE default**: OVN dùng GENEVE options để truyền internal packet metadata. Khi dùng VXLAN với OVN, identifier bị giảm xuống 12-bit → chỉ còn 4096 networks (trở về đúng vấn đề ban đầu muốn giải quyết).
- **L2 population là critical ở scale**: Không có L2 pop, VXLAN flood BUM traffic đến mọi VTEP. Ở 100+ compute nodes, đây là performance problem nghiêm trọng.
- **max_header_size = 38 cho OVN**: Default config trong ml2_conf.ini là 30, không đủ cho OVN. Đây là common misconfiguration.
- **GRE đang fade out**: Không support OVN, poor ECMP, không có ecosystem momentum. Chỉ còn trong legacy deployments.

## Quotes

> "Self-service networks use overlay protocols such as VXLAN or GRE because they can support many more networks than layer-2 segmentation using VLAN tagging (802.1q)." — Neutron docs

> "Because of limited space in VXLAN VNI to pass over the needed information that requires OVN to identify a packet, the header size to contain the segmentation ID is reduced to 12 bits, that allows a maximum number of 4096 networks." — OVN install manual

> "The data plane is generic and extensible enough to support current and future control planes." — RFC 8926 (GENEVE design goal)

> "For best performance, 10+ Gbps physical network infrastructure should support jumbo frames." — Neutron deployment guide

## Sources chính

- [Overlay protocols — Neutron 2024.1](https://docs.openstack.org/neutron/2024.1/admin/intro-overlay-protocols.html)
- [ML2 Plug-in — Neutron latest](https://docs.openstack.org/neutron/latest/admin/config-ml2.html)
- [ml2_conf.ini reference — Neutron 2025.1](https://docs.openstack.org/neutron/2025.1/configuration/ml2-conf.html)
- [OVN manual install — Neutron 2025.1](https://docs.openstack.org/neutron/2025.1/install/ovn/manual_install.html)
- [OVS self-service networks — Neutron 2025.1](https://docs.openstack.org/neutron/2025.1/admin/deploy-ovs-selfservice.html)
- [RFC 7348 — VXLAN](https://www.rfc-editor.org/rfc/rfc7348)
- [RFC 8926 — GENEVE](https://www.rfc-editor.org/rfc/rfc8926)

## Links

- [[permanent/vlan-4094-limit]]
- [[permanent/vxlan-vni-scalability]]
- [[permanent/openstack-neutron-overlay-protocols]]
- [[permanent/openstack-ml2-overlay-config]]
