---
title: "Swift Single-Node Setup (Kolla-Ansible)"
aliases: ["swift kolla", "openstack swift single node", "swift loopback disk"]
tags: [openstack, swift, kolla-ansible, object-storage, infrastructure]
created: 2026-04-10
updated: 2026-04-10
---

Swift là OpenStack object storage service, mandatory cho Trove backup và HA replication. Setup single-node Swift với Kolla-Ansible có một số gotchas về disk labeling, ring configuration, và swap prerequisite.

## Swap Prerequisite

Swift deploy thêm ~15 containers vào ctrl-1. Trước khi deploy, ctrl-1 cần ít nhất 4GB swap:

```bash
# Kiểm tra hiện tại
free -h

# Tạo swap file 4GB
fallocate -l 4G /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile

# Persistent
echo '/swapfile none swap sw 0 0' >> /etc/fstab
```

Nếu không có đủ swap, một số Swift containers sẽ OOMKill.

## Loopback Disk Setup

Swift cần dedicated disk(s) với specific GPT và filesystem labels. Trong lab (không có spare disk), dùng loopback:

```bash
# Tạo loopback file
truncate -s 10G /srv/swift-disk

# Attach loopback device
LOOP=$(losetup -f)
losetup $LOOP /srv/swift-disk

# Tạo GPT partition với label KOLLA_SWIFT_DATA
sgdisk -n 1:0:0 -t 1:8300 -c 1:"KOLLA_SWIFT_DATA" $LOOP

# Format XFS với label d0
mkfs.xfs -L d0 ${LOOP}p1

# Mount
mkdir -p /srv/node/d0
mount -L d0 /srv/node/d0
```

### Tại Sao Labels Quan Trọng

Kolla-Ansible tìm Swift disks qua **GPT partition label** (`KOLLA_SWIFT_DATA`) và **filesystem label** (`d0`, `d1`, ...). Đây là cơ chế auto-discovery — không cần hardcode device paths trong config.

```yaml
# globals.yml
swift_devices_match_mode: "strict"
swift_devices_name: "KOLLA_SWIFT_DATA"
```

## Systemd Mount Persistence

Loopback devices không persist qua reboot. Cần systemd service:

```ini
# /etc/systemd/system/swift-loopback.service
[Unit]
Description=Swift loopback disk
Before=docker.service

[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=/bin/bash -c '\
  LOOP=$(losetup -f); \
  losetup $LOOP /srv/swift-disk; \
  mount -L d0 /srv/node/d0'
ExecStop=/bin/bash -c '\
  umount /srv/node/d0; \
  losetup -d $(losetup -j /srv/swift-disk -O NAME --noheadings)'

[Install]
WantedBy=multi-user.target
```

```bash
systemctl enable swift-loopback
```

## Ring Builder (Single-Node)

Swift rings định nghĩa data distribution. Single-node setup dùng `replicas=1`:

```bash
cd /etc/kolla/config/swift

# Object ring
swift-ring-builder object.builder create 10 1 1
swift-ring-builder object.builder add --region 1 --zone 1 \
  --ip 10.16.90.66 --port 6200 --device d0 --weight 100
swift-ring-builder object.builder rebalance

# Container ring
swift-ring-builder container.builder create 10 1 1
swift-ring-builder container.builder add --region 1 --zone 1 \
  --ip 10.16.90.66 --port 6201 --device d0 --weight 100
swift-ring-builder container.builder rebalance

# Account ring
swift-ring-builder account.builder create 10 1 1
swift-ring-builder account.builder add --region 1 --zone 1 \
  --ip 10.16.90.66 --port 6202 --device d0 --weight 100
swift-ring-builder account.builder rebalance
```

Tham số `create <part_power> <replicas> <min_part_hours>`:

- `part_power=10` → 2^10 = 1024 partitions
- `replicas=1` → single copy (lab only — production cần ≥3)

## Verify Swift

```bash
# Upload test object
echo "hello swift" | openstack object create test-container hello.txt

# Download
openstack object save test-container hello.txt

# Verify Trove có thể dùng Swift
openstack datastore backup create <instance-id> test-backup
```

## Connections

- [[permanent/openstack-trove-postgresql-ha]] — Swift là mandatory dependency
- [[permanent/kolla-ansible-deployment-patterns]] — Kolla deploy Swift cùng với globals.yml

## Sources

- [[literature/proxmox-dbaas-lab-day1-to-day4.5]]
