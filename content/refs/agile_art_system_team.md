---
title: "ART System Team"
alias:
  - The System Team
tags: [ DevOps, Agile ]
---

> [!info]
> System team là một team trong [[refs/agile_art|ART]] có trách nhiệm về tooling và automation cho Continuous Delivery Pipeline.
> System team thì làm việc cùng với các team khác trong [[refs/agile_art|ART]] để giúp đỡ về solution cho việc delivery value.

System team thì có thể theo 1 hoặc 1 vài [[refs/devops_topology|DevOps topology]], có thể là DevOps team with an expiration date, DevOps advocacy team.

## Responsibility
### Building infrastructure for solution developement.

Setting: pre-build, phần [[refs/continuous_integration|Continuous Integration] và [[refs/continuous_deployment|Continuous deployment]] của [[refs/cicd|CI-CD Pipeline]], tích hợp các công nghệ. Đó gần như là một phần liền mạch của Continuous Delivery Pipeline.

### Spearheading solution integration - Tiên phong tích hợp giải pháp

Là một phần của việc duy trì CI, system team có thể tham gia xác định quy trình build sau khi change đã được commit vào version control. System team maintain các script/cấu hình CI phù hợp, nếu chưa setup được automation, system team có thể thực hiện việc build và integration.

### Setting up end-to-end testing - Thiết lập kiểm thử đầu cuối
Để hỗ trợ các nhóm khác, system team có thể giúp tester tạo và tối ưu hóa các automation test. Họ cũng có thể làm việc với các nhóm khác để tổng hợp các bài kiểm thử riêng biệt thành các bộ thử nghiệm được xác định rõ ràng cho các loại thử nghiệm khác nhau, chẳng hạn như smoke test.

#### Hỗ trợ demo
ART tích hợp công việc từ tất cả các nhóm của mình và sẽ có hoạt động demo giải pháptại một thời điểm nhất định.
Việc tích hợp và demo này được gọi là bản demo hệ thống và diễn ra theo nhịp độ đều đặn. Là người maintain Continuous Delivery Pipeline, system team đảm bảo rằng các môi trường kỹ thuật hoạt động cho tất cả các nhóm để bản demo hệ thống diễn ra liền mạch.

#### Facilitating the release - Tạo điều kiện cho release
Vì System team có cái nhìn toàn diện về quy trình, họ có thể được yêu cầu xác minh rằng các deployment cho môi trường production và final release là hợp lệ.
System team có thể được coi là nhóm DevOps cho ART. Nhóm này có thể tuân theo một trong các  [[refs/devops_topology|DevOps topology]] như một cách cộng tác với các nhóm Agile khác. Trách nhiệm của nhóm này chủ yếu liên quan đến việc cấu hình tự động hóa, nhưng nhóm này có thể hỗ trợ các nhóm Agile theo những cách khác khi toàn bộ ART nỗ lực mang lại giá trị.
