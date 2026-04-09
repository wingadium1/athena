# literature/ — One Note Per Source

Created during INGEST only. One file per distinct source (article, book, research session).

## Filename

`slug-of-source-title.md` — based on the source title, lowercase, hyphens.

## Required Frontmatter

```yaml
---
title: "Source Title"
type: literature
source: "https://url-or-filename"
author: "..." # see author rules below
date-read: YYYY-MM-DD
tags: [domain]
---
```

## Author Field Rules

| Situation                                | Format                                                      |
| ---------------------------------------- | ----------------------------------------------------------- |
| Single source, known author              | `author: "Name Surname"`                                    |
| Official docs / organization             | `author: "Organization Name"`                               |
| Research session across multiple sources | `author: "Research session — Source A, Source B, Source C"` |

**Never** fabricate a composite name like `"OpenStack Docs / RFC 7348"`. That format implies a single co-authored source, which is misleading. Use the research session format instead.

## Required Sections

1. **Summary** — 2–4 sentences, your synthesis. NOT copy-paste.
2. **Key Ideas** — bullet list, each idea self-contained
3. **Quotes** — verbatim excerpts worth keeping (attribute with source URL)
4. **My Take** — owner's perspective (Vietnamese if personal, English if technical)
5. **Links** — `[[permanent/...]]` wikilinks to notes this source informs

## INGEST Gate (mandatory before creating this file)

Step 2 of INGEST is non-negotiable:

> **Discuss with owner first.** Ask 1–2 questions about their perspective before writing anything.

If owner hasn't responded to your discussion questions yet — do not create this file.
