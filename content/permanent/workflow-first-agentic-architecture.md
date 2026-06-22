---
title: "Workflow-First Agentic Architecture"
aliases: ["incremental complexity", "workflow over agents"]
tags: [claude, ai, architecture]
created: 2026-06-22
updated: 2026-06-22
---

When a task has a predictable input, a known output shape, and a design-time controllable step sequence, a deterministic workflow should be the default architecture. That keeps the system cheaper, faster, and easier to debug than giving the model autonomous control.

Agentic behavior becomes justified only when the task is genuinely open-ended, the tool sequence cannot be pre-specified, or the system must replan dynamically during execution. In other words: start with the simplest workflow, then add autonomy only to replace a proven failure point.

## Connections

- [[permanent/software-architect-role]] — this is an architecture decision rule; the trade-off is system-wide and hard to reverse.
- [[permanent/llm-wiki-pattern]] — the wiki itself is a workflow-first system: ingest, query, and lint are deterministic operations.

## Sources

- [[literature/certified-claude-architect-masterclass-2026-section-1-agentic-architecture-foundation]]
