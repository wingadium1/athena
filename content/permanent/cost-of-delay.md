---
title: "Cost of Delay"
aliases: ["CoD", "cost of delay", "Don Reinertsen CoD"]
tags: [product, metrics, flow, prioritization, economics]
created: 2026-04-10
updated: 2026-04-10
---

Cost of Delay (CoD) là giá trị kinh tế bị mất đi do trì hoãn việc deliver một feature hoặc sản phẩm. Concept này được Don Reinertsen phổ biến trong _The Principles of Product Development Flow_ như một công cụ để đưa ra quyết định ưu tiên dựa trên kinh tế thay vì cảm tính.

CoD không đo chi phí của việc _làm chậm_ — mà đo **chi phí của thời gian**. Mỗi sprint, mỗi tuần trì hoãn một feature có một con số kinh tế cụ thể: doanh thu bị mất, market share không giành được, penalty hợp đồng, hoặc chi phí cơ hội.

## CD3 — Cost of Delay Divided by Duration

Reinertsen đề xuất dùng **CD3** (CoD / Duration) để so sánh và ưu tiên các item trong backlog:

$$CD3 = \frac{\text{Cost of Delay ($/week)}}{\text{Duration (weeks)}}$$

Item có CD3 cao nhất được ưu tiên trước. Điều này tự nhiên ưu tiên những việc vừa **quan trọng** vừa **nhanh** — tránh rơi vào bẫy chỉ làm việc quan trọng (mà mất nhiều thời gian) hoặc chỉ làm việc nhanh (nhưng không có giá trị).

## Tại sao quan trọng

Trong hầu hết tổ chức, quyết định ưu tiên được đưa ra mà không có con số kinh tế cụ thể. CoD buộc cuộc trò chuyện phải đặt câu hỏi: "Nếu feature này trễ 1 tuần, tổ chức mất bao nhiêu tiền?" Câu trả lời thường làm thay đổi thứ tự ưu tiên đáng kể.

## Connections

- [[permanent/flow-framework]] — Flow Time kéo dài trực tiếp tạo ra Cost of Delay
- [[permanent/little-s-law]] — Little's Law giải thích tại sao WIP cao tăng Flow Time và từ đó tăng CoD

## Sources

- [[refs/don_reinertsen_cost_of_delay]]
