---
title: "Seekable OCI (SOCI)"
aliases: ["SOCI", "Seekable OCI", "lazy loading container image"]
tags: [aws, containers, infrastructure, devops, performance]
created: 2026-04-10
updated: 2026-04-10
---

Seekable OCI (SOCI) là công nghệ open-source do AWS phát triển, cho phép **khởi động container nhanh hơn** bằng kỹ thuật **lazy loading** — container có thể start và chạy trước khi toàn bộ image được tải về.

## Vấn đề SOCI giải quyết

Theo mô hình OCI (Open Container Initiative) thông thường, container runtime phải pull toàn bộ image layers về local trước khi start container. Với các image lớn (vài GB), đây là bottleneck nghiêm trọng cho cold start latency — đặc biệt trong serverless, auto-scaling, hoặc CI/CD pipelines.

## Cơ chế hoạt động

SOCI tạo một **SOCI Index** cho các layers trong container image. Index này là metadata cho phép runtime biết chính xác vị trí của từng file trong layer mà không cần tải toàn bộ layer về. Khi container cần một file cụ thể, nó chỉ fetch phần đó — tương tự như seeking trong một file thay vì đọc toàn bộ từ đầu.

Quá trình:

1. Build image bình thường (OCI-compatible)
2. Generate SOCI Index cho image (chạy `soci create`)
3. Push cả image lẫn SOCI Index lên registry
4. Container runtime hỗ trợ SOCI (như containerd với SOCI snapshotter) sử dụng index để lazy-load

## Use cases

- **AWS Fargate**: SOCI được tích hợp native, giảm task startup time đáng kể
- **AWS Lambda container images**: Faster cold starts
- **EKS / ECS với large images**: ML model images, JVM applications

## Connections

- [[permanent/devops-pipeline-stages]] — SOCI tối ưu Deploy stage bằng cách giảm image pull time
- [[permanent/continuous-deployment]] — Faster container startup → faster deployment pipeline end-to-end
- [[permanent/feature-flag]] — Fast startup enables rapid rollout/rollback strategies

## Sources

- [[refs/Seekable_OCI]] (archived)
