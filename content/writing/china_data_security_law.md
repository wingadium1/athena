---
title: "Luật An Ninh Dữ Liệu - Data Security Law (DSL)"
tags: [ Software Architecture, developertype/write]
---

## Luật An Ninh Dữ Liệu - Data Security Law (DSL)

Như tên gọi thì luật An Ninh dữ liệu tập trung vào dữ liệu, chủ yếu là các hoạt động xử lý dữ liệu trong và ngoài Trung Quốc khi liên quan đến dữ liệu của Trung Quốc (thông tin cá nhân, thông tin của tổ chức tại Trung Quốc)


**Các vấn đề chính cần chú ý của DSL**
* **Phạm vi** của DSL: đại khái là cái gì dữ liệu dính đến Trung Quốc từ tổ chức, cá nhân đều trong phạm vi, hơn nữa còn bao gồm toàn bộ vòng đời của dữ liệu (entire lifecycle of data): thu thập, bào trì, tổng hợp, sử dụng, phân tích, công bố, lưu trữ và xoá dữ liệu.
* Ở DSL thì dữ liệu cần được phân loại và có biện pháp bảo vệ riêng ([[https://www.china-briefing.com/news/china-data-classification-standards-important-data|ref]]), trong đó được đề cập đến trong tài liệu [[https://www.tc260.org.cn/upload/2024-03-21/1711023239820042113.pdf|Data security technology — Rules for data classification and grading GB/T 43697-2024]], dưới đây là các thuật ngữ được đề cập đến khi nói về dữ liệu trong Trung Quốc.

| Data                           | Any record of information in electronic or other forms.|
|--------------------------------|------------------------------------------------------------------------------------------|
| Important data - dữ liệu quan trọng | Dữ liệu cụ thể cho một số lĩnh vực, nhóm và khu vực nhất định hoặc đạt đến một mức độ chính xác và quy mô nhất định, khi bị rò rỉ, giả mạo hoặc phá hủy, có thể gây nguy hiểm trực tiếp đến an ninh quốc gia, hoạt động kinh tế, ổn định xã hội, sức khỏe cộng đồng và an toàn. Trung Quốc có quy định các ngành công nghiệp quan trọng (key industry có tiêu chuẩn)|
| Core data (hoặc là core national data) | Dữ liệu có phạm vi bao phủ cao, đạt đến mức độ chính xác cao, có quy mô lớn và đạt đến một độ sâu nhất định trong một lĩnh vực, nhóm hoặc khu vực mà khi bị sử dụng hoặc chia sẻ bất hợp pháp, có thể ảnh hưởng trực tiếp đến an ninh chính trị.<br><br>**Lưu ý: Dữ liệu cốt lõi chủ yếu bao gồm dữ liệu liên quan đến các lĩnh vực trọng điểm về an ninh quốc gia, đường dây kinh tế quốc gia, sinh kế của người dân và các lợi ích công cộng lớn, được các sở ban ngành nhà nước có liên quan đánh giá và xác định.** |
| General data - dữ liệu chung | Các loại dữ liệu khác không bao gồm dữ liệu quan trọng và cốt lõi.|
| Personal information - thông tin cá nhân | Các loại thông tin khác nhau liên quan đến các cá nhân đã xác định hoặc có thể xác định được được ghi lại ở dạng điện tử hoặc các dạng khác. <br><br>**Lưu ý: thông tin cá nhân cũng được định nghĩa trong Luật Bảo vệ Thông tin Cá nhân (PIPL), cũng như các quy định khác.**|
| Sensitive personal information - Thông tin cá nhân nhạy cảm | Một loại thông tin cá nhân, một khi bị rò rỉ hoặc sử dụng trái phép, có thể dễ dàng dẫn đến xâm phạm nhân phẩm của cá nhân hoặc gây nguy hiểm cho an ninh cá nhân và tài sản của họ. <br><br>**Lưu ý: Thông tin cá nhân nhạy cảm được định nghĩa chi tiết hơn trong các quy định và tiêu chuẩn khác.**|
| Dữ liệu công nghiệp | Dữ liệu được thu thập và tạo ra trong quá trình thực hiện nhiệm vụ công tác hoặc hoạt động kinh doanh trong một ngành công nghiệp nhất định theo quy định của pháp luật|
| Public data - Dữ liệu công khai | Dữ liệu do các sở ban ngành chính phủ các cấp, các tổ chức có chức năng quản lý và dịch vụ công thu thập và tạo ra, cũng như các đơn vị hỗ trợ kỹ thuật của họ trong quá trình thực hiện hợp pháp các nhiệm vụ quản lý công vụ hoặc cung cấp dịch vụ công.|
| Organization data | Dữ liệu do các tổ chức thu thập và tạo ra trong các hoạt động sản xuất và vận hành của riêng họ không liên quan đến thông tin cá nhân và lợi ích công cộng.|
| Derived data - Dữ liệu phái sinh | Dữ liệu được tạo ra thông qua các hoạt động thống kê, tương quan, khai thác, tổng hợp, xóa nhận dạng và các hoạt động xử lý khác.|
  * Dựa vào các tiêu chuẩn này, dữ liệu được phân loại và quản lý dựa trên ngành công nghiệp liên quan, và sẽ có cơ quan quản lý riêng cho từng ngành
  * Các dữ liệu cũng được chia tiếp theo các thuộc tính phân loại ngành kinh doanh (Business Attribute Classification): domain, Responsible department, Descriptive targets, Process Stages, Data Subjects (Public, organizational, or personal data), Content themes (topics of data), Data purpose, Data processing, Data Source
  * Và một số loại dữ liệu đặc biệt
* Sau quá trình phân loại là quá trình đánh giá dữ liệu (Data grading), dựa trên [[https://www.chinesestandard.us/products/gbt20984-2022?srsltid=AfmBOoq1vA_mPA2NKaPiN-wyD9-BtQ4tZKZf5H9grMpD7NbusOi4ydKS|Tiêu chuẩn Quốc gia này]]
  * Dựa trên các thuật ngữ bên trên, các cơ quan đánh giá dữ liệu thành 3 loại: Core Data, Important Data, General Data.
  * Các bước phân loại bao gồm: Xác định mục tiêu (mục dữ liệu, tập dữ liệu, dữ liệu phái sinh và dữ liệu liên ngành), Nhận biết các Yếu tố Phân loại (domain, khu vực, độ chính xác, quy mô, độ sâu và tầm quan trọng), Phân tích Tác động (Đánh giá tác hại tiềm ẩn và mức độ tác động), Xác định Mức độ Toàn diện (Hoàn thiện mức độ rủi ro dựa trên các yếu tố phân loại định tính và định lượng)
* Cross-border Data Transfers: DSL không xác định rõ ràng tất cả các quy tắc cho việc truyền dữ liệu xuyên biên giới nhưng hoạt động kết hợp với các luật và quy định khác (ví dụ: PIPL, CSL và các quy tắc cụ thể theo từng lĩnh vực), sau bước data grading, chúng ta về cơ bản xác định được dữ liệu thuộc loại nào (Core Data, Important Data, General Data và thêm một loại hình nữa theo PIPL đó là Personal data) và mỗi loại có một quy định riêng về việc chuyển dữ liệu xuyên biên giới
  * Important Data: Các thực thể (công ty, tập đoàn, cá nhân) chuyển dữ liệu quan trọng ra nước ngoài phải tiến hành đánh giá bảo mật và gửi đến Cục Quản lý Không gian mạng Trung Quốc (CAC) hoặc các cơ quan có thẩm quyền để phê duyệt.
    * Đánh giá bảo mật đánh giá:
      * Tính cần thiết của việc chuyển giao.
      * Các rủi ro đối với an ninh quốc gia và lợi ích công cộng.
      * Tính đầy đủ của các biện pháp bảo vệ dữ liệu tại quốc gia đích.
    * Dữ liệu quan trọng được thu thập hoặc tạo ra tại Trung Quốc bởi các Nhà điều hành cơ sở hạ tầng thông tin quan trọng (CIIOs) phải được lưu trữ tại Trung Quốc.
    * Chỉ được phép chuyển dữ liệu ra nước ngoài sau khi vượt qua quá trình đánh giá bảo mật.
  * Core National Data: mức độ bảo vệ cao nhất, thường bị cấm chuyển ra nước ngoài trừ khi được **chính phủ** chấp thuận một cách rõ ràng rõ ràng.
  * Dữ liệu chung thường có thể được chuyển ra nước ngoài mà không cần sự chấp thuận trước của chính phủ, nhưng các công ty vẫn phải đảm bảo tuân thủ các biện pháp bảo mật cơ bản và các tiêu chuẩn bảo vệ dữ liệu.
  * Thông tin cá nhân: Việc chuyển thông tin cá nhân xuyên biên giới được quy định theo Luật bảo vệ thông tin cá nhân (PIPL), trong đó yêu cầu:
    * Sự đồng ý: Sự đồng ý rõ ràng của cá nhân để chuyển dữ liệu cá nhân của họ ra nước ngoài.
    * Đánh giá bảo mật: Đánh giá bảo mật đối với các lần chuyển dữ liệu liên quan đến khối lượng lớn dữ liệu cá nhân hoặc thông tin cá nhân nhạy cảm.
    * Hợp đồng: Ký các điều khoản hợp đồng chuẩn (SCC) hoặc xin chứng nhận bảo vệ dữ liệu.