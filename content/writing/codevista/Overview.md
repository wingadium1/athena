
# Sample CodeVista Use Cases & Prompts

Let me differentiate CodeVista from Zed. While Zed is primarily a code editor with collaborative features, CodeVista is an AI-powered development assistant that integrates with your existing IDE. Let's explore specific use cases and prompts that highlight CodeVista's unique capabilities.

## 1. Code Architecture & Design

### Design Pattern Implementation
```text
Prompts:
- "Suggest design pattern for @selected_code considering SOLID principles"
- "Refactor @class:UserService to implement Repository pattern"
- "Convert @selected_code to use Observer pattern for event handling"
```

#### Sample promt design pattern
* **Source context**: https://github.com/wingadium1/java-spring-boot-boilerplate/blob/main/src/main/java/com/example/javaspringbootboilerplate/service/HighValueCouponService.java
* **Promt**: 
	* Suggest design pattern for @selected_code considering SOLID principles
	* Explain how SOLID principles apply here.
* **Output**:
	To improve the design of the HighValueCouponBySlotService class while adhering to SOLID principles, you can consider applying several design patterns. Here are some suggestions:
	1. **Single Responsibility Principle (SRP)** Ensure that each class has only one reason to change. The HighValueCouponBySlotService class currently handles multiple responsibilities, such as coupon creation, claiming, and queue processing. You can refactor it into smaller classes, each with a single responsibility.
	2. **Factory Pattern** Use the Factory pattern to create instances of Coupon and ClaimRequest. This encapsulates the creation logic and makes the code more maintainable.
	3. **Strategy Pattern** Use the Strategy pattern to handle different coupon claim strategies. This allows you to easily switch between different claim mechanisms without modifying the core logic.
	4. **Template Method Pattern** Use the Template Method pattern to define the skeleton of the queue processing algorithm. This allows you to define the steps of the algorithm and let subclasses override specific steps.
	
	##### **Refactored Code**
	``` java
	public class CouponFactory {
	    public static Coupon createCoupon(CouponCreationInfo info) {
	        return new Coupon(info.getCode(), info.getNumberOfSlot(), info.getNumberOfSlot());
	    }
	}
	```
	
	```java
	public class ClaimRequestFactory {
	    public static ClaimRequest createClaimRequest(String couponCode, String userId) {
	        return new ClaimRequest(couponCode, userId, UUID.randomUUID().toString(), System.currentTimeMillis());
	    }
	}
	```
	
	```java
	CouponClaimStrategy.javajavapublic interface CouponClaimStrategy {
	    CouponClaimResponse claimCoupon(String userId, String couponCode, String email) throws Exception;
	}
	```
	
	```java
	public class DefaultCouponClaimStrategy implements CouponClaimStrategy {
	    private final RedisTemplate<String, Object> template;
	    private final ObjectMapper objectMapper;
	
	    public DefaultCouponClaimStrategy(RedisTemplate<String, Object> template, ObjectMapper objectMapper) {
	        this.template = template;
	        this.objectMapper = objectMapper;
	    }
	
	    @Override
	    public CouponClaimResponse claimCoupon(String userId, String couponCode, String email) throws Exception {
	        try {
	            ClaimRequest request = ClaimRequestFactory.createClaimRequest(couponCode, userId);
	            String requestJson = objectMapper.writeValueAsString(request);
	
	            String queueKey = COUPON_CLAIM_QUEUE_PREFIX + couponCode;
	            template.opsForList().rightPush(queueKey, requestJson);
	
	            return waitForClaimResult(request.getRequestId());
	
	        } catch (Exception e) {
	            log.error("Error while queueing coupon claim", e);
	            return CouponClaimResponse.failure(ClaimResult.ERROR, "System error occurred");
	        }
	    }
	
	    private CouponClaimResponse waitForClaimResult(String requestId) {
	        String resultKey = COUPON_RESULT_PREFIX + requestId;
	        long startTime = System.currentTimeMillis();
	
	        while (System.currentTimeMillis() - startTime < MAX_WAIT_TIME) {
	            Object result = template.opsForValue().get(resultKey);
	            if (result != null) {
	                try {
	                    template.delete(resultKey);
	                    log.info("result: " + result.toString());
	                    return objectMapper.readValue(result.toString(), CouponClaimResponse.class);
	                } catch (Exception e) {
	                    log.error("Error parsing result", e);
	                    break;
	                }
	            }
	            try {
	                Thread.sleep(POLLING_INTERVAL);
	            } catch (InterruptedException e) {
	                Thread.currentThread().interrupt();
	                break;
	            }
	        }
	
	        return CouponClaimResponse.failure(ClaimResult.ERROR, "Request timeout");
	    }
	}
	```
	
	```java
	@Service
	@Slf4j
	public class HighValueCouponBySlotService {
	    private final RedisTemplate<String, Object> template;
	    private final ObjectMapper objectMapper;
	    private final CouponClaimStrategy claimStrategy;
	
	    @Value("${coupon.queue.processor.enabled:true}")
	    private boolean queueProcessorEnabled;
	
	    @Autowired
	    public HighValueCouponBySlotService(@Qualifier("redisTxTemplate") RedisTemplate<String, Object> template, ObjectMapper objectMapper) {
	        this.template = template;
	        this.objectMapper = objectMapper;
	        this.claimStrategy = new DefaultCouponClaimStrategy(template, objectMapper);
	    }
	
	    @PostConstruct
	    public void startQueueProcessor() {
	        if (queueProcessorEnabled) {
	            log.info("Enable QueueProcessor");
	            new Thread(this::processClaimQueue).start();
	        }
	    }
	
	    public Optional<Coupon> getCoupon(String couponCode) {
	        Object couponData = template.opsForHash().get(RedisInterfaceConst.LIST_PREFIX + COUPON + couponCode, "details");
	
	        if (couponData == null) {
	            return Optional.empty();
	        }
	
	        try {
	            Coupon coupon = objectMapper.convertValue(couponData, Coupon.class);
	            coupon.setNumberOfRemainSlot(countAvailableSlots(coupon));
	            return Optional.of(coupon);
	        } catch (Exception e) {
	            log.error("Error deserializing coupon data", e);
	            return Optional.empty();
	        }
	    }
	
	    public int countAvailableSlots(Coupon coupon) {
	        String couponSlotPrefix = RedisInterfaceConst.SINGLE_PREFIX + COUPON + coupon.getCode() + ":slot";
	        return Math.toIntExact(template.opsForZSet().count(couponSlotPrefix, 0, 0));
	    }
	
	    public Coupon createCoupon(CouponCreationInfo info) {
	        return new SessionCallback<>() {
	            @Override
	            public Coupon execute(RedisOperations operations) throws DataAccessException {
	                operations.multi();
	                Coupon coupon = CouponFactory.createCoupon(info);
	                String couponCodeKey = RedisInterfaceConst.LIST_PREFIX + COUPON + info.getCode();
	                String couponSlotPrefix = RedisInterfaceConst.SINGLE_PREFIX + COUPON + info.getCode() + ":slot";
	                for (int i = 0; i < coupon.getNumberOfSlot(); i++) {
	                    operations.opsForZSet().add(couponSlotPrefix, i, 0);
	                }
	                template.opsForHash().put(couponCodeKey, "details", coupon);
	
	                operations.exec();
	                return coupon;
	            }
	        }.execute(template);
	    }
	
	    public CouponClaimResponse claimCoupon(String userId, String couponCode, String email) throws Exception {
	        return claimStrategy.claimCoupon(userId, couponCode, email);
	    }
	
	    private void processClaimQueue() {
	        while (queueProcessorEnabled) {
	            try {
	                Set<String> queueKeys = template.keys(COUPON_CLAIM_QUEUE_PREFIX + "*");
	                for (String queueKey : queueKeys) {
	                    log.info("processClaimQueue found key: " + queueKey);
	                    processQueue(queueKey);
	                }
	                Thread.sleep(500);
	            } catch (Exception e) {
	                log.error("Error in queue processor", e);
	            }
	        }
	    }
	
	    private void processQueue(String queueKey) {
	        String couponCode = queueKey.substring(COUPON_CLAIM_QUEUE_PREFIX.length());
	        log.info("process coupon code: " + couponCode + " at queueKey" + queueKey);
	        String userSetKey = RedisInterfaceConst.LIST_PREFIX + COUPON + couponCode + ":couponslots:" + "userId";
	
	        while (true) {
	            Object requestJson = template.opsForList().leftPop(queueKey);
	            if (requestJson == null) break;
	
	            try {
	                ClaimRequest request = objectMapper.readValue(requestJson.toString(), ClaimRequest.class);
	                CouponClaimResponse response = processClaimRequest(request, userSetKey);
	
	                String resultKey = COUPON_RESULT_PREFIX + request.getRequestId();
	                template.opsForValue().set(resultKey, objectMapper.writeValueAsString(response), 30, TimeUnit.SECONDS);
	
	            } catch (Exception e) {
	                log.error("Error processing claim request", e);
	            }
	        }
	    }
	
	    private CouponClaimResponse processClaimRequest(ClaimRequest request, String userSetKey) {
	        return template.execute(new SessionCallback<CouponClaimResponse>() {
	            @Override
	            public CouponClaimResponse execute(RedisOperations operations) throws DataAccessException {
	                String userId = request.getUserId();
	                String couponCode = request.getCouponCode();
	
	                String couponSlotPrefix = RedisInterfaceConst.SINGLE_PREFIX + COUPON + couponCode + ":slot";
	
	                operations.watch(Arrays.asList(couponSlotPrefix, userSetKey));
	
	                if (Boolean.TRUE.equals(operations.opsForSet().isMember(userSetKey, userId))) {
	                    operations.unwatch();
	                    return CouponClaimResponse.failure(ClaimResult.ALREADY_CLAIMED, "user already claimed this coupon");
	                }
	
	                log.info("User has not claimed this coupon yet");
	
	                int numberOfRemainSlot = Math.toIntExact(operations.opsForZSet().count(couponSlotPrefix, 0, 0));
	                log.info("Number of remaining slots: " + numberOfRemainSlot);
	                if (numberOfRemainSlot == 0) {
	                    operations.unwatch();
	                    return CouponClaimResponse.failure(ClaimResult.NO_SLOTS);
	                }
	
	                operations.multi();
	
	                Set<String> range = operations.opsForZSet().range(couponSlotPrefix, 0, 0);
	                String firstElement = range.isEmpty() ? null : range.iterator().next();
	
	                operations.opsForZSet().remove(couponSlotPrefix, firstElement);
	                operations.opsForSet().add(userSetKey, userId);
	
	                List<Object> results = operations.exec();
	
	                if (results == null || results.isEmpty()) {
	                    return CouponClaimResponse.failure(ClaimResult.ERROR, "Transaction failed, please try again");
	                }
	
	                int remainingSlots = Math.toIntExact(template.opsForZSet().count(couponSlotPrefix, 0, 0));
	                return CouponClaimResponse.success(couponCode, remainingSlots);
	            }
	        });
	    }
	}
	```
	**Key Improvements**
	1. Single Responsibility Principle (SRP): Split responsibilities into smaller classes such as CouponFactory, ClaimRequestFactory, and DefaultCouponClaimStrategy.
	2. Factory Pattern: Used CouponFactory and ClaimRequestFactory to encapsulate the creation logic.
	3. Strategy Pattern: Introduced CouponClaimStrategy and DefaultCouponClaimStrategy to handle different claim strategies.
	4. Template Method Pattern: The processClaimQueue method can be further refactored to use the Template Method pattern if there are variations in the queue processing logic.
	These changes aim to enhance the maintainability and reliability of the code while adhering to SOLID principles. Adjust the implementation as needed to fit your specific requirements and environment.

