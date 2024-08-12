---
title: "Site Reliability Engineering"
alias:
  - SRE
tags: [  ]
---

> [!info]
> Một nghề (Job Title) hoặc cũng là một practice sử dụng các công cụ phần mềm để tự động hóa các nhiệm vụ cơ sở hạ tầng CNTT như quản lý hệ thống và giám sát ứng dụng.
> Công việc cụ thể sẽ là đảm bảo các ứng dụng phần mềm của họ vẫn đáng tin cậy giữa các bản cập nhật thường xuyên từ các nhóm phát triển

# SRE vs [[refs/DevOps|DevOps]]

SRE tập trung vào vấn đề delivery và độ ổn định của hệ thống ở trên môi trường production trong khi [[refs/DevOps|DevOps]] thì tập trung vào việc lifecycle của hệ thống (ứng dụng) từ đầu đến cuối: development, deployment, maintenance, và eventual decommissioning.

Trong khi [[refs/DevOps|DevOps]] vẫn chủ yếu nhìn nhận vấn đề theo hướng triết lý hay văn hoá làm việc giữa Dev và Ops cùng với các practice liên quan với objective là Software release, tất cả các metric của Devops đều liên quan đến việc triển khai. SRE lại xuất phát để phục vụ cho một hệ thống thực sự lớn, phức tạp và quan tâm đến SLO và SLI, SRE sử dụng cách tiếp cận dựa trên dữ liệu (data-driven) để quản lý các mục tiêu cấp độ dịch vụ (SLO) và Error Budget, cân bằng độ tin cậy với tốc độ phát triển.

Nhìn chung cả 2 đều dựa trên những mục tiêu như tự động hoá và hợp tác.