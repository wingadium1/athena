---
title: "Flow Framework"
aliases: ["Flow Framework", "Flow Metrics", "Mik Kersten flow", "value stream flow"]
tags: [devops, metrics, flow, value-stream, safe]
created: 2026-04-10
updated: 2026-04-10
---

Flow Framework là framework của Mik Kersten (tác giả _Project to Product_) để đo hiệu suất của value stream trong software delivery. Kersten lập luận rằng các tổ chức cần chuyển từ tư duy project-based sang product-based với long-lived value streams — và Flow Framework cung cấp bộ metric để làm điều đó.

Framework theo dõi 4 loại **Flow Items** và đo chúng qua 5 **Flow Metrics**.

## 4 Flow Items

| Flow Item   | Mô tả                                                  |
| ----------- | ------------------------------------------------------ |
| **Feature** | Tính năng mang lại business value trực tiếp            |
| **Defect**  | Bug fixes — được kéo bởi khách hàng                    |
| **Risk**    | Công việc giảm thiểu rủi ro: NFRs, compliance enablers |
| **Debt**    | Giảm technical debt: arch enablers, infra enablers     |

**Flow Distribution** theo dõi tỉ lệ phân bổ giữa 4 loại này — một value stream quá nặng Debt hoặc Defect là dấu hiệu hệ thống đang suy giảm.

## 5 Flow Metrics

| Metric                | Đo gì                                               |
| --------------------- | --------------------------------------------------- |
| **Flow Velocity**     | Số Flow Items hoàn thành trong một đơn vị thời gian |
| **Flow Time**         | Thời gian để hoàn thành một Flow Item (end-to-end)  |
| **Flow Load**         | WIP hiện tại — số Flow Items đang in-progress       |
| **Flow Efficiency**   | Tỉ lệ active time / Flow Time (loại bỏ wait time)   |
| **Flow Distribution** | Phân bổ % theo từng loại Flow Item                  |

## Liên quan đến SAFe

Trong SAFe, Flow Framework được áp dụng ở cấp Value Stream. Features map sang Feature/Story trong PI Planning; Debt và Risk map sang Enablers. Đây là cầu nối giữa SAFe's planning cadence và metrics thực tế của engineering.

## Connections

- [[permanent/little-s-law]] — Little's Law là nền toán học đằng sau Flow Time và Flow Load
- [[permanent/dora-metrics]] — DORA đo ở cấp deployment pipeline; Flow Framework đo ở cấp value stream (rộng hơn)
- [[permanent/cost-of-delay]] — Cost of Delay quantify business impact của Flow Time kéo dài

## Sources

- [[refs/flow_framework]]
