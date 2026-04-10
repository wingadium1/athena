---
title: "Kubernetes Node Consolidation"
aliases: ["node consolidation k8s", "kubernetes cost optimization", "cluster right-sizing"]
tags: [kubernetes, cloud, cost-optimization, infrastructure]
created: 2026-04-10
updated: 2026-04-10
---

Node consolidation là chiến lược phân phối workload lên **số node tối thiểu cần thiết**, giảm resource footprint và chi phí cloud. Core idea: thay vì có nhiều node nhỏ với utilization thấp, gom workload lại để tận dụng tốt hơn capacity hiện có.

**Tại sao làm điều này?** Ít node = ít tiền. Mỗi node idle vẫn tốn phí. Với large cluster, ngay cả 10-20% over-provisioning có thể đại diện cho hàng nghìn USD/tháng.

## Ba bước triển khai

**Step 1: Cluster Autoscaler** — thiết lập tự động scale số node theo demand thực tế. Không để cluster "fat" bằng node nhàn rỗi.

**Step 2: Overprovisioning buffer** — thêm một lượng tài nguyên dự phòng nhỏ (ví dụ: 1-2 "phantom" pod với priority thấp chiếm tài nguyên, bị evict khi workload thật cần). Đây là "bảo hiểm" cho các spike load bất ngờ khi Cluster Autoscaler chưa kịp thêm node.

**Step 3: Monitor and Adjust** — theo dõi utilization thực tế, điều chỉnh node pool size và resource requests/limits. Tools như Kubecost cho visibility về waste.

## Giới hạn và trade-off

### Workload biến đổi nhiều

Consolidation giả định workload có thể dự đoán được. Với workload unpredictable (đặc biệt feature mới release), autoscaling cần thời gian spin up node mới — spike load có thể gây domino failure. **Giải pháp**: không áp dụng consolidation cho new/unstable workloads; tách chúng vào node pool riêng với overprovisioning.

### Stateful applications

Consolidate aggressive quá với stateful apps (Cassandra, PostgreSQL...) tăng nguy cơ mất dữ liệu khi rescheduling sau node failure. **Giải pháp**: luôn giữ stateful app trong node pool riêng, ổn định, ít thay đổi.

### Resource fragmentation

HPA/VPA + consolidation đôi khi để lại các "mảnh" tài nguyên nhỏ rải rác — tổng cluster dường như có đủ resource nhưng không pod nào schedule được vì mỗi node đơn lẻ không đủ. **Tác động**: scaling failure silent. Với large cluster, fragmentation thường chấp nhận được; với small cluster, cần watch.

### Network latency sau consolidation

Khi gom nhiều workload lên ít node hơn, network topology thay đổi — pod thường xuyên giao tiếp nhau có thể bị đặt xa nhau hơn. Kết hợp với [[permanent/kubernetes-node-pool-design]] affinity rules để giảm vấn đề này.

### Delay scaling

HPA/VPA phản ứng sau khi load đã tăng — không proactive. Slow scale-up ảnh hưởng performance; slow scale-down chỉ ảnh hưởng cost.

## Cost vs Performance trade-off

Không có điểm tối ưu tuyệt đối. Thực tế là: consolidation mạnh tay → chi phí thấp nhưng SLA risk cao hơn. Với critical workloads có strict SLA, chấp nhận một mức over-provisioning nhất định như một chi phí bảo hiểm có chủ ý.

## Connections

- [[permanent/kubernetes-node-pool-design]] — node pool design là tiền đề cho consolidation; mỗi pool có strategy consolidation khác nhau
- [[permanent/resilience-vs-robustness]] — aggressive consolidation làm giảm Resilience; cần cân bằng MTTR vs cost
- [[permanent/theory-of-constraints]] — bottleneck node sau consolidation có thể trở thành constraint của toàn cluster

## Sources

- `writing/node_consolidation.md`
- `writing/optimize_cost_k8s.md`
