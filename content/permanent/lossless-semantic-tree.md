---
title: "Lossless Semantic Tree"
aliases: ["LST", "semantic tree testing", "lossless semantic tree"]
tags: [testing, software-engineering, ai-assisted, methodology]
created: 2026-04-10
updated: 2026-04-10
---

Lossless Semantic Tree (LST) là một phương pháp phân tích code trước khi viết test, nhằm xây dựng **bản đồ đầy đủ** của toàn bộ business logic, execution paths, và dependencies trong một service class. "Lossless" vì không được bỏ sót bất kỳ nhánh hay dependency nào — kể cả các nhánh tưởng như không thể reach được.

## Vấn đề LST giải quyết

Unit test backend phức tạp thường dẫn đến:

- Test chỉ cover happy path, bỏ sót edge cases
- Mock thiếu vì không biết tất cả dependencies trong private methods sâu
- Không verify các _negative case_ — những thứ **không nên** bị gọi sau khi validation fail

LST buộc ta phải hiểu toàn bộ code trước khi viết test, thay vì khám phá từng case trong quá trình viết.

## Cấu trúc LST

Một LST cho service class cần document:

1. **Class architecture** — dependencies được inject, lifecycle, state
2. **Functional domain flows** — các business operation theo user journey
3. **Method call flows** — call graph từ public methods xuống private methods
4. **Data flows** — dữ liệu đi qua hệ thống như thế nào, transform ở đâu
5. **Cross-cutting concerns** — exception handling patterns, logging, transaction boundaries

## Workflow

```
1. Build LST cho service
2. Từ LST: identify tất cả test scenarios và branches (bao gồm cả "impossible" branches)
3. Setup mocks cho tất cả dependencies
4. Viết test cho mỗi scenario
5. Verify cả positive (cái nên gọi) và negative (cái không nên gọi)
6. Đảm bảo tất cả branches và exception paths có coverage
```

## Các pattern quan trọng đi kèm LST

### Mock Verification Pattern

- Mỗi stubbed method phải có `verify()` tương ứng
- **Zero-call verification**: dùng `verify(..., times(0))` để đảm bảo downstream methods _không bị gọi_ sau khi validation fail sớm

```java
// Validation fail → các downstream không được gọi
verify(tokenRepository, times(1)).findByProductOwnId(anyLong());
verify(carRepository, times(0)).getPayCarInfo(any(), any());   // MUST NOT be called
verify(planRepository, times(0)).getPayPlanInfo(any(), any()); // MUST NOT be called
```

### ArgumentCaptor

- Luôn dùng ArgumentCaptor để capture và assert _actual values_ truyền vào mock
- Không dùng `any()` trong `verify()` — test sẽ không fail kể cả khi wrong arguments

### Exception & Edge Case Coverage

- Test tất cả exception paths
- Verify không có side effect xảy ra sau khi exception được thrown

## LST + AI = Multiplier

Khi có LST đầy đủ, AI coding assistants (GitHub Copilot, v.v.) hoạt động hiệu quả hơn đáng kể:

- LST cung cấp **structured context** mà AI thiếu khi chỉ nhìn source code
- AI có thể generate test systematically dựa trên LST thay vì guess
- Khi gặp nhánh khó, prompt "kiểm tra branch này đã cover chưa trong LST" cho kết quả chuẩn xác hơn nhiều

Sức mạnh thực sự đến từ **human (phân tích LST) + AI (implementation speed)** — con người hiểu system và business logic, AI thực thi nhanh và không bỏ sót.

## Connections

- [[permanent/ai-assisted-testing]] — LST là nền tảng methodology để AI-assisted testing có chất lượng
- [[permanent/continuous-integration]] — test quality là gating factor của CI pipeline; LST cải thiện CI reliability
- [[permanent/resilience-vs-robustness]] — test coverage thực sự (không chỉ line coverage) là một thành phần của Resilience engineering

## Sources

- `writing/Dùng AI đưa Unit Test Vào Khuôn Khổ.md`
