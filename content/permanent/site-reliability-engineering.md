---
title: "Site Reliability Engineering (SRE)"
aliases: ["SRE", "Site Reliability Engineer"]
tags: [devops, sre, reliability, engineering-culture]
created: 2026-04-10
updated: 2026-04-10
---

SRE là một practice (và job title) xuất phát từ Google, dùng kỹ thuật phần mềm để giải quyết các vấn đề vận hành hạ tầng. Thay vì Ops thủ công, SRE tự động hóa toàn bộ những gì lặp đi lặp lại, duy trì độ tin cậy hệ thống bằng code.

Điểm phân biệt SRE với DevOps tổng quát: SRE tập trung hẹp hơn vào **production stability** — cụ thể là SLO/SLI/Error Budget. DevOps bao trùm toàn bộ lifecycle từ development đến decommission và thiên về triết lý văn hóa. SRE là một cách cụ thể để triển khai DevOps với bộ metric rõ ràng. Cả hai đều chia sẻ nguyên tắc tự động hóa và cộng tác.

## Các khái niệm cốt lõi

- **SLI** (Service Level Indicator): metric đo lường thực tế (latency, error rate, throughput)
- **SLO** (Service Level Objective): mục tiêu target cho SLI (ví dụ: 99.9% availability)
- **Error Budget**: lượng downtime/error được phép trong một kỳ — khi cạn, dev team phải ưu tiên reliability thay vì feature mới

## Connections

- [[permanent/devops]] — SRE là một cách implement DevOps cụ thể
- [[permanent/devops-topology]] — SRE team là một trong 9 topology patterns

## Sources

- [[refs/sre]]
