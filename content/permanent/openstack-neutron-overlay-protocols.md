---
title: "OpenStack Neutron Overlay Protocols: GRE, VXLAN, GENEVE"
aliases: ["Neutron tunnel types", "OpenStack overlay", "GENEVE vs VXLAN OpenStack"]
tags: [networking, OpenStack, Neutron, overlay, GRE, VXLAN, GENEVE, cloud]
created: 2026-04-09
updated: 2026-04-09
---

OpenStack Neutron cung cấp **self-service networks** thông qua overlay (tunnel) protocols. Các protocols này cho phép create isolated L2 networks không bị giới hạn bởi 4094 VLAN IDs của physical infrastructure.

Docs chính thức: [Overlay (tunnel) protocols — Neutron docs](https://docs.openstack.org/neutron/2024.1/admin/intro-overlay-protocols.html)

## Ba loại overlay protocols

### 1. GRE (Generic Routing Encapsulation)

- **RFC**: Multiple RFCs (2784, 2890, etc.)
- **Transport**: IP Protocol 47 (IP-over-IP, không phải UDP)
- **Identifier**: 32-bit tunnel key (nhưng thực tế thường dùng ít hơn)
- **OpenStack support**: ML2 + Open vSwitch (không support OVN)
- **Đặc điểm**: Point-to-point tunnels. Không có built-in authentication mạnh. GRE là "foundation protocol" — đơn giản nhưng thiếu nhiều tính năng hiện đại.
- **ECMP limitation**: GRE không có port-based load balancing tự nhiên như UDP-based protocols. Underlay switches khó distribute traffic.
- **Trạng thái**: Ít được dùng trong deployments mới. VXLAN và GENEVE đã thay thế.

### 2. VXLAN (Virtual Extensible LAN)

- **RFC**: 7348 (2014)
- **Transport**: UDP port 4789
- **Identifier**: 24-bit VNI → 16,777,216 segments
- **OpenStack support**: ML2 + Open vSwitch + Linux Bridge + OVN (với caveats)
- **Đặc điểm**: MAC-in-UDP encapsulation. Phổ biến nhất, ecosystem rộng nhất.
- **Xem chi tiết**: [[permanent/vxlan-vni-scalability]]

### 3. GENEVE (Generic Network Virtualization Encapsulation)

- **RFC**: 8926 (2020)
- **Transport**: UDP port 6081
- **Identifier**: 24-bit VNI (giống VXLAN) → 16,777,216 segments
- **OpenStack support**: ML2 + Open vSwitch + OVN (**default cho OVN**)
- **Đặc điểm**: **Extensible TLV-based header** — đây là điểm khác biệt cốt lõi

#### Tại sao GENEVE vượt trội hơn VXLAN về metadata

VXLAN header là **fixed 8 bytes**, không có field nào cho metadata:

```
VXLAN Header (8 bytes, fixed):
[Flags 8b][Reserved 24b][VNI 24b][Reserved 8b]
```

GENEVE header là **variable-length**, có TLV (Type-Length-Value) options:

```
GENEVE Header (16 bytes base + variable options):
[Version 2b][Options Length 6b][Flags][Protocol Type 16b]
[VNI 24b][Reserved 8b]
[TLV Options... (variable)]
```

Điều này có nghĩa là:
- **In-band metadata**: Security policy tags, region IDs, telemetry data có thể được truyền cùng packet
- **Service chaining**: NSH (Network Service Header) có thể được embed trong GENEVE options
- **Transport security**: Có thể mang cryptographic material
- **Extensibility**: Vendors có thể define custom TLV option classes qua IANA — không cần protocol version bump

RFC 8926 mô tả: *"The data plane is generic and extensible enough to support current and future control planes."* — VXLAN bị locked vào một control plane model (multicast flood-and-learn), GENEVE thì không.

## So sánh tổng hợp

| Feature | VLAN (802.1Q) | GRE | VXLAN | GENEVE |
|---|---|---|---|---|
| RFC | IEEE 802.1Q | 2784/2890 | 7348 | 8926 |
| Identifier size | 12-bit | 32-bit key | **24-bit VNI** | **24-bit VNI** |
| Max segments | 4,094 | ~4B | ~16.7M | ~16.7M |
| Transport | L2 tag | IP (proto 47) | UDP:4789 | UDP:6081 |
| ECMP friendly | N/A | Poor | **Yes** | **Yes** |
| Header size | +4 bytes | +4 bytes | +50 bytes | +60+ bytes (variable) |
| Extensible metadata | No | No | **No** (fixed) | **Yes** (TLV) |
| OVN support | Yes | **No** | Partial* | **Yes (default)** |
| Transport security | N/A | Weak | No | Yes (via TLV) |
| Service chaining | No | No | No | Yes |
| In-band telemetry | No | No | No | Yes |
| Kernel requirement | N/A | Any | Any | ≥ 3.18 |

_*OVN + VXLAN: identifier bits reduced to 12-bit internally, limiting to 4096 networks and 4096 ports/network_

## OVN default: GENEVE

Kể từ khi OVN (Open Virtual Network) trở thành preferred backend cho Neutron, **GENEVE là default tunnel type** trong ML2/OVN deployments. OVN sử dụng GENEVE options để truyền metadata cần thiết cho internal forwarding logic. VXLAN không đủ header space cho điều này.

Từ Neutron docs (OVN install manual):
> *"Because of limited space in VXLAN VNI to pass over the needed information that requires OVN to identify a packet, the header size to contain the segmentation ID is reduced to 12 bits, that allows a maximum number of 4096 networks."*

## Trạng thái năm 2026

- **GENEVE + OVN**: Architecture được recommend cho deployments mới. OpenStack Gazpacho (2026.1) tiếp tục đầu tư vào OVN driver (thêm BGP support, north-south routing cho SR-IOV ports).
- **VXLAN + OVS**: Vẫn mature, phổ biến, đặc biệt với legacy deployments và khi cần Linux Bridge support.
- **GRE**: Không support OVN. Ít được dùng trong deployments mới. Chủ yếu còn trong legacy hoặc khi cần specific point-to-point tunneling.
- **IETF NVO3 WG**: Đã chọn GENEVE làm "common encapsulation" cho network virtualization overlays.

## Connections

- [[permanent/vlan-4094-limit]] — vấn đề mà các protocols này giải quyết
- [[permanent/vxlan-vni-scalability]] — deep dive vào VXLAN VNI
- [[permanent/openstack-ml2-overlay-config]] — cách configure trong ml2_conf.ini

## Sources

- [[literature/openstack-neutron-overlay-research-2026-04]]