### API Design
```text
Prompts:
- "Design RESTful endpoints for @class:OrderService following best practices"
- "Suggest GraphQL schema for @selected_code"
- "Optimize API response structure in @function:getCustomerDetails"
```

#### Sample chat 01

To design a GraphQL schema for the `HighValueCouponBySlotService`, you need to define the types, queries, and mutations that represent the operations and data structures used in the service. Below is a suggested GraphQL schema that captures the core functionalities of the service:
**GraphQL Schema**

```graphql
# Define the Coupon type with its fields
type Coupon {
  code: String!
  numberOfSlot: Int!
  numberOfRemainSlot: Int!
}

# Define the ClaimRequest type with its fields
type ClaimRequest {
  couponCode: String!
  userId: String!
  requestId: String!
  timestamp: String!
}

# Define the CouponClaimResponse type with its fields
type CouponClaimResponse {
  success: Boolean!
  message: String
  remainingSlots: Int
}

# Define the Query type for fetching data
type Query {
  # Get details of a specific coupon by its code
  getCoupon(couponCode: String!): Coupon
}

# Define the Mutation type for modifying data
type Mutation {
  # Create a new coupon with specified details
  createCoupon(code: String!, numberOfSlot: Int!): Coupon

  # Claim a coupon for a user
  claimCoupon(userId: String!, couponCode: String!, email: String!): CouponClaimResponse
}

# Define the schema with query and mutation types
schema {
  query: Query
  mutation: Mutation
}
```

