---
title: "Multi-Cloud Architecture"
aliases: ["multi cloud", "hybrid cloud", "multi-cloud k8s", "cloud portability"]
tags: [cloud, architecture, kubernetes, infrastructure, multi-cloud]
created: 2026-04-10
updated: 2026-04-10
---

Multi-cloud architecture là chiến lược triển khai hệ thống trên **nhiều cloud provider** (hoặc private cloud + public cloud) đồng thời, thay vì phụ thuộc vào một nhà cung cấp duy nhất. Mỗi cloud được giao vai trò phù hợp với thế mạnh của nó.

## Tại sao Multi-Cloud

- **Risk Mitigation**: tránh vendor lock-in và single point of failure ở tầng infrastructure
- **Cost Optimization**: chọn compute rẻ nhất cho từng workload (ví dụ: private cloud compute ~1/3 giá AWS, nhưng storage private cloud đắt hơn AWS ~60%)
- **Regulatory Compliance**: đặt dữ liệu theo region/jurisdiction phù hợp với luật địa phương (xem [[permanent/china-data-regulation]])
- **Best-of-breed capabilities**: tận dụng dịch vụ đặc thù của từng provider (AWS SageMaker, GCP BigQuery, Azure Active Directory)

## Distribution Strategy điển hình

Pattern phổ biến nhất: **Private Cloud làm primary, Public Cloud làm secondary/burst**:

| Workload                              | Cloud              | Lý do                      |
| ------------------------------------- | ------------------ | -------------------------- |
| Compute-intensive, latency-sensitive  | Private Cloud      | Chi phí compute thấp hơn   |
| Data phải ở trong region (compliance) | Private Cloud      | Data residency requirement |
| Stateless apps cần auto-scaling       | Public Cloud (AWS) | Elastic scaling dễ hơn     |
| Storage-heavy workloads               | Public Cloud       | Chi phí storage thấp hơn   |
| DR / Disaster Recovery site           | Public Cloud       | Geographic separation      |
| Database replicas cho HA              | Public Cloud       | Redundancy                 |

## Kubernetes là nền tảng multi-cloud

K8s giữ vai trò trung tâm vì cung cấp một lớp trừu tượng **cloud-agnostic**:

- **Portability**: workload chạy trên bất kỳ K8s-compatible infrastructure nào
- **Consistent deployment model**: cùng Helm chart, cùng YAML manifest chạy trên private và AWS
- **Service Discovery**: uniform naming và routing bất kể underlying cloud

## Thành phần kỹ thuật cốt lõi

### Networking

Kết nối cross-cloud cần đảm bảo hiệu suất và bảo mật:

- **Istio** service mesh cho east-west traffic encryption và observability
- **VPN / MPLS / SD-WAN** cho connectivity giữa private cloud và public cloud
- Cân nhắc bandwidth cost của cross-cloud traffic (egress fees)

### Storage

Cloud-native distributed storage để abstracted khỏi underlying provider:

- **Portworx** — enterprise K8s storage, hỗ trợ multi-cloud data replication
- **Ceph** — open-source distributed storage, phù hợp private cloud

### GitOps / Deployment

Quản lý deployment đồng nhất trên nhiều cluster:

- **ArgoCD** hoặc **Flux** — declarative GitOps, sync desired state từ Git sang mọi cluster
- Terraform — infrastructure as code đồng nhất across clouds

### Observability

Centralized monitoring bất kể cloud:

- **Prometheus + Grafana** — metrics
- **ELK stack** — logs
- **Rancher / Google Anthos / Azure Arc** — multi-cluster management plane

### Security

- [[permanent/zero-trust-network-kubernetes]] — mTLS và identity-based policies cần hoạt động cross-cluster
- Cross-cloud security fabric: RBAC đồng nhất, secret management tập trung

## Thách thức thực tế

- **Operational complexity**: mỗi cloud có quirks riêng; team cần expertise cho tất cả
- **Security consistency**: đảm bảo policy đồng nhất giữa private và public cloud
- **Cost visibility**: khó track total cost khi bill đến từ nhiều nguồn
- **Stateful workloads**: database replication cross-cloud phức tạp, latency cao hơn

## Triển khai theo giai đoạn

1. **Assessment & Planning** — đánh giá workload, xác định distribution strategy
2. **Tool selection** — chọn management, CI/CD, monitoring, security stack
3. **PoC** — validate connectivity, performance, cost model trên small scale
4. **Migration & Deployment** — migrate từng workload theo risk profile
5. **Optimize & Improve** — right-sizing, cost optimization, automation

## Connections

- [[permanent/kubernetes-node-pool-design]] — node pool design áp dụng trong từng cluster của multi-cloud setup
- [[permanent/zero-trust-network-kubernetes]] — security model cần được mở rộng cross-cluster trong multi-cloud
- [[permanent/china-data-regulation]] — compliance requirements thường là driver chính của multi-cloud/hybrid cloud decisions
- [[permanent/continuous-delivery]] — GitOps (ArgoCD/Flux) là cách CD hoạt động trong multi-cluster

## Sources

- `writing/multi_cloud_setup.md`
- `writing/multi cloud arch/overview_approach.md`
