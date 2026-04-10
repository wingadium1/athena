---
title: "Overfitting và Underfitting"
aliases: ["Overfitting", "Underfitting", "model generalization"]
tags: [machine-learning, model-training, statistics]
created: 2026-04-10
updated: 2026-04-10
---

Overfitting và Underfitting là hai thái cực của cùng một vấn đề: **generalization** — khả năng model hoạt động tốt trên dữ liệu chưa thấy (unseen data), không chỉ trên dữ liệu đã train.

## Overfitting

Model học **quá khớp** với training data — kể cả noise và các chi tiết không đại diện. Kết quả: accuracy cao trên train set, thấp trên test set.

**Nguyên nhân:**

- Training data quá nhỏ, không đại diện đủ cho không gian dữ liệu thực
- Training data chứa nhiều noise
- Train quá lâu (model memorize thay vì learn)
- Model quá phức tạp so với bài toán

## Underfitting

Model **quá đơn giản**, không capture được relationship có ý nghĩa giữa input và output. Accuracy thấp trên cả train lẫn test set.

**Nguyên nhân:** Model capacity quá thấp (e.g., dùng linear model cho bài toán phi tuyến), train quá ít, features không đủ thông tin.

## Phát hiện và xử lý

**Phát hiện**: Tách một phần training data ra làm validation set — so sánh performance trên train vs. validation.

**K-fold cross-validation**: Chia train data thành K tập con. Mỗi lần: dùng 1 tập làm validation, K−1 tập còn lại làm train. Lặp K lần, lấy average performance. Cho ước lượng generalization robust hơn so với single split.

**Xử lý overfitting:**

- Thêm data
- Regularization (L1/L2)
- Dropout (neural networks)
- Early stopping
- Giảm model complexity

**Xử lý underfitting:**

- Tăng model complexity
- Thêm features
- Train lâu hơn

## Connections

- [[permanent/machine-learning]] — overfitting/underfitting là vấn đề core trong mọi ML workflow
- [[permanent/k-means-clustering]] — unsupervised methods như K-Means ít bị overfitting hơn
- [[permanent/naive-bayes-classifier]] — NBC's simplicity làm nó resistant to overfitting

## Sources

- [[refs/overfitting]] (archived)
