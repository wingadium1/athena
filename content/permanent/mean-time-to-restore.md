---
title: "Mean Time to Restore (MTTR)"
aliases: ["MTTR", "mean time to repair", "mean time to recovery"]
tags: [devops, metrics, reliability, incident-management]
created: 2026-04-10
updated: 2026-04-10
---

MTTR (Mean Time to Restore/Repair) là thời gian trung bình cần thiết để phục hồi một hệ thống sau khi xảy ra sự cố. Đây là một trong bốn DORA metrics, nằm trong nhóm đo **stability** — cụ thể là khả năng phản ứng khi có incident.

MTTR thấp không có nghĩa là ít lỗi hơn, mà là tổ chức phát hiện và phục hồi nhanh hơn. Điều này phụ thuộc vào: chất lượng monitoring/alerting, độ rõ ràng của runbooks, mức độ automation trong rollback, và văn hóa incident response của nhóm.

## MTTR trong bối cảnh DORA

MTTR là cặp với **Change Failure Rate** (CFR): CFR đo xác suất một deploy gây ra sự cố, MTTR đo thời gian để xử lý khi sự cố đó xảy ra. Một tổ chức tốt cần cả hai chỉ số thấp — tránh được sự cố và phục hồi nhanh khi không tránh được.

## Connections

- [[permanent/dora-metrics]] — MTTR là một trong bốn DORA metrics, nhóm stability
- [[permanent/site-reliability-engineering]] — SRE quản lý reliability qua Error Budget, MTTR là một signal quan trọng

## Sources

- [[refs/mean_time_to_recovery]]
