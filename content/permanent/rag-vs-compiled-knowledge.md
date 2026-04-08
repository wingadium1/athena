---
title: "RAG vs Compiled Knowledge"
aliases: ["retrieval-augmented generation", "RAG", "compiled knowledge base"]
tags: [PKM, LLM, RAG]
created: 2026-04-08
updated: 2026-04-08
---

Hai paradigm khác nhau để LLM làm việc với tài liệu:

**RAG (Retrieval-Augmented Generation)**: Khi có query, system retrieve các chunks liên quan từ raw docs, feed vào LLM context, LLM generate answer. Knowledge **không được tích lũy** — mỗi lần hỏi là rediscover từ đầu. NotebookLM, ChatGPT file uploads, hầu hết các RAG systems hoạt động theo cách này.

**Compiled Knowledge (LLM Wiki)**: LLM đọc sources và **build một wiki trung gian** — structured, interlinked markdown files. Khi có query, LLM đọc wiki (đã được synthesize) thay vì raw docs. Knowledge **tích lũy** qua thời gian: cross-references đã có sẵn, contradictions đã được flag, synthesis đã phản ánh mọi thứ đã đọc.

## Analogy

RAG ~ interpreter: re-execute mỗi lần.
Compiled knowledge ~ compiler: compile một lần, optimize, execute nhanh.

## Trade-offs

|                    | RAG                                | Compiled Knowledge                     |
| ------------------ | ---------------------------------- | -------------------------------------- |
| Setup              | Đơn giản hơn                       | Cần schema + maintenance workflow      |
| Query quality      | Tốt với câu hỏi đơn giản           | Tốt hơn với câu hỏi cần synthesis      |
| Compounding        | Không                              | Có — mỗi source enriches wiki          |
| Hallucination risk | Thấp hơn (gần raw text)            | Cao hơn (AI có thể embed sai vào wiki) |
| Scale              | Tốt khi docs thay đổi thường xuyên | Tốt khi knowledge base ổn định         |

## Connections

- [[permanent/llm-wiki-pattern]] — implementation của compiled knowledge approach
- [[permanent/zettelkasten]] — method tổ chức notes mà compiled knowledge thường dùng

## Sources

- [[literature/karpathy-llm-wiki]]
