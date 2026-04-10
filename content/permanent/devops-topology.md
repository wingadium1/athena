---
title: "DevOps Topology"
aliases: ["DevOps team topology", "DevOps org patterns"]
tags: [devops, engineering-culture, team-topology, organization]
created: 2026-04-10
updated: 2026-04-10
---

DevOps Topology là framework phân loại các cách tổ chức nhóm Dev và Ops, được Matthew Skelton mô tả năm 2013. Framework định nghĩa cả **anti-types** (patterns cần tránh) lẫn **beneficial topologies** (structures có thể hoạt động tốt). Hiện tại có 8 anti-types và 9 topology có ích.

Điểm cốt lõi: không có một topology nào phù hợp với tất cả tổ chức. Lựa chọn phụ thuộc vào quy mô, sản phẩm, và mức độ trưởng thành kỹ thuật. Một số topology chỉ phù hợp như bước chuyển tiếp (ví dụ "DevOps team with expiration date").

## 8 Anti-types cần tránh

1. Dev and Ops Silos — hai nhóm hoàn toàn tách biệt
2. Permanent DevOps Team Silo — DevOps team trở thành silo mới
3. Dev Doesn't Need Ops — Dev bỏ qua Ops hoàn toàn
4. DevOps as Tool Team — DevOps team chỉ lo tooling
5. Rebranded Sysadmins — Ops đổi tên thành DevOps nhưng không thay đổi gì
6. Ops Embedded in Dev Team — Ops bị cô lập trong từng team nhỏ
7. Dev and DBA Silos — tách biệt giữa Dev và DBA
8. Fake SRE — đặt tên SRE nhưng vẫn là Ops truyền thống

## 9 Beneficial Topologies

1. **Dev and Ops Collaboration** — lý tưởng nhất, yêu cầu thay đổi văn hóa lớn
2. **Fully Shared Ops** — Dev tự quản lý operations (Netflix, Facebook model)
3. **Ops as IaaS** — Ops là platform provider, Dev tự deploy
4. **DevOps as External Service** — outsource DevOps để học rồi internalize
5. **DevOps Team (with expiration date)** — team bridge tạm thời, sau đó giải thể
6. **DevOps Advocacy Team** — facilitator liên tục giữa Dev và Ops
7. **SRE Team** — model của Google: SRE nhận app từ Dev khi đạt threshold chất lượng
8. **Container-driven Collaboration** — container trừu tượng hóa infra, giảm sự cần thiết của Ops
9. **Dev and DBA Collaboration** — cho hệ thống database-centric

## Connections

- [[permanent/devops]] — topology là cách implement DevOps culture
- [[permanent/site-reliability-engineering]] — SRE Team là topology #7

## Sources

- [[refs/devops_topology]]
