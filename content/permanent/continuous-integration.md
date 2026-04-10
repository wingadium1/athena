---
title: "Continuous Integration (CI)"
aliases: ["CI", "continuous integration"]
tags: [devops, cicd, automation, testing]
created: 2026-04-10
updated: 2026-04-10
---

Continuous Integration là thực hành tự động hóa tất cả các bước xảy ra sau khi một commit được push lên version control: compile, unit test, static analysis, security scan, và merge vào main branch. Mục tiêu là phát hiện lỗi sớm — càng gần thời điểm code được viết càng tốt.

Triết lý "shift-left" là trung tâm của CI: đưa việc kiểm thử và bảo mật vào sớm nhất có thể trong pipeline thay vì để cuối giai đoạn phát triển. CI không deploy lên môi trường thật — đó là việc của CD.

## CI vs CD vs Continuous Deployment

| Bước                  | Phạm vi                                       | Output                      |
| --------------------- | --------------------------------------------- | --------------------------- |
| CI                    | Compile → Test → Package                      | Artifact đã được kiểm chứng |
| Continuous Delivery   | Đưa artifact vào môi trường, sẵn sàng release | Release-ready build         |
| Continuous Deployment | Tự động release lên production                | Live feature                |

Cả ba đều yêu cầu pipeline, nhưng mỗi bước là một quyết định tổ chức riêng biệt.

## Connections

- [[permanent/cicd]] — CI là một nửa của cặp CI/CD
- [[permanent/continuous-delivery]] — CD bắt đầu từ artifact CI tạo ra
- [[permanent/continuous-deployment]] — bước tự động hóa tiếp theo sau CD
- [[permanent/devops-pipeline-stages]] — CI chiếm giai đoạn Build → Verify → Package

## Sources

- [[refs/continuous_integration]]
