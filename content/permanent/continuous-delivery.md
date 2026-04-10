---
title: "Continuous Delivery (CD)"
aliases: ["CD", "continuous delivery", "phân phối liên tục"]
tags: [devops, cicd, automation, software-delivery]
created: 2026-04-10
updated: 2026-04-10
---

Continuous Delivery là thực hành đảm bảo rằng codebase **luôn ở trạng thái có thể release** lên production bất cứ lúc nào — nhưng việc release thực sự là quyết định có chủ ý của con người, không phải tự động. Đây là điểm phân biệt then chốt với Continuous Deployment.

CD nhận artifact từ CI, đưa qua các môi trường (staging, UAT), chạy thêm integration test và acceptance test, rồi đặt code ở trạng thái "release-ready". Nhóm vận hành chọn thời điểm release dựa trên nhu cầu business (marketing window, feature bundling), không bị chặn bởi kỹ thuật.

## CD vs Continuous Deployment

Dễ nhầm lẫn vì cùng viết tắt là "CD":

- **Continuous Delivery**: code _có thể_ được deploy bất cứ lúc nào — nhưng người quyết định
- **Continuous Deployment**: mọi commit pass CI/CD đều _tự động_ lên production, không cần approval

Để thực hiện được Continuous Deployment, phải làm được Continuous Delivery trước.

## Mối quan hệ với DevOps

CD rộng hơn DevOps về kỹ thuật nhưng hẹp hơn về phạm vi. DevOps là triết lý văn hóa bao trùm; CD là một practice cụ thể trong đó. Một tổ chức có thể thực hiện CD mà chưa áp dụng DevOps đầy đủ.

## Connections

- [[permanent/cicd]] — CD là nửa còn lại của CI/CD
- [[permanent/continuous-integration]] — CI cung cấp artifact cho CD
- [[permanent/continuous-deployment]] — bước tiếp theo: tự động hóa hoàn toàn
- [[permanent/devops]] — CD là practice cốt lõi trong DevOps

## Sources

- [[refs/continuous_delivery]]
