---
title: "SAFe Enablers"
aliases: ["SAFe Enabler", "Enabler story", "Architectural Enabler", "Infrastructure Enabler"]
tags: [safe, agile, backlog, architecture, technical-debt]
created: 2026-04-10
updated: 2026-04-10
---

Enabler trong SAFe là backlog item không deliver business feature trực tiếp cho người dùng, nhưng mở rộng Architectural Runway hoặc cải thiện Development Value Stream. Chúng là công cụ để SAFe balance giữa feature delivery và technical health của hệ thống.

## 4 loại Enabler

| Loại               | Mục đích                                                                     |
| ------------------ | ---------------------------------------------------------------------------- |
| **Exploration**    | Research thêm, xác định approach tốt nhất, thử công nghệ mới, làm rõ yêu cầu |
| **Architectural**  | Nâng cao kiến trúc mà các feature/story phụ thuộc vào                        |
| **Infrastructure** | Cải thiện pipeline build, test, deploy — cách product được phát triển        |
| **Compliance**     | Đảm bảo tuân thủ quy định, tiêu chuẩn (ISO, GDPR, FDA...)                    |

## Mối quan hệ với Flow Framework

Enablers trong SAFe map sang **Debt** (Architectural + Infrastructure) và **Risk** (Compliance) trong Flow Framework của Kersten. Flow Distribution theo dõi tỉ lệ Enabler so với Feature — một ART sức khỏe tốt không nên có quá nhiều Enabler so với Feature.

## Architectural Runway

Enablers, đặc biệt là Architectural Enablers, xây dựng **Architectural Runway** — nền tảng kỹ thuật cho phép các feature tương lai được phát triển nhanh mà không cần rework lớn. Runway cần được duy trì chủ động, không phải chờ đến khi feature bị block.

## Connections

- [[permanent/safe]] — Enablers là cơ chế SAFe dùng để manage technical health
- [[permanent/flow-framework]] — Enablers map sang Debt và Risk flow items
- [[permanent/safe-agile-release-train]] — ART prioritize Enablers cùng với Features trong PI Planning

## Sources

- [[refs/safe_enabler]]
