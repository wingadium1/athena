# Copilot Core Instructions for Unit Testing in Backend Services (Masked Version)

## Prerequisites: Understanding Complex Business Logic

### Step 0: Create Lossless Semantic Trees
**CRITICAL FIRST STEP**: Before writing tests for complex service classes, create Lossless Semantic Trees to understand the complete business logic, conditional execution paths, and dependencies.

#### Semantic Tree Categories to Document:
- Class architecture
- Functional domain flows
- Method call flows
- Data flows
- Cross-cutting concerns (exception handling, logging, etc.)

#### Using Semantic Trees for Test Design:
- Identify all test scenarios and branches
- Use trees to determine required mocks
- Ensure all exception and integration paths are covered

## Core Testing Architecture

### 1. Test Class Structure
- Use dependency injection and proper mock annotations
- Organize test data and setup in @BeforeEach

```java
@SpringBootTest
class PaymentServiceImplTest {
	@Autowired
	private PaymentService paymentService;

	@MockBean
	private CarRepository carRepository;
	@MockBean
	private PlanRepository planRepository;
	// ...other repositories/services

	@Captor
	ArgumentCaptor<Long> idCaptor;

	private PayRequestBody testRequest;

	@BeforeEach
	void setUp() {
		testRequest = new PayRequestBody();
		// setup default mocks
	}
}
```

### 2. Mock Verification Pattern
- **Every stubbed method must have a corresponding verify()**
- **Zero-Call Verification:** Use `verify(..., times(0))` to ensure critical methods are NOT called in failure scenarios

```java
// Arrange
when(carRepository.findById(anyLong())).thenReturn(Optional.of(car));

// Act
paymentService.processPayment(testRequest);

// Assert
verify(carRepository, times(1)).findById(anyLong());
verify(planRepository, times(0)).save(any()); // Zero-call verification
```

### 3. ArgumentCaptor Best Practices
- Always use ArgumentCaptor to capture and assert actual values
- Never use any() in verify()
- Use times(n) to verify exact call counts
- For zero-call, use times(0) without captors

```java
@Captor
ArgumentCaptor<Long> idCaptor;

verify(carRepository, times(1)).findById(idCaptor.capture());
assertEquals(EXPECTED_ID, idCaptor.getValue());
```

### 4. Branch Coverage Analysis
- Use semantic trees to identify all conditional branches
- Test every branch, even those unreachable in normal flows

```java
// Example: Test unreachable branch
PayRequestBody requestWithNullToken = new PayRequestBody();
requestWithNullToken.setTokenId(null);
when(tokenRepository.findByProductOwnId(anyLong())).thenReturn(tokens);
assertThrows(ServiceException.class, () -> paymentService.processPayment(requestWithNullToken));
```

### 5. Exception and Edge Case Testing
- Test all exception paths and error handling
- Verify that no downstream operations are called after early failures

```java
when(carRepository.findById(anyLong())).thenReturn(Optional.empty());
assertThrows(NotFoundException.class, () -> paymentService.processPayment(testRequest));
verify(planRepository, times(0)).save(any());
```

### 6. Naming and Organization
- Use descriptive, consistent test method names
- Organize tests by scenario: happy path, validation, error, edge case, integration

```java
@Test
void testProcessPayment_ValidRequest_ReturnsSuccess() {}

@Test
void testProcessPayment_InvalidCar_ThrowsNotFoundException() {}

@Test
void testProcessPayment_NullToken_ThrowsServiceException() {}
```

### 7. Data Setup Patterns
- Use builders for complex objects
- Setup default mocks to prevent brittle tests

```java
@BeforeEach
void setUp() {
	testRequest = PayRequestBody.builder()
		.tokenId("token123")
		.planId(1L)
		.build();
	when(carRepository.findById(anyLong())).thenReturn(Optional.of(car));
	// ...other default mocks
}
```

### 8. Usage Workflow
1. Create semantic trees for the service
2. Identify all test scenarios and branches
3. Setup mocks for all dependencies
4. Write tests for each scenario, using ArgumentCaptor and zero-call verification
5. Ensure all branches and exception paths are covered
6. Maintain clear naming and documentation