**Explanation**
1. Types:
	* `Coupon`: Represents a coupon with fields for the code, total number of slots, and remaining slots.
	* `ClaimRequest`: Represents a claim request with fields for the coupon code, user ID, request ID, and timestamp.
	* `CouponClaimResponse`: Represents the response of a coupon claim operation, including success status, message, and remaining slots.
2. Queries:
	* `getCoupon`: Fetches the details of a specific coupon by its code. Returns a `Coupon` object.
3. Mutations:
	* `createCoupon`: Creates a new coupon with the specified code and number of slots. Returns the created `Coupon` object.
	* `claimCoupon`: Claims a coupon for a user, identified by user ID and coupon code. Returns a `CouponClaimResponse` object indicating the success of the operation and the number of remaining slots.
	This schema provides a structured way to interact with the HighValueCouponBySlotService using GraphQL, allowing clients to query and mutate coupon data efficiently. Adjust the schema as needed to fit your specific requirements and data structures.

#### Sample chat 02

**Input chat:** Design RESTful endpoints for @class:HighValueCouponBySlotService following best practices
**Output:**

To design RESTful endpoints for the `HighValueCouponBySlotService` following best practices, you need to define clear and consistent URL patterns, HTTP methods, and request/response formats. Below is a suggested design for the RESTful API endpoints:

