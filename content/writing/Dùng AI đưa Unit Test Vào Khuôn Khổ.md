---
title:  "Dùng AI đưa Unit Test Vào Khuôn Khổ"
date:   2025-08-13 12:00:00
tags: [AI, Github, Github Copilot, LLM, type/write]
---

# Hành Trình Đưa Unit Test Vào Khuôn Khổ: Từ Hỗn Loạn Đến Rõ Ràng

*Lưu ý: Bài viết này được viết với sự hỗ trợ của AI (GitHub Copilot).*

## Giới thiệu

Khi các dịch vụ backend của chúng ta ngày càng phức tạp, việc viết unit test đáng tin cậy, dễ bảo trì cũng trở nên khó khăn hơn. Chúng tôi từng đối mặt với dependency rối rắm, nhánh ẩn, mock dễ vỡ. Để giải quyết, chúng tôi đã chắt lọc kinh nghiệm thành một bộ hướng dẫn rõ ràng, có thể tái sử dụng—hiện đã có trong file [[assets/code/copilot-unit-test-instructions-core]]. Bài viết này chia sẻ hành trình, bài học và cách bạn có thể áp dụng các pattern này để kiểm thử hiệu quả.

---

## Vì Sao Cần Một Phương Pháp Hệ Thống

Ban đầu, test của chúng tôi rất thiếu nhất quán. Có test chỉ cover happy path, có test bỏ sót edge case hoặc không verify đúng các tương tác quan trọng. Chúng tôi thường gặp khó khăn với:
- Test logic private sâu mà không lộ nội bộ
- Mock đủ mọi dependency, nhất là các nhánh ít dùng
- Đảm bảo các luồng lỗi, exception thực sự được cover

---

## Bước Ngoặt: Lossless Semantic Trees

Chúng tôi nhận ra muốn test đầy đủ phải hình dung được toàn bộ business logic và dependency. Đó là lý do bộ hướng dẫn bắt đầu bằng:

```
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
```

Nhờ vẽ các tree này, chúng tôi có thể xác định hệ thống mọi scenario, dependency và edge case cần test. Đây là kết quả khi chúng tôi tạo LST [[assets/code/PaymentServiceImpl_Semantic_Trees_masked]]
```

```

---

## Các Thực Hành Chủ Chốt Đã Áp Dụng

### 1. Mock Verification Pattern

```
- **Every stubbed method must have a corresponding verify()**
- **Zero-Call Verification:** Use `verify(..., times(0))` to ensure critical methods are NOT called in failure scenarios
```
Chúng tôi luôn verify cả cái nên gọi và KHÔNG nên gọi, đặc biệt sau validation hoặc xử lý lỗi.

### 2. ArgumentCaptor Best Practices

```
- Always use ArgumentCaptor to capture and assert actual values
- Never use any() in verify()
- Use times(n) to verify exact call counts
- For zero-call, use times(0) without captors
```
Đảm bảo kiểm tra giá trị thực tế, không chỉ là có gọi hàm hay không.

### 3. Exception and Edge Case Testing

```
- Test all exception paths and error handling
- Verify that no downstream operations are called after early failures
```
Chúng tôi luôn test các luồng lỗi, edge case và xác nhận không có side effect sau khi lỗi xảy ra.

### 4. Branch Coverage Analysis

```
- Use semantic trees to identify all conditional branches
- Test every branch, even those unreachable in normal flows
```
Không bỏ sót nhánh nào, kể cả nhánh tưởng như không thể xảy ra.

### 5. Usage Workflow

```
1. Create semantic trees for the service
2. Identify all test scenarios and branches
3. Setup mocks for all dependencies
4. Write tests for each scenario, using ArgumentCaptor and zero-call verification
5. Ensure all branches and exception paths are covered
6. Maintain clear naming and documentation
```
Quy trình này giúp test luôn hệ thống, dễ bảo trì.

---

