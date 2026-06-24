---
title: "Sequential Execution"
aliases: []
tags: [agentic-architecture, planning]
created: 2026-06-24
updated: 2026-06-24
---

Sequential Execution is a workflow where tasks are performed one after another in a fixed, deterministic order.

**Why it matters**: It is easy to debug and audit, but it can be slower than parallel execution if tasks are independent. It is the correct choice when there are true data dependencies between steps.

**Connections**:
- [[permanent/sequential-pipeline]]
- [[permanent/parallel-execution]]

**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-2-agentic-architecture-task-decomposition-planning]]
