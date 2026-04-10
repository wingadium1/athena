---
title: "Pgpool-II"
aliases: ["pgpool", "pgpool2", "pgpool connection pooler", "pgpool load balancer"]
tags: [postgresql, pgpool, connection-pooling, load-balancing, proxy, infrastructure, database]
created: 2026-04-10
updated: 2026-04-10
---

Pgpool-II là một middleware proxy ngồi giữa applications và PostgreSQL cluster. Nó không phải database, không lưu data — nhiệm vụ của nó là quản lý _cách_ applications kết nối tới PostgreSQL: pooling connections, phân phối queries, và routing writes/reads đúng node.

## Ba chức năng chính

**1. Connection Pooling**
PostgreSQL tạo một process mới cho mỗi connection — đây là overhead không nhỏ với OLTP workloads có nhiều short-lived connections. Pgpool-II giữ một pool của persistent connections tới backend PostgreSQL, reuse chúng cho client requests. Giảm đáng kể số lần fork process.

**2. Read Load Balancing**
SELECT queries được phân phối tới primary hoặc standbys dựa trên `backend_weight`. Write queries (INSERT/UPDATE/DELETE) luôn đi tới primary. Pgpool-II phân biệt read vs write bằng cách parse SQL — không cần application biết gì về cluster topology.

**3. HA Awareness (Routing)**
Pgpool-II biết backend nào là primary qua `sr_check` — định kỳ query `pg_stat_replication`. Sau failover (do Patroni hoặc bất kỳ mechanism nào thực hiện), pgpool-II tự cập nhật routing mà không cần restart hay reconfiguration.

## Pgpool Quorum vs Patroni

Pgpool-II có cơ chế HA riêng cho _chính nó_ (watchdog + quorum voting) — để tránh single pgpool node là SPOF. Đây khác hoàn toàn với Patroni:

- **Patroni** = quyết định ai là PostgreSQL primary
- **pgpool watchdog** = quyết định pgpool node nào nhận traffic (virtual IP)

Khi combine cả hai, bạn có HA ở cả tầng database lẫn tầng proxy.

## Port conventions

- `9999` — application connection port (pgpool frontend)
- `9898` — PCP (Pgpool Control Protocol) — admin commands
- `8008` — Patroni REST API (không phải pgpool, nhưng liên quan trong combined setup)

## Limitations cần lưu ý

- **Query parsing không hoàn hảo**: Pgpool-II parse SQL để phân loại read/write, nhưng có edge cases (stored procedures, multi-statement queries). Cần test kỹ với workload thực.
- **`delay_threshold`**: Nếu standby lag quá threshold (bytes), pgpool-II ngừng route reads tới đó — tránh stale reads. Nhưng nếu threshold quá thấp và replication lag cao, mọi reads sẽ dồn về primary.
- **Separate from failover**: Pgpool-II _detect_ failover nhưng không _orchestrate_ nó. Nếu dùng standalone (không có Patroni), cần script `failover_command` để trigger promote thủ công.

## Connections

- [[permanent/patroni]] — HA orchestrator bổ sung; Patroni xử lý failover, pgpool-II xử lý routing
- [[permanent/postgresql-ha-patroni-pgpool-combo]] — combined pattern và phân chia trách nhiệm rõ ràng
- [[permanent/openstack-trove-postgresql-ha]] — so sánh với Trove managed HA (không dùng pgpool)

## Sources

- [[literature/postgresql-ha-patroni-pgpool-ii]]
