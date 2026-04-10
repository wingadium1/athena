---
title: "Machine Learning"
aliases: ["ML", "học máy"]
tags: [machine-learning, ai, data-science]
created: 2026-04-10
updated: 2026-04-10
---

Machine Learning là tập con của AI, bao gồm các thuật toán có khả năng **học từ dữ liệu** để đưa ra dự báo hoặc quyết định mà không cần được lập trình tường minh cho từng trường hợp. Thay vì viết rules, ta cho model học patterns từ examples.

## Hai nhánh chính

### Supervised Learning — Học có giám sát

Huấn luyện trên dữ liệu đã được gắn nhãn (labeled data): mỗi input đi kèm với expected output. Model học mapping từ input → output, sau đó dùng mapping đó để dự đoán trên dữ liệu mới chưa thấy.

Ví dụ: ảnh chữ số viết tay được annotate với số tương ứng → model học nhận dạng chữ số. Sau khi train xong, model trả lời được "ảnh này là số mấy?" cho bất kỳ ảnh mới nào.

Thuật toán tiêu biểu: [[permanent/naive-bayes-classifier|Naive Bayes]], linear regression, decision tree, SVM, neural network.

### Unsupervised Learning — Học không có giám sát

Huấn luyện trên dữ liệu **không có nhãn**. Model tự tìm structure, pattern, hoặc grouping ẩn trong dữ liệu mà không có "đáp án đúng" để so sánh.

Thường dùng khi: không biết đang tìm gì, muốn discover hidden variables, hoặc muốn group các objects có tính chất tương đồng.

Kết quả thường là clusters hoặc reduced representations. Ví dụ: nhóm các bài báo từ nhiều nguồn vào danh mục Sports/Crime/Politics mà không cần training labels.

Thuật toán tiêu biểu: [[permanent/k-means-clustering|K-Means Clustering]], PCA, autoencoders.

## Vấn đề phổ biến

- **[[permanent/overfitting-underfitting|Overfitting]]**: Model học quá tốt trên training data, fail trên data mới
- **Underfitting**: Model quá đơn giản, không capture được pattern quan trọng
- **Data quality**: Garbage in, garbage out — chất lượng dữ liệu quyết định chất lượng model

## Connections

- [[permanent/naive-bayes-classifier]] — supervised learning algorithm dựa trên Bayes theorem
- [[permanent/k-means-clustering]] — unsupervised learning algorithm phân cụm dữ liệu
- [[permanent/overfitting-underfitting]] — vấn đề phổ biến nhất khi training model

## Sources

- [[refs/machine_learning]] (archived)
- [[refs/supervised_learning]] (archived)
- [[refs/unsupervised_learning]] (archived)
