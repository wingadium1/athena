---
title: "LLM Wiki Pattern"
aliases: ["LLM-maintained wiki", "AI wiki", "compiled knowledge base"]
tags: [PKM, LLM, second-brain]
created: 2026-04-08
updated: 2026-04-08
---

LLM Wiki là một pattern để xây dựng personal knowledge base trong đó **LLM là người viết và duy trì toàn bộ wiki**, còn con người là người cung cấp nguồn và định hướng.

Kiến trúc gồm 3 lớp:

1. **Raw sources** — immutable, LLM chỉ đọc không sửa
2. **Wiki** — tập hợp markdown files do LLM tạo và cập nhật: entity pages, concept pages, summaries, cross-links
3. **Schema** — file instructions (AGENTS.md / CLAUDE.md) dạy LLM conventions: format, workflow, ngôn ngữ

Điểm khác biệt với RAG là knowledge được **compile một lần** và cập nhật incremental, thay vì re-derive mỗi lần query. Khi thêm source mới, LLM không chỉ index nó — mà đọc, extract key info, và integrate vào wiki hiện có: cập nhật entity pages, revise summaries, flag contradictions, strengthen synthesis. Kết quả là một artifact **persistent và compounding**.

## Why it matters

- Humans abandon wikis vì maintenance burden grows faster than value. LLM không chán, không quên update cross-reference, có thể touch 15 files trong một pass.
- Con người làm phần thú vị: curate sources, ask good questions, think about connections. LLM làm phần boring: bookkeeping, cross-referencing, filing.
- Good answers từ query cũng được file lại thành permanent notes — explorations compound vào knowledge base.

## 3 Operations

- **Ingest**: Drop source → LLM tạo literature note + update concept pages (5–15 files/source)
- **Query**: Hỏi → LLM đọc index + relevant pages → synthesize + cite → optionally file answer
- **Lint**: Health check — orphans, contradictions, stale claims, missing cross-refs

## Connections

- [[permanent/rag-vs-compiled-knowledge]] — so sánh với RAG approach
- [[permanent/zettelkasten]] — method ghi chú mà pattern này build on top
- [[permanent/workflow-first-agentic-architecture]] — ingest/query/lint are deterministic workflows, not free-form agent loops

## Sources

- [[literature/karpathy-llm-wiki]]
