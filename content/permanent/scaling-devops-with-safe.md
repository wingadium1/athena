---
title: "Scaling DevOps với SAFe"
aliases: ["SAFe DevOps scaling", "DevOps at scale SAFe"]
tags: [devops, safe, agile, scaling]
created: 2026-04-10
updated: 2026-04-10
---

[[permanent/safe|SAFe]] cung cấp một bộ khung để scale DevOps lên cấp độ tổ chức, không chỉ dừng ở team-level CI/CD. Điều này quan trọng vì DevOps ở quy mô nhỏ thường thành công nhờ culture và communication trực tiếp — nhưng khi tổ chức lớn lên, cần cấu trúc rõ ràng hơn.

**4 cấu hình SAFe**: Essential → Large Solution → Portfolio → Full SAFe. Hầu hết tổ chức bắt đầu với Essential SAFe — khối nền tảng đơn giản nhất, vận hành quanh một [[permanent/safe-agile-release-train|Agile Release Train (ART)]].

**Cấu trúc ART**: Team 5–12 người (Scrum Master + Product Owner + members) tham gia vào ART — một "team của các team". Ba role cốt lõi điều phối ART:

- **Release Train Engineer (RTE)**: Chief Scrum Master của ART, loại bỏ blockers, đảm bảo ART vận hành.
- **Product Management (PM)**: Định hướng phát triển sản phẩm, duy trì tầm nhìn, quản lý feature backlog và prioritization.
- **System Architect (SA)**: Cân bằng kiến trúc hiện tại với thiết kế mới của các team; đầu mối kỹ thuật cho cả ART.

**Công việc được đóng khung trong [[permanent/safe-planning-interval|Program Increments (PI)]]** (8–12 tuần), chia thành sprint, và breakdown thành stories — tương tự cách Scrum team làm nhưng ở quy mô lớn hơn.

**DevOps trong SAFe được tiếp cận qua 4 khía cạnh:**

1. Mô hình hóa cách tiếp cận DevOps bằng [[permanent/calmr|CALMR]] (Culture, Automation, Lean Flow, Measurement, Recovery)
2. Thiết lập và duy trì value streams
3. Áp dụng CD pipeline vào value stream
4. Tích hợp built-in quality và security vào quá trình phát triển

## Connections

- [[permanent/safe]] — SAFe là framework chứa cách tiếp cận scaling DevOps này
- [[permanent/safe-agile-release-train]] — ART là đơn vị vận hành chính trong Essential SAFe
- [[permanent/safe-planning-interval]] — PI là khung thời gian tổ chức công việc
- [[permanent/calmr]] — CALMR là mô hình DevOps chính thức trong SAFe
- [[permanent/safe-value-stream]] — Value stream là đơn vị tổ chức công việc ở cấp cao hơn ART
- [[permanent/devops]] — DevOps practices được scale thông qua SAFe

## Sources

- [[refs/scaling_devops_with_safe]] (archived)
