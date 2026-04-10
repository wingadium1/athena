---
title: "Zero Trust Network in Kubernetes"
aliases: ["zero trust k8s", "mTLS kubernetes", "service mesh security", "Calico Istio zero trust"]
tags: [kubernetes, security, networking, service-mesh, zero-trust]
created: 2026-04-10
updated: 2026-04-10
---

Zero Trust Network là mô hình bảo mật theo nguyên tắc **"never trust, always verify"** — không service nào được tin tưởng mặc định chỉ vì nó đang chạy trong cùng cluster hay network. Mọi kết nối đều phải được xác thực và ủy quyền, bất kể nguồn gốc.

Trong Kubernetes, zero trust được thực hiện bằng cách kết hợp **network policy** (layer 3/4) với **service mesh mTLS** (layer 7), sử dụng **workload identity** thay vì IP address để xác định nguồn gốc request.

## Tại sao cần Zero Trust trong K8s

Default Kubernetes: mọi pod trong cluster có thể giao tiếp với nhau. Nếu một pod bị compromise, attacker có thể lateral move sang toàn bộ cluster. Zero trust giới hạn blast radius — service A chỉ được nói chuyện với service B theo đúng protocol đã định nghĩa.

## Hai approach chính

### Approach 1: Calico + Istio (sidecar mode)

**Calico** xử lý network policy (L3/L4), **Istio** xử lý mTLS và authorization (L7).

1. Cài Calico + enable Policy Sync API trên Felix
2. Cài Istio, bật `PeerAuthentication` ở mode **STRICT** toàn cluster:

```yaml
apiVersion: security.istio.io/v1beta1
kind: PeerAuthentication
metadata:
  name: default-strict-mode
  namespace: istio-system
spec:
  mtls:
    mode: STRICT
```

3. Label namespace để inject Istio sidecar
4. Thiết lập **workload identity** qua Kubernetes ServiceAccounts
5. Viết **allow-list policies** theo ServiceAccount (deny-all by default, allow theo principle of least privilege):

```yaml
# Chỉ ServiceAccount 'customer' được vào 'summary'
apiVersion: projectcalico.org/v3
kind: GlobalNetworkPolicy
spec:
  selector: app == 'summary'
  ingress:
    - action: Allow
      source:
        serviceAccounts:
          names: ["customer"]
```

### Approach 2: Istio Ambient Mode

Ambient mode loại bỏ sidecar proxy — thay vào đó dùng **ztunnel** (node-level) cho mTLS L4, và **waypoint proxy** (optional) cho L7 policy. Nhẹ hơn về resource overhead so với sidecar.

1. Cài Calico (CNI) + Istio ambient
2. Label namespace: `istio.io/dataplane-mode=ambient`
3. Define `AuthorizationPolicy` kiểm soát traffic theo **principals** (ServiceAccount SPIFFE URI):

```yaml
apiVersion: security.istio.io/v1
kind: AuthorizationPolicy
spec:
  selector:
    matchLabels:
      app: httpbin
  action: ALLOW
  rules:
    - from:
        - source:
            principals:
              - cluster.local/ns/default/sa/sleep # chỉ sleep ServiceAccount
```

## Thành phần cốt lõi

| Thành phần                                 | Vai trò                                            |
| ------------------------------------------ | -------------------------------------------------- |
| **Calico NetworkPolicy**                   | L3/L4 ingress/egress control                       |
| **Istio PeerAuthentication (STRICT mTLS)** | Encrypt tất cả traffic, xác thực certificate       |
| **Istio AuthorizationPolicy**              | L7 allow/deny theo identity, method, path          |
| **Kubernetes ServiceAccount**              | Workload identity — identity nguồn gốc của request |
| **SPIFFE/SPIRE**                           | Certificate issuance cho workload identity         |

## Nguyên tắc triển khai

- **Deny-all by default** — bắt đầu từ zero, explicit allow từng connection
- **Identity-based, not IP-based** — IP thay đổi liên tục trong K8s; ServiceAccount là stable identity
- **Least privilege** — mỗi service chỉ nhận traffic từ đúng callers cần thiết
- **Encrypt in transit** — mTLS STRICT trên tất cả service-to-service communication

## Connections

- [[permanent/kubernetes-node-pool-design]] — security policy cần kết hợp với node pool design (GPU/sensitive workloads trong isolated pool)
- [[permanent/multi-cloud-architecture]] — trong multi-cluster, zero trust phức tạp hơn: cần cross-cluster identity federation
- [[permanent/container-runtime-security-falco]] — Falco bổ sung zero trust ở tầng runtime syscall detection

## Sources

- `writing/setup_zero_trust_network_with_calico.md`
- `writing/setup_zero_trust_network_with_istio_ambient.md`
