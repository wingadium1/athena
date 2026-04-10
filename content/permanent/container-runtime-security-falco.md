---
title: "Container Runtime Security with Falco"
aliases:
  ["Falco", "runtime security", "container runtime monitoring", "syscall monitoring kubernetes"]
tags: [kubernetes, security, devops, monitoring, devsecops]
created: 2026-04-10
updated: 2026-04-10
---

Container runtime security là **tuyến phòng thủ cuối cùng** trong Kubernetes security. Mọi kiểm soát bảo mật pre-deploy (image scanning, RBAC, network policy, mTLS) đều xảy ra _trước khi_ container chạy — chúng không thể phát hiện những gì xảy ra bên trong container đang hoạt động. Runtime monitoring lấp đầy khoảng trống này.

**Nguyên lý**: dù pre-deploy controls tốt đến đâu, breach vẫn sẽ xảy ra. Mục tiêu không phải là ngăn chặn hoàn toàn mà là **phát hiện sớm và phản ứng nhanh** — thời gian là yếu tố tối quan trọng.

## Tại sao syscalls?

Mỗi khi container mở file, thiết lập kết nối mạng, hoặc spawn process — nó thực hiện **system call** thông qua isolated namespace đến kernel. Tất cả các hành động này đều có liên quan đến bảo mật. Bằng cách giám sát syscalls ở tầng kernel, ta có cái nhìn đầy đủ về mọi thứ container làm, bất kể application-level logic.

## Falco: How It Works

[Falco](https://falco.org) (Sysdig) là công cụ open-source cho container runtime security. Kiến trúc:

1. **Kernel module** (hoặc eBPF probe) lắng nghe tất cả syscall events trên node
2. Falco daemon nhận events và đối chiếu với **rule set** đã định nghĩa
3. Khi match rule → sinh ra **security alert** (stdout, file, webhook, Falcosidekick...)
4. Ngoài syscalls, Falco cũng audit **Kubernetes API events**: pod creation/deletion, ConfigMap/Secret changes

Events đáng ngờ điển hình mà Falco phát hiện:

- Shell spawned inside container (`/bin/bash` trong production container)
- Unexpected network connections từ container
- Sensitive file access (đọc `/etc/passwd`, `/etc/shadow`)
- Privilege escalation attempts
- Crypto mining processes (Falco có documented cases phát hiện mining pods trong K8s clusters)

## Tích hợp với security stack

```
Syscalls → Kernel module → Falco → Falcosidekick → Slack/PagerDuty/SIEM
                                               → Grafana (visualization)
                                               → Incident response automation
```

Kết hợp Falco với Grafana tạo thành một DevSecOps observability stack đầy đủ: infrastructure metrics (Prometheus) + application logs + **security events** (Falco).

## Vai trò trong layered security

| Layer               | Tool          | Mục tiêu                            |
| ------------------- | ------------- | ----------------------------------- |
| Pre-deploy: image   | Trivy, Snyk   | Phát hiện CVE trong image           |
| Pre-deploy: network | Calico, Istio | Giới hạn traffic giữa services      |
| Pre-deploy: access  | RBAC, OPA     | Kiểm soát quyền K8s API             |
| **Runtime**         | **Falco**     | **Phát hiện anomaly khi đang chạy** |

Falco không thay thế các lớp trên — nó bổ sung bằng cách cover những gì các lớp trên không thể thấy.

## Connections

- [[permanent/zero-trust-network-kubernetes]] — Falco là runtime complement của zero trust network: network policy kiểm soát traffic, Falco kiểm soát behavior bên trong container
- [[permanent/site-reliability-engineering]] — runtime security alerts cần được xử lý trong incident response flow như mọi operational alert khác
- [[permanent/resilience-vs-robustness]] — runtime monitoring là một phần của "Monitoring" trong Resilience Engineering (Hollnagel's 4 capabilities)

## Sources

- `writing/falco.md`
- [Falco detect cryptomining](https://falco.org/blog/falco-detect-cryptomining/)
