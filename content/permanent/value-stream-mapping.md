---
title: "Value Stream Mapping (VSM)"
aliases: ["VSM", "value stream mapping", "bản đồ chuỗi giá trị"]
tags: [lean, safe, flow, process-improvement, metrics]
created: 2026-04-10
updated: 2026-04-10
---

Value Stream Mapping là công cụ trực quan từ Lean, dùng để phân tích toàn bộ dòng chảy của công việc — vật liệu, thông tin, và các hoạt động — từ đầu đến cuối một value stream. Mục tiêu là xác định waste (lãng phí), nút thắt cổ chai, và cơ hội cải thiện.

VSM buộc team nhìn end-to-end thay vì chỉ tối ưu từng bước riêng lẻ (local optimization). Một bước được tối ưu mà không nhìn toàn hệ thống có thể thực ra làm chậm toàn bộ flow.

## 4 thành phần chính của VSM

1. **Steps/Activities** — các bước trong quá trình
2. **Materials Flow** — luồng vật liệu/sản phẩm qua các bước
3. **Information Flow** — dữ liệu và tín hiệu điều phối process
4. **Metrics** — lead time, cycle time, WIP, %C&A tại mỗi bước

## VSM Metrics

| Metric                | Định nghĩa                                                            |
| --------------------- | --------------------------------------------------------------------- |
| **Lead Time (LT)**    | Tổng thời gian từ request đến delivery (bao gồm wait time)            |
| **Cycle Time (CT)**   | Thời gian hoàn thành một đơn vị công việc tại một bước cụ thể         |
| **Process Time (PT)** | Thời gian làm việc thực tế (không tính chờ đợi)                       |
| **%C&A**              | Tỉ lệ output của một bước được nhận đúng và đầy đủ bởi bước tiếp theo |

### Value Stream metrics (tổng hợp)

- **Total Process Time** = tổng PT của tất cả các bước
- **Total Lead Time** = tổng LT của tất cả các bước
- **Activity Ratio** = Total Process Time / Total Lead Time (lý tưởng → 1)
- **Rolled %C&A** = %C&A₁ × %C&A₂ × ... × %C&Aₙ (tích lũy quality loss)

Activity Ratio thấp có nghĩa là phần lớn thời gian là wait time, không phải active work — đây là nơi cần cải thiện.

## 5 bước thực hiện VSM

1. Define scope — chọn product/service/process cụ thể
2. Map current state — ghi lại flow hiện tại với metrics thực tế
3. Analyze for waste — xác định delays, redundancies, non-value-added activities
4. Design future state — thiết kế flow tối ưu hóa
5. Implement and monitor — triển khai và theo dõi improvement

## Connections

- [[permanent/safe-value-stream]] — VSM là công cụ để visualize và cải thiện SAFe value streams
- [[permanent/devops-transformation-canvas]] — Canvas dùng VSM metrics làm nền tảng
- [[permanent/little-s-law]] — Activity Ratio liên quan trực tiếp đến Flow Efficiency trong Little's Law
- [[permanent/flow-framework]] — Flow Metrics là phiên bản hiện đại của VSM metrics cho software

## Sources

- [[refs/safe_value_stream_mapping]]
- [[refs/safe_metrics_in_value_stream_mapping]]
