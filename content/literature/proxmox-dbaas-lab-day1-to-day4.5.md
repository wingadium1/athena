---
title: "Proxmox DBaaS Lab — Day 1 to Day 4.5"
type: literature
source: "/Users/sonht2.gmo/git/openstack-101/lab/proxmox-dbaas-lab/"
author: "Research session — proxmox-dbaas-lab Day 1–4.5 command packs + live journal"
date-read: 2026-04-10
tags: [openstack, kolla-ansible, trove, proxmox, dbaas, infrastructure]
---

## Summary

Hands-on lab xây dựng một OpenStack DBaaS cluster từ đầu trên Proxmox, trải dài từ tạo VM template (Day 1) đến Trove PostgreSQL HA replication với failover (Day 4.5). Lab không chỉ là happy path — nó được thực thi thực tế với 5+ incidents được ghi chép đầy đủ trong live journal, bao gồm hostname resolution trap trong RabbitMQ, OVS bridge takeover làm mất SSH, và OVN logical IP vs Linux IP gây guest agent mất kết nối. Kết quả cuối: một Trove cluster đầy đủ tính năng với PostgreSQL primary/replica, Swift backup, và failover đã kiểm chứng.

## Key Ideas

- **Cloud-init version lock**: Kolla-Ansible 2024.2 yêu cầu Ubuntu 24.04 Noble, không phải 22.04 Jammy — phải tạo template đúng từ đầu để tránh phải rebuild toàn bộ
- **RAM là hard requirement**: `ctrl-1` cần tối thiểu 12GB; 8GB gây MariaDB bootstrap timeout âm thầm
- **Hostname resolution là Erlang trap**: Ubuntu cloud-init mặc định ghi `127.0.1.1 ctrl-1` vào `/etc/hosts` — Erlang (RabbitMQ) bind vào loopback thay vì management IP; fix = `manage_etc_hosts: false` + IP thật; phải restart `kolla_toolbox` sau fix (toolbox snapshot `/etc/hosts` lúc start)
- **Nova cell_v2**: Sau khi thêm compute node, hypervisor không tự xuất hiện — phải chạy `nova-manage cell_v2 discover_hosts --verbose`
- **OVS L2 takeover**: Thêm management NIC (eth0) vào OVS bridge làm mất SSH ngay lập tức vì OVS "sở hữu" NIC ở L2 — IP trên eth0 không còn reachable; recovery qua Proxmox noVNC console
- **Provider net routing không cần floating IP**: Static route từ `tools-1` qua `ctrl-1` đến `10.10.1.0/24` đủ cho infra tools; gateway IP phải ở trên `br-ex`, không trên raw `enp6s19`
- **OVN IP vs Linux IP**: `10.10.1.1` là OVN logical router IP — tồn tại trong OVS datapath, không reach Linux kernel; phải thêm `10.10.1.66/24` là real Linux IP trên `br-ex` để Trove guest agent có thể kết nối RabbitMQ và Keystone
- **RabbitMQ dual listener**: Trove guest agents connect từ provider-net nên RabbitMQ cần listen thêm trên `10.10.1.66:5672` (edit host-side `/etc/kolla/rabbitmq/rabbitmq.conf`, không phải container config)
- **Keystone DNAT**: `10.10.1.66:5000 → 10.16.90.50:5000` (VIP) để guest VMs reach Keystone qua provider-net
- **Swift mandatory cho Trove**: Backup và HA replication đều dùng Swift; loopback disk với GPT label `KOLLA_SWIFT_DATA` + XFS label `d0` là yêu cầu bắt buộc
- **Swap prerequisite**: ctrl-1 cần 4GB swap trước khi deploy Swift (15 containers thêm vào)
- **PostgreSQL replica security group**: Security group không tự attach vào replica instance — phải add manual qua Neutron port sau khi tạo
- **`trove-guestagent.conf` là immutable**: Config chỉ apply cho NEW instances; instances đang chạy không bị ảnh hưởng retroactively
- **Failover limitation**: Backup từ promoted standby (timeline 2) fails; phải backup từ primary mới (timeline 1) sau failover
- **Packer image lifecycle**: `candidate` → validation boot → `approved`; `use_floating_ip = false` cho môi trường không có floating IP

## Quotes

> "10.10.1.1 is the OVN logical router gateway IP — it exists inside the OVS datapath and is never handed up to the Linux kernel. That's why pinging it from ctrl-1 works (OVS responds) but nothing running on ctrl-1 can bind a socket to it."

> "manage_etc_hosts: false — This prevents cloud-init from writing 127.0.1.1 ctrl-1. Then your /etc/hosts entry 10.16.90.66 ctrl-1 becomes the only resolution path. RabbitMQ's Erlang runtime will now bind to the correct management IP."

> "kolla_toolbox takes a snapshot of /etc/hosts at container start. So even after you fix /etc/hosts on the host, the already-running toolbox container still has the old broken mapping."

## My Take

Lab này là một trong những nguồn học tốt nhất về OpenStack operations vì nó không giấu incidents. Ba bài học quan trọng nhất với mình:

1. OVN vs Linux IP boundary là một mental model quan trọng khi làm việc với OpenStack Neutron — rất nhiều "network không hiểu sao không work" problems quy về việc nhầm OVN logical IP với Linux real IP.
2. RabbitMQ + Erlang hostname resolution là một gotcha kinh điển mà documentation ít nói đến — lab này documented nó rất rõ.
3. `trove-guestagent.conf` immutability là một design decision của Trove — không phải bug — nhưng gây confusion lớn nếu không biết.

## Links

- [[permanent/kolla-ansible-deployment-patterns]]
- [[permanent/ovs-bridge-management-nic-pitfall]]
- [[permanent/openstack-provider-net-routing]]
- [[permanent/packer-openstack-image-pipeline]]
- [[permanent/openstack-trove-guest-agent-connectivity]]
- [[permanent/openstack-trove-postgresql-ha]]
- [[permanent/swift-single-node-setup-kolla]]
- [[permanent/openstack-neutron-overlay-protocols]]
- [[permanent/openstack-external-network-mapping]]
