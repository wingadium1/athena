---
title:  "Multi Cloud Setup"
date:   2024-06-16 12:00:00
tags: [Cloud, k8s, type/write]
---

# Appeal
* Risk Mitigation
* Cost Optimize
* Regulatory Compliance: phép các tổ chức đáp ứng các yêu cầu quy định đa dạng bằng cách định vị chiến lược các dịch vụ và dữ liệu ở các khu vực khác nhau: ví dụ phân vùng bắc nam cho các công ty toàn quốc.
* Innovation and Flexibility

## k8s
K8S ở đây đóng vai trò quan trọng, gần như là một framework cung cấp cho chúng ta container orchestration trên bất kì cơ sở hạ tầng nào. Các đặc tính vượt trội của nó hỗ trợ triển khai nhiều multi cloud:
  * Portability
  * Scalability
  * Service Discovery and Load Balancing

# Các ưu điểm khác Multi Cloud
* Strategic Agility: tất nhiên rồi, chúng ta sẽ có một cách tiếp cận linh hoạt hơn nhiều
* Competitive Advantage: thực ra sẽ đúng nếu chúng ta chọn 1 cloud public trong số: AWS, GCP, Azure, Alibaba, các cloud này có các dịch vụ quá vượt trội so với các loại khác, qua đó chúng ta tận dụng các công nghệ tiên tiến từ các nhà cung cấp khác nhau và duy trì lợi thế cạnh tranh.

# Thách thức
* Vận hành phức tạp:
* Tính đồng nhất về bảo mật:

# Cân nhắc về kiến trúc
## Quản lý tập trung và thống nhất về theo dõi (Observability)

Management Tool:
  * Rancher
  * Google Anthos
  * Azure Arc

## Networking

Việc kết nối giữa các nền tảng cloud cũng rất quan trọng phải đảm bảo hiệu suất, bảo mật

Tool:
  * Istio:

## Security / Compliance
## Data Management và Storage

Ưu tiên sử dụng các công cụ Cloud Native:
  * Portworx
  * Ceph

# Deep dive planning
Step:
  1. Assessment và Planning
  2. Lựa chọn tools: Management, CICD, Monitoring, Security
  3. PoC
  4. Migration - Deployment
  5. Optimize - Improvement

## Business Assesment
## Selecting Vendor: Compare
## Architectural Blueprints
* Networking Architectures
* Data Strategy

## Cluster Configuration
* HA
* Multi Tenant
* Auto Scaling (node pool designs, taints, tolerations)

### Detail about networking and security
* Cross-Cloud Security Fabric
* Hybrid Connectivity Models:
  * VPN
  * MPLS
  * SD-WAN

### Deployment Strategies
GitOps:
  * ArgoCD
  * Flux
### Operational Management
### Observability and Monitoring
Cross-Cloud Logging and Monitoring:
  * Prometheus
  * ELK

### Stages
Tools:
  * k8s
  * terraform
  * helm

# Future-Proofing and Optimization
## Cost Optimization
## Scaling and Evolving the Multi-Cloud Ecosystem
* Cluster Federation and Management
* Emerging Technologies and Trends
## Automated Scaling and Resource Optimization
* HPA
* VPA
## Networking Across Cloud

## Unified Multi-Cloud Management
## Overcoming Challenges in Multi-Cloud Kubernetes
Complexity in Management
Security and Compliance
Cost Management

## Future Trends in Multi-Cloud Kubernetes
* AI and Machine Learning
* Serverless Kubernetes
* Sustainability in Cloud Computing

