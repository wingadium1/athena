---
title: "DORA Metrics"
aliases:
  [
    "DORA",
    "DevOps metrics",
    "Four Keys",
    "deployment frequency",
    "lead time for changes",
    "change failure rate",
    "MTTR DevOps",
  ]
tags: [devops, metrics, performance, software-delivery]
created: 2026-04-10
updated: 2026-04-10
---

DORA metrics (từ DevOps Research and Assessment) là bộ 4 chỉ số đo hiệu suất của một tổ chức DevOps, được nghiên cứu và chuẩn hóa qua nhiều năm khảo sát trong ngành. Hai metric đo **velocity** (tốc độ delivery), hai metric đo **stability** (độ ổn định sau delivery).

## Bốn DORA metrics

| Metric                          | Đo gì                                                 | Phân loại |
| ------------------------------- | ----------------------------------------------------- | --------- |
| **Deployment Frequency**        | Tần suất deploy thành công lên production             | Velocity  |
| **Lead Time for Changes**       | Thời gian từ commit đến production                    | Velocity  |
| **Change Failure Rate**         | Tỉ lệ deploy gây ra incident (failures / deployments) | Stability |
| **Mean Time to Restore (MTTR)** | Thời gian trung bình để phục hồi sau incident         | Stability |

## Performance levels

DORA phân tổ chức thành 4 nhóm: **Elite, High, Medium, Low**. Elite performers deploy nhiều lần mỗi ngày với lead time dưới 1 giờ, trong khi Low performers deploy vài tháng một lần với lead time vài tháng.

Điểm quan trọng từ nghiên cứu: **velocity và stability không đánh đổi nhau**. Elite performers vừa nhanh vừa ổn định — tốc độ cao không đồng nghĩa với nhiều lỗi hơn.

## Tại sao 4 metrics này?

Chúng đo hai tension cốt lõi trong software delivery: muốn ship nhanh (velocity) nhưng phải ship an toàn (stability). Bất kỳ tổ chức nào tối ưu chỉ một phía đều tạo ra bottleneck ở phía kia. DORA metrics buộc tổ chức nhìn cả hai cùng lúc.

## Connections

- [[permanent/site-reliability-engineering]] — SRE dùng SLO/Error Budget, DORA dùng MTTR/CFR — hai frameworks đo reliability khác nhau
- [[permanent/devops]] — DORA metrics là cách đo lường thành công của DevOps transformation
- [[permanent/continuous-deployment]] — Deployment Frequency là kết quả trực tiếp của pipeline automation

## Sources

- [[refs/The_DORA_KPI_metrics]]
