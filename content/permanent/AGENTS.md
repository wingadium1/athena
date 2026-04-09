# permanent/ — Atomic Concept Notes

One note = one atomic concept. This is the Zettelkasten backbone.

## Filename

`concept-slug.md` — lowercase, hyphens, no dates, no version numbers.

## Required Frontmatter

```yaml
---
title: "Concept Name"
aliases: ["alt name", "abbreviation"]
tags: [domain, subdomain]
created: YYYY-MM-DD
updated: YYYY-MM-DD
---
```

To archive a duplicate: add `archived: true`. Never delete.

## Required Sections

1. Body — 1–3 paragraphs, own words, explain to a smart colleague
2. `## Connections` — wikilinks to related permanent notes
3. `## Sources` — wikilinks to literature notes that informed this

## Enforcement Checklist (before creating a new note)

- [ ] Check `index.md` — does this concept already exist under a different name?
- [ ] Is this truly ONE atomic concept, or should it be split?
- [ ] Owner explicitly triggered INGEST/QUERY→file for this concept?
- [ ] No copy-paste from source — all paraphrased?

## Common Violations (DO NOT)

- Two overlapping notes for the same concept — merge, archive the duplicate
- `archived: true` note still receiving backlinks from active notes — redirect backlinks first
- Creating this note from a background agent research output without owner confirmation
