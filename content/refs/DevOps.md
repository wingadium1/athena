---
title: "DevOps"
tags: [ type/term ]
---

# DevOps

> [!info]
> DevOps là một tập hợp các thực hành kết hợp giữa phát triển phần mềm (Dev) và vận hành công nghệ thông tin (Ops). Mục
> tiêu của DevOps là rút ngắn chu kỳ phát triển phần mềm và cung cấp liên tục với chất lượng phần mềm cao. DevOps nhấn
> mạnh sự hợp tác, tự động hóa và tích hợp giữa các nhà phát triển và đội ngũ vận hành công nghệ thông tin để cải thiện
> hiệu suất và triển khai phần mềm một cách đáng tin cậy và thường xuyên.

# Tương quan với CI/CD

DevOps and [[refs/cicd|Continuous Integration-Continuous Deployment]] là các khái niệm có liên quan chặt chẽ với nhau,
phối hợp với nhau để cải thiện quá trình triển khai và phát triển phần mềm.

[[refs/CI|CI]] phù hợp với các nguyên tắc cộng tác và tự động hóa của DevOps. Bằng cách tích hợp các thay đổi mã liên
tục, các nhóm có thể đảm bảo rằng các tính năng và bản sửa lỗi mới được kiểm tra và xác thực nhanh chóng, thúc đẩy chu
kỳ phát triển nhanh hơn.

Trong khi đó [[refs/continuous_delivery|CD]] là hoạt động cốt lõi trong DevOps, nhằm mục đích phát hành phần mềm thường xuyên và đáng tin
cậy. Tự động hóa các quy trình triển khai giúp giảm nguy cơ xảy ra lỗi do con người và nâng cao tốc độ cũng như độ tin
cậy trong việc cung cấp các bản cập nhật cho người dùng.

## Integration

* **Tự động hóa**: DevOps khuyến khích tự động hóa các tác vụ lặp đi lặp lại và quy trình [[refs/cicd|CICD]] là quy trình công
việc tự động xử lý việc tích hợp, thử nghiệm và triển khai mã nguồn.
* **Hợp tác**: Cả DevOps và [[refs/cicd|CICD]] đều nhấn mạnh đến việc cải thiện khả năng giao tiếp và cộng tác giữa các nhóm
phát triển và vận hành, phá vỡ các rào cản và thúc đẩy văn hóa chia sẻ trách nhiệm.
* **Phản hồi liên tục**: Cả quy trình [[refs/cicd|CICD]] cung cấp phản hồi liên tục cho các developer về tình trạng và chất
lượng mã nguồn của họ, phù hợp với trọng tâm của DevOps là cải tiến liên tục và phát triển lặp lại.