**RESTful API Endpoints**
1. Get Coupon Details 
	* Endpoint: `/api/coupons/{couponCode}`
	* Method: GET
	* Description: Retrieve details of a specific coupon
	* Response: JSON representation of the Coupon object.
2. Create Coupon
	* Endpoint: /api/coupons
	* Method: POST
	* Description: Create a new coupon with specified details.
	* Request Body:
```json
{
	"code": "COUPON123",
	"numberOfSlot": 100
}
```
	* Response: JSON representation of the created Coupon object.
3. Claim Coupon
	* Endpoint: `/api/coupons/{couponCode}/claim`
	* Method: POST
	* Description: Claim a coupon for a user.
	* Request Body:
```json
{
	"userId": "user123",
	"email": "user@example.com"
}
```
	* Response: JSON representation of the `CouponClaimResponse` object.
**Implementation in Spring Boot**
	To implement these endpoints, you can create a controller class in Spring Boot:
	
```java

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/coupons")
public class HighValueCouponController {

	private final HighValueCouponBySlotService couponService;

	@Autowired
	public HighValueCouponController(HighValueCouponBySlotService couponService) {
		this.couponService = couponService;
	}

	@GetMapping("/{couponCode}")
	public ResponseEntity<Coupon> getCoupon(@PathVariable String couponCode) {
		return couponService.getCoupon(couponCode)
				.map(ResponseEntity::ok)
				.orElse(ResponseEntity.notFound().build());
	}

	@PostMapping
	public ResponseEntity<Coupon> createCoupon(@RequestBody CouponCreationInfo info) {
		Coupon coupon = couponService.createCoupon(info);
		return ResponseEntity.ok(coupon);
	}

	@PostMapping("/{couponCode}/claim")
	public ResponseEntity<CouponClaimResponse> claimCoupon(
			@PathVariable String couponCode,
			@RequestBody ClaimRequest request) {
		try {
			CouponClaimResponse response = couponService.claimCoupon(request.getUserId(), couponCode, request.getEmail());
			return ResponseEntity.ok(response);
		} catch (Exception e) {
			return ResponseEntity.status(500).body(CouponClaimResponse.failure(ClaimResult.ERROR, "System error occurred"));
		}
	}
}
```
**Best Practices Followed**
1. **Consistent Naming**: Use clear and consistent naming conventions for endpoints and HTTP methods.
2. **HTTP Methods**: Use appropriate HTTP methods (GET for retrieval, POST for creation and actions).
3. **Path Variables**: Use path variables for identifying specific resources (e.g., couponCode).
4. **Request and Response Formats**: Use JSON for request and response bodies, which is a common format for RESTful APIs.
5. **Response Status Codes**: Return appropriate HTTP status codes (e.g., 200 for success, 404 for not found, 500 for server errors).
6. **Error Handling**: Handle exceptions and return meaningful error messages in the response.This design provides a structured and consistent way to interact with the `HighValueCouponBySlotService` using RESTful principles. Adjust the implementation as needed to fit your specific requirements and environment.

## 2. Performance Optimization

### Database Query Optimization
```text
Prompts:
- "Analyze and optimize database queries in @selected_code"
- "Suggest indexing strategy for @function:searchProducts"
- "Review and optimize ORM usage in @class:ProductRepository"
```

