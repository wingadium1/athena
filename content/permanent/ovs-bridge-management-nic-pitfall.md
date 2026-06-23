---
title: "OVS Bridge Management NIC Pitfall"
aliases: ["OVS takeover", "ovs bridge ssh loss", "openstack provider bridge incident", "ovs bridge nic no ip"]
tags: [openstack, ovs, networking, incident, infrastructure]
created: 2026-04-10
updated: 2026-05-22
---

Một trong những mistakes nguy hiểm nhất khi cấu hình OpenStack provider network: thêm management NIC (eth0 / interface đang dùng cho SSH) vào OVS bridge. Kết quả là mất SSH ngay lập tức, không có warning.

## Tại Sao OVS "Nuốt" NIC

OVS hoạt động ở Layer 2. Khi bạn add một NIC vào OVS bridge (ví dụ `br-ex`), OVS trở thành L2 owner của NIC đó. Linux kernel không còn nhận L2 frames từ NIC nữa — OVS datapath intercepts tất cả. Điều này có nghĩa:

- IP address đang gán cho eth0 (`10.16.90.66`) trở nên unreachable từ bên ngoài
- SSH connections bị dropped ngay lập tức
- Ping từ management network không còn reply

Đây không phải bug — đây là cách OVS được thiết kế. Vấn đề là operator nhầm lẫn giữa "provider NIC" và "management NIC".

## Recovery Procedure

Khi đã mất SSH, cách duy nhất là dùng **out-of-band console** (Proxmox noVNC, AWS EC2 serial console, hoặc IPMI):

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

## RHOSO NIC Pattern B: OVS Slave Must Have No IP

Trong RHOSO 18, NIC assignment pattern B tách traffic thành 2 nhóm:

**Control Group** (bond0 — LACP, 802.3ad):
- ctlplane (native), storage (VLAN 20), internalapi (VLAN 30)

**Data Group** (OVS bridge `br-ex`):
- tenant (VLAN 40), provider (VLAN 100)
- Physical NIC enslaved to `br-ex`

**Critical rule**: Physical NIC enslaved to `br-ex` must have **NO IP address**:

```
✅ Correct:
br-ex (OVS bridge) — has IP (provider network gateway)
  └── ens1f0 (physical NIC) — NO IP, enslaved to br-ex

❌ Incorrect (causes VM unreachability):
br-ex (OVS bridge) — has IP
ens1f0 (physical NIC) — ALSO has IP ← dual routing path → asymmetric routing
```

Nếu physical NIC giữ IP, kernel tạo duplicate routing path → return traffic cho VM packets có thể exit qua NIC thay vì OVS bridge → VMs unreachable từ external networks.

## Cloud Nested-Virt Constraints (AWS)

Khi chạy OpenStack trên EC2 (nested virtualization), có thêm constraints:

- **eth0 must NEVER be enslaved to OVS bridge** — AWS ENI MAC rewrite cause OVS flow breakage
- **OVS flow workarounds needed** for AWS ENI behavior (MAC rewrite on outbound, GARP suppression)
- **VIP auto-migration blocked** — AWS OVS behavior ngăn VIP migration qua Gratuitous ARP (GARP)
- **OVS flows ephemeral on restart** — phải re-apply sau mỗi lần restart OVS

Đây là artifacts của nested virtualization, **không áp dụng cho bare-metal** nơi native L2 broadcast/GARP hoạt động bình thường.

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
- [[literature/openstack-dbaas-work-wiki]]
