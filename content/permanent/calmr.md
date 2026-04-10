---
title: "CALMR"
aliases: ["Culture Automation Lean Flow Measurement Recovery", "CALMS"]
tags: [devops, safe, agile, culture]
created: 2026-04-10
updated: 2026-04-10
---

CALMR là mô hình 5 trụ cột của [[permanent/devops|DevOps]] trong bối cảnh SAFe. Tên gọi là viết tắt của **C**ulture, **A**utomation, **L**ean Flow, **M**easurement, **R**ecovery.

Lịch sử phát triển: John Willis và Damon Edwards giới thiệu CMAS (Culture–Automation–Measurement–Sharing) tại DevOpsDay 2010. Jez Humble bổ sung **L** (Lean Flow) để thành CALMS. Scaled Agile nhận ra Sharing đã nằm trong Culture nên bỏ S, thêm **R** (Recovery) — nhấn mạnh rằng khả năng phục hồi từ failure cũng là một năng lực kỹ thuật chủ động, không phải phản ứng thụ động.

Năm trụ cột:

- **Culture**: Xây dựng văn hóa chia sẻ trách nhiệm giữa các nhóm Dev, Ops, Security, Business. Đây là nền tảng — không có Culture thì Automation và Measurement chỉ là công cụ không có ai dùng đúng.
- **Automation**: Tự động hóa càng nhiều càng tốt trong [[permanent/cicd|Continuous Delivery Pipeline]]. Giảm manual handoffs, tăng tốc độ, giảm human error.
- **Lean Flow**: Làm việc với batch size nhỏ, visualize toàn bộ công việc, tránh quá nhiều WIP. Liên kết trực tiếp với [[permanent/little-s-law|Little's Law]] và [[permanent/flow-framework|Flow Framework]].
- **Measurement**: Đo flow, chất lượng, và performance trên mọi môi trường. Kiểm tra liên tục xem có đang deliver value hay không. Nền tảng cho [[permanent/dora-metrics|DORA Metrics]].
- **Recovery**: Thiết kế release low-risk. Đầu tư vào chuẩn bị khả năng phục hồi từ failure — không chỉ là monitoring mà là runbooks, chaos engineering, rollback strategies.

## Connections

- [[permanent/devops]] — CALMR là mô hình hóa cách tiếp cận DevOps trong SAFe
- [[permanent/safe]] — SAFe dùng CALMR làm DevOps framework chính thức
- [[permanent/cicd]] — Automation trụ cột chính là CD Pipeline
- [[permanent/flow-framework]] — Lean Flow trụ cột ánh xạ trực tiếp vào Flow metrics
- [[permanent/dora-metrics]] — Measurement trụ cột đo bằng DORA 4 metrics
- [[permanent/feature-flag]] — Recovery trụ cột, feature flag là một cơ chế low-risk release

## Sources

- [[refs/CALMR]] (archived)
- [[refs/scaling_devops_with_safe]] (archived)
