---
title: "PostgreSQL HA: Patroni + Pgpool-II Combined Pattern"
aliases: ["patroni pgpool combo", "postgresql ha stack", "postgresql standalone ha"]
tags: [postgresql, ha, patroni, pgpool, infrastructure, database, architecture]
created: 2026-04-10
updated: 2026-05-22
---

Khi kết hợp Patroni và Pgpool-II, bạn có một PostgreSQL HA stack đầy đủ không cần managed service hay cloud infrastructure. Điểm mấu chốt là hai tool này giải quyết **hai vấn đề khác nhau** — và bổ sung nhau mà không overlap.

## Phân chia trách nhiệm

```
Applications
     │
     ▼
[ Pgpool-II :9999 ]          ← Connection pooling, read/write routing, load balancing
     │              │
     ▼              ▼
[ pg-node1 ]   [ pg-node2 ]  ← PostgreSQL instances
     │              │
     └──── etcd ────┘         ← Patroni dùng để consensus (Raft leader election)
```

| Layer       | Tool            | Trách nhiệm                                                              |
| ----------- | --------------- | ------------------------------------------------------------------------ |
| Database HA | Patroni         | Leader election, automatic failover, streaming replication management    |
| Consensus   | etcd            | Distributed state store — ai là primary, replication lag, cluster health |
| Proxy       | Pgpool-II       | Connection pooling, route writes → primary, distribute reads → any node  |
| Proxy HA    | pgpool watchdog | Virtual IP, pgpool node failover (nếu triển khai multi-pgpool)           |

## Tại sao không dùng mỗi một tool?

**Chỉ Patroni (không Pgpool-II)**:

- Application phải tự biết endpoint của primary → thường dùng HAProxy hoặc virtual IP riêng
- Không có connection pooling → mỗi client connection = 1 PostgreSQL backend process
- Không có read load balancing tự động

**Chỉ Pgpool-II (không Patroni)**:

- Pgpool-II có thể detect failover nhưng không tự orchestrate promote
- Cần `failover_command` script thủ công trigger `pg_ctl promote` hoặc `repmgr`
- Không có Raft consensus → split-brain risk nếu script chạy sai timing

**Kết hợp cả hai**: Patroni lo toàn bộ việc quyết định primary, Pgpool-II lo routing và pooling. Pgpool failover scripts chỉ cần log — không can thiệp vào quyết định của Patroni.

## Luồng failover thực tế

1. pg-node1 (primary) xuống
2. Patroni trên pg-node2 detect: heartbeat của leader trong etcd hết TTL
3. pg-node2 acquire etcd lock → promote PostgreSQL lên primary (qua LSN comparison — node có highest LSN wins)
4. Pgpool-II `sr_check` phát hiện pg-node1 unreachable, pg-node2 giờ là primary
5. Pgpool-II redirect writes tới pg-node2
6. Applications kết nối qua Pgpool-II không thấy gì thay đổi (chỉ thấy latency spike ngắn)

Thời gian failover: ~10–30 giây (dominated bởi Patroni TTL, thường cấu hình `ttl: 30`, `loop_wait: 10`).

## Split-brain Protection

Patroni ngăn split-brain qua cơ chế **DCS quorum fencing**:

- Chỉ một node có thể giữ DCS leader key tại một thời điểm
- Node thua bị **fence** trước khi promote (STONITH/watchdog hoặc `pg_ctl stop` qua SSH)
- Nếu etcd cluster mất quorum (≥2 of 3 etcd nodes fail), Patroni vào **read-only mode** — không failover mới, primary hiện tại tiếp tục serve reads
- **Implication**: etcd cluster MUST ≥3 nodes. Single-node etcd = SPOF

## etcd Sizing

| Scenario | Nodes | Tolerated failures |
|----------|-------|--------------------|
| Lab/assessment | 1 | 0 (SPOF) |
| Minimum HA | 3 | 1 |
| Production | 5 | 2 |

Trên RHOSO 18, etcd có thể chạy như pods trên OCP nodes — 3 OCP nodes = 3 etcd pods = minimum HA.

## Phased Migration Path

**Phase 1 — MVP (Pgpool-only)**: Deploy Pgpool-II với manual failover scripts. Không etcd dependency. Failover 30-60s, phù hợp lab/assessment.

**Phase 2 — Patroni Control Plane**: Thêm Patroni vào PostgreSQL cluster hiện có + etcd 3-node. Patroni takeover automatic failover. Pgpool giữ lại cho connection pooling.

**Phase 3 — Hybrid Production**: Pgpool-II per-tenant (deploy qua Heat/Ansible). Patroni + etcd HA. Full read/write split + <30s auto failover. Monitoring qua `patronictl` + Prometheus `postgres_exporter`.

## Key Operational Commands

```bash
# Check cluster status
patronictl -c /etc/patroni/patroni.yml list

# Manual failover (controlled switchover)
patronictl -c /etc/patroni/patroni.yml switchover --master <current-primary>

# Restart Patroni without touching PostgreSQL
patronictl -c /etc/patroni/patroni.yml restart <cluster-name> <node-name>

# Check Pgpool node status
psql -h <pgpool-host> -p 9999 -U pgpool_admin -c "SHOW POOL_NODES;"
```

## So sánh với Trove PostgreSQL HA

Trove (OpenStack DBaaS) cũng cung cấp PostgreSQL HA nhưng theo hướng hoàn toàn khác:

|                             | Patroni + pgpool-II        | Trove PostgreSQL HA                            |
| --------------------------- | -------------------------- | ---------------------------------------------- |
| Infrastructure              | Bare metal / bất kỳ VM     | OpenStack mandatory                            |
| Replication seeding         | Direct pg_basebackup       | Qua Swift backup/restore                       |
| Failover                    | Automatic (Patroni/Raft)   | Manual (`openstack database instance promote`) |
| Connection routing          | Pgpool-II proxy            | Application tự quản lý endpoint                |
| Timeline issue sau failover | Không có vấn đề            | Timeline 2 backup fails                        |
| Operational complexity      | Setup phức tạp hơn lúc đầu | Đơn giản hơn nếu đã có OpenStack               |

Trove phù hợp khi bạn đã có OpenStack và muốn managed experience. Patroni + pgpool phù hợp khi bạn muốn control nhiều hơn hoặc không có OpenStack.

## Connections

- [[permanent/patroni]] — chi tiết Raft consensus và leader election
- [[permanent/pgpool-ii]] — chi tiết connection pooling và load balancing
- [[permanent/openstack-trove-postgresql-ha]] — alternative: managed HA qua OpenStack Trove

## Sources

- [[literature/postgresql-ha-patroni-pgpool-ii]]
- [[literature/openstack-dbaas-work-wiki]]
