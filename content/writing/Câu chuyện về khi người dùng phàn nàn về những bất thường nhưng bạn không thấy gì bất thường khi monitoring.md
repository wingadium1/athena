Trong các hệ thống phân tán, một trong những vấn đề khó khăn nhất là xử lý các trường hợp trễ bất thường (latency outliers). 

Hình dung như này, ngay cả khi bạn có một hệ thống với response time trung bình của một service là chấp nhận được, có một tỷ lệ nhỏ các yêu cầu có thể mất thời gian dài hơn đáng kể, gây ra trải nghiệm người dùng kém. 
![[Screenshot 2025-03-21 at 21.59.40.png]]
Ở hình minh hoạ này, khi theo dõi target response time avg trên load balancer, chúng ta có thể thấy chỉ số khá tốt, tuy nhiên khi xét phân vị p99 và Max, con số khác xa nhưng với p80 con số lại gần với average, tức là đâu đó có một phần nhỏ một vài % số request bị chậm hơn hẳn.

Vấn đề này thường được gọi là vấn đề "**[tail latency](https://andrewpakhomov.com/posts/latency-tail-latency-and-response-time-in-distributed-systems/)**", và nó được minh hoạ như này
![[Pasted image 20250321214431.png]]

Một người bạn cùng trường đại học đã [đề cập](https://quanghoang.substack.com/p/50-days-of-sd-hedged-request) rằng tail latency là một thứ đáng quan tâm ở các công ty lớn, và ngay cả ở một project không được optimize nhiều lắm trong quá trình làm việc của tôi thì con số những request chậm hơn hẳn cũng chỉ là 1-5%, vậy điều gì làm chúng ta phải lo lắng về việc nó chậm một số lượng request nhỏ đến thế.

Dưới đây là đoạn viết lại với ví dụ về việc dựng một service feed dựa trên khoảng 10 service độc lập:

---

Giả sử chúng ta cần xây dựng một service feed bằng cách gọi khoảng 10 service độc lập. Mỗi service có độ trễ tại phân vị thứ 99 (p99) là 1 giây, nghĩa là mỗi service có 1% khả năng phản hồi chậm hơn 1 giây.

Do độ trễ cuối cùng của service feed phụ thuộc vào service trả về kết quả chậm nhất, xác suất để người dùng gặp phải độ trễ lớn hơn 1 giây được tính như sau:

- Xác suất mỗi service phản hồi nhanh (≤ 1 giây):  
$$
P(\text{fast}) = 99\% = 0.99
$$
- Xác suất cả 10 service đều phản hồi nhanh (≤ 1 giây):  
  $$
	P(\text{all fast}) = 0.99^{10} \approx 0.9044 = 90.44\%
   $$

- Như vậy, xác suất có ít nhất một service phản hồi chậm hơn 1 giây là:  
$$
P(\text{at least 1 slow}) = 1 - P(\text{all fast}) = 1 - 0.9044 = 0.0956 = 9.56\%
$$
Điều này cho thấy, dù mỗi service riêng lẻ chỉ có 1% khả năng bị chậm, nhưng khi kết hợp 10 service lại với nhau, xác suất người dùng gặp độ trễ lớn hơn 1 giây đã tăng lên khoảng 9.56%. Nói cách khác, một tỷ lệ nhỏ latency bất thường ở từng service riêng lẻ cũng có thể gây ảnh hưởng đáng kể đến trải nghiệm chung của người dùng. Ở ví dụ trên con số của chúng tôi cũng tầm tầm đó, vậy có cách nào giải quyết không?

May thay khi tìm hiểu về gRPC, Google có đề cập đến một kỹ thuật gọi là [Request Hedging hay Hedged Request](https://grpc.io/docs/guides/request-hedging/) gì đó

![[Pasted image 20250321221056.png]]
## Hedged Requests là gì?

Hedged Requests là mô hình gửi nhiều yêu cầu giống hệt nhau tới các instance khác nhau của một service, đồng thời hoặc sau một khoảng thời gian ngắn. Client sẽ sử dụng phản hồi đầu tiên nhận được và hủy các yêu cầu còn lại. Cách tiếp cận này giúp tránh các ảnh hưởng của các trường hợp trễ bất thường bằng cách "đua" nhiều instance service với nhau.

Các đặc điểm chính của mô hình Hedged Requests:

1. **Nhiều yêu cầu giống hệt nhau**: Cùng một yêu cầu được gửi tới nhiều service instance.
2. **Phản hồi đầu tiên thắng**: Client sử dụng phản hồi nhận được đầu tiên.
3. **Hủy các yêu cầu còn lại**: Khi một phản hồi được nhận, các yêu cầu khác đang chạy sẽ bị hủy để tránh công việc không cần thiết.
4. **Xem xét tài nguyên**: Vì mô hình này làm tăng tải hệ thống, cần sử dụng một cách cẩn thận.
## Khi nào nên sử dụng Hedged Requests

Mô hình này đặc biệt hữu ích trong các trường hợp sau:
- Có nhiều instance của cùng một service.
- Các trường hợp trễ đột ngột xảy ra thường xuyên.
- Chi phí xử lý các yêu cầu trùng lặp là chấp nhận được.
- Độ trễ thấp là rất quan trọng đối với trải nghiệm người dùng.
Các trường hợp sử dụng phổ biến bao gồm:
- Các truy vấn tìm kiếm.
- Các thao tác đọc cơ sở dữ liệu.
- API gateways.
- Các thao tác chủ yếu đọc, không bị ảnh hưởng bởi nhiều yêu cầu giống nhau.
## Sample Hedged Requests

Mình sẽ code thử  Hedged Requests bằng Spring Boot và WebFlux. Triển khai của chúng ta gồm hai service:

1. Một service cố ý tạo ra độ trễ ngẫu nhiên.
2. Một client sử dụng Hedged Requests để giảm thiểu độ trễ này.
3. Và một client call trực tiếp để đối chiếu

### Code

Đầu tiên, service tạo độ trễ ngẫu nhiên như sau:

```java
package com.example.service;  
  
import lombok.extern.log4j.Log4j2;  
import org.springframework.boot.SpringApplication;  
import org.springframework.boot.autoconfigure.SpringBootApplication;  
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;  
import org.springframework.web.bind.annotation.GetMapping;  
import org.springframework.web.bind.annotation.RestController;  
  
import java.util.Random;  
  
@Log4j2  
@RestController  
@SpringBootApplication  
@EnableDiscoveryClient  
public class ServiceApplication {  
  
    private long random(int seconds) {  
        return new Random().nextInt(seconds) * 1000;  
    }  
  
    @GetMapping("/hi")  
    String greet() throws InterruptedException {  
        long delay = Math.max(random(5), random(5));  
        Thread.sleep(delay);  
        final String msg = "Hello, after " + delay + " ms.";  
        log.info("returning (" + msg + ")");  
        return msg;  
    }  
  
    public static void main(String[] args) {  
        SpringApplication.run(ServiceApplication.class, args);  
    }  
}
```

Service này có một endpoint `/hi` tạo độ trễ ngẫu nhiên từ 0 đến 5 giây trước khi trả lời, mô phỏng các trường hợp trễ bất thường.

### Triển khai client với Hedged Requests

Client sử dụng Hedged Requests như sau:

```java
package com.example.client;

import lombok.extern.log4j.Log4j2;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.ServiceInstance;
import org.springframework.cloud.client.discovery.DiscoveryClient;
import org.springframework.cloud.client.loadbalancer.LoadBalancerClient;
import org.springframework.context.annotation.Bean;
import org.springframework.stereotype.Component;
import org.springframework.util.Assert;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.reactive.function.client.*;
import reactor.core.publisher.Flux;
import reactor.core.publisher.Mono;

import java.net.URI;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

@SpringBootApplication
public class ClientApplication {

    public static void main(String[] args) {
        SpringApplication.run(ClientApplication.class, args);
    }

    @Bean
    WebClient client(HedgeExchangeFilterFunction eff) {
        return WebClient.builder().filter(eff).build();
    }
}

@Log4j2
@Component
Component  
public class HedgeExchangeFilterFunction implements ExchangeFilterFunction {  
  
    private final DiscoveryClient discoveryClient;  
    private final LoadBalancerClient loadBalancerClient;  
    private final int attempts, maxAttempts;  
  
    @Autowired  
    HedgeExchangeFilterFunction(DiscoveryClient dc, LoadBalancerClient lbc) {  
        this(dc, lbc, 3);  
    }  
  
    HedgeExchangeFilterFunction(  
            DiscoveryClient discoveryClient,  
            LoadBalancerClient loadBalancerClient,  
            int attempts) {  
        this.discoveryClient = discoveryClient;  
        this.loadBalancerClient = loadBalancerClient;  
        this.attempts = attempts;  
        this.maxAttempts = attempts * 2;  
    }  
  
    @Override  
    public Mono<ClientResponse> filter(ClientRequest request, ExchangeFunction next) {  
        // Get the original URI and extract the service ID (host)  
        URI originalURI = request.url();  
        String serviceId = originalURI.getHost();  
          
        // Get all instances of the service  
        List<ServiceInstance> serviceInstanceList = this.discoveryClient.getInstances(serviceId);  
          
        // Ensure there are enough instances to satisfy our hedging requirements  
        Assert.state(serviceInstanceList.size() >= this.attempts, "there must be at least " +  
                this.attempts + " instances of the service " + serviceId + "!");  
          
        // Initialize counter and map to store requests to different instances  
        int counter = 0;  
        Map<String, Mono<ClientResponse>> ships = new HashMap<>();  
          
        // Create requests to different service instances  
        while (ships.size() < this.attempts && (counter++ < this.maxAttempts)) {  
            ServiceInstance lb = this.loadBalancerClient.choose(serviceId);  
            String asciiString = lb.getUri().toASCIIString();  
            ships.computeIfAbsent(asciiString, str -> this.invoke(lb, originalURI, request, next));  
        }  
          
        // Return the first response that arrives  
        return Flux  
                .first(ships.values())  
                .singleOrEmpty();  
    }  
  
    private Mono<ClientResponse> invoke(ServiceInstance serviceInstance,  
                                        URI originalURI,  
                                        ClientRequest request,  
                                        ExchangeFunction next) {  
        // Reconstruct the URI for the specific service instance  
        URI uri = this.loadBalancerClient.reconstructURI(serviceInstance, originalURI);  
          
        // Create a new request with the reconstructed URI  
        ClientRequest newRequest = ClientRequest  
                .create(request.method(), uri)  
                .headers(h -> h.addAll(request.headers()))  
                .cookies(c -> c.addAll(request.cookies()))  
                .attributes(a -> a.putAll(request.attributes()))  
                .body(request.body())  
                .build();  
  
        // Execute the request and log when it's launched  
        return next  
                .exchange(newRequest)  
                .doOnNext(cr -> log.info("launching " + newRequest.url()));  
    }  
}

@RestController  
class HedgingRestController {  
  
    private final WebClient client;  
    @Autowired  
    private ServiceClient serviceClient;  
  
    HedgingRestController(WebClient client) {  
        this.client = client;  
    }  
  
    @GetMapping("/hedge")  
    Flux<String> greet() {  
        return this.client  
                .get()  
                .uri("http://service/hi")  
                .retrieve()  
                .bodyToFlux(String.class);  
    }  
	// compare endpoint
    @GetMapping("/hi")  
    public ResponseEntity<String> greetingWithoutHedge() {  
  
        // Make HTTP call to external service  
        String response = serviceClient.hi();  
  
        // Return the response from external service to client  
        return ResponseEntity.ok(response);  
    }  
}

@org.springframework.stereotype.Service  
public class Service {  
    @Autowired  
    private ServiceClient serviceClient;  
  
    public String hi() {  
        return serviceClient.hi();  
    }  
}

@FeignClient(name = "service")  
public interface ServiceClient {  
  
    @RequestMapping(value = "/hi", method = RequestMethod.GET)  
    String hi();  
}
```

Ngoài ra thì còn có 1 eureka server để làm service discovery
## Test

Mình sẽ dùng artilery để chạy thử để đối chiếu kết quả

```yaml
config:  
  target: "http://localhost:8081" # Change to your application base URL  
  phases:  
    - duration: 30  
      arrivalRate: 5  
      rampTo: 50  
      name: "Warm up phase"  
    - duration: 30  
      arrivalRate: 50  
      name: "Sustained load phase"  
  plugins:  
    metrics-by-endpoint:  
      - counters: true  
  processor: "./custom-functions.js"  
  defaults:  
    headers:  
      Accept: "application/json"  
      Content-Type: "application/json"  
  
scenarios:  
  - name: "Compare hedged vs non-hedged requests"  
    flow:  
      - loop:  
          - get:  
              url: "/hedge"  
              name: "Hedged endpoint"  
              expect:  
                - statusCode: 200  
          - think: 1  
          - get:  
              url: "/hi"  
              name: "Non-hedged endpoint"  
              expect:  
                - statusCode: 200  
          - think: 1  
        count: 10  
  
  - name: "Hedged only"  
    weight: 2  
    flow:  
      - get:  
          url: "/hedge"  
          name: "Hedged endpoint only"  
          expect:  
            - statusCode: 200  
  
  - name: "Non-hedged only"  
    weight: 2  
    flow:  
      - get:  
          url: "/hi"  
          name: "Non-hedged endpoint only"  
          expect:  
            - statusCode: 200
```

Kết quả đây https://app.artillery.io/share/sh_d1937b16db8fe61e6d8bbce7f1657c27b201adf6bf8ba126fb105d5ada78e0ed

![[Screenshot 2025-03-21 at 13.50.01.png]]

Nhìn nhanh thì có thể thấy với `/hi` thì kết quả đâu đó sẽ gần giống với distribution của hàm random, với p95 ở cỡ 4.5s và mean ở khoảng 2.5-3s, nhưng với `/hedge` thì thấp hơn kha khá với chỉ ~2-2.5 s, tức là cải thiện được khoảng 20% một con số khá đáng để đánh đổi (tất nhiên mình sẽ check kỹ hơn về performance để xem load của hệ thống tăng bao nhiêu)
## Kết luận

Mô hình Hedged Requests là công cụ hiệu quả để cải thiện độ tin cậy và hiệu suất của hệ thống phân tán, đặc biệt khi xử lý các vấn đề tail latency.
Tất nhiên nó không giải quyết được gốc rễ của vấn đề high latency, việc này vẫn đòi hỏi tính cần thiết của việc optimize từng chút một.
Tuy nhiên, cần cân nhắc kỹ về tải hệ thống và tài nguyên trước khi áp dụng mô hình này.
## Đọc thêm

- [Spring Tips: Hedging Client Requests](https://spring.io/blog/2019/01/23/spring-tips-hedging-client-requests-with-the-reactive-webclient-and-a-service-registry)
- [gRPC Request Hedging](https://grpc.io/docs/guides/request-hedging/)
- [Tail at Scale của Google](https://research.google/pubs/pub40801/)

