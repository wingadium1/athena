---
title:  "Node Consolidation"
date:   2024-06-16 12:00:00
tags: [Cloud, k8s, type/write]
---

# Node Consolidation
* Work load demand base
* Effiency distributed on **Minimal** number of required nodes
* Reduce resource footprint

## Tại sao?
* Cost Reduction (Optimization, Effiencty): ít node tức là ít tiền
* Resource Utilization: right size pod on right node

# Step 1: Cluster Autoscaler
# Step 2: Overprovisioning
Nếu chúng ta không có các bước như loadtest hay performance test, chúng ta gần như không thể biết được tính chất của 1 ứng dụng hay service sẽ được deploy lên K8S. Đây gần như là một gói bảo hiểm để chúng ta sống sót qua những đợt spike load đầu tiên.
# Step 3: Monitor and Adjust
## Những thiếu sót của Node Consolidation trong Kubernetes
Mặc dù Node Consolidation mang lại nhiều lợi ích cho môi trường Kubernetes, bao gồm tiết kiệm chi phí và cải thiện việc sử dụng tài nguyên, nhưng chúng có những hạn chế. Nhìn chung vấn đề vẫn là trade off, cân bằng và hiệu quả

### Thiếu hiệu quả với các workload biến đổi nhiều
* **Limitation**: Nhìn chung Node Consolidation dựa trên việc chúng ta có thể dự đoán được workload trong một khoảng thời gian để có thể tối ưu hoá việc phân bổ tài nguyên.
Với các loại ứng dụng không thể phán đoán được, chúng ta có thể gặp khó khăn trong việc cân bằng giữa performance và cost effiency.
* **Impact**: Trong trường hợp spike load, đặc biệt khi chúng ta thêm tính năng mới, gần như chúng ta không hề có kinh nghiệm xử lý trong case này, autoscaling cần thời gian để thêm node vào cluster và schedule các pod mới. Đây là một trong những trường hợp phổ biến mà chúng ta sẽ gặp phải. Các spike load service này có thể gây ảnh hưởng đến các service khác vô hình chung có thể dẫn đến hiệu ứng domino gây sụp đổ hệ thống
* **Resolution**: Cách đơn giản và hiệu quả nhất đó là không áp dụng node consolidation cho các ứng dụng mới hoặc big change release, điều này giúp chúng ta tách biệt workload sẵn có đã ổn định. Chúng ta có thể overprovisioning cho các workload mới này mà không tăng quá nhiều chi phí tổng thể.

### Stateful Application
* **Limitation**: Bất chấp những đã có nhiều cải tiến trong việc quản lý các ứng dụng loại này trên Kubernetes, chúng ta có những thách thức cụ thể cho các workload kiểu này, đặc biệt là data persistence, data replication và data restore.
* **Impact**: Đối với các service vừa yêu cầu HA và data consistency một cách nghiêm ngặt, nếu chúng ta hợp nhất các node quá mạnh tay có thể làm tăng nguy cơ mất dữ liệu hoặc không nhất quán trong khi một node bị lỗi và khi cố scheduling lại một pod.
* **Resolution**: Gần như chúng ta sẽ phải quản lý các Stateful application này trong một node pool riêng mà ít biến đổi nhất có thể

### Network Latency và Complexity
* **Limitation**: Node Consolidation thường không tính đến network topology và latency, điều này có thể vô tình làm tăng thời gian kết nối giữa các service, ảnh hưởng đến hiệu suất ứng dụng.
* **Impact**: Các ứng dụng nhạy cảm với độ trễ mạng có thể bị suy giảm hiệu suất, đặc biệt nếu các nhóm thường xuyên giao tiếp được đặt trên các nút ở xa hoặc trên các vùng đám mây.
* * **Resolution**:

