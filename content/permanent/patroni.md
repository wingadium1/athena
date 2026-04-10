---
title: "Patroni"
aliases: ["patroni ha", "patroni failover", "patroni etcd"]
tags: [postgresql, ha, patroni, distributed-systems, infrastructure, database]
created: 2026-04-10
updated: 2026-04-10
---

Patroni là một HA agent cho PostgreSQL, chạy trên mỗi database node và chịu trách nhiệm duy nhất: đảm bảo cluster luôn có đúng một primary. Nó không tự mình quyết định — nó dùng một distributed consensus store (thường là etcd) để đồng thuận với các nodes khác về trạng thái cluster, tránh split-brain.

## Cơ chế hoạt động

Patroni dùng **Raft consensus** thông qua etcd (hoặc Consul, ZooKeeper). Mỗi node Patroni liên tục:

1. Heartbeat vào etcd để giữ leader lease
2. Nếu primary mất heartbeat (TTL hết hạn), etcd xóa lock
3. Các standby nodes race để acquire lock mới — winner promote thành primary
4. Node thắng update cluster state trong etcd; các node còn lại đọc state và tự cấu hình làm replica

Đây là automatic failover không cần human intervention. Thời gian failover thường 10–30 giây tùy cấu hình `ttl` và `loop_wait`.

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
