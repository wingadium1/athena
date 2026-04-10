---
title: "Packer OpenStack Image Pipeline"
aliases: ["packer glance", "openstack image lifecycle", "packer openstack plugin"]
tags: [openstack, packer, glance, image-pipeline, infrastructure]
created: 2026-04-10
updated: 2026-04-10
---

Packer với OpenStack plugin cho phép build immutable VM images và push thẳng vào Glance. Pattern này tạo ra một image lifecycle có kiểm soát: build → validate → approve — thay vì import images thủ công và không có versioning.

## Packer Template Cấu Trúc

```hcl
packer {
  required_plugins {
    openstack = {
      source  = "github.com/hashicorp/openstack"
      version = "~> 1"
    }
  }
}

source "openstack" "ubuntu_pg16" {
  image_name       = "ubuntu-24.04-pg16-candidate"
  source_image_name = "ubuntu-24.04-base"
  flavor           = "m1.medium"
  networks         = ["provider-net-id"]
  use_floating_ip  = false          # Quan trọng: không cần floating IP
  communicator     = "ssh"
  ssh_username     = "ubuntu"
}

build {
  sources = ["source.openstack.ubuntu_pg16"]

  provisioner "shell" {
    scripts = ["scripts/install-postgresql.sh"]
  }
}
```

## `use_floating_ip = false`

Mặc định Packer tạo floating IP để SSH vào build VM. Trong môi trường không có floating IP (hoặc dùng static route như lab này), phải set:

```hcl
use_floating_ip = false
```

Packer sẽ dùng direct provider-net IP của build VM để SSH. Điều này yêu cầu build host (`tools-1`) có thể reach provider network trực tiếp — đúng với [[permanent/openstack-provider-net-routing]].

## Image Lifecycle: candidate → approved

Một pattern kiểm soát chất lượng image:

1. **Build**: Packer push image với name suffix `-candidate` vào Glance
2. **Validate**: Boot một VM từ candidate image, chạy smoke tests (PostgreSQL start, basic queries)
3. **Promote**: Rename image sang `-approved` sau khi validate pass
4. **Cleanup**: Xóa candidate image (hoặc retain với `archived` tag)

```bash
# Promote candidate to approved
openstack image set --name "ubuntu-24.04-pg16-approved" \
  $(openstack image show ubuntu-24.04-pg16-candidate -f value -c id)

# Tag với metadata
openstack image set --property status=approved \
  --property build-date=$(date +%Y-%m-%d) \
  <image-id>
```

## Glance Metadata Tagging

Tags trong Glance giúp Trove và automation tools filter đúng image:

```bash
openstack image set \
  --property db_type=postgresql \
  --property db_version=16 \
  --property os_version=24.04 \
  <image-id>
```

Trove sử dụng `image_tags` filter trong datastore config để tìm đúng image.

## Connections

- [[permanent/openstack-provider-net-routing]] — `use_floating_ip = false` cần static route
- [[permanent/openstack-trove-postgresql-ha]] — approved image được Trove dùng để tạo instances
- [[permanent/openstack-external-network-mapping]] — provider network topology

## Sources

- [[literature/proxmox-dbaas-lab-day1-to-day4.5]]
