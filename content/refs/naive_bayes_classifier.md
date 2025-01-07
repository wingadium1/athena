---
title: "Naive Bayes classifier"
alias:
  - NBC
tags: [ Machine Learning ]
---

> [!info] Naive Bayes classifiers
> A Naive Bayes classifiers - Phân loại Naive Bayes hay còn gọi là Phân loại Bayes ngây thơ, một tập các thuật toán dựa trên Bayes’ Theorem.
> Bất chấp giả định “ngây thơ” về tính độc lập của đặc điểm, các bộ phân loại này được sử dụng rộng rãi vì tính đơn giản và hiệu quả của chúng trong [[refs/machine_learning|Machine Learning]]
> Các thuật toán này thuộc nhóm [[refs/supervised_learning|Supervised Learning]].


Nói một cách đơn giản như này
$$
P(A/B) = \frac{P(B|A) * P(A)}{P(B))}
$$


hoặc

$$
posterior = \frac{prior * likelihood}{evidence}
$$