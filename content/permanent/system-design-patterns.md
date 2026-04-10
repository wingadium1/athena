---
title: "System Design Patterns"
aliases: ["system design", "distributed system patterns", "scalability patterns"]
tags: [system-design, distributed-systems, architecture, infrastructure]
created: 2026-04-10
updated: 2026-04-10
---

Một tập hợp các vấn đề phổ biến trong thiết kế hệ thống phân tán và giải pháp tương ứng. Đây là cheat sheet về các trade-off cốt lõi — mỗi giải pháp đều có chi phí riêng.

## 8 vấn đề điển hình và giải pháp

### 1. Read-Heavy System

**Vấn đề**: Database bị overload bởi read traffic.  
**Giải pháp**: **Caching** (Redis, Memcached) — serve reads từ in-memory store. Trade-off: eventual consistency, cache invalidation phức tạp.

### 2. High-Write Traffic

**Vấn đề**: Database không kịp handle write throughput.  
**Giải pháp**: **Async writes** — ghi vào queue (Kafka, SQS) rồi process sau. Cân nhắc **LSM-tree databases** (Cassandra, RocksDB) tối ưu cho write throughput. Trade-off: durability risk, không có immediate consistency.

### 3. Single Point of Failure (SPOF)

**Vấn đề**: Một component down → toàn hệ thống down.  
**Giải pháp**: **Redundancy + failover** — mọi critical component phải có replica. Auto-failover (health check + routing). Trade-off: tăng complexity và cost.

### 4. High Availability

**Vấn đề**: Service phải luôn sẵn sàng ngay cả khi có component failure.  
**Giải pháp**: **Load balancing** (phân tán request) + **Replication** (data/service redundancy). Kết hợp với geographic distribution cho HA across AZ/region. Trade-off: consistency challenges (CAP theorem).

### 5. High Latency

**Vấn đề**: Response time chậm, đặc biệt với users ở xa.  
**Giải pháp**: **CDN** (cache static content ở edge nodes gần user). Kết hợp với [[permanent/hedged-requests]] cho dynamic content. Trade-off: CDN cost, cache freshness.

### 6. Handling Large Files

**Vấn đề**: Files quá lớn cho một request/database record.  
**Giải pháp**: **Object storage** (S3, GCS) cho unstructured large files + **Block storage** cho structured/random access. Pre-signed URLs cho direct upload/download. Trade-off: eventual consistency, egress cost.

### 7. Monitoring and Alerting

**Vấn đề**: Không biết khi nào hệ thống gặp vấn đề.  
**Giải pháp**: **Centralized logging** (ELK stack, Loki) + **Metrics** (Prometheus) + **Alerting** (PagerDuty, alertmanager). Trade-off: observability infra cost, alert fatigue.

### 8. Slow Database Queries

**Vấn đề**: Queries chậm làm toàn bộ application chậm.  
**Giải pháp**: **Proper indexes** (phân tích query plan, add indexes cho frequent patterns) + **Horizontal sharding** (chia data ra nhiều DB instances theo key). Trade-off: cross-shard queries phức tạp, index maintenance overhead.

## Connections

- [[permanent/tail-latency]] — High Latency problem và CDN/caching/hedging solutions đều liên quan đến latency management
- [[permanent/resilience-vs-robustness]] — SPOF elimination và HA là kỹ thuật Resilience engineering
- [[permanent/kubernetes-node-consolidation]] — Autoscaling giải quyết High Write và Read-Heavy peaks
- [[permanent/cassandra-kubernetes-ha]] — Cassandra là LSM-tree DB cho High-Write Traffic use case
- [[permanent/site-reliability-engineering]] — SRE owns Monitoring/Alerting và availability guarantees

## Sources

- `journal/8_most_important_system_design_concepts.md`
