---
title: "Scaling Devops với SAFe®"
alias:
tags: [ type/term, DevOps, SAFe® ]
---

> [!info]
> SAFe® là một framework được áp dụng phổ biến để tích hợp Agile, trong đó được biết đến như là: một nền tảng kiến ​​thức về các nguyên tắc, thực tiễn và năng lực tích hợp đã được chứng minh nhằm đạt được sự linh hoạt trong kinh doanh bằng cách sử dụng Lean, Agile và [[refs/DevOps|DevOps]].”

Các tổ chức có thể chọn vận hành theo 1 trong 4 cấu hình của SAFe® (Essential SAFe, Large Solution SAFe, Portfolio SAFe, Full SAFe), phần lớn thì mọi tổ chức sẽ bắt đầu với Essential SAFe (khối xây dựng cơ bản cho tất cả các cấu hình SAFe và là điểm khởi đầu đơn giản nhất để triển khai)

Trong Essential SAFe, team size từ 5-12 người bao gồm Scrum master, Product Owner và các team member. Mọi người sẽ tham gia vào 1 *team của các team* gọi là Agile Release Train (ART). ART có thể phát triển một sản phẩm hoặc một giải pháp.

Để ART vận hành, thì sẽ có 3 role cần thiết:
* Release Train Engineer (RTE): Chief Scrum Master of the ART. RTE loại bỏ các trở ngại, tạo điều kiện và đảm bảo cho ART hoạt động
* Product Management (PM): PM chịu trách nhiệm hướng dẫn quá trình phát triển của sản phẩm bằng cách tạo và duy trì tầm nhìn về sản phẩm cũng như hướng dẫn tạo ra các tính năng ở backlog và đặt mức độ ưu tiên
* System Architect (SA): SA duy trì kiến ​​trúc bằng cách tạo ra các công cụ hỗ trợ. Họ đóng vai trò là đầu mối cho các nhóm về ART trong việc cân bằng thiết kế mới của các nhóm với kiến trúc khởi tạo ban đầu (có chủ đích).

Công việc mà một team thực hiện được đóng khung trong một khoảng thời gian trong Scrum team và công việc của ART cũng được đóng khung như thế.

Tính năng thì nên được hoàn thiện trong một [[refs/safe_pi|Program Increment (PI)]] (8-12 tuần), PI cũng được chia thành các sprint và cũng thực hiện breakdown thành các story và deliver chúng.

Dựa trên bối cảnh của Essential SAFe và ART, việc tiếp cận DevOps được nêu trong SAFe bao gồn các khía cạnh:
* Modeling the DevOps approach using [[refs/CALMR|Culture, Automation, Lean Flow, Measurement, and  Recovery (CALMR)]]
* Setting up and maintaining value streams
* Applying the CD pipeline against the value stream
* Including built-in quality and security in the process