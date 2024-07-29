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

### Phân mảnh tài nguyên
* **Limitation**: Các chiến lược cấp phát quá mức cần thiết và tự động mở rộng (HPA/VPA) có thể dẫn đến sự phân mảnh tài nguyên, trong đó một lượng nhỏ tài nguyên không được sử dụng nằm rải rác trên cụm, bản thân phần nhỏ này không đủ để schedule cho bất kì pod nào.

* **Impact**: Điều này có thể dẫn đến tình huống trong đó cụm dường như có đủ tài nguyên về tổng thể, nhưng không thể tiếp tục schedule cho pod. Tuy nhiên điểm này cũng chỉ là một tình huống nhỏ, thường với các cluster lớn điều này có thể chấp nhận được.

### Delay scale
* **Limitation**: Khả năng đáp ứng của các cơ chế tự động scale (HPA và VPA) có thể không phải lúc nào cũng phù hợp với tốc độ thay đổi nhanh chóng của khối lượng công việc, dẫn đến thiếu hụt tài nguyên cục bộ hoặc dư thừa ngay sau khi scale.

* **Impact**: Sự chậm trễ đôi chút trong việc scale có thể ảnh hưởng đến hiệu suất và khi chậm scale down thì thường không ảnh hưởng, có chăng là chi phí.

## Hiệu quả chi phí đánh đổi hiệu suất
* **Limitation**: Việc đạt được sự cân bằng tối ưu giữa tiết kiệm chi phí và hiệu suất ứng dụng có thể là một thách thức. Các chiến lược tối ưu hóa chi phí linh hoạt có thể ảnh hưởng đến khả năng đáp ứng hoặc tính khả dụng của ứng dụng.

Tác động: Các tổ chức có thể gặp khó khăn trong việc đáp ứng SLA hiệu suất trong khi cố gắng giảm thiểu chi phí, đặc biệt đối với các ứng dụng quan trọng có yêu cầu nghiêm ngặt.

Không tương thích với các ứng dụng cũ
Thiếu sót: Các ứng dụng cũ không được thiết kế cho môi trường dựa trên nền tảng đám mây có thể gặp phải các vấn đề về khả năng tương thích với các chiến lược hợp nhất nút và mở rộng quy mô động.

Tác động: Các ứng dụng như vậy có thể yêu cầu tái cấu trúc hoặc kiến ​​trúc lại đáng kể để hưởng lợi từ khả năng điều phối của Kubernetes, vốn có thể tốn nhiều tài nguyên và thời gian.

Những cạm bẫy phổ biến trong việc hợp nhất nút và các chiến lược phòng ngừa và giải quyết
Hợp nhất thành công các nút trong Kubernetes có thể giảm đáng kể chi phí và nâng cao hiệu quả. Tuy nhiên, một số cạm bẫy phổ biến có thể phát sinh trong quá trình này. Hiểu được những thách thức này cũng như biết cách ngăn chặn và giải quyết chúng là điều quan trọng để duy trì môi trường Kubernetes lành mạnh và tiết kiệm chi phí.

Cung cấp quá mức vượt quá nhu cầu
Cạm bẫy: Việc cung cấp quá nhiều tài nguyên “để đề phòng” có thể dẫn đến những chi phí không cần thiết, cản trở việc hợp nhất nút tiết kiệm được cho là sẽ mang lại.

Phòng ngừa: Sử dụng các công cụ đo lường và giám sát để hiểu chính xác việc sử dụng tài nguyên của khối lượng công việc của bạn. Điều chỉnh chiến lược cung cấp vượt mức của bạn dựa trên dữ liệu và xu hướng lịch sử thay vì giả định.

Giải pháp: Thường xuyên xem xét việc sử dụng tài nguyên của cụm của bạn. Các công cụ như Kubecost có thể cung cấp thông tin chuyên sâu về nơi tài nguyên đang bị lãng phí và đề xuất các biện pháp tối ưu hóa.

Khối lượng công việc quan trọng được cung cấp dưới mức
Cạm bẫy: Hợp nhất nút quá tích cực có thể dẫn đến không đủ tài nguyên cho khối lượng công việc, ảnh hưởng đến hiệu suất và tính khả dụng.

Phòng ngừa: Triển khai các yêu cầu và giới hạn tài nguyên một cách thận trọng để đảm bảo các ứng dụng quan trọng có được tài nguyên cần thiết. Sử dụng các lớp Chất lượng dịch vụ (QoS) của Kubernetes để