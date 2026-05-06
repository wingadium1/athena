---
title: "OpenStack Disk QoS — Cinder Volume QoS và Nova Ephemeral Disk Throttling"
type: literature
source: "Research session — Cinder 2026.1 docs, Nova Rocky spec, OpenStackClient docs, libvirt/QEMU iotune reference"
author: "Research session — Cinder 2026.1 docs, Nova Rocky spec (specs.openstack.org), OpenStackClient docs, nova/virt/libvirt/config.py"
date-read: 2026-05-06
tags: [infrastructure, openstack, storage, qos, iops]
---

## Summary

OpenStack cung cấp hai cơ chế kiểm soát disk I/O hoàn toàn tách biệt: **Cinder QoS specs** (cho volumes, đầy đủ token bucket với burst) và **Nova flavor extra specs** (cho ephemeral disk, chỉ có sustained rate). Cả hai đều cuối cùng enforce qua libvirt `<iotune>` XML trên compute host — nhưng chỉ Cinder QoS mới expose burst params mà QEMU hỗ trợ. Consumer `front-end` là lựa chọn an toàn nhất vì hoạt động với mọi storage backend bất kể driver.

## Key Ideas

- **Token bucket trong Cinder QoS**: `total_iops_sec` = refill rate (sustained), `total_iops_sec_max` = peak drain rate (burst ceiling), `size_iops_sec` = bucket capacity (số I/O ops). Burst duration ≈ `size_iops_sec / (max - sustained)`
- **`consumer=front-end`** enforce tại QEMU layer — Nova đọc QoS spec từ volume `connection_info` khi attach và inject vào libvirt domain XML. Không cần restart VM, apply ngay khi hot-attach
- **`consumer=back-end`** pass spec tới Cinder driver — Nova bỏ qua hoàn toàn. Chỉ dùng nếu storage array hỗ trợ và driver implement QoS API riêng
- **Ephemeral disk QoS giới hạn**: 6 flavor keys (`quota:disk_[read|write|total]_[bytes|iops]_sec`), không có burst. Nova docs ghi rõ "poorly tested and poorly maintained". Burst params tồn tại ở libvirt/QEMU nhưng không được expose qua Nova flavor
- **Capacity-based QoS**: `total_iops_sec_per_gb` scale theo volume size, kết hợp với `total_iops_sec_max` để tạo floor/ceiling policy
- **Mutual exclusion**: không dùng `total_*` cùng với `read_*`/`write_*` cho cùng metric trong một spec
- **Verify**: `virsh dumpxml <instance> | grep -A 15 "<iotune>"` — xem trực tiếp params đã được inject

## Quotes

> "Front-end QoS settings are only supported when using the libvirt driver" — Nova Rocky release notes

> Nova docs về ephemeral disk QoS: _"poorly tested and poorly maintained"_

> Rocky blueprint: _"No QoS specs are provided for local drives provided directly by Nova"_ — burst params chỉ cho Cinder volumes

## My Take

_(Chưa có — đang trong giai đoạn tìm hiểu, chưa deploy thực tế)_

## Links

- [[fleeting/2026-05-06-openstack-disk-qos]]

## Sources

- [Cinder 2026.1 Basic Volume QoS](https://docs.openstack.org/cinder/2026.1/admin/basic-volume-qos.html)
- [Cinder 2026.1 Capacity-based QoS](https://docs.openstack.org/cinder/2026.1/admin/capacity-based-qos.html)
- [Nova Rocky spec — Enhanced KVM Storage QoS](https://specs.openstack.org/openstack/nova-specs/specs/rocky/implemented/enhanced-kvm-storage-qos.html)
- [OpenStackClient volume qos reference](https://docs.openstack.org/python-openstackclient/2025.1/cli/command-objects/volume-qos.html)
- [nova/virt/libvirt/config.py — LibvirtConfigGuestDisk](https://github.com/openstack/nova/blob/master/nova/virt/libvirt/config.py)
- [nova/api/validation/extra_specs/quota.py](https://github.com/openstack/nova/blob/master/nova/api/validation/extra_specs/quota.py)
