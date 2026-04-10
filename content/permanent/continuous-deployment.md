---
title: "Continuous Deployment"
aliases: ["continuous deployment", "auto-deploy to production"]
tags: [devops, cicd, automation, software-delivery]
created: 2026-04-10
updated: 2026-04-10
---

Continuous Deployment là bước tiến xa nhất trong chuỗi CI/CD: mọi commit vượt qua toàn bộ automated test pipeline đều **tự động được deploy lên production** mà không cần approval thủ công. Đây là mức độ tự động hóa cao nhất có thể đạt được.

Điều này đòi hỏi mức độ tin tưởng rất cao vào test coverage và monitoring: nếu tests pass mà production vẫn có lỗi, không có ai "bắt" trước khi user thấy. Vì vậy Continuous Deployment thường đi kèm với feature flags (để kiểm soát visibility độc lập với deployment), canary releases, và automated rollback.

## Điều kiện tiên quyết

Không thể thực hiện Continuous Deployment nếu chưa có:

- Continuous Delivery hoàn chỉnh (pipeline luôn ở trạng thái release-ready)
- Test coverage đủ cao để tự tin auto-deploy
- Monitoring và alerting tốt để phát hiện regression nhanh
- Cơ chế rollback nhanh (feature flag, blue-green, canary)

## So sánh với Continuous Delivery

|                        | Continuous Delivery           | Continuous Deployment     |
| ---------------------- | ----------------------------- | ------------------------- |
| Release quyết định bởi | Con người (business decision) | Automated pipeline        |
| Phù hợp khi            | Cần kiểm soát release window  | Trust vào test suite cao  |
| Rủi ro                 | Thấp hơn                      | Cao hơn nếu test không đủ |

## Connections

- [[permanent/continuous-delivery]] — tiền đề bắt buộc của Continuous Deployment
- [[permanent/continuous-integration]] — CI cung cấp artifact đã được kiểm chứng
- [[permanent/cicd]] — Continuous Deployment là điểm cuối của chuỗi CI/CD

## Sources

- [[refs/continuous_integration]] — có mô tả so sánh CI/CD/Deployment
- [[refs/devops_pipelines_and_toolchains]] — mô tả pipeline stages bao gồm deploy
