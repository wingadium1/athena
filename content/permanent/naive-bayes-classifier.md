---
title: "Naive Bayes Classifier"
aliases: ["NBC", "Naive Bayes", "Bayes classifier"]
tags: [machine-learning, supervised-learning, probability, statistics, algorithm]
created: 2026-04-10
updated: 2026-04-10
---

Naive Bayes Classifier (NBC) là một họ thuật toán [[permanent/machine-learning|Supervised Learning]] dựa trên **Bayes' Theorem**, với giả định "ngây thơ" rằng các features là **độc lập có điều kiện** với nhau. Dù giả định này hiếm khi đúng trong thực tế, NBC vẫn hoạt động tốt một cách đáng ngạc nhiên — đặc biệt trong text classification và spam detection.

## Bayes' Theorem

$$
P(A \mid B) = \frac{P(B \mid A) \cdot P(A)}{P(B)}
$$

Hay diễn đạt theo ngôn ngữ ML:

$$
\text{posterior} = \frac{\text{likelihood} \times \text{prior}}{\text{evidence}}
$$

- **Prior** P(A): Xác suất ban đầu của hypothesis trước khi thấy dữ liệu
- **Likelihood** P(B|A): Xác suất quan sát được dữ liệu B nếu hypothesis A đúng
- **Evidence** P(B): Xác suất tổng của dữ liệu B (normalization factor)
- **Posterior** P(A|B): Xác suất hypothesis A đúng sau khi quan sát dữ liệu B

## Multinomial Distribution trong NBC

Khi dữ liệu là discrete counts (ví dụ: word counts trong văn bản), NBC thường dùng **Multinomial distribution** — phân phối xác suất cho các thử nghiệm liên quan đến từ hai biến trở lên. Ví dụ: xác suất xuất hiện của mỗi từ trong một class (spam vs. not spam) tuân theo multinomial distribution.

## Ứng dụng điển hình

- **Spam filtering**: P(spam | các từ trong email)
- **Text classification**: phân loại bài báo theo chủ đề
- **Sentiment analysis**: positive/negative review
- **Medical diagnosis**: P(bệnh | triệu chứng)

## Ưu và nhược điểm

**Ưu**: Đơn giản, train nhanh, cần ít data, hoạt động tốt với high-dimensional data (nhiều features).

**Nhược**: Giả định independence thường sai trong thực tế; không capture được correlation giữa features; ước lượng xác suất kém chính xác (nhưng classification vẫn tốt).

## Connections

- [[permanent/machine-learning]] — NBC là supervised learning algorithm
- [[permanent/overfitting-underfitting]] — NBC ít bị overfitting do model đơn giản

## Sources

- [[refs/naive_bayes_classifier]] (archived)
- [[refs/multinomial_distribution]] (archived)
