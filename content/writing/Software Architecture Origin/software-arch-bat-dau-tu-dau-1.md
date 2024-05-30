---
title:  "Software Architecture bắt đầu từ đâu? - 1"
tags: [Software Architecture, developer, type/write]
---

Motivation
-------
> Đầu tiên mình định viết về Solution Architecture theo quyển Solutions Architect's Handbook nhưng nghĩ lại thì Software Architecture sẽ hợp lý để bắt đầu hơn.

Disclaimer
-----
> Bài viết này không nhằm mục đích hướng dẫn hay đề ra một con đường trong sự nghiệp, chỉ là chia sẻ và những gì mình đã trải nghiệm thôi.

Developer đến Software Architecture
=================
Mọi người sẽ thắc mắc tại sao lại là Developer chứ không phải Coder.
Về cơ bản thì Coder - Người code tức là họ chỉ biết hoặc cũng chỉ muốn code (Ở một phương diện nào đó), còn Developer - Người phát triển phần mềm, tức là họ xử lý tất cả các vấn đề liên quan đến quy trình phát triển phần mềm (Software Development Process) từ nói chuyển với stakeholders, thiết kế ứng dụng (ở một mức độ nào đó), deploy phần mềm, debug, sửa lỗi và cải tiến phần mềm.
Một cách đơn giản trong đa số trường hợp Coder tương đương với Junior Developer.

Ồ!!!

Vậy Developer sẽ trở thành Software Architecture như nào và điểm khác biệt là gì khi developer đã có thể thiết kế ứng dụng (như mình có nói ở trên).
Mình sẽ đưa ra ví dụ như này cho mọi người dễ hình dung:
> Trong một dự án xây dựng hệ thống và phần mềm di động đi kèm một chiếc máy ảnh, người dùng có thể download một số sticker và chỉnh sửa lên bức ảnh của mình, Backend sẽ xây dựng một vài API để phục vụ.
Trong trường hợp này Developer đã có thể thiết kế được API, kiến trúc của Backend dựa trên ExpressJS, deploy chúng lên AWS, xử lý memcache để tốc độ phản hồi tốt hơn, người dùng nhận được sticker mới ngay khi chúng được upload.

Vậy có thể thấy rằng hầu hết công việc đã được Developer xử lý (ở đây tạm thời không phân chia Junior-Middle-Senior nhé) và dự án golive thành công. 5 tháng sau đội dự án bị đập tơi bời từ khách hàng vì bill từ AWS vài $10000 cho mỗi tháng, trong trường hợp này nếu là bạn Software Architecture sẽ xử lý như nào

> (Thực ra có vài cách trong trường hợp này nhưng mình sẽ đưa ra một cách cho thấy sự tương phản nhất)
Thay vì đưa trực tiếp API cho Mobile như bạn Software Developer, API sẽ được xử lý như một file tĩnh (JSON-XML) từ S3 cùng với sự hỗ trợ của CDN, file này sẽ được cập nhật khoảng 30' một lần nếu có sự thay đổi về danh sách sticker. Như vậy người dùng sẽ chờ tối đa khoảng 30' để có thể nhận được sticker mới, nhưng thay vào đó bill từ AWS sẽ chỉ khoảng $500.

Vậy bạn Developer có thể biết cái gì tối ưu nhất cho hệ thống nhưng sẽ bị hạn chế khi xử lý các vấn đề tradeoffs-đánh đổi. Trong khi đó Architect sẽ cần biết cả 2.

Software architecture?
==================

Đang nói về thiết kế ứng dụng chứ không phải job title nhé :P.
Mình sẽ gạch đầu dòng thôi vì chỗ nào cũng có định nghĩa:

* Các tiêu chuẩn, khái niệm tôn chỉ (gì gì đó) của một hệ thống.
* Cách tổ chức, các component quan trọng, cách tương tác, những thành phần nhỏ hơn và interface giữa chúng (không cụ thể nhé).

Về cơ bản thì Software architecture là những hiểu biết chung về thiết kế hệ thống trong dự án phần mềm, tất nhiên sẽ bao gồm cả việc hệ thống được chia nhỏ như nào, và interface giữa chúng là gì nữa, nhưng chỉ dừng lại ở những component và interface mà mọi người đều cần hiểu thôi nhé.
> Ví dụ: chi tiết về việc iOS team lưu trữ password hay load thông tin từ database ở local device như nào sẽ không là một phần trong architecture của cả dự án nhưng việc chỉ ra những gì cần lưu trữ ở local như một lớp caching thì lại có.

Ông anh [Martin Fowler](https://martinfowler.com/) thì định nghĩa khá đơn giản ở [đây](http://martinfowler.com/ieeeSoftware/whoNeedsArchitect.pdf):
> So, this makes it hard to tell people how to describe their architecture. “Tell us what is important.” Architecture is about the important stuff. Whatever that is.

Vậy cần những gì để trở thành một Software Architecture?
Có rất nhiều bài viết nói về cái này, nhưng mình sẽ chỉ ra một số cái mình nghĩ là quan trọng nhất:
* Hard skill hay technical skill gì gì đó:
  * Bao giờ cũng là các application architecture phổ biến, pattern, anti-pattern, cái này thì thực ra khá dễ để nhìn ra và dễ tiếp cận
  * Integration architecture, à thì không phải lúc nào cũng xây dựng một cái gì đó từ đầu (from scratch), nên integration luôn là sự lựa chọn tốt.
  * Enterprise architecture, nó liên quan đến tất cả mọi thứ của một công ty, cá thể kinh doanh, từ chiến lược kinh doanh, mô hình vận hành, hoạt động kinh doanh và cả những thứ liên quan đến IT và Infrastructure nữa, nên nếu có bị giới hạn về mặt hiểu biết, mình có thể sẽ không đi sâu về cái này.
* Soft skill cái này quan trọng này, có thể mọi người sẽ nghĩ đến những thứ khá cao siêu như Leadership hay Organization nhưng mình sẽ đi từ những thứ nhỏ nhỏ trước:
  * Ra quyết định - Decisions, tất nhiên là bạn cần quyết định nhiều thứ, đặc biệt là về kiến trúc hoặc đơn giản hơn là trong team sẽ làm việc với nhau như nào (communication decision)
  * [[refs/continuous-delivery|Continuous Delivery]], nghe có vẻ là hard skill nhưng thực tế là soft skill. Về cơ bản phần mềm hay hệ thống sẽ vẫn là một thứ gì đấy trừu tượng (ở trên bản vẽ - mượn từ archiecture - kỹ sư trong ngành xây dựng), cho đến khi nó được deploy trên môi trường thực tế. Và khi phần mềm được deploy càng sớm và càng liên tục thì việc chứng mình architecture hợp lý càng nhanh và chính xác, mọi người cũng trưởng thành hơn về mặt thực hành.

Bài tiếp theo mình sẽ viết tiếp về các skill nhé.