---
title: "Software Architecture: Bắt đầu từ đâu? - 4: Hard skills và tổng kết"
tags: [ Software Architecture, developer, cicd, continuous_delivery.md, type/write]
---

Như mình đã nói ở phần đầu hard skill của developer và software architect khá rõ ràng về mặt tiếp cận, có nhiều nguồn
tài liệu để đọc mình có thể list ra ở đây

* Domain-Driven Design: Tackling Complexity in the Heart of Software
* [Software Architecture for Developers](https://leanpub.com/software-architecture-for-developers)
* Software Systems Architecture
* Design It! - From Programmer to Software Architect
* Software Architecture in Practice
* Design Patterns - Elements of Reusable Object-Oriented Software
* Domain Driven Design
* Systems Architecting: Creating & Building Complex Systems
* Systemantics: How Systems Work and Especially How They Fail
* Release It!: Design and Deploy Production-Ready Software (Pragmatic Programmers)
* The Clean Architecture
* Patterns of Enterprise Application Architecture
* The Art of Scalability: Scalable Web Architecture, Processes, and Organizations for the Modern Enterprise
* Martin Fowler - Patterns of Enterprise Application Architecture (2002)

Trong đó quyển của Martin Fowler khá đầy đủ và chi tiết.

> Vậy chúng ta cần đọc bao nhiêu sách, biết bao nhiêu loại pattern? Câu trả lời luôn là càng nhiều càng tốt

## Chọn architecture cho dự án

Hay nói đúng hơn là khi bắt đầu dự án, chúng ta sẽ chọn kiến trúc nào: [microservice](microservice), [event-driven](event-driven) hay layered theo cách
truyền thống.
Đấy là câu hỏi thường gặp và chả có gì sai khi hỏi như vậy cả vì ai cũng biết rằng phần mềm mà đưa ra thị trường kiểu gì
chẳng được thiết kế theo kiến trúc nào đó.
Tuy nhiên câu trả lời lại thường khiến chúng ta bất ngờ
> Big bang architecture

Nghe giống như [Big Bang Model](big-bang-model) (một loại mô hình phát triển phần mềm), và đúng là như thế.

Khi dự án mới bắt đầu, chúng ta chưa hiểu gì về hệ thống cả, chỉ nên thiết kế và làm kiến trúc chỉ những gì cần thiết.
Ví dụ chúng ta chỉ nên quyết định có thể cần integrate 2 hệ thống với nhau mà chưa cần quyết định xem sẽ làm việc đó như
nào. Về cơ bản, chúng ta cần tiến hóa architecture theo dự án: những đều chúng ta biết thêm về hệ thống.
> Rất lạ là yêu cầu phần mềm, công nghệ có thể sử dụng và đã sử dụng và business của công ty cũng như khách hàng cần
> thay đổi hằng ngày, nhưng chúng ta thường nghĩ rằng architecture cần phải cố định.

> Thường thì mình sẽ chọn một architecture vừa đủ dùng trong một thời gian phù hợp với dự án và đội phát triển (tất
> nhiên không phải là phần mềm kiểu enterprise rồi)

Và gần như kiến trúc này sẽ được cải tiến cùng với CI/CD trong phần trước đó, nếu kiến trúc đó có vấn đề khi tích hợp
với Pipeline thì có nghĩa rằng kiến trúc hoặc Pipeline hoặc cả 2 đều có vấn đề và chúng cần được cải tiến.

> Một dự án cần phát triển một ứng dụng như Tiki chẳng hạn, nhưng Architect lại được bắt đầu với multi tier truyền thống
> và bạn sẽ có một monolithic web application, ai cũng sẽ thấy rằng, khi bạn cần sửa dù chỉ một phần nhỏ trong header của
> request, bạn sẽ cần deploy lại cả hệ thống, điều đó không sai nhưng rõ ràng có vấn đề. Điều tất yếu xảy ra kiến trúc sẽ
> được cải tiến.

Gần như hiện tại trong quá trình làm dự án, chúng ta có thể thấy rằng nếu như hệ thống có hình dáng giống với một hệ
thống nào đó trong quá khứ thì chúng ta sẽ gần như kế thừa và giữ nguyên, trong khi đó các hệ thống chưa xuất hiện bao
giờ thì sẽ được quyết định bởi trực giác của Architect trong một quá trình khám phá có kiểm soát.
> Các quyết định này có thể đúng, sai, tăng tiến độ hoặc thậm chí phá hỏng mọi thứ và chúng ta phải rework.

Khi đó chúng ta có một thứ gọi là Accidental Architecture, tuy nhiên, điều này chưa hẳn đã xấu vì gần như chúng ta không
thể tránh khỏi trong quá trình làm dự án và xây dựng hệ thống. Chỉ khi bắt đầu biến những kiến trúc ngẫu nhiên này thành
những kiến trúc có chủ đích, chúng ta mới nâng cao hiểu biết của mình về kiến trúc phần mềm.

## Final words

Có lẽ serires này sẽ dừng ở đây, mình sẽ cố gắng sắp xếp lại một số bài viết liên quan đến chủ đề này trong nhưng bài
viết khác.

THE END.