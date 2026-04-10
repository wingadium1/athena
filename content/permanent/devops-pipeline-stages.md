---
title: "DevOps Pipeline Stages"
aliases: ["CI/CD pipeline stages", "DevOps toolchain", "pipeline foundation"]
tags: [devops, cicd, pipeline, automation, testing, security]
created: 2026-04-10
updated: 2026-04-10
---

Một DevOps pipeline đầy đủ gồm 9 stage liên tiếp, thường được visualize thành vòng lặp vô tận (infinity loop). Chia thành hai nửa lớn: CI (Plan → Package) và CD (Configure → Monitor).

## Các stage

| Stage           | Thuộc         | Mô tả                                                                         |
| --------------- | ------------- | ----------------------------------------------------------------------------- |
| **Plan**        | CI foundation | Tracking tiến độ, từ request đến release. Dùng Agile tools, backlog, sprints  |
| **Create**      | CI foundation | Viết code + documentation. Version control lưu cả test script                 |
| **Build**       | CI            | Compile, orchestrate changes                                                  |
| **Verify**      | CI            | Test quality: unit test, static analysis (lint, SAST, dependency scan)        |
| **Package**     | CI            | Tạo artifact (container image, binary) sẵn sàng cho deployment                |
| **Configure**   | CD            | IaC: provision/update resources trong các environments                        |
| **Release**     | CD            | Deploy artifact vào môi trường, kiểm soát visibility qua feature flags        |
| **Verify (CD)** | CD            | Advanced testing trong môi trường thật: functional, load/perf, DAST, IaC scan |
| **Monitor**     | CD            | Performance monitoring, log collection, alerting                              |

## Shift-Left Security (DevSecOps)

Triết lý "shift-left" đưa security vào sớm nhất có thể, ngay trong giai đoạn CI:

- **SAST** (Static Application Security Testing) — phân tích code tĩnh
- **Dependency scanning** — kiểm tra vulnerability trong dependencies
- **License scanning** — compliance
- **DAST** (Dynamic Application Security Testing) — giả lập tấn công trong môi trường CD
- **IaC scanning** — kiểm tra Terraform/Helm templates (Trivy)
- **Container scanning** — kiểm tra image (Trivy)

## Feature Flags trong Release Stage

Feature flags tách biệt **deployment** khỏi **release**: code có thể được deploy lên production nhưng chưa visible với user. Cho phép:

- Gradual rollout (canary, progressive delivery)
- Nhanh chóng rollback bằng cách tắt flag, không cần redeploy

Tools phổ biến: LaunchDarkly, Flagsmith, CloudBees Feature Management.

## Connections

- [[permanent/cicd]] — pipeline là cơ chế thực thi CI/CD
- [[permanent/continuous-integration]] — CI chiếm stage Build → Package
- [[permanent/continuous-deployment]] — CD chiếm stage Configure → Monitor

## Sources

- [[refs/devops_pipelines_and_toolchains]]
