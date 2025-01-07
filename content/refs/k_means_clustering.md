---
title: "K-Means Clustering"
tags: [ Machine Learning, Mathematic, Statistic ]
---

> [!info] K-Means Clustering
> hay còn gọi là phân cụm K-means, là thuật toán cơ bản nhất trong [[refs/unsupervised_learning|Unsupervised Learning]].
> Trong thuật toán này, chúng ta không biết nhãn (label) của từng điểm dữ liệu. Mục đích là làm thể nào để phân dữ liệu thành các cụm (cluster) khác nhau sao cho dữ liệu trong cùng một cụm có tính chất giống nhau.

Step
* chọn K điểm trọng tâm (K-means, K centroids) một cách ngẫu nhiên
* Thử chia dữ liệu thành K groups gần nhất với K trọng tâm.
* [[refs/unsupervised_learning|Unsupervised Learning]] - sử dụng các vị trí của mỗi điểm dữ liệu
  * Tính toán lại trọng tâm dựa trên vị trí trung bình của mỗi điểm trọng tâm
  * Lặp lại cho đến khi kết quả ổn định
* Nếu muốn dự đoán cluster cho các điểm mới, thử tìm điểm trọng tâm mà gần nó nhất.

Một số điều lưu ý
* Chọn K: hãy thử tăng K đến khi squared error (khoảng cách từ mỗi datapoint đến điểm trọng tâm của nó) ngừng giảm, tức là số liệu bắt đầu ổn định
* avoiding local minima - tránh các cực tiểu cục bộ
  * việc lựa chọn ngẫu nhiên các tâm ban đầu có thể mang lại các kết quả khác nhau
  * hãy chạy nó một vài lần chỉ để đảm bảo rằng các kết quả ban đầu của bạn không bị lệch lạc
* labeling clusters
  * K-Means không gắn ý nghĩa cho từng cluster được tìm thấy
  * Thông tin kết quả hoàn toàn phụ thuộc vào bạn