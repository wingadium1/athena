---
title: "AI-Assisted Testing"
aliases: ["AI unit testing", "Copilot testing", "LLM test generation"]
tags: [testing, ai, software-engineering, github-copilot]
created: 2026-04-10
updated: 2026-04-10
---

AI-assisted testing là cách tiếp cận kết hợp AI coding assistants (GitHub Copilot, Cursor, v.v.) với phương pháp phân tích có hệ thống để tạo unit test chất lượng cao — nhanh hơn và ít miss edge case hơn so với viết tay thuần túy.

## Điều kiện tiên quyết: AI không tự nhiên giỏi test

AI coding assistants giỏi ở pattern matching và boilerplate generation, nhưng gặp khó khăn với:

- Hiểu **full dependency graph** của một service class phức tạp
- Xác định **"impossible" branches** cần test
- Nhớ verify **negative cases** (những gì không nên bị gọi)

Kết quả: nếu chỉ prompt "write tests for this class", AI sẽ generate test coverage thấp, bỏ sót edge cases, và mock thiếu.

## Cách làm cho AI-assisted testing hoạt động

### Bước 1: Build Lossless Semantic Tree trước

Trước khi yêu cầu AI viết test, xây dựng [[permanent/lossless-semantic-tree]] — bản đồ đầy đủ của service. Đây là structured context mà AI cần nhưng không tự tạo ra được.

### Bước 2: Cung cấp LST cho AI

Đưa LST vào context (paste vào chat, hoặc reference file). Khi AI có LST:

- Nó có thể map từng test case sang đúng branch trong LST
- Coverage trở nên systematic thay vì opportunistic
- AI có thể phát hiện gaps: "branch này trong LST chưa có test"

### Bước 3: Iterate với structured prompts

Khi test generation gặp branch phức tạp:

> "Đối chiếu với LST, kiểm tra branch [X] đã test chưa, mock đã đủ chưa, exception đã cover chưa?"

AI sẽ tự động sinh test, bổ sung mock, và gợi ý edge cases mà human dễ bỏ sót.

## Mô hình Human + AI

| Vai trò     | Human                                              | AI                                                            |
| ----------- | -------------------------------------------------- | ------------------------------------------------------------- |
| Trách nhiệm | Phân tích LST, hiểu business logic, review kết quả | Implementation speed, boilerplate, catch patterns             |
| Thế mạnh    | System thinking, domain knowledge, judgment        | Exhaustive enumeration, code generation                       |
| Hạn chế     | Mệt mỏi, miss edge cases khi code phức tạp         | Không hiểu ẩn ý business, pattern-match sai khi thiếu context |

Sự kết hợp loại bỏ điểm yếu của nhau: human cung cấp structured context, AI thực thi nhanh và đầy đủ.

## Kết quả thực tế

Với LST + GitHub Copilot:

- Bắt được bug ở "impossible branches"
- Test dễ maintain hơn (logic rõ ràng từ đầu)
- Onboarding dễ hơn — LST là documentation sống của service
- Đạt **real coverage** (branch coverage) thay vì chỉ line coverage

## Connections

- [[permanent/lossless-semantic-tree]] — LST là nền tảng methodology; AI-assisted testing là cách ứng dụng LST với AI tooling
- [[permanent/continuous-integration]] — test quality từ AI-assisted testing cải thiện CI reliability
- [[permanent/continuous-delivery]] — high-quality test suite là điều kiện để CD hoạt động an toàn

## Sources

- `writing/Dùng AI đưa Unit Test Vào Khuôn Khổ.md`
