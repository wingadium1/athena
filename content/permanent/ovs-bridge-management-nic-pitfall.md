---
title: "OVS Bridge Management NIC Pitfall"
aliases: ["OVS takeover", "ovs bridge ssh loss", "openstack provider bridge incident"]
tags: [openstack, ovs, networking, incident, infrastructure]
created: 2026-04-10
updated: 2026-04-10
---

Một trong những mistakes nguy hiểm nhất khi cấu hình OpenStack provider network: thêm management NIC (eth0 / interface đang dùng cho SSH) vào OVS bridge. Kết quả là mất SSH ngay lập tức, không có warning.

## Tại Sao OVS "Nuốt" NIC

OVS hoạt động ở Layer 2. Khi bạn add một NIC vào OVS bridge (ví dụ `br-ex`), OVS trở thành L2 owner của NIC đó. Linux kernel không còn nhận L2 frames từ NIC nữa — OVS datapath intercepts tất cả. Điều này có nghĩa:

- IP address đang gán cho eth0 (`10.16.90.66`) trở nên unreachable từ bên ngoài
- SSH connections bị dropped ngay lập tức
- Ping từ management network không còn reply

Đây không phải bug — đây là cách OVS được thiết kế. Vấn đề là operator nhầm lẫn giữa "provider NIC" và "management NIC".

## Recovery Procedure

Khi đã mất SSH, cách duy nhất là dùng **Proxmox noVNC console** (out-of-band access):

```bash
# 1. Stop OVS switching daemon
systemctl stop openvswitch-switch

# 2. Xóa bridge đã tạo nhầm
ovs-vsctl del-br br-float   # hoặc tên bridge bạn đã tạo

# 3. Start lại OVS
systemctl start openvswitch-switch

# 4. Verify management IP trở lại
ip addr show eth0
```

Sau khi OVS không còn own eth0, kernel sẽ nhận lại L2 frames và IP cũ trở nên reachable.

## Rule: Provider NIC vs Management NIC

> **Chỉ add provider NIC vào OVS bridge. KHÔNG BAO GIỜ add management NIC.**

| NIC          | Vai trò                              | Có thể add vào OVS? |
| ------------ | ------------------------------------ | ------------------- |
| eth0         | Management (SSH, API, inter-node)    | ❌ KHÔNG            |
| eth1/enp6s19 | Provider/tenant network (VM traffic) | ✅ OK               |

Trong Kolla-Ansible config (`globals.yml`):

```yaml
neutron_external_interface: "enp6s19" # Provider NIC — add vào br-ex
network_interface: "eth0" # Management NIC — KHÔNG touch
```

## Hệ Quả Thiết Kế

Incident này dẫn đến quyết định bỏ floating IP approach. Thay vì routing VM traffic qua management network, dùng static route từ `tools-1` qua `ctrl-1` đến provider network (`10.10.1.0/24`). Đơn giản hơn và không có nguy cơ loop.

## Connections

- [[permanent/openstack-provider-net-routing]] — decision sau incident
- [[permanent/kolla-ansible-deployment-patterns]] — globals.yml config
- [[permanent/openstack-external-network-mapping]] — br-ex architecture

## Sources

- [[literature/proxmox-dbaas-lab-day1-to-day4.5]]
