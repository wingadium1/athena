---
title: "OpenStack Trove PostgreSQL HA Replication"
aliases: ["trove ha", "trove postgresql replication", "trove failover", "trove replica"]
tags: [openstack, trove, postgresql, ha, dbaas, replication, infrastructure]
created: 2026-04-10
updated: 2026-04-10
---

Trove cung cấp managed database service trên OpenStack. PostgreSQL HA replication trong Trove dựa trên streaming replication với Swift làm transport layer cho backup và initial replica seeding — không phải direct network replication.

## Architecture Overview

```
Primary Instance (trove-pg-primary)
    │
    │  Swift backup → restore → streaming replication
    ▼
Replica Instance (trove-pg-replica)
    │
    └── openstack database instance promote → role swap
```

## Required Components

Trove + HA cần các Kolla services sau:

- `trove_api`, `trove_conductor`, `trove_taskmanager`
- `heat_api`, `heat_api_cfn`, `heat_engine` — Heat dùng cho instance orchestration
- Swift — mandatory cho backup và replica seeding (xem [[permanent/swift-single-node-setup-kolla]])

## Tạo Primary Instance

```bash
openstack database instance create trove-pg-primary \
  --flavor m1.medium \
  --size 10 \
  --nic net-id=<provider-net-id> \
  --datastore postgresql \
  --datastore-version 16 \
  --databases mydb \
  --users admin:secret
```

## Tạo Replica

```bash
openstack database instance create trove-pg-replica \
  --flavor m1.medium \
  --size 10 \
  --nic net-id=<provider-net-id> \
  --datastore postgresql \
  --datastore-version 16 \
  --replica-of <primary-instance-id>
```

Quá trình tạo replica:

1. Trove tạo Swift backup từ primary
2. Restore backup vào replica VM
3. Cấu hình streaming replication

Có thể mất 10–20 phút tùy kích thước data.

## Security Group Gotcha

> **Security group KHÔNG tự attach vào replica instance.**

Sau khi tạo replica, phải manually add security group qua Neutron port:

```bash
# Tìm port của replica
REPLICA_IP=$(openstack database instance show trove-pg-replica -f value -c ip)
PORT_ID=$(openstack port list --fixed-ip ip-address=$REPLICA_IP -f value -c id)

# Add security group
openstack port set --security-group <sg-id> $PORT_ID
```

Nếu bỏ qua bước này, connections từ primary đến replica sẽ bị block bởi default security group.

## backup_docker_image Phải Explicit

Trong `trove-guestagent.conf`, phải chỉ định rõ image backup:

```ini
[postgresql]
backup_docker_image = quay.io/openstack.trove/db-backup-postgresql:12
```

Nếu để trống hoặc dùng default, backup sẽ fail với cryptic error về image pull.

## trove-guestagent.conf là Immutable

Config trong `trove-guestagent.conf` chỉ apply cho **NEW instances** tạo sau khi apply config. Instances đang chạy không bị ảnh hưởng. Đây là design decision, không phải bug.

Hệ quả: Nếu bạn fix một config issue, bạn phải xóa và tạo lại instance, không phải restart.

## Failover

```bash
# Promote replica thành primary mới
openstack database instance promote <replica-instance-id>
```

Quá trình:

1. Replica ngắt replication
2. Replica promote thành standalone primary
3. Old primary (nếu còn alive) trở thành replica mới — hoặc detach nếu unreachable

## Failover Limitation: Timeline 2 Backup

Sau khi promote, PostgreSQL replica trở thành một new timeline (timeline 2). Backup từ promoted primary (timeline 2) **fails** với lỗi về timeline inconsistency.

Workaround: Tạo một fresh primary instance và backup từ đó.

```
Timeline 1: original primary → original replica
Timeline 2: promoted replica (new primary) ← backup fails từ đây
```

## Verify Replication

```bash
# Trên primary VM
psql -c "SELECT * FROM pg_stat_replication;"

# Expect: 1 row với state = streaming
```

## Connections

- [[permanent/openstack-trove-guest-agent-connectivity]] — prerequisite: guest agent phải kết nối được
- [[permanent/swift-single-node-setup-kolla]] — Swift mandatory cho backup/replication
- [[permanent/packer-openstack-image-pipeline]] — image được dùng để tạo Trove instances
- [[permanent/kolla-ansible-deployment-patterns]] — Kolla deploy Trove components

## Sources

- [[literature/proxmox-dbaas-lab-day1-to-day4.5]]
