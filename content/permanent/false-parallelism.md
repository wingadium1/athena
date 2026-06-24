---
title: "False Parallelism"
aliases: []
tags: [agentic-architecture, parallel-execution, anti-patterns]
created: 2026-06-24
updated: 2026-06-24
---

False parallelism is an anti-pattern in agentic architecture where tasks that have a hidden dependency on each other are treated as independent and run in parallel.

**Why it matters**: This can lead to data hazards, race conditions, and incorrect results, as the execution order is not guaranteed and one task may proceed with stale or incomplete data from another.

**Connections**:
- [[permanent/parallel-decomposition]]
- [[permanent/true-vs-artificial-dependency]]

**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-2-agentic-architecture-task-decomposition-planning]]
