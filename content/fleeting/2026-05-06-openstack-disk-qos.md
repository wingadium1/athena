---
title: "OpenStack Disk QoS — Token Bucket cho Cinder và Ephemeral"
type: fleeting
date: 2026-05-06
tags: [infrastructure, openstack, storage, qos]
---

Đang tìm hiểu về disk IOPS QoS trong OpenStack — hai luồng chính:

**Cinder Volume QoS** (production-grade, có burst):

- Cấu hình qua `openstack volume qos create` → associate với Volume Type
- `consumer=front-end` → Nova/libvirt enforce qua `<iotune>` XML — hoạt động với mọi backend (kể cả Ceph)
- Có đầy đủ token bucket: sustained rate (`total_iops_sec`) + burst ceiling (`total_iops_sec_max`) + bucket size (`size_iops_sec`)

**Nova Ephemeral Disk QoS** (basic, không có burst):

- Cấu hình qua flavor extra specs: `quota:disk_[read|write|total]_[bytes|iops]_sec`
- Chỉ 6 keys, không có `*_max` hay `size_iops_sec`
- Nova docs tự nhận là "poorly tested and poorly maintained"

**Key insight chưa rõ**: Tại sao ephemeral disk không được bổ sung burst params dù libvirt/QEMU hỗ trợ đầy đủ?

→ Sẽ promote sang permanent notes sau khi hiểu rõ hơn qua thực tế.

## Links

- [[literature/openstack-disk-qos-research-2026-05]]