### Memory Management
```text
Prompts:
- "Identify memory leaks in @selected_code"
- "Optimize memory usage in @function:processLargeDataset"
- "Suggest caching strategy for @class:DataProcessor"
```

## 3. Security Enhancement

### Security Audit
```text
Prompts:
- "Review @selected_code for security vulnerabilities"
- "Implement input validation for @function:userRegistration"
- "Suggest security improvements for @class:AuthenticationService"
```

### Secure Coding Patterns
```text
Prompts:
- "Add CSRF protection to @selected_code"
- "Implement rate limiting in @function:loginAttempt"
- "Review and secure JWT implementation in @class:TokenService"
```

## 4. Testing Strategies

### Unit Test Generation
```text
Prompts:
- "Generate unit tests with mocking for @class:PaymentService"
- "Create test cases covering edge cases for @function:calculateDiscount"
- "Write integration tests for @selected_code using [framework]"
```

### Test Coverage Improvement
```text
Prompts:
- "Identify uncovered code paths in @class:OrderProcessor"
- "Generate test scenarios for error conditions in @selected_code"
- "Add boundary value tests for @function:validateInput"
```

## 5. Code Refactoring

### Clean Code Transformation
```text
Prompts:
- "Refactor @selected_code to improve readability"
- "Apply clean code principles to @class:DataManager"
- "Simplify complex conditions in @function:processRules"
```

### Modern Syntax Updates
```text
Prompts:
- "Convert @selected_code to use modern JavaScript features"
- "Update @class:AsyncHandler to use async/await"
- "Refactor to use latest language features in @function:dataProcessor"
```

## 6. Documentation Enhancement

### API Documentation
```text
Prompts:
- "Generate OpenAPI documentation for @selected_code"
- "Create detailed method documentation for @class:ServiceLayer"
- "Add usage examples to @function:utilityHelper documentation"
```

### Code Documentation
```text
Prompts:
- "Add comprehensive JSDoc to @class:ComplexService"
- "Document error handling scenarios in @selected_code"
- "Generate TypeScript interfaces for @class:DataModel"
```

## 7. Performance Profiling

### Resource Usage Analysis
```text
Prompts:
- "Profile CPU usage in @function:heavyComputation"
- "Analyze memory consumption patterns in @class:DataProcessor"
- "Identify bottlenecks in @selected_code"
```

### Optimization Suggestions
```text
Prompts:
- "Suggest async optimizations for @selected_code"
- "Optimize resource cleanup in @class:ResourceManager"
- "Improve rendering performance in @function:updateUI"
```

## 8. Error Handling

### Robust Error Management
```text
Prompts:
- "Implement comprehensive error handling in @selected_code"
- "Add retry mechanism to @function:apiCall"
- "Design error recovery strategy for @class:TransactionManager"
```

### Logging Enhancement
```text
Prompts:
- "Add structured logging to @class:ServiceLayer"
- "Implement debug logging in @function:complexOperation"
- "Optimize log levels in @selected_code"
```

## 9. Code Migration

### Framework Migration
```text
Prompts:
- "Convert @selected_code from [old_framework] to [new_framework]"
- "Migrate @class:Component to React hooks"
- "Update @function:handler to use latest AWS SDK"
```

### Language Migration
```text
Prompts:
- "Convert @selected_code from JavaScript to TypeScript"
- "Migrate @class:Service from Python 2 to Python 3"
- "Transform @function:processor to use modern PHP syntax"
```

## 10. Performance Testing

### Load Test Generation
```text
Prompts:
- "Generate load test scenarios for @class:APIEndpoint"
- "Create stress test cases for @function:dataProcessor"
- "Design performance benchmarks for @selected_code"
```

### Benchmark Creation
```text
Prompts:
- "Create performance benchmarks for @class:Algorithm"
- "Generate comparison tests for @function:sortingMethod"
- "Design memory usage tests for @selected_code"
```

## Best Practices for Using Prompts

1. **Be Specific**:
   - Include exact requirements
   - Specify frameworks or versions
   - Mention performance constraints

2. **Use Context**:
   - Reference related code using @symbols
   - Provide business logic context
   - Mention existing patterns or conventions

3. **Iterative Improvement**:
   - Start with basic prompts
   - Refine based on responses
   - Build upon previous results

4. **Combine Features**:
   - Use web search for validation
   - Combine with Autofix for implementation
   - Leverage inline chat for clarifications

This expanded set of use cases and prompts showcases CodeVista's capabilities across different aspects of software development, helping developers maximize their productivity and code quality.