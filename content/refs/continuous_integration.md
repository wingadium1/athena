---
title: "Continuous Integration"
alias:
  - CI
tags: [  DevOps ]
---

# Continuous Integration

## Continuous Integration vs [[refs/continuous_delivery|Continuous Delivery]] vs [[refs/continuous_deployment|Continuous deployment]]

CI mô phỏng các hoạt động có thể tự động chạy sau khi một thay đổi đã được commit vào version control.
Bao gồm việc compile, test, security scan, và bắt đầu được merge vào source code live.

[[refs/continuous_delivery|Continuous Delivery]] tiến xa hơn việc tích hợp ở trên bằng cách package và đưa package đến các môi trường. Sau đó có thể chạy thêm một số bài kiểm thử tự động (cả functional và bảo mật), các bài test này cho phép tổ chức luôn sẵn sàng release.

[[refs/continuous_deployment|Continuous deployment]] là bước tiến tiếp theo của [[refs/continuous_delivery|Continuous Delivery]]  khi các bài test hoàn tất trong môi trường production, các tính năng mới sẽ tự động được release để cho phép khách hàng sử dụng chúng ngay lập tức.

Cả 3 đều yêu cầu pipeline.
