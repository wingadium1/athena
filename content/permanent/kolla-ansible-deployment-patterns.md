---
title: "Kolla-Ansible Multi-Node Deployment Patterns"
aliases: ["kolla ansible", "kolla multinode", "kolla-ansible pitfalls"]
tags: [openstack, kolla-ansible, deployment, infrastructure]
created: 2026-04-10
updated: 2026-04-10
---

Kolla-Ansible đơn giản hóa việc deploy OpenStack bằng cách chạy mọi service trong Docker containers, với Ansible quản lý configuration và orchestration. Nhưng một số implicit assumptions của nó — về hostname resolution, RAM, và service lifecycle — có thể gây ra failures mà rất khó debug nếu không biết trước.

## Hostname Resolution Trap (RabbitMQ / Erlang)

Ubuntu cloud-init mặc định ghi `127.0.1.1 <hostname>` vào `/etc/hosts`. Erlang (runtime của RabbitMQ) dùng hostname để bind network interface, và khi nó resolve ra `127.0.1.1`, RabbitMQ sẽ bind vào loopback thay vì management IP. Các nodes khác trong cluster không thể kết nối được.

Fix:

```yaml
# cloud-init user-data
manage_etc_hosts: false
```

Sau đó, manually add dòng đúng vào `/etc/hosts` trên tất cả nodes:

```
10.16.90.66 ctrl-1
```

**Critical**: `kolla_toolbox` container snapshot `/etc/hosts` lúc start. Nếu bạn fix `/etc/hosts` sau khi toolbox đang chạy, phải restart container này:

```bash
docker restart kolla_toolbox
```

## RAM Requirements

| Node      | Minimum RAM | Lý do                                   |
| --------- | ----------- | --------------------------------------- |
| ctrl-1    | 12GB        | MariaDB Galera + RabbitMQ + tất cả APIs |
| compute-x | 4GB         | nova-compute + libvirt + neutron-agent  |
| tools-1   | 4GB         | Spring Boot + PostgreSQL + Redis        |

8GB trên ctrl-1 gây MariaDB bootstrap timeout âm thầm — cluster bị stuck không có error message rõ ràng.

## Nova Cell Discovery

Sau khi thêm compute node mới vào cluster, hypervisor **không tự xuất hiện** trong `openstack hypervisor list`. Phải chạy thủ công:

```bash
docker exec nova_api nova-manage cell_v2 discover_hosts --verbose
```

Đây không phải bug — đây là design của Nova cell architecture. Cell_v2 cần explicit discovery để register hypervisors mới.

## Deploy Flow (Multi-Node)

```
kolla-ansible -i multinode bootstrap-servers
kolla-ansible -i multinode prechecks
kolla-ansible -i multinode pull
kolla-ansible -i multinode deploy
kolla-ansible post-deploy
```

Sau `post-deploy`, source `admin-openrc.sh` và chạy `openstack complete` để verify.

## OS Version Lock

Kolla-Ansible 2024.2 yêu cầu **Ubuntu 24.04 Noble**, không phải 22.04 Jammy. Dùng cloud image `ubuntu-24.04-server-cloudimg-amd64.img` để tạo template. Nếu dùng sai version, một số services sẽ không start hoặc có dependency conflicts.

## Connections

- [[permanent/openstack-neutron-overlay-protocols]] — overlay config trong globals.yml
- [[permanent/ovs-bridge-management-nic-pitfall]] — incident xảy ra sau khi deploy thành công
- [[permanent/swift-single-node-setup-kolla]] — deploy Swift thêm ~15 containers, cần swap

## Sources

- [[literature/proxmox-dbaas-lab-day1-to-day4.5]]
