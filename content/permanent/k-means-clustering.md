---
title: "K-Means Clustering"
aliases: ["K-Means", "phân cụm K-means"]
tags: [machine-learning, unsupervised-learning, clustering, algorithm]
created: 2026-04-10
updated: 2026-04-10
---

K-Means Clustering là thuật toán phân cụm cơ bản nhất trong [[permanent/machine-learning|Unsupervised Learning]]. Mục tiêu: phân dữ liệu thành **K nhóm (clusters)** sao cho các điểm trong cùng cluster có tính chất gần nhau, còn các cluster khác nhau thì cách xa nhau.

## Thuật toán

1. Chọn **K điểm trọng tâm (centroids)** ngẫu nhiên trong không gian dữ liệu
2. Gán mỗi điểm dữ liệu vào cluster của centroid gần nó nhất
3. Tính lại centroid mới = trung bình vị trí của tất cả điểm trong cluster đó
4. Lặp bước 2–3 cho đến khi assignments không thay đổi (convergence)
5. Để dự đoán cluster cho điểm mới: tìm centroid gần nhất

## Những điều cần lưu ý

**Chọn K**: Không có công thức cứng. Cách thực tế: tăng K dần, vẽ đồ thị squared error (tổng khoảng cách điểm → centroid của nó). Tìm "elbow point" — nơi error ngừng giảm mạnh.

**Local minima**: Centroid khởi tạo ngẫu nhiên → kết quả có thể khác nhau mỗi lần chạy. Giải pháp: chạy nhiều lần với random seeds khác nhau, chọn kết quả tốt nhất.

**Labeling**: K-Means không tự gán ý nghĩa cho từng cluster. Việc hiểu "cluster này là gì" hoàn toàn là công việc của người phân tích — cần domain knowledge để interpret kết quả.

## Connections

- [[permanent/machine-learning]] — K-Means là thuật toán unsupervised learning điển hình
- [[permanent/overfitting-underfitting]] — K-Means ít bị overfitting hơn supervised methods, nhưng chọn K sai vẫn gây underfitting

## Sources

- [[refs/k_means_clustering]] (archived)
