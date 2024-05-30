---
title: "Cost-effective, High Availability Cassandra with AWS EKS and EC2 Spot instance."
tags: [ AWS, EC2, Spot Instance, HA, Cassandra, Iaas, type/blog ]
---

# Mở đầu

Cassandra hay Apache Cassandra, là một hệ thống quản lý cơ sở dữ liệu NoSQL, mã nguồn mở, miễn phí, phân tán dựa trên
mạng ngang hàng P2P, hiện tại thường dùng dễ lưu trữ dữ liệu dưới dạng timeseries.

Bản thân Cassandra đã có khả năng High availability với thiết kế
no [single point of failure](https://en.wikipedia.org/wiki/Single_point_of_failure) và bản thân Cassandra cũng hỗ trợ
việc mở rộng node một cách dễ dàng, vậy tại sao không mang sức mạnh của EC2 Spot Instance (chi phí rẻ cho khả năng tính
toán lớn).

Chúng ta lợi dụng một số tính năng sau của Cassandra để xử lý:

* Data Center

  ![image](https://s3.ap-southeast-1.amazonaws.com/techover.storage/wp-content/uploads/2021/08/17145816/Datacenter.png)

    * Trong đó data center sẽ đóng vai trò như một cụm node, Cassandra có thể live backups giữa các data center, data sẽ tự động copy async sang DC khác, khi một DC down các DC khác vẫn hoạt động bình thường

* Seed nodes: Seed nodes sẽ là nơi các node mới connect và thông báo về việc chúng join cluster
  Seed node hoạt động như các điểm chung chuyển, các node sẽ trao đổi với các node seeds hơn các node khác, và các node
  này thường sẽ có các thông tin mới nhất và đầy đủ nhất, nhưng chúng sẽ gặp vấn đề overhead nên đừng sử dụng mọi node
  làm seeds.
* [Data replication](https://docs.datastax.com/en/cassandra-oss/3.0/cassandra/architecture/archDataDistributeReplication.html#archDataDistributeReplication__rep-strategy-ul):
  Cassandra lưu trữ dữ liệu trên nhiều node để đảm bảo tính toàn vẹn và fault tolerance (mình khá không thích dịch tiếng
  việt tự này, có thể dịch là khả năng chịu lỗi). Có 2 strategy: Simple và NetworkTopology, vì chúng ta dự định sử dụng
  data center nên hãy chọn NetworkTopology

Như vậy về mặt lý thuyết chúng ta có thể xử lý được toàn bộ vấn đề, hãy mapping chúng với, K8S và AWS thậm chí hoàn toàn
có thể xử lý được với trường hợp sử dụng Spot Instance.

![image](https://s3.ap-southeast-1.amazonaws.com/techover.storage/wp-content/uploads/2021/08/17172124/HA-cassandra.png)

Chúng ta sẽ sử dụng luôn khái niệm Availability Zone của AWS cho tương ứng với data center.
Như vậy sẽ có 1 Statefulset cài đặt Cassandra, 1 Service để expose với mỗi DC.

# Cài đặt nào

Thực ra script đã được chuẩn bị ở [đây](https://gist.github.com/wingadium1/8455b97b25c64782d2a0378f20c53686) rồi.

Mình sẽ giải thích một vài điểm cần chú ý

Chúng ta add label cho các pod, việc này để các service có thể chọn được các pod của cassandra

``` yaml
  template:
    metadata:
      labels:
        app: cassandra
        interface: cassandraa
```

Chọn node để cài đặt cassandra, chúng ta có thể dùng các key khác nhưng để cho tiện thì mình dùng tạm key này, việc này
đảm bảo node của DC được cài đặt theo AZ của AWS đúng tinh thần High availability

``` yaml
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: topology.kubernetes.io/zone
                operator: In
                values:
                - ap-southeast-1a
```

Cassandra seeds node, node đầu tiên của các statefulset được chọn làm seed, ở đây mình xử lý dùng 1 service cassandra
thay vì dùng 3 service cho 3 AZ (một điểm nho nhỏ khác biệt), việc này không ảnh hưởng lắm.

``` yaml
  - name: CASSANDRA_SEEDS
    value: cassandraa-0.cassandra.thingsboard.svc.cluster.local,cassandrab-0.cassandra.thingsboard.svc.cluster.local,cassandrac-0.cassandra.thingsboard.svc.cluster.local
```

Chúng ta không cần tất cả các seed cùng một lúc, nên ngay cả khi seed down thì node vẫn hoạt động bình thường.

Ở các pod sử dụng cassandra này thì cần chỉ rõ DC nào của Cassandra để kết nối đến, ở đây mình đang cài
đặt [Thingsboard](https://thingsboard.io
) nên sẽ thêm environment variable sau, tất nhiên sẽ phải xử lý tách application của bạn ra 3 statefulset hoặc
deployment khác nhau:

``` yaml
  - name: CASSANDRA_LOCAL_DATACENTER
    value: ap-southeast-1a
```

Tada

``` log
2021-08-17 18:08:36
2021-08-17 11:08:36,628 [main] INFO  o.s.o.j.LocalContainerEntityManagerFactoryBean - Initialized JPA EntityManagerFactory for persistence unit 'default'
2021-08-17 18:08:52
2021-08-17 11:08:52,732 [main] INFO  c.d.o.d.internal.core.ContactPoints - Contact point cassandra:9042 resolves to multiple addresses, will use them all ([cassandra/10.0.1.112, cassandra/10.0.2.149, cassandra/10.0.1.150])
2021-08-17 18:08:53
2021-08-17 11:08:53,734 [main] INFO  c.d.o.d.i.c.DefaultMavenCoordinates - DataStax Java driver for Apache Cassandra(R) (com.datastax.oss:java-driver-core) version 4.10.0
2021-08-17 18:08:54
2021-08-17 11:08:54,956 [Thingsboard Cluster-admin-0] INFO  c.d.o.d.internal.core.time.Clock - Could not access native clock (see debug logs for details), falling back to Java system clock
2021-08-17 18:08:56
2021-08-17 11:08:56,621 [Thingsboard Cluster-admin-0] WARN  c.d.o.d.i.c.l.h.OptionalLocalDcHelper - [Thingsboard Cluster|default] You specified ap-southeast-1b as the local DC, but some contact points are from a different DC: Node(endPoint=cassandra/10.0.1.112:9042, hostId=a56560a6-1274-43f9-b72e-d8b1e7b33bf8, hashCode=6a745db0)=ap-southeast-1a, Node(endPoint=cassandra/10.0.1.150:9042, hostId=c7f9bc1c-c066-40d6-9def-7fbe58af90bb, hashCode=2c4f4fec)=ap-southeast-1a; please provide the correct local DC, or check your contact points
```

Nếu dòng log cuối gây confuse thì hãy sử dụng các sevice riêng biệt nhé

*Happy Coding*