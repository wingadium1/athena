---
title: "LLM Wiki — Karpathy"
type: literature
source: "https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f"
author: "Andrej Karpathy"
date-read: 2026-04-08
tags: [PKM, LLM, Zettelkasten, second-brain]
---

## Summary

Karpathy đề xuất một pattern để xây dựng personal knowledge base bằng LLM: thay vì RAG (retrieval mỗi lần hỏi), LLM **xây và duy trì một wiki liên tục** — mỗi source được ingest sẽ cập nhật các concept pages, cross-references, và synthesis. Knowledge không bị rediscover từ đầu mỗi lần query mà được _compile_ và _compound_ theo thời gian. Con người làm sourcing và thinking; LLM làm toàn bộ maintenance.

## Key Ideas

- **RAG vs LLM Wiki**: RAG re-derives knowledge mỗi lần query. LLM Wiki compile knowledge một lần, cập nhật liên tục — giống compiler vs interpreter cho knowledge.
- **3 lớp kiến trúc**: Raw sources (immutable) → Wiki (LLM-owned markdown) → Schema (AGENTS.md/CLAUDE.md dạy LLM conventions).
- **3 operations**: Ingest (drop source → LLM cập nhật 10–15 pages), Query (hỏi → LLM đọc wiki + synthesize), Lint (health check: orphans, contradictions, stale claims).
- **Persistent, compounding artifact**: Cross-references đã được duy trì, contradictions đã được flagged, synthesis đã phản ánh mọi thứ bạn đọc. Không mất vào chat history.
- **Tooling**: Obsidian (viewer) + LLM agent (programmer) + wiki (codebase). Web Clipper để clip articles, Marp để generate slides, Dataview cho queries.
- **Scalability**: Index.md đủ dùng đến ~hundreds of pages. Lớn hơn thì dùng `qmd` (hybrid BM25/vector search) hoặc build custom search script.
- **The Memex analogy**: Vannevar Bush (1945) — private curated knowledge với associative trails. LLM giải quyết phần Bush không giải được: ai làm maintenance.

## Quotes

> "The wiki is a persistent, compounding artifact. The cross-references are already there. The contradictions have already been flagged. The synthesis already reflects everything you've read."

> "The human's job is to curate sources, direct the analysis, ask good questions, and think about what it all means. The LLM's job is everything else."

> "Humans abandon wikis because the maintenance burden grows faster than the value. LLMs don't get bored, don't forget to update a cross-reference, and can touch 15 files in one pass."

## My Take

Thấy compelling và đúng hướng. Friction của việc ghi chú thủ công luôn là rào cản — idea hay đọc xong rồi để đó, không bao giờ file. LLM làm phần bookkeeping đó giúp mình tập trung vào phần thú vị hơn: đọc, suy nghĩ, kết nối ideas. Điểm quan trọng nhất là _compounding_: mỗi source không chỉ tồn tại độc lập mà enriches các khái niệm đã có.

## Links

- [[permanent/llm-wiki-pattern]]
- [[permanent/rag-vs-compiled-knowledge]]
- [[permanent/zettelkasten]]