## Ví Dụ Thực Tế: Cái Bẫy Private Method Sâu

Giả sử có một private method xử lý lỗi hiếm gặp, chỉ trigger khi token ID null. Framework thường chặn trường hợp này, nhưng để đủ coverage, ta vẫn phải test. Dùng semantic tree, chúng tôi:
- Tìm public method gọi đến logic private đó
- Tạo input để reach nhánh cần test
- Mock mọi dependency, kể cả chỉ dùng trong private method
- Dùng `verify(..., times(0))` để đảm bảo downstream không bị gọi sau khi fail sớm

Ví dụ test thực tế (đã masking data):

```java
@Test
void testProcessPayment_TokenIdNull_ThrowsServiceException() {
    // Arrange
    PayRequestBody requestWithNullToken = PayRequestBody.builder()
        .tokenId(null)
        .planId(1L)
        .build();
    when(tokenRepository.findByProductOwnId(anyLong())).thenReturn(tokens);

    // Act & Assert
    assertThrows(ServiceException.class, () -> paymentService.processPayment(requestWithNullToken));

    // Verify chỉ các bước validation được gọi
    verify(tokenRepository, times(1)).findByProductOwnId(anyLong());
    // CRITICAL: Đảm bảo các dependency phía sau KHÔNG bị gọi
    verify(carRepository, times(0)).getPayCarInfo(any(), any());
    verify(planRepository, times(0)).getPayPlanInfo(any(), any());
}
```

---

## Kết Quả

Nhờ cách làm này:
- Bắt được bug ở nhánh “không thể đến”
- Test dễ bảo trì, ít vỡ hơn
- Thành viên mới dễ tiếp cận nhờ playbook
- Đạt coverage thực sự, không chỉ là line coverage

---

## Kết Luận

Test backend phức tạp là khó—nhưng nếu có phương pháp hệ thống, mọi thứ đều khả thi. Hãy bắt đầu mọi test suite bằng cách quote và làm theo hướng dẫn trong [[assets/code/copilot-unit-test-instructions-core]]. Dùng semantic tree, verify mọi tương tác, đừng bỏ qua edge case. Đó là cách chúng tôi biến hỗn loạn thành rõ ràng—và bạn cũng có thể làm được.

---

## Vai Trò Của LST Và Sự Kết Hợp Giữa Con Người & AI

Một điểm đặc biệt trong hành trình này là: sau khi có Lossless Semantic Tree (LST), GitHub Copilot có thể viết test một cách thông minh, hệ thống và sát với nghiệp vụ hơn rất nhiều. Bạn có thể thấy, tôi gần như không phải code tay dòng nào—mọi thứ từ dựng test, refactor, thậm chí dựng cả LST đều do Copilot thực hiện. Tuy nhiên, tôi là người định hướng, chỉ dẫn Copilot dựng LST, và luôn nhắc Copilot đối chiếu với LST khi gặp khó khăn hoặc nhánh phức tạp.

Chính sự kết hợp này tạo nên sức mạnh:
- **Tôi**: Có kiến thức hệ thống, hiểu nghiệp vụ, biết cách phân tích và dựng LST chuẩn xác.
- **GitHub Copilot**: Hỗ trợ coding, tự động hóa, hiểu logic và viết test rất tốt nếu được instruct rõ ràng, đặc biệt khi có LST làm nền tảng.

Khi gặp nhánh khó, tôi chỉ cần nhắc Copilot: “Hãy đối chiếu với LST, kiểm tra branch này đã test chưa, mock đã đủ chưa, exception đã cover chưa?”—Copilot sẽ tự động sinh test, bổ sung mock, thậm chí gợi ý các edge case mà con người dễ bỏ sót.

**Bài học lớn:** Nếu bạn có nền tảng phân tích hệ thống tốt, Copilot sẽ trở thành cộng sự AI cực kỳ mạnh mẽ, giúp bạn tăng tốc, giảm lỗi và đạt coverage thực sự.
