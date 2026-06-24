---
title: "Prompt Chaining"
aliases: []
tags: [agentic-architecture, workflow-patterns]
created: 2026-06-24
updated: 2026-06-24
---

Prompt Chaining is the simplest workflow pattern, where the output of one LLM call is passed directly as input to the next. This creates a linear, testable, and easy-to-debug workflow.

**Why it matters**: It's a foundational pattern for building more complex deterministic workflows and is often the simplest place to start.

**Connections**:
- [[permanent/workflow-patterns]]
- [[permanent/sequential-pipeline]]

**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-1-agentic-architecture-foundation]]
