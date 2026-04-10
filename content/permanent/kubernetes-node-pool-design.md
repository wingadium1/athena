---
title: "Kubernetes Node Pool Design"
aliases: ["K8s node pool", "kubernetes workload segregation", "taint toleration affinity"]
tags: [kubernetes, cloud, cost-optimization, infrastructure]
created: 2026-04-10
updated: 2026-04-10
---

Node pool design là việc tổ chức các node trong Kubernetes cluster thành các nhóm chuyên biệt, mỗi nhóm có phần cứng và cấu hình phù hợp với loại workload cụ thể. Mục tiêu: **chạy đúng pod trên đúng node**, tránh dồn dập tài nguyên (free-for-all scheduling) gây lãng phí chi phí và ảnh hưởng hiệu suất.

## Cơ chế scheduling

Kubernetes cung cấp hai bộ công cụ bổ trợ nhau:

### Taints & Tolerations

- **Taint** đặt trên node: "node này chỉ chấp nhận pod có đặc điểm nhất định"
- **Toleration** đặt trên pod/workload: "pod này chấp nhận được điều kiện của node kia"
- Node _không có taint_ có thể nhận bất kỳ pod nào kể cả pod có toleration
- Node _có taint_ chỉ nhận pod có toleration khớp key-value

```yaml
# Ví dụ: taint node GPU
kubectl taint nodes gpu-node-1 type=gpu:NoSchedule

# Toleration tương ứng trên pod
tolerations:
- key: "type"
  operator: "Equal"
  value: "gpu"
  effect: "NoSchedule"
```

### Affinity & Anti-Affinity

- **Node affinity**: pod ưu tiên hoặc yêu cầu chạy trên node có label nhất định
- **Pod affinity**: pod muốn chạy _gần_ pod khác (cùng node/zone) — giảm network latency giữa các service hay giao tiếp nhiều
- **Pod anti-affinity**: pod muốn chạy _xa_ pod khác — tăng HA, tránh SPOF

## Phân loại node pool điển hình

| Node Pool        | Hardware            | Workload                   | Chi phí               |
| ---------------- | ------------------- | -------------------------- | --------------------- |
| General purpose  | vCPU + RAM cân bằng | Stateless microservices    | Trung bình            |
| CPU-optimized    | High vCPU/RAM ratio | Compute-heavy workloads    | Cao                   |
| Memory-optimized | High RAM            | In-memory DB, caching      | Cao                   |
| GPU              | GPU + vCPU          | ML/AI inference/training   | Rất cao               |
| Spot/Preemptible | Bất kỳ              | Batch jobs, non-critical   | Thấp (60-80% cheaper) |
| Stateful         | Local SSD, stable   | Databases, Cassandra, etc. | Trung bình-cao        |

## Cost optimization với node pools

- **Spot/Preemptible nodes** cho batch jobs và non-critical workloads — tiết kiệm 60-80% so với on-demand
- **Resource Quotas + LimitRanges** ngăn một workload chiếm dụng quá nhiều tài nguyên
- **HPA (Horizontal Pod Autoscaler)** + **VPA (Vertical Pod Autoscaler)** để right-size
- **Cluster Autoscaler**: tự động thêm/xóa node dựa trên demand — cần kết hợp với overprovisioning buffer
- Affinity rules giảm **hidden network cost**: related microservices trên cùng host/zone ↓ cross-AZ traffic

## Connections

- [[permanent/kubernetes-node-consolidation]] — node pool design là tiền đề; consolidation là bước tối ưu tiếp theo
- [[permanent/multi-cloud-architecture]] — trong multi-cloud, node pool strategy có thể khác nhau theo provider
- [[permanent/resilience-vs-robustness]] — stateful app cần node pool riêng biệt ổn định để đảm bảo data consistency

## Sources

- `writing/node_pool_design_for_k8s.md`
- `writing/optimize_cost_k8s.md`
