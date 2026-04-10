---
title: "Resilience vs Robustness"
aliases: ["system resilience", "software robustness", "resilience engineering"]
tags: [devops, reliability, continuous-delivery, software-architecture]
created: 2026-04-10
updated: 2026-04-10
---

Robustness và Resilience là hai triết lý đối lập trong việc thiết kế hệ thống phần mềm đáng tin cậy. Robustness ưu tiên **giảm tần suất sự cố** (MTBF cao), trong khi Resilience ưu tiên **giảm thời gian phục hồi khi sự cố xảy ra** (MTTR thấp). Cách phân biệt trực quan: hệ thống Robust như chai thủy tinh cứng — ít vỡ, nhưng khi vỡ thì vỡ hẳn; hệ thống Resilient như dây cao su — co giãn, chịu được tác động và phục hồi.

## Tại sao Robustness có vấn đề

Triết lý Robustness giả định rằng production environment là một hệ thống có thể dự đoán được — tức là chỉ xử lý các **known unknowns**. Thực tế, production environment là **Complex domain** theo Cynefin framework: chứa đầy các unknown unknowns mà không thể liệt kê trước. Khi đó, các practice của Robustness trở thành gánh nặng:

- **End-to-end testing** tốn thời gian và chi phí lớn, nhưng không thể vét sạch mọi tình huống thực tế.
- **Change Boards** tạo ra cơ chế quan liêu làm chậm mọi quyết định deploy.
- **Change Freezes** kéo dài lead time, tích lũy nợ kỹ thuật và nợ cập nhật dependency.

Nghịch lý: dự án chú trọng Robustness thường tạo ra hệ thống _giòn_ (fragile) — khi sự cố xảy ra, toàn bộ hệ thống sụp đổ thay vì chỉ một phần nhỏ. Chi phí sự cố = `doanh thu/giờ × thời gian phục hồi`, một phép nhân rất đau.

## Resilience Engineering

Theo Erik Hollnagel (_Resilience Engineering in Practice_), hệ thống Resilient cần 4 năng lực:

1. **Anticipation** — Hiểu trước những gì hệ thống có thể đối mặt (spike traffic, AZ outage, dependency failure).
2. **Monitoring** — Biết cần theo dõi thứ gì và nhận ra bất thường sớm.
3. **Response** — Phản ứng nhanh và giảm thiểu impact khi sự cố xảy ra.
4. **Learn** — Phân tích sự cố, chia sẻ bài học trong toàn dự án (blameless post-mortems).

Về kỹ thuật, Resilience system cần:

- **Adaptive architecture**: service/component độc lập, có khả năng scale theo chiều ngang và dọc.
- **Feature flags**: cho phép tắt/bật tính năng mà không cần redeploy toàn bộ.
- **Circuit Breaker + Service Registry**: phát hiện service lỗi trong vài giây, không phải vài ngày.
- **Chaos Engineering + Smoke tests**: chủ động tìm lỗi tiềm ẩn trước khi production tìm ra chúng.
- **IaC (Infrastructure as Code)**: môi trường giống nhau ở mọi stage, giảm biến số do con người.
- **Observability đầy đủ**: log, metrics, alarm, và user analytics.
- **Blameless culture**: "You Built It, You Run It" — mọi người chia sẻ kiến thức, không đổ lỗi cá nhân.

## Con đường chuyển đổi từ Robustness sang Resilience

1. **Version hoá mọi thứ** — code, config, infrastructure — để biết chính xác cái gì thay đổi khi có sự cố.
2. **Đo đạc** — thiết lập baseline về stability và delivery throughput.
3. **Tăng độ tin cậy production từng bước** — rearchitect dần dần (không big bang), thiết lập monitoring.
4. **Áp dụng Continuous Delivery** — khi hệ thống đã đủ điều kiện: deployment ổn định, khả năng tự phục hồi cơ bản.

Change Boards và Change Freezes dần được thay bởi **Blue/Green Deployments**, **Canary releases**, và **Dark launches**. End-to-end testing nhường chỗ cho Test Pyramid (unit tests thường xuyên, acceptance test chọn lọc, smoke test tối giản).

## Connections

- [[permanent/continuous-delivery]] — Resilience là điều kiện để CD hoạt động hiệu quả; Robustness cản trở CD
- [[permanent/continuous-deployment]] — Deploy thường xuyên chỉ khả thi với Resilient system
- [[permanent/mean-time-to-restore]] — MTTR là metric cốt lõi của triết lý Resilience
- [[permanent/dora-metrics]] — DORA đo lường cả stability (CFR, MTTR) lẫn throughput (deployment frequency, lead time)
- [[permanent/feature-flag]] — Feature flags là công cụ Resilience: tắt tính năng lỗi không cần rollback
- [[permanent/westrum-culture-typology]] — Generative culture (Westrum) là nền tảng văn hoá của Resilience engineering

## Sources

- `writing/robustness_or_resilience.md` — bài viết gốc tiếng Việt của owner
- `writing/Software Architecture Origin/software-arch-bat-dau-tu-dau-3.md` — series về CD và soft skills
