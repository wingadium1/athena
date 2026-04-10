---
title: "SLSA — Supply-chain Levels for Software Artifacts"
aliases: ["SLSA", "supply chain security framework"]
tags: [devops, security, supply-chain, devsecops]
created: 2026-04-10
updated: 2026-04-10
---

SLSA (đọc là "salsa") là một security framework dạng checklist, định nghĩa các cấp độ bảo vệ chuỗi cung ứng phần mềm (software supply chain). Mục tiêu là ngăn chặn tampering, cải thiện integrity của artifact, và bảo vệ cả package lẫn hạ tầng build.

Framework chia thành 4 level (SLSA 1–4), mỗi level yêu cầu thêm các kiểm soát: từ việc có build script có thể tái tạo (level 1) đến toàn bộ build pipeline được hardened và có provenance đầy đủ (level 4). Đây là tiêu chuẩn do Google khởi xướng và hiện được OpenSSF quản lý.

## Connections

- [[permanent/devops]] — SLSA thuộc phạm vi DevSecOps, hardening pipeline CI/CD
- [[permanent/cicd]] — provenance và integrity check được tích hợp vào CI/CD pipeline

## Sources

- [[refs/slsa]]
