---
title: "ART System Team"
aliases: ["System Team", "ART System Team"]
tags: [safe, agile, devops, team-topology, cicd]
created: 2026-04-10
updated: 2026-04-10
---

System Team là một team đặc biệt trong ART chịu trách nhiệm về tooling, automation, và Continuous Delivery Pipeline cho toàn bộ ART. Trong khi các Agile teams khác deliver feature, System Team deliver **khả năng delivery** của các team đó.

System Team thường follow một trong các DevOps topology — thường là "DevOps team with expiration date" hoặc "DevOps advocacy team" — tức là đóng vai bridge để dần dần embed DevOps capability vào các teams, không trở thành silo vĩnh viễn.

## Trách nhiệm chính

**Build CI/CD infrastructure**: setup và maintain CI pipeline, CD pipeline, integration environments. Đây là nền móng kỹ thuật của ART.

**Solution integration**: maintain build scripts, CI configuration. Khi automation chưa sẵn sàng, System Team làm build và integration thủ công trong giai đoạn đầu.

**End-to-end testing**: giúp các teams khác tạo và tối ưu automation test, tổng hợp thành các test suites rõ ràng (smoke, regression, performance).

**System demo facilitation**: đảm bảo môi trường kỹ thuật hoạt động để ART demo system demo đúng nhịp cadence.

**Release facilitation**: verify deployment và final release là hợp lệ trước khi đưa ra production.

## Connections

- [[permanent/safe-agile-release-train]] — System Team là một team trong ART
- [[permanent/devops-topology]] — System Team follow một trong 9 DevOps topologies
- [[permanent/cicd]] — System Team build và maintain CI/CD pipeline cho ART
- [[permanent/continuous-delivery]] — System Team là người implement CD capability cho ART

## Sources

- [[refs/agile_art_system_team]]
