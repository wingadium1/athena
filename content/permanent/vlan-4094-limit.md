---
title: "The 4094 VLAN Limit"
aliases: ["802.1Q limit", "VLAN scalability problem", "12-bit VID"]
tags: [networking, cloud, multi-tenancy, overlay]
created: 2026-04-09
updated: 2026-04-09
---

The IEEE 802.1Q standard defines a **12-bit VLAN Identifier (VID)** field in the Ethernet frame header. 12 bits = 4096 possible values. Trừ đi VID 0 (untagged) và VID 4095 (reserved), còn lại **4094 usable VLAN IDs** trong bất kỳ một L2 broadcast domain nào.

Với public/private clouds quy mô lớn — hàng nghìn tenants, mỗi tenant cần nhiều isolated networks — con số 4094 đủ nhanh để trở thành bottleneck. Trên thực tế còn tệ hơn: Cisco Nexus chỉ support VLANs up đến 3967 (3968–4094 là system reserved).

## Vấn đề cốt lõi

1. **Scalability**: 4094 < số tenants trong multi-cloud production
2. **L2 failure domains quá rộng**: STP (Spanning Tree Protocol) vẫn được dùng trong VLAN networks, làm broadcast storms lan rộng
3. **Không cross L3 boundary**: VLAN gắn liền với L2, không thể span qua multiple routers mà không có complex config
4. **Workload mobility bị giới hạn**: Di chuyển VM giữa physical hosts qua L3 boundaries là khó

## Tại sao phải vượt qua giới hạn này?

OpenStack Neutron tạo một **tenant network** cho mỗi project network. Trong large deployments: hàng nghìn tenants × nhiều networks/tenant = cần identifier space vượt xa 4094. Overlay protocols giải quyết điều này bằng cách dùng **24-bit VNI** (hoặc tương đương), cho phép ~16 triệu isolated segments.

## Connections

- [[permanent/openstack-neutron-overlay-protocols]] — các giải pháp overlay trong Neutron
- [[permanent/vxlan-vni-scalability]] — VXLAN 24-bit VNI cụ thể
- [[permanent/openstack-ml2-overlay-config]] — cách configure trong ML2 plugin

## Sources

- [[literature/openstack-neutron-overlay-research-2026-04]]
