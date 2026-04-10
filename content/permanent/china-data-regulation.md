---
title: "China Data Regulation"
aliases: ["China data law", "CSL DSL PIPL", "China cybersecurity law", "Chinese data localization"]
tags: [compliance, data-governance, china, software-architecture]
created: 2026-04-10
updated: 2026-04-10
---

Trung Quốc xây dựng chế độ quản lý dữ liệu trên **3 đạo luật nền tảng**, mỗi luật có phạm vi khác nhau nhưng bổ trợ lẫn nhau, tạo thành một framework toàn diện ảnh hưởng đến bất kỳ tổ chức nào xử lý dữ liệu có liên quan đến Trung Quốc.

## Ba đạo luật nền tảng

### 1. Cybersecurity Law (CSL) — 2017

Luật đầu tiên, đặt nền móng. Áp dụng cho **nhà khai thác mạng** và **cơ sở hạ tầng thông tin quan trọng (CII)**. Các nghĩa vụ chính:

- Triển khai biện pháp bảo mật ngăn chặn truy cập trái phép và rò rỉ dữ liệu.
- **Data localization**: CII operators phải lưu dữ liệu cá nhân và dữ liệu quan trọng _trong lãnh thổ Trung Quốc_. Chuyển ra nước ngoài chỉ khi có lý do kinh doanh thực sự + đánh giá bảo mật + đồng ý của cá nhân.
- Sản phẩm mạng không được cài malware; phải vá lỗi bảo mật kịp thời.
- Thiết bị và dịch vụ ảnh hưởng an ninh quốc gia phải qua đánh giá của CAC.

### 2. Data Security Law (DSL)

Mở rộng phạm vi ra toàn bộ **vòng đời của dữ liệu** (thu thập → lưu trữ → xử lý → truyền → xóa), bao gồm cả tổ chức nước ngoài xử lý dữ liệu liên quan đến Trung Quốc.

DSL yêu cầu **phân loại và đánh giá dữ liệu** theo 3 cấp:

| Loại               | Mô tả                                                                            | Cross-border transfer                                             |
| ------------------ | -------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| **Core Data**      | Dữ liệu ảnh hưởng an ninh chính trị, lợi ích quốc gia tối cao                    | Gần như cấm hoàn toàn trừ khi chính phủ phê duyệt rõ ràng         |
| **Important Data** | Dữ liệu mà nếu lộ/phá hủy có thể gây nguy hiểm an ninh quốc gia, kinh tế, xã hội | Phải qua security assessment + phê duyệt của CAC trước khi chuyển |
| **General Data**   | Tất cả dữ liệu còn lại                                                           | Có thể chuyển tự do nhưng phải đảm bảo biện pháp bảo mật cơ bản   |

Ngoài ra PIPL bổ sung thêm phân loại **Personal Data** (thông tin cá nhân) với quy định riêng.

### 3. Personal Information Protection Law (PIPL)

Tương đương GDPR của châu Âu, nhưng cho thông tin cá nhân người dùng tại Trung Quốc. Yêu cầu:

- **Sự đồng ý rõ ràng** trước khi thu thập và xử lý thông tin cá nhân.
- Đối với chuyển dữ liệu cá nhân ra nước ngoài: cần đồng ý của cá nhân + đánh giá bảo mật (nếu khối lượng lớn) + ký Standard Contractual Clauses (SCC) hoặc chứng nhận bảo vệ dữ liệu.

## Cơ chế tuân thủ cross-border transfer

Ba con đường hợp pháp để chuyển dữ liệu ra khỏi Trung Quốc:

1. **Vượt qua security assessment** do CAC (Cyberspace Administration of China) chỉ đạo — bắt buộc với Important Data và Personal Data quy mô lớn.
2. **Nhận chứng nhận** từ tổ chức được chính phủ chỉ định.
3. **Ký SCC** — standard contractual clauses do chính phủ ban hành.

## Tác động thực tiễn với công ty nước ngoài

- Nếu muốn kinh doanh chính thức tại Trung Quốc: phải đăng ký, tuân thủ CSL, lưu trữ dữ liệu locally.
- Nếu hệ thống xử lý dữ liệu người dùng Trung Quốc từ nước ngoài: vẫn thuộc phạm vi DSL/PIPL.
- Thiết kế hệ thống phải xem xét: data residency requirements, kiến trúc multi-region, quy trình xử lý consent.

> Lưu ý: Ngôn ngữ trong CSL/DSL thường mơ hồ, cần theo dõi thêm các hướng dẫn và tiêu chuẩn quốc gia bổ sung (như GB/T 43697-2024 về data classification).

## Connections

- [[permanent/multi-cloud-architecture]] — Multi-cloud và data residency: Chinese data regulations thường thúc đẩy nhu cầu thiết kế hệ thống tách biệt theo region
- [[permanent/software-architecture-role]] — Compliance như China data law là một trong những constraint mà architect phải xử lý

## Sources

- `writing/china_cross_border_data.md`
- `writing/china_cybersecurity_law.md`
- `writing/china_data_security_law.md`
