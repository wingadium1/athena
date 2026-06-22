---
title: "Software Architect Role"
aliases: ["software architect", "architect vs developer", "architecture decision making"]
tags: [software-architecture, career, decision-making]
created: 2026-04-10
updated: 2026-04-10
---

Software Architect không phải là Developer cấp cao hơn — đó là một vai trò khác biệt về _loại quyết định_ mà người đó chịu trách nhiệm. Developer tối ưu trong phạm vi đã định; Architect xác định phạm vi đó và cân bằng trade-off giữa technical constraints và business constraints.

## Developer vs Architect: Sự khác biệt cốt lõi

Martin Fowler định nghĩa architecture là "những thứ quan trọng — bất kể đó là gì". Đây không phải định nghĩa mơ hồ mà là thực tế: architecture là tập hợp những quyết định mà **toàn bộ hệ thống phụ thuộc vào**, không thể dễ dàng thay đổi sau này.

Ví dụ minh hoạ:

- **Developer**: xây dựng API cho mobile app, tối ưu performance với memcache. Hệ thống hoạt động tốt.
- **Architect nhìn vào bài toán**: thay vì gọi API mỗi request, serve static JSON từ S3+CDN, update 30 phút/lần. Kết quả: chi phí AWS từ $10,000/tháng → $500/tháng.

Developer có kỹ năng implement; Architect có kỹ năng nhìn ra trade-off và cost implication.

## Skills của Software Architect

### Hard Skills

- **Application architecture patterns** — biết khi nào dùng microservices vs monolith, event-driven vs request-response, API Gateway vs Backend-for-Frontend
- **Integration architecture** — không phải lúc nào cũng build from scratch; integration với existing systems thường là lựa chọn đúng hơn
- **Cost-awareness** — mọi technical decision đều có chi phí. Architect phải ước lượng được: networking cost, compute cost, storage cost, operational cost

### Soft Skills (thường quan trọng hơn hard skills)

- **Architectural Decision Making** — khả năng evaluate trade-offs, justify decisions với team, document reasoning
- **Communication** — translate giữa technical world và business world
- **Continuous Delivery mindset** — xem là soft skill vì: architecture chỉ được prove đúng hay sai khi nó chạy trên production. Deploy sớm và liên tục là cách duy nhất để validate architecture nhanh.

## Architecture Decision Making Framework

Một quyết định là architectural nếu thoả mãn:

1. **Ảnh hưởng system-wide** — không phải chỉ một component
2. **Khó thay đổi** sau khi implement
3. **Ảnh hưởng đến technology direction** của team

Ví dụ quyết định: API Gateway vs Backend-for-Frontend. Quá trình evaluate:

- Liệt kê constraints thực tế (client types, bandwidth, service granularity)
- Identify trade-offs của từng lựa chọn
- Chọn dựa trên constraints, không phải dựa trên preference

## Documentation và Knowledge Management

Architect là bottleneck tự nhiên — họ có mental model đầy đủ nhất về hệ thống. Nếu không có knowledge management:

- Quyết định lý do bị mất → decision reversed vô lý sau này
- Onboarding chậm
- Bus factor cao

**Giải pháp**: document mọi architectural decision (ADR — Architecture Decision Records) và lưu trong wiki/shared space mà team có thể tiếp cận. Architectural Knowledge Management là một discipline riêng.

## Connections

- [[permanent/continuous-delivery]] — CD là công cụ validate architecture; deploy liên tục = học nhanh hơn về system behavior
- [[permanent/resilience-vs-robustness]] — architect phải quyết định: optimize MTBF hay MTTR? Đây là architectural trade-off
- [[permanent/dora-metrics]] — DORA metrics đo hiệu quả của architectural decisions ở tầng tổ chức
- [[permanent/devops-topology]] — structure của team phản ánh và bị phản ánh bởi architecture (Conway's Law)
- [[permanent/workflow-first-agentic-architecture]] — concrete AI architecture decision: workflow-first, agent only when needed

## Sources

- `writing/Software Architecture Origin/software-arch-bat-dau-tu-dau-1.md`
- `writing/Software Architecture Origin/software-arch-bat-dau-tu-dau-2.md`
