---
title: "CI/CD — Continuous Integration / Continuous Delivery"
aliases: ["CI/CD", "CICD", "continuous integration continuous delivery"]
tags: [devops, cicd, automation, software-delivery]
created: 2026-04-10
updated: 2026-04-10
---

CI/CD là cặp khái niệm thường đi cùng nhau, tạo thành xương sống kỹ thuật của DevOps. CI tự động hóa việc tích hợp và kiểm thử code; CD tự động hóa việc đưa code đã được xác nhận đến môi trường production (hoặc staging).

Hai khái niệm này tuy liên quan nhưng độc lập: có thể thực hiện CI mà không có CD. Ngược lại, CD phụ thuộc CI — không thể deliver liên tục nếu không tích hợp liên tục. Pipeline là phương tiện kỹ thuật để kết nối cả hai.

## Connections

- [[permanent/continuous-integration]] — CI: tích hợp, build, test tự động
- [[permanent/continuous-delivery]] — CD: đảm bảo code luôn sẵn sàng release
- [[permanent/continuous-deployment]] — bước tiếp theo của CD: tự động release lên production
- [[permanent/devops]] — CI/CD là cơ chế kỹ thuật cốt lõi của DevOps
- [[permanent/devops-pipeline-stages]] — chi tiết các stage trong một pipeline đầy đủ

## Sources

- [[refs/cicd]]
