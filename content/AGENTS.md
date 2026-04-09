# content/ — Wiki Layer Rules

This directory is the live wiki. Every file here is owned by the agent but **gated by the owner**.

## Hard Rules (apply to all subdirectories)

1. **Never create or modify files without an explicit owner trigger.** Research output from background agents stays in context only — never write it to disk autonomously.
2. **Never delete files.** Archive with `archived: true` in frontmatter instead.
3. **Always update `index.md` and `log.md`** after any create/modify operation.
4. **`log.md` is append-only.** Valid labels: `ingest`, `query`, `capture`, `lint`, `refactor`, `setup`.

## Directory Map

| Folder        | Purpose                                            | Owner-gated?                |
| ------------- | -------------------------------------------------- | --------------------------- |
| `permanent/`  | Atomic concept notes — one idea per file           | YES                         |
| `literature/` | One note per source, created during INGEST         | YES                         |
| `fleeting/`   | Quick idea captures                                | YES                         |
| `maps/`       | Maps of Content — created when domain has 5+ notes | YES                         |
| `wip/`        | Long-form drafts not ready for permanent           | YES                         |
| `writing/`    | Published/draft articles and blog posts            | READ-ONLY unless owner asks |
| `refs/`       | Legacy — preserve as-is, link gradually            | READ-ONLY unless owner asks |
| `til/`        | Legacy — preserve as-is                            | READ-ONLY unless owner asks |
| `journal/`    | Legacy — preserve as-is                            | READ-ONLY unless owner asks |
| `cooking/`    | Recipes and food notes                             | YES                         |

## Before Writing Any File

Ask yourself:

- Did the owner explicitly trigger an INGEST / CAPTURE / QUERY → file operation?
- For INGEST: have I discussed key takeaways with the owner (step 2)?
- Does a permanent note for this concept already exist? (check `index.md` first)

If any answer is NO — stop and ask.
