---
title: "Hedged Requests"
aliases: ["request hedging", "hedging pattern"]
tags: [distributed-systems, performance, reliability, patterns]
created: 2026-04-10
updated: 2026-04-10
---

Hedged Requests là kỹ thuật client-side giảm thiểu tail latency bằng cách gửi **cùng một request đến nhiều instance** của một service — đồng thời hoặc sau một khoảng delay ngắn — rồi sử dụng **response đầu tiên trả về**, hủy các request còn lại. Ý tưởng: thay vì chờ một instance có thể đang chậm, ta "đua" nhiều instance với nhau.

Google mô tả kỹ thuật này trong paper _Tail at Scale_ và gRPC hỗ trợ nó natively qua [Request Hedging](https://grpc.io/docs/guides/request-hedging/).

## Cơ chế hoạt động

1. Client gửi request đến instance A.
2. Sau một khoảng thời gian (hedging delay, thường là p95 latency của service), nếu chưa có response, client gửi thêm request đến instance B (và có thể C).
3. Khi bất kỳ instance nào trả về response đầu tiên → client sử dụng response đó, hủy các request còn lại.
4. Kết quả: p99 latency giảm vì khi instance A chậm, instance B thường nhanh hơn.

## Khi nào nên dùng

- Có **nhiều instance** của cùng một service (điều kiện bắt buộc)
- Tail latency xảy ra thường xuyên và ảnh hưởng UX
- Operation là **idempotent / read-only** — gửi nhiều request không gây side effects
  - Các truy vấn tìm kiếm, read từ database, API gateway reads
- Chi phí server tăng thêm là chấp nhận được

## Khi nào KHÔNG nên dùng

- Write operations — gửi trùng lặp gây duplicate data hoặc double-charge
- Khi hệ thống đang gần đầy tải — hedging tăng thêm ~N lần request, có thể gây cascade failure

## Đánh đổi

|             | Với hedging                                | Không có hedging |
| ----------- | ------------------------------------------ | ---------------- |
| p99 latency | Giảm ~20-40%                               | Cao              |
| Server load | Tăng (gần N× cho hedged %)                 | Baseline         |
| Complexity  | Cao hơn (cần multi-instance, cancel logic) | Thấp             |

Trong benchmark thực tế (Spring Boot + WebFlux + Eureka): `/hedge` endpoint có p95 ~2-2.5s so với `/hi` (non-hedged) ở ~4.5s — cải thiện ~20% với workload random delay 0-5 giây.

## Triển khai

Trong Spring Boot + WebFlux: dùng `ExchangeFilterFunction` kết hợp `DiscoveryClient` để lấy danh sách instances, gửi parallel `Mono<ClientResponse>`, rồi dùng `Flux.first()` lấy winner.

gRPC: cấu hình `hedging_policy` trong service config với `maxAttempts` và `hedgingDelay`.

## Connections

- [[permanent/tail-latency]] — hedged requests là giải pháp trực tiếp cho tail latency problem
- [[permanent/resilience-vs-robustness]] — pattern này thuộc nhóm Resilience (giảm impact thay vì ngăn sự cố)
- [[permanent/site-reliability-engineering]] — SRE dùng hedging để đảm bảo SLO latency

## Sources

- `writing/Câu chuyện về khi người dùng phàn nàn về những bất thường nhưng bạn không thấy gì bất thường khi monitoring.md`
- [gRPC Request Hedging](https://grpc.io/docs/guides/request-hedging/)
- [Spring Tips: Hedging Client Requests](https://spring.io/blog/2019/01/23/spring-tips-hedging-client-requests-with-the-reactive-webclient-and-a-service-registry)
- [Tail at Scale — Google Research](https://research.google/pubs/pub40801/)
