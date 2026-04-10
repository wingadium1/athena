---
title: "DevOps Transformation Canvas"
aliases: ["DevOps Canvas", "Transformation Canvas"]
tags: [devops, safe, value-stream, continuous-exploration]
created: 2026-04-10
updated: 2026-04-10
---

DevOps Transformation Canvas là công cụ workshop trong phần Continuous Exploration của SAFe, giúp tổ chức visualize và cải thiện value stream end-to-end. Canvas kết hợp Value Stream Mapping với các mục tiêu transformation cụ thể.

**9 thành phần của Canvas:**

- **Value Stream**: Chuỗi các bước deliver value từ ý tưởng đến khách hàng
- **Trigger**: Sự kiện khởi động value stream (ý tưởng mới, feature request)
- **First Step**: Bước đầu tiên — thường là phân tích và prioritize yêu cầu, định nghĩa feature
- **Last Step**: Bước cuối — deliver sản phẩm đến tay khách hàng, đo lường giá trị
- **Demand Rate**: Tần suất yêu cầu công việc mới trong một khoảng thời gian
- **Current State**: Bức tranh hiện tại của value stream với các metrics đo lường
- **Future State**: Trạng thái mục tiêu sau khi cải thiện
- **Boundaries and Limitations**: Các ràng buộc về tổ chức, kỹ thuật, hay quy trình
- **Improvement Items**: Danh sách hành động cụ thể để đạt Future State

**Quy trình sử dụng Canvas** gồm hai giai đoạn:

_Giai đoạn 1 — Current State analysis_: Đo Value Stream metrics hiện tại bằng ba chỉ số từ [[permanent/value-stream-mapping|VSM]]:

- **Process Time (PT)**: Thời gian thực sự làm việc
- **Lead Time (LT)**: Tổng thời gian từ trigger đến completion
- **%C&A (Percent Complete and Accurate)**: Tỉ lệ output đúng ngay lần đầu, không cần rework

Từ đó xác định bottleneck trong flow.

_Giai đoạn 2 — Future State design_: Dựa trên bottleneck, đặt ra Improvement Items. Các câu hỏi dẫn dắt:

- Làm sao loại bỏ bottleneck đáng kể nhất?
- Cái gì có thể tự động hóa?
- Rủi ro nào có thể phòng ngừa trước?

First Step và Last Step đều kết nối về phía khách hàng, nhấn mạnh rằng mọi optimization phải được đo bằng giá trị thực sự delivered — không chỉ là tốc độ nội bộ.

## Connections

- [[permanent/value-stream-mapping]] — Canvas dựa trên VSM methodology và metrics (PT, LT, %C&A)
- [[permanent/safe-value-stream]] — Value stream là đơn vị phân tích chính của Canvas
- [[permanent/safe]] — Canvas xuất phát từ SAFe Continuous Exploration practice
- [[permanent/flow-framework]] — Flow metrics bổ sung cho góc nhìn Canvas về throughput và delivery
- [[permanent/cost-of-delay]] — Bottleneck identification trong Canvas có thể được định lượng bằng Cost of Delay

## Sources

- [[refs/devops_transforation_canvas]] (archived)
- [[refs/safe_value_stream_mapping]] (archived)
