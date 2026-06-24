---
title: "Sequential Decomposition"
aliases: []
tags: [agentic-architecture, planning]
created: 2026-06-24
updated: 2026-06-24
---

Sequential Decomposition is a task decomposition pattern where tasks are executed in a defined order. The output of each task serves as the input for the next, making it straightforward but potentially inefficient if tasks do not have true data dependencies.

**Why it matters**: This pattern is appropriate whenever genuine data dependencies exist between steps, ensuring a clear and auditable flow of execution.

**Connections**:
- [[permanent/task-decomposition]]
- [[permanent/hierarchical-decomposition]]
- [[permanent/parallel-decomposition]]
- [[permanent/sequential-pipeline]]

**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-2-agentic-architecture-task-decomposition-planning]]
