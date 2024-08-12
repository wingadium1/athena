---
title: "Seekable OCI"
tags: [  AWS, DevOps ]
---

> [!info]
> hay SOCI là một công nghệ nguồn mở do AWS cung cấp khả năng khởi động container nhanhhonw bằng cách
> sử dụng lazy loading container image.
>
> SOCI sẽ tạo một index (SOCI index) cho những layer trong container image, đây là kỹ thuật chính trong
> việc khởi động container nhanh hơn, cung cấp khả năng trích xuất một tệp riêng lẻ từ container image
> trước khi tải xuống toàn bộ.