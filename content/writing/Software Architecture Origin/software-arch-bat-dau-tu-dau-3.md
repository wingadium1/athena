---
title: "Software Architecture bắt đầu từ đâu? - 3: Continuous Delivery"
tags: [ Software Architecture, developer, cicd, continuous-delivery]
---

Trong phần trước, mình đã nói về [[writing/Software-Architecture-Origin/software-arch-bat-dau-tu-dau-2|Soft Skill — Decision]], trong phần này sẽ đề cập
tới một loại khác mà sẽ gây nhiều tranh cãi: [[tags/continuous-delivery|Continuous Delivery]].

[[tags/continuous-delivery|Continuous Delivery]], hãy nói về mặt khả năng, chính xác hơn là điểm khác biệt với Manual Delivery
ở 3 yếu tố: an toàn,
nhanh chóng và bền vững (hoặc dùng từ Việt một chút là ổn định).

> Trong bài viết này sẽ không đánh đồng [[tags/continuous-delivery|Continuous Delivery]] với các kỹ thuật automate, mà mọi
> người hay khá nhầm lẫn với việc Automation Delivery (một trong những kỹ thuật của CD). Dù sao thì, mình chỉ muốn xem
> xét
> về mặt văn hóa trong dự án phần mềm.

# Tại sao cần [[tags/continuous-delivery|Continuous Delivery]]?

Thời gian — tất nhiên rồi, ai cũng muốn đưa những tính năng mới nhất cho người dùng một cách nhanh nhất, đưa hệ thống
trở lại khi downtime nhanh nhất (Mean Time To Repair) đồng nghĩa với việc business được duy trì, tiền kiếm được không bị
ngừng trệ.

Hãy xem xét 2 loại thời gian:

* Lead time: lượng thời gian từ khi bắt đầu và kết thúc của một công việc trong quy trình
* Cycle time: khoảng thời gian mà giữa 2 công việc liên tiếp được kết thúc, hay còn có thể hiểu là Deployment Frequency
  cho dễ hiểu

[[tags/continuous-delivery|Continuous Delivery]] Pipeline sẽ được hình dung như này

