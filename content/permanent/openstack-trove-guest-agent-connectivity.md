---
title: "OpenStack Trove Guest Agent Connectivity (OVN Context)"
aliases: ["trove guest agent", "ovn logical ip vs linux ip", "trove rabbitmq connectivity"]
tags: [openstack, trove, ovn, networking, dbaas, infrastructure]
created: 2026-04-10
updated: 2026-04-10
---

Trove guest agent (chạy bên trong database VM) cần kết nối RabbitMQ và Keystone trên control plane. Trong môi trường dùng OVN (Open Virtual Network), có một OVN logical IP vs Linux IP boundary mà nếu không hiểu sẽ khiến guest agent mất kết nối hoàn toàn.

## OVN Logical IP vs Linux IP — Core Problem

Khi bạn tạo một Neutron router với `--external-gateway` trên provider network, OVN tạo ra một logical router gateway port với IP ví dụ `10.10.1.1`. IP này:

- **Tồn tại trong OVS datapath** — OVN flow table intercepts packets destined cho `10.10.1.1`
- **Không tồn tại ở Linux kernel level** — không có interface nào trên control node có IP `10.10.1.1`
- **Ping từ VM works** — OVN flow responds to ARP/ICMP
- **TCP connections fail** — không có socket nào binding trên `10.10.1.1` ở Linux userspace

Kết quả: Guest agent có thể ping gateway, nhưng không thể connect TCP đến RabbitMQ (`5672`) hay Keystone (`5000`).

## Fix: Real Linux IP trên br-ex

Giải pháp là thêm một real Linux IP trên `br-ex` (OVS internal port) — IP này visible đến Linux kernel, khác với OVN logical IP:

```bash
# Trên ctrl-1
ip addr add 10.10.1.66/24 dev br-ex

# Persistent (Netplan)
# /etc/netplan/60-br-ex.yaml
network:
  version: 2
  bridges:
    br-ex:
      addresses: [10.10.1.66/24]
```

Giờ `10.10.1.66` là một real Linux IP — services có thể bind socket trên đó.

## RabbitMQ Dual Listener

Mặc định RabbitMQ trong Kolla-Ansible chỉ listen trên management network (`10.16.90.x`). Guest agents ở provider network (`10.10.1.x`) không thể reach nó.

Fix: thêm listener thứ 2 trên `10.10.1.66`. Edit **host-side** config (không phải container config — container chỉ mount file này):

```bash
# /etc/kolla/rabbitmq/rabbitmq.conf
listeners.tcp.1 = 10.16.90.50:5672   # Management network (existing)
listeners.tcp.2 = 10.10.1.66:5672    # Provider network (new)
```

Restart RabbitMQ container sau khi edit:

```bash
docker restart rabbitmq
```

## Keystone DNAT

Guest agents cần reach Keystone để lấy token. Keystone bind trên VIP (`10.16.90.50:5000`) — không reachable từ provider network. Setup DNAT:

```bash
# Trên ctrl-1
iptables -t nat -A PREROUTING \
  -d 10.10.1.66 -p tcp --dport 5000 \
  -j DNAT --to-destination 10.16.90.50:5000

# Persist
iptables-save > /etc/iptables/rules.v4
```

Trong `trove-guestagent.conf`:

```ini
[keystone_authtoken]
auth_url = http://10.10.1.66:5000
```

## SNAT cho Internet Access (Guest VMs)

Guest VMs cần internet để apt-get, etc. Setup SNAT:

```bash
# Trên ctrl-1
iptables -t nat -A POSTROUTING \
  -s 10.10.1.0/24 -o eth0 \
  -j MASQUERADE
```

Và OVN router static route:

```bash
openstack router set --route destination=0.0.0.0/0,gateway=10.10.1.66 <router-id>
```

## Subnet Gateway phải dùng br-ex IP

Khi tạo provider subnet trong Neutron, gateway phải trỏ đến real Linux IP (`10.10.1.66`), không phải OVN logical IP (`10.10.1.1`):

```bash
openstack subnet set --gateway 10.10.1.66 provider-subnet
```

Nếu dùng `10.10.1.1` (OVN), DNS và internet trong guest VM sẽ không hoạt động vì SNAT rule trên Linux kernel không fire được.

## TCP Blackhole Fix (OVN RST)

Khi guest VMs cố gắng kết nối port mà OVN biết không có service ở đó (vì OVN flow không forward đến Linux), OVN trả về RST ngay lập tức. Để tránh điều này, đảm bảo services thực sự bind trên các IPs được advertise.

## Connections

- [[permanent/openstack-provider-net-routing]] — br-ex IP setup
- [[permanent/ovs-bridge-management-nic-pitfall]] — tại sao br-ex, không phải raw NIC
- [[permanent/openstack-trove-postgresql-ha]] — guest agent là prerequisite cho Trove HA
- [[permanent/kolla-ansible-deployment-patterns]] — Kolla network config
- [[permanent/openstack-neutron-overlay-protocols]] — OVN vs OVS context

## Sources

- [[literature/proxmox-dbaas-lab-day1-to-day4.5]]
