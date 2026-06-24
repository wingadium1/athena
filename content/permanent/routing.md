---
title: "Routing (Workflow Pattern)"
aliases: []
tags: [agentic-architecture, workflow-patterns]
created: 2026-06-24
updated: 2026-06-24
---

Routing is a workflow pattern that uses a classifier to sort inputs before processing. While the router may call an LLM to make the classification decision, the overall topology of the workflow remains fixed.

**Why it matters**: Routing is an effective method to handle complexity in a deterministic workflow while preserving control and auditability.

**Connections**:
- [[permanent/workflow-patterns]]
- [[permanent/prompt-chaining]]
- [[permanent/parallelization]]

**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-1-agentic-architecture-foundation]]