![image](https://www.gocd.org/assets/images/blog/cd-metrics/gocd-pipelines-6356c22e.png)

Còn đây là Lead time ![image](https://www.gocd.org/assets/images/blog/cd-metrics/lead-time-diagram-a4a572a6.jpg)

Đây là Cycle time
nè ![image](https://www.gocd.org/assets/images/blog/cd-metrics/deployment-frequency-concept-f47c6623.jpg)

Vậy thời gian bao nhiêu là đủ?

![image](https://www.gocd.org/assets/images/blog/cd-metrics/continuous-delivery-benchmarking-329817c5.png)

> Credit: Forsgren PhD, Nicole. Accelerate: The Science of Lean Software and DevOps: Building and Scaling High
> Performing Technology Organizations (Kindle Location 564). IT Revolution Press. Kindle Edition.

Nhìn chung thì khi chúng ta cố gắng tăng tần suất deploy chúng ta sẽ có thể giảm MTTR, có nghĩa chúng ta sẽ không tạo ra
một sản phẩm có tính bền vững cao (robustness — như cách tiếp cận truyền thống), khi đó khả năng tự phục hồi sau lỗi (
resilience) của sản phẩm được tăng lên (một cách tự nhiên), cùng với đó là giảm khả năng xảy ra lỗi vì gần như khi tăng
tần suất deploy đến hằng ngày hoặc nhiều lần một ngày sản phẩm sẽ gần như lúc nào cũng sẵn sàng để đưa lên môi trường
production (production ready).

Như vậy chúng ta sẽ có một mô hình:

1. Continuous Integration:

* Integration sớm và thường xuyên
* Mọi người cần đồng bộ trunk (ý nói đến phiên bản được deploy) hằng ngày

2. Continuous Deployment:

* Điều này có để dễ dàng đạt được, khi source code ở trunk khá ổn định và Deploy chỉ là stage cuối cùng của Continuous
  Integration, như vậy việc phát hiện các lỗi và fix chúng cũng nhanh hơn, kể cả khi bạn có những thành viên không quá
  giỏi trong tay. Với các công cụ khác nhau như Sonarquebe hay chính Unit Test sẽ hỗ trợ developer tạo nên những sản
  phẩm tốt hơn.

3. [[tags/continuous-delivery|Continuous Delivery]]:

* Sau 2 vấn đề bên trên source code ổn định, deploy thường xuyên, bản vá được xử lý nhanh chóng ít ảnh hưởng, cuối chùng
  chúng ta có một phần mềm luôn ở trạng thái sẵn sàng.

#### Nhưng

Có thể các bạn sẽ nghĩ hay có câu hỏi như này: ***Lỗi vẫn xảy ra thì sao?***

Ồ thì tất nhiên lỗi vẫn xảy ra, nhưng chúng ta sẽ có lỗi ở những thời điểm sớm hơn.
Hầu hết mọi người sẽ gặp các vấn đề thường xảy ra khi chúng ta tích hợp (các thành phần trong phần mềm với nhau), điều
này khá khó chịu, và mọi người sẽ có xu hướng trì hoãn.
Tuy nhiên hãy nghĩ đến trường hợp này: trong ví dụ ở
phần [trước](software-arch-bat-dau-tu-dau-2.md) giả sử client cứ làm theo requirement hoặc UX/UI được cung cấp, backend
thiết kế hoàn toàn khác tuy nhiên vẫn đầy đủ thông tin, client
sẽ phải call nhiều API cho màn hình thay vì 1 API như trong suy nghĩ, điều đó dẫn đến việc một trong 2 cần thay đổi,
việc này tốn khá nhiều công và tiềm ẩn rủi ro. Vậy nên khi tích hợp sớm chúng ta sẽ làm việc trơn tru trong quãng thời
gian còn lại và sẵn sàng release thay vì dành vài tuần hoặc vài ngày cuối để thay đổi toàn bộ interface.

#### Bring the pain forward

Hay đầy đủ là ***if it hurts, do it more often, and bring the pain forward***.

![image](https://martinfowler.com/bliki/images/frequency-reduces-difficulty/graph.png)

Quay lại vấn đề giữa mobile và backend, giả sử sẽ vẫn có những khác biệt về suy nghĩ xây dựng interface giữa 2 bên, tuy
nhiên chúng ta đã làm nó sớm, và chia nhỏ nó ra và bằng cách backend thường xuyên release phiên bản API mới (chưa nói
đến việc backward compatibility nhé), mobile sẽ tích hợp tốt hơn, nhanh hơn vì những thay đổi khá nhỏ và dễ dàng đáp
ứng. Trong khi đó mobile team cũng build ra app thường xuyên hơn, tester có thể verify nhanh hơn, sản phẩm được ổn định
hơn, ít tiềm ẩn lỗi hơn.

Điều này khá giống trong lý thuyết phát triển phần mềm, chi phí sửa lỗi càng nhiều cho các lỗi ở giai đoạn muộn của dự
án.

# [[tags/continuous-delivery|Continuous Delivery]] bằng cách nào

![image](https://ptgmedia.pearsoncmg.com/images/art_humble_continuousdelivery/elementLinks/humble_fig01.jpg)

[[tags/continuous-delivery|Continuous Delivery]] cho chúng ta một cách tiếp cận khác về việc quản lý dự án, chúng ta có thể
định nghĩa lại ***done***
không phải là code xong mà là được delivery thành công.
Toàn bộ quá trình nên sử dụng đối đa các nền tảng tự động như Unit-test, Automation Test, điều đó làm tăng tốc độ
feedback trên các bản vá của source code cũng như infrastructure.
Điều này càng quan trọng hơn khi chi phí End-to-end testing càng ngày càng lớn, chúng nên được thay thế bằng các danh
mục testing như dưới đây ([multi-faceted testing portfolio](https://www.youtube.com/watch?v=afEzqDExCTE)), điều này càng
quan trọng hơn trong thế giới hiện nay khi Devops là xu hướng, và các quyết định về release càng ngày càng gắn liên với
tình huống về mặt nghiệp vụ thay vì sự quyết định của một đội quản lý về vận hành.

| Practice              | Quantity     | Frequency            | Duration    | Environment         |
|-----------------------|--------------|----------------------|-------------|---------------------|
| Unit Testing          | 100 to 1000+ | Per build            | < 30s total | Local and Build     |
| Acceptance Testing    | 10 to 100+   | Per build            | < 10m total | Local and Build     |
| Exploratory Testing   | 10 to 100+   | Per build            | Timebox     | Local and 3rd Party |
| Contract Testing      | ~20          | Per 3rd party deploy | < 1m        | 3rd Party           |
| Smoke Testing         | ~5           | Per deploy           | < 5m        | All                 |
| Monitoring            | 10 to 100+   | Always               | < 10s       | All                 |
| Anomaly detection     | 10 to 100+   | < 1m                 | < 10s       | All                 |
| Adaptive architecture | N/A          | Always               | N/A         | All                 |

Như vậy có thể thấy là việc xử lý [[tags/continuous-delivery|Continuous Delivery]] gần như không mang tính kỹ thuật mà mang
tính định hướng về văn hóa làm việc cho dự án nhiều hơn, chính vì thế tại sao mình lại đặt nó là Soft Skill.