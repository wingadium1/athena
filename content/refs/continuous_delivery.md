---
title: "Continuous Delivery"
alias:
  - CD
tags: [ type/term, DevOps ]
---

> [!info]
> Phân phối liên tục (tiếng Anh: Continuous delivery - viết tắt: CD) là một cách tiếp cận của kỹ thuật phần mềm, trong
> đó các đội sẽ sản xuất phần mềm trong chu kỳ ngắn, đảm bảo rằng các phần mềm có thể được phát hành một cách tin cậy
> bất
> cứ lúc nào. Nó nhằm mục đích build, kiểm thử, và phát hành phần mềm nhanh hơn và thường xuyên hơn. Cách tiếp cận này
> giúp giảm chi phí, thời gian và nguy cơ khi thay đổi bằng cách gia tăng cập nhật các ứng dụng trong sản phảm. Một quá
> trình triển khai đơn giản và lặp lại là điều quan trọng cho Phân phối liên tục ([[refs/continuous_delivery|CD]]).

## Tương quan với [[refs/DevOps|DevOps]]

[[refs/continuous_delivery|CD]] và [[refs/DevOps|DevOps]] tương tự nhau như ở mặt ý nghĩa của chúng, và thường bị hòa trộn vào nhau,
nhưng chúng là hai khái niệm
khác nhau.[[refs/DevOps|DevOps]] có phạm vi rộng hơn và xoay quanh thay đổi văn hóa, đặc biệt là sự hợp tác của các đội
khác nhau
khi tham gia vào làm phần mềm (nhà phát triển, nhà điều hành, QA,...) cũng như việc tự động hóa quá trình phân phối phần
mềm.[[refs/continuous_delivery|CD]] mặt khác, là một cách tiếp cận để tự động hóa khía cạnh phân phối, và tập trung vào việc mang các quá
trình
lại với nhau để thực hiện chúng nhanh hơn và thường xuyên hơn. Như vậy, [[refs/DevOps|DevOps]] có thể là một sản phẩm
của [[refs/continuous_delivery|CD]], và [[refs/continuous_delivery|CD]]
đi trực tiếp vào [[refs/DevOps|DevOps]]

## Tương quan với Continuous Deployment

Continuous Delivery đôi khi bị nhầm lẫn với Continuous Deployment (Triển khai liên tục). Continuous Deployment có nghĩa là
mọi thay đổi được triển khai tự động lên môi trường production. Phân phối liên tục có nghĩa là đội làm việc đảm bảo rằng
mọi thay đổi **có thể** được triển khai lên môi trường production, nhưng cũng **có thể không** cần phải triển khai, thường do lý
do về phục vụ kinh doanh, marketing. Để làm được Continuous Deployment người ta phải làm được cả Continuous Delivery.