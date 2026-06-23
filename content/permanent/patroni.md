---
title: "Patroni"
aliases: ["patroni ha", "patroni failover", "patroni etcd"]
tags: [postgresql, ha, patroni, distributed-systems, infrastructure, database]
created: 2026-04-10
updated: 2026-05-22
---

Patroni là một HA agent cho PostgreSQL, chạy trên mỗi database node và chịu trách nhiệm duy nhất: đảm bảo cluster luôn có đúng một primary. Nó không tự mình quyết định — nó dùng một distributed consensus store (thường là etcd) để đồng thuận với các nodes khác về trạng thái cluster, tránh split-brain.

## Cơ chế hoạt động

Patroni dùng **Raft consensus** thông qua etcd (hoặc Consul, ZooKeeper). Mỗi node Patroni liên tục:

1. Heartbeat vào etcd để giữ leader lease
2. Nếu primary mất heartbeat (TTL hết hạn), etcd xóa lock
3. Các standby nodes race để acquire lock mới — winner promote thành primary
4. Node thắng update cluster state trong etcd; các node còn lại đọc state và tự cấu hình làm replica

Đây là automatic failover không cần human intervention. Thời gian failover thường 10–30 giây tùy cấu hình `ttl` và `loop_wait`.

## Production Gotchas

Bốn gotcha quan trọng phát hiện qua validation thực tế trên RHOSO:

### 1. `arping` callback phải dùng `nohup ... & disown`

Patroni giết tất cả callback processes vẫn đang chạy sau `loop_wait` (mặc định 10s). Nếu `arping` cho VIP failover callback chạy lâu hơn 10s, nó bị kill trước khi hoàn thành:

```bash
# ❌ Sai — bị Patroni kill sau 10s
arping -c 5 -I eth0 <VIP>

# ✅ Đúng — chạy background, không bị kill
nohup arping -c 5 -I eth0 <VIP> & disown
```

### 2. `allowed-address-pairs` cho VIP phải set trên TẤT CẢ 3 DB VM ports

Neutron security group behavior: nếu VIP chỉ được allowed trên port của primary, traffic từ primary gửi với source VIP sẽ bị drop khi đến replica. **Phải set `allowed-address-pairs` trên cả 3 Neutron ports của DB VMs** — nếu không, traffic từ leader mới bị silently drop.

### 3. Callbacks nằm dưới `postgresql:` section (không phải root level)

```yaml
# ❌ Sai
callbacks:
  on_role_change: /path/to/callback.sh

# ✅ Đúng
postgresql:
  callbacks:
    on_role_change: /path/to/callback.sh
```

### 4. `systemctl restart patroni` — SIGHUP không reload callbacks

Thay đổi callback config yêu cầu **full restart** của Patroni service. `SIGHUP` hoặc `systemctl reload` không reload callback definitions — chỉ reload PostgreSQL parameters.

## etcd v2 API Dependency

`python-etcd` (thư viện Python client cho etcd) yêu cầu etcd v2 API — phải explicitly enable:

```
etcd --enable-v2=true
```

Không có flag này, Patroni không thể kết nối tới etcd cluster. Đây là known limitation của `python-etcd` (không hỗ trợ v3 gRPC API). Mitigation: thêm `--enable-v2=true` khi start etcd. Long-term: migrate sang `python-etcd3` hoặc `etcd3gw`.

## Patroni vs pgpool Quorum

Pgpool-II cũng có khái niệm HA nhưng dùng **quorum voting** — các pgpool nodes vote cho nhau. Đây là mechanism khác về bản chất:

|                 | Patroni                  | pgpool Quorum                     |
| --------------- | ------------------------ | --------------------------------- |
| Mechanism       | Raft consensus qua etcd  | Majority voting giữa pgpool nodes |
| Consensus store | etcd (external)          | Built-in giữa pgpool nodes        |
| What it decides | Ai là PostgreSQL primary | Pgpool node nào active            |
| Scope           | Database-level HA        | Proxy-level HA                    |

Patroni giải quyết vấn đề ở tầng database; pgpool quorum giải quyết vấn đề ở tầng proxy. Chúng không thay thế nhau.

## etcd dependency

etcd phải highly available riêng. Single-node etcd (như trong lab setup) là SPOF — nếu etcd down, Patroni không thể reach consensus và sẽ demote primary xuống read-only để tránh split-brain. Production setup cần etcd cluster 3 hoặc 5 nodes.

## REST API

Patroni expose HTTP REST API (mặc định port 8008) cho health check và management:

- `GET /` — cluster status (primary trả `200`, replica trả `503` → dùng được làm health check cho load balancer)
- `POST /failover` — manual failover
- `patronictl` CLI wrap API này

## Connections

- [[permanent/postgresql-ha-patroni-pgpool-combo]] — pattern kết hợp với pgpool-II
- [[permanent/pgpool-ii]] — proxy layer, bổ sung connection pooling và read load balancing
- [[permanent/openstack-trove-postgresql-ha]] — so sánh: Trove managed HA vs standalone Patroni

## Sources

- [[literature/postgresql-ha-patroni-pgpool-ii]]
- [[literature/openstack-dbaas-work-wiki]]
