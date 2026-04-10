---
title: "Demystifying HA PostgreSQL with Patroni and Pgpool-II on Ubuntu"
type: literature
source: "https://medium.com/@joaovic32/demystifying-high-availability-postgresql-with-patroni-and-pgpool-ii-on-ubuntu-428c91a55b1a"
author: "joao victor silva de oliveira"
date-read: 2026-04-10
tags: [postgresql, ha, patroni, pgpool, infrastructure, database]
---

## Summary

Bài viết hướng dẫn triển khai HA PostgreSQL cluster trên Ubuntu bằng cách kết hợp Patroni (failover orchestration qua etcd/Raft) và Pgpool-II (connection proxy, pooling, load balancing). Kiến trúc dùng 3 nodes: 2 PostgreSQL nodes (do Patroni quản lý) và 1 Pgpool-II node làm entry point cho applications. Điểm đáng chú ý là hai tool này phân chia trách nhiệm rõ ràng — Patroni làm toàn bộ việc quyết định ai là primary, Pgpool-II chỉ routing và phục vụ connections.

## Key Ideas

- **Phân chia trách nhiệm không overlap**: Patroni (via etcd) xử lý leader election và automatic failover; Pgpool-II xử lý connection routing, pooling, và read load balancing. Pgpool-II failover scripts chỉ log, không trigger bất cứ thứ gì — vì Patroni đã lo phần đó.
- **etcd là distributed brain**: Patroni dùng Raft consensus qua etcd để đồng thuận về cluster state (ai là leader, replication lag). Single-node etcd trong bài là ok cho lab, nhưng production cần dedicated etcd cluster (3 hoặc 5 nodes).
- **pgpool-II awareness**: Pgpool-II biết ai là primary thông qua `sr_check` — kiểm tra `pg_stat_replication` định kỳ. Khi Patroni đã promote một standby lên primary, Pgpool-II tự điều chỉnh routing mà không cần được notify.
- **Connection pooling giảm overhead**: Applications không open direct connections tới PostgreSQL; Pgpool-II reuse existing connections, giảm đáng kể cost của connection establishment với OLTP workloads nhiều short-lived connections.
- **Read scalability qua load balancing**: SELECT queries được phân phối giữa primary và standbys (cấu hình qua `backend_weight`). Write operations luôn đi tới primary.
- **Bootstrap chỉ trên node đầu tiên**: Patroni config `bootstrap` section chỉ cần trên pg-node1. pg-node2 sẽ tự join cluster và clone data từ primary — không cần pre-seed.

## Quotes

> "Patroni ensures that one of your standbys (replicas) quickly takes over as the new primary, keeping your database online. It needs a shared 'brain' to make decisions…"

> "Pgpool-II comes in! It's a connection proxy that sits between your applications and your PostgreSQL servers."

> "Since Patroni manages PostgreSQL HA, these Pgpool-II scripts can be simple, mainly for logging purposes." — Đây là key insight: pgpool failover scripts chỉ log, không can thiệp.

## My Take

Thú vị ở chỗ combo này không redundant — không phải "dùng 2 tool làm cùng 1 việc". Patroni giải quyết vấn đề _ai là primary và khi nào switch_, còn Pgpool-II giải quyết _applications kết nối tới đâu và connection được reuse như thế nào_. So sánh với Trove PostgreSQL HA (dùng Swift backup làm transport cho replication seeding), approach này là standalone — không cần OpenStack infrastructure, thích hợp hơn cho bare metal hoặc non-OpenStack environments.

## Links

- [[permanent/patroni]] — HA orchestrator dùng Raft/etcd
- [[permanent/pgpool-ii]] — Connection proxy và pooler
- [[permanent/postgresql-ha-patroni-pgpool-combo]] — Pattern kết hợp, phân chia trách nhiệm
- [[permanent/openstack-trove-postgresql-ha]] — So sánh: managed HA trên OpenStack vs standalone
