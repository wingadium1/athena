---
title: "Theory of Constraints (TOC)"
aliases: ["TOC", "Lý thuyết ràng buộc"]
tags: [lean, flow, management, optimization, devops]
created: 2026-04-10
updated: 2026-04-10
---

Theory of Constraints (TOC) là mô hình quản lý do Eliyahu Goldratt phát triển, dựa trên tiền đề: **bất kỳ hệ thống hướng mục tiêu nào cũng bị giới hạn bởi ít nhất một ràng buộc (constraint)**. Nếu không có ràng buộc thì throughput của hệ thống sẽ là vô hạn — điều không thể xảy ra trong thực tế.

Hệ quả quan trọng: **chỉ tăng throughput qua chính constraint thì mới tăng được throughput tổng thể.** Tối ưu mọi thứ khác trước khi xử lý constraint là lãng phí.

## 5 bước tập trung (Five Focusing Steps)

1. **Xác định constraint**: Tìm bottleneck — bước nào đang giới hạn throughput của toàn hệ thống?
2. **Khai thác constraint**: Tối đa hóa throughput qua constraint hiện tại mà không cần đầu tư thêm (giảm downtime, loại bỏ rework, ưu tiên đúng việc)
3. **Subordinate everything else**: Mọi bước khác phải phục vụ constraint — đừng tối ưu non-constraint đến mức tạo thêm WIP chất đống trước constraint
4. **Vượt qua constraint**: Nếu bước 2–3 chưa đủ, đầu tư thêm capacity cho constraint (thuê người, mua thiết bị, refactor code)
5. **Lặp lại — nhưng cảnh giác**: Sau khi constraint bị phá vỡ, một constraint mới sẽ xuất hiện ở nơi khác. Quay lại bước 1. **Không để quán tính cũ trở thành constraint mới.**

## Liên hệ với DevOps và Lean

TOC là nền tảng lý thuyết đằng sau nhiều practices trong [[permanent/flow-framework|Flow Framework]] và [[permanent/value-stream-mapping|Value Stream Mapping]]. Khi VSM xác định được bottleneck (bước có Lead Time cao nhất hoặc %C&A thấp nhất), đó chính là constraint theo nghĩa của TOC.

[[permanent/little-s-law|Little's Law]] và [[permanent/kingmans-formula|Kingman's Formula]] đều mô tả cùng hiện tượng từ góc độ toán học: utilization cao tại constraint → wait time tăng phi tuyến → toàn bộ system chậm lại.

## Connections

- [[permanent/value-stream-mapping]] — VSM là công cụ identify constraint trong software delivery
- [[permanent/flow-framework]] — Flow metrics đo throughput qua các constraint trong value stream
- [[permanent/kingmans-formula]] — VUT equation mô tả tác động của constraint khi utilization cao
- [[permanent/little-s-law]] — L = λW: WIP tích lũy tại constraint là biểu hiện của bottleneck
- [[permanent/cost-of-delay]] — constraint trực tiếp gây ra Cost of Delay vì nó trì hoãn value delivery
- [[permanent/continuous-learning-culture]] — "Optimize the whole" trong Relentless Improvement là biểu hiện của TOC thinking

## Sources

- [[refs/theory_of_constraints]] (archived)
