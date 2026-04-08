# Wiki Log

Append-only record of operations. Format: `## [YYYY-MM-DD] operation | description`

---

## [2026-04-08] capture | Kinh nghiệm coldbrew tại nhà

Ghi lại journey: Hario Filter-in Bottle (vỡ) → Soriso 600ml (vấn đề filter nhỏ) → workaround túi lọc trà + fine robusta 1:10.
Pages touched: coldbrew-tai-nha.md (mới), pourover-setup-ca-nhan.md (cập nhật link), cafe.md (cập nhật MOC)

---

## [2026-04-08] ingest | Phương pháp 4:6 — Tetsu Kasuya + capture setup cá nhân

Key concepts added: tetsu-kasuya-46-method (literature), pourover-setup-ca-nhan (cooking)
Pages touched: 1 literature note, 1 cooking note (mới), cafe.md (cập nhật thành MOC), index.md

---

## [2026-04-08] ingest | LLM Wiki — Andrej Karpathy (gist)

Key concepts added: llm-wiki-pattern, rag-vs-compiled-knowledge, zettelkasten
Pages touched: 1 literature note, 3 permanent notes created, index.md updated

---

## [2026-04-08] setup | Khởi tạo Athena LLM Wiki

Chuyển đổi từ manual wiki sang AI-maintained wiki theo pattern của Karpathy.

**Thay đổi:**

- Tạo cấu trúc Zettelkasten: `content/fleeting/`, `content/literature/`, `content/permanent/`, `content/maps/`
- Tạo `raw/` directory cho sources (articles, books, assets)
- Viết `AGENTS.md` — schema + instructions cho AI agent
- Legacy folders (`refs/`, `til/`, `journal/`) được giữ nguyên, link dần vào permanent notes
- Tạo `index.md` catalog với danh sách legacy notes

**Notes hiện có:** ~100 files trong legacy folders
**Permanent notes:** 0 (chưa ingest source nào)
