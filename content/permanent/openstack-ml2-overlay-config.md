---
title: "OpenStack ML2 Plugin: Cấu hình Overlay Networks"
aliases: ["ML2 overlay config", "ml2_conf.ini overlay", "Neutron VXLAN config", "Neutron GENEVE config"]
tags: [networking, OpenStack, Neutron, ML2, configuration, VXLAN, GENEVE]
created: 2026-04-09
updated: 2026-04-09
---

ML2 (Modular Layer 2) là core plugin của Neutron. Nó phân tách hai chiều:
- **Type drivers**: "How a network is technically realized" (Flat, VLAN, GRE, VXLAN, GENEVE)
- **Mechanism drivers**: "How it's applied to the actual infrastructure" (Open vSwitch, OVN, SRIOV, MacVTap)

Docs: [ML2 Plug-in — Neutron latest](https://docs.openstack.org/neutron/latest/admin/config-ml2.html)

## Driver support matrix

| Type Driver | Open vSwitch | OVN | Linux Bridge |
|---|---|---|---|
| Flat | ✅ | ✅ | ✅ |
| VLAN | ✅ | ✅ | ✅ |
| VXLAN | ✅ | ✅ (OVN ≥ 20.09, caveats*) | ✅ |
| GRE | ✅ | **❌** | **❌** |
| GENEVE | ✅ | **✅ (default)** | **❌** |

_*OVN + VXLAN: internal 12-bit limit, max 4096 networks_

**L2 population** (BUM traffic optimization): chỉ hoạt động với VXLAN, GRE, GENEVE + Open vSwitch.

## ml2_conf.ini — Configuration Reference

### Base ML2 config (bất kỳ overlay nào)

```ini
# /etc/neutron/plugins/ml2/ml2_conf.ini

[ml2]
# Load all desired type drivers
type_drivers = flat,vlan,vxlan,gre,geneve

# Default tenant network type (first in list = default)
tenant_network_types = vxlan    # hoặc geneve

# Mechanism driver
mechanism_drivers = openvswitch,l2population  # OVS
# hoặc:
mechanism_drivers = ovn                        # OVN (preferred mới)

# Extension drivers
extension_drivers = port_security
```

### VXLAN configuration

```ini
[ml2_type_vxlan]
# Range of VNI IDs cho tenant networks
# Max usable: 1–16,777,215 (24-bit)
vni_ranges = 1:1000         # 1000 tenant networks
# hoặc large deployment:
vni_ranges = 1:100000

# Optional: multicast group (không dùng với OVS agent)
# vxlan_group = 239.1.1.1
```

Trên compute/network nodes (openvswitch_agent.ini):

```ini
[ovs]
local_ip = OVERLAY_INTERFACE_IP  # IP của overlay network interface

[agent]
tunnel_types = vxlan
l2_population = True   # Bật để tránh multicast BUM traffic
```

### GENEVE configuration (ML2/OVN)

```ini
[ml2]
mechanism_drivers = ovn
type_drivers = local,flat,vlan,geneve
tenant_network_types = geneve
extension_drivers = port_security
overlay_ip_version = 4   # hoặc 6 cho IPv6 overlay

[ml2_type_geneve]
# Range — số lượng networks = (max - min + 1)
vni_ranges = 1:65536     # 65,536 tenant networks

# Max GENEVE header size (bytes)
# Default 30 = base GENEVE header only (quá nhỏ cho OVN!)
# OVN requires >= 38
max_header_size = 38
```

> [!warning] Default max_header_size = 30 là sai cho OVN
> OVN cần ít nhất 38 bytes. Nếu để default 30, tunnels sẽ không hoạt động đúng. Đây là một footgun phổ biến khi setup ML2/OVN.

Trên compute nodes (OVN encap type):

```bash
# Bật GENEVE (và optionally VXLAN cho VTEP gateways)
ovs-vsctl set open . external-ids:ovn-encap-type=geneve,vxlan

# Set local overlay IP
ovs-vsctl set open . external-ids:ovn-encap-ip=OVERLAY_IP
```

### GRE configuration

```ini
[ml2_type_gre]
# Range của GRE tunnel IDs
tunnel_id_ranges = 1:1000
```

Lưu ý: GRE không cần cấu hình đặc biệt thêm trên server side. Nhưng **không support OVN**, chỉ dùng với Open vSwitch.

### VLAN configuration (để so sánh)

```ini
[ml2_type_vlan]
# physnet:min_vlan:max_vlan
network_vlan_ranges = physnet1,physnet2:1001:2000
# → physnet1: admin-only (không có range)
# → physnet2: users có thể tạo VLANs 1001-2000
```

## L2 Population — Critical for scale

L2 population là mechanism driver đặc biệt, phải dùng cùng OVS:

```ini
[ml2]
mechanism_drivers = openvswitch,l2population
```

Không có L2 pop: VXLAN flood BUM (Broadcast, Unknown unicast, Multicast) traffic đến **tất cả** VTEPs trong cluster. Với hàng trăm compute nodes, điều này sinh ra storm.

L2 pop: Neutron tự động populate VXLAN forwarding tables từ API. Mỗi compute node chỉ gửi traffic đến VTEP đúng — unicast thay vì multicast/flood.

## Production recommendations (large-scale)

1. **Dùng GENEVE + OVN** cho deployments mới. OVN native L3 eliminates dedicated network nodes, provides distributed routing và DHCP.
2. **Dùng VXLAN + OVS + L2 population** nếu cần Linux Bridge support hoặc legacy compatibility. **Bắt buộc bật l2population** ở scale.
3. **Tránh GRE** trong deployments mới: không support OVN, poor ECMP, ít ecosystem support.
4. **Jumbo frames**: Physical interfaces trong overlay network nên support MTU 9000 để accommodate overhead mà không cần fragment.
5. **Dedicated overlay network**: Tách overlay traffic khỏi management và provider networks bằng dedicated NIC/VLAN.
6. **OVN BGP** (OpenStack 2026.1+): Với deployments cần BGP route advertisement cho tenant networks, ML2/OVN hiện đã support.

## Connections

- [[permanent/vlan-4094-limit]] — vấn đề root cause
- [[permanent/vxlan-vni-scalability]] — chi tiết về VXLAN/VNI
- [[permanent/openstack-neutron-overlay-protocols]] — so sánh các protocols

## Sources

- [[literature/openstack-neutron-overlay-research-2026-04]]
