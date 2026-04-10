---
title: "Little's Law"
aliases: ["Little's law", "L = λW", "WIP cycle time throughput"]
tags: [flow, queuing-theory, metrics, agile, math]
created: 2026-04-10
updated: 2026-04-10
---

Little's Law là định lý của John Little, mô tả mối quan hệ giữa ba biến trong bất kỳ hệ thống ổn định nào — không phụ thuộc vào phân phối thống kê của arrival hay service time.

$$L = \lambda W$$

| Biến  | Ý nghĩa                                      | Trong software delivery |
| ----- | -------------------------------------------- | ----------------------- |
| **L** | Số items trung bình trong hệ thống           | WIP (Work in Progress)  |
| **λ** | Arrival rate (items/thời gian)               | Throughput              |
| **W** | Thời gian trung bình một item trong hệ thống | Cycle Time              |

Viết lại: $\text{Cycle Time} = \frac{\text{WIP}}{\text{Throughput}}$

## Implication thực tế

Muốn giảm cycle time (delivery nhanh hơn), có hai đòn bẩy:

1. **Tăng throughput** — thường khó và tốn kém (thêm người, tối ưu process)
2. **Giảm WIP** — thường dễ hơn và có hiệu quả ngay lập tức

Đây là nền tảng lý thuyết cho việc giới hạn WIP trong Kanban và Scrum. Một team nhận quá nhiều việc cùng lúc không thể deliver nhanh hơn — họ chỉ tăng cycle time.

## Tính phổ quát

Little's Law áp dụng cho _bất kỳ hệ thống ổn định nào_: hàng đợi tại siêu thị, request trong microservice, feature trong sprint backlog. Điều kiện duy nhất là hệ thống phải ở trạng thái steady-state (arrival rate ≈ departure rate).

## Connections

- [[permanent/kingmans-formula]] — Kingman chi tiết hóa Little's Law cho trường hợp queue với variability
- [[permanent/flow-framework]] — Flow Load = L (WIP), Flow Time = W, Flow Velocity = λ

## Sources

- [[refs/little_s_law]]
