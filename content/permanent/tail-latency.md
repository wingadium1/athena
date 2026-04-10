---
title: "Tail Latency"
aliases: ["latency outliers", "p99 latency", "long tail latency"]
tags: [distributed-systems, performance, reliability, observability]
created: 2026-04-10
updated: 2026-04-10
---

Tail latency là hiện tượng một tỷ lệ nhỏ các request trong hệ thống phân tán có thời gian phản hồi cao hơn đáng kể so với phần còn lại. Đây là lý do tại sao theo dõi **average** hay **p50** response time đôi khi che giấu vấn đề thực sự — vì con số đó không đại diện cho những người dùng xui xẻo nhất.

## Tại sao tail latency quan trọng: compound effect

Vấn đề trở nên nghiêm trọng hơn khi một request phải gọi nhiều service song song hoặc nối tiếp nhau. Xét ví dụ: một service feed cần gọi 10 service độc lập, mỗi service có p99 = 1 giây (tức 1% khả năng phản hồi chậm hơn 1 giây):

$$P(\text{ít nhất 1 service chậm}) = 1 - 0.99^{10} \approx 9.56\%$$

Tức là dù mỗi service riêng lẻ chỉ có 1% "bad luck", toàn bộ request trở nên chậm với xác suất gần **10%**. Với hệ thống có nhiều tầng microservice, con số này có thể tăng rất nhanh.

## Nguyên nhân phổ biến

- Garbage collection pauses (JVM, Go)
- Resource contention (CPU, disk I/O, network)
- Cold start sau deployment hay scale-out
- Queue buildup trong load balancer
- Variable workload distribution — một số instance nhận nhiều request nặng hơn

## Cách phát hiện

Luôn monitor bằng **percentile metrics** thay vì average:

- **p50**: median, chỉ cho biết trải nghiệm trung bình
- **p95 / p99**: trải nghiệm của nhóm người dùng chậm nhất — đây mới là số quan trọng
- **Max**: đỉnh xấu nhất, thường bị noise nhưng hữu ích khi điều tra

Nếu p80 gần với average nhưng p99 rất cao → có tail latency problem.

## Giải pháp

- **[[permanent/hedged-requests]]** — gửi nhiều request song song, lấy response đầu tiên (giải quyết symptom)
- **Optimize từng service** — profiling, caching, connection pooling (giải quyết root cause)
- **Load balancing cải tiến** — least-connections hay power of two choices thay vì round-robin
- **Timeout + retry có chiến lược** — giới hạn thời gian chờ, không để một service chậm block toàn bộ

## Connections

- [[permanent/hedged-requests]] — kỹ thuật client-side để giảm tail latency bằng cách "đua" nhiều instance
- [[permanent/resilience-vs-robustness]] — tail latency là một trong những lý do cần Resilience engineering
- [[permanent/site-reliability-engineering]] — SRE sử dụng SLO/SLA đo bằng percentile latency

## Sources

- `writing/Câu chuyện về khi người dùng phàn nàn về những bất thường nhưng bạn không thấy gì bất thường khi monitoring.md`
- [Tail at Scale — Google Research](https://research.google/pubs/pub40801/)
