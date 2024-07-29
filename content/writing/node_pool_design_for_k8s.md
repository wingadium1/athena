---
title:  "Thiết kế node pool"
date:   2024-06-16 12:00:00
tags: [Cloud, k8s, type/write]
---

Nhìn chung việc sử dụng K8S là khó, tại sao có khái niệm "taining" nhưng tại sao lại có khái niệm "tolerate" nữa.

Về cơ bản mọi người sẽ bắt đầu sử dụng k8s với các câu lệnh kiểu như:

```
kubectl run my-nginx --image=nginx --replicas=2 --port=80
```
Nhưng khi triển khai ứng dụng trong tình huống thực tế, chúng ta gần như sẽ không có những việc dễ dàng như vậy. Nhìn chung chúng ta sẽ phải vận dụng các đặc tính auto-scaling và self-healing của k8s cùng với các cơ chế điều phối (scheduling) của nó.

Vấn đề ở đây đặt ra là: Một số pod yêu cầu các node được tối ưu hóa và sử dụng nhiều CPU hoặc Bộ nhớ. Một số pod đang xử lý thuật toán ML/AI và cần các node hỗ trợ GPU. Các node hỗ trợ GPU này chỉ nên được sử dụng bởi một số nhóm nhất định vì chúng quá đắt đỏ. Một số pod/job muốn tận dụng các node dạng spot hay preemptible để giảm chi phí.

Điều này có nghĩa là chạy các Pod ứng dụng trên đúng node với thông số kỹ thuật chính xác - có thể là node sử dụng nhiều CPU hoặc nhiều bộ nhớ, hay chạy trên một ổ cứng nhanh hơn. Tương tự, điều này cũng có nghĩa là một số microservice sẽ hoạt động tốt nhất (hoặc tốt hơn), với tư cách là một phần của toàn bộ hệ thống lớn nếu chúng hoạt động gần với nhau hơn(affinity), trong khi trong các trường hợp khác, bạn muốn đảm bảo rằng chúng sẽ không hoạt động cùng trên node trên cùng node với pod khác
khác(anti-affinity).

Mục đích chính là để tách biệt hoặc phân bổ workload phù hợp và để tránh việc dồn dập việc lập lịch cho các Pod không có hạn chế (free-for-all) — việc sử dụng tài nguyên kém sẽ gây tác động tiêu cực lên cloud cost.

## TL;DR
Cơ bản thì chúng ta sẽ cần phối hợp sử dụng [[https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/|taint và toleration]] kết hợp với [[https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#affinity-and-anti-affinity|affinit và anti-affinity]] ở trên để đặt chính xác pod vào chỗ mà chúng ta mong muốn. Nó hoạt động thế nào thì hãy tham khảo tài liệu đầy đủ của k8s nhé.

Tóm tắt thì là như này:
- taint sẽ cho phép node từ chối pod có không cùng toleration key-value
- toleration sẽ được đặt ở pod và workload có cùng taint key-value, vế này là bị động
- node không có taint có thể dùng để chạy bất kì pod nào, kể cả pod có toleration.



https://medium.com/contino-engineering/why-separate-your-kubernetes-workload-with-nodepool-segregation-and-affinity-rules-cb5225953788

https://stackoverflow.com/questions/74623525/why-there-is-no-concept-of-nodepool-in-kubernetes

https://overcast.blog/optimizing-node-groups-in-kubernetes-06fa54b6315d


https://blenderfox.com/category/kubernetes/


