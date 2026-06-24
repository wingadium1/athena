---
title: "Instruction Design Principles"
aliases: []
tags: [agentic-architecture, multi-agent-orchestration]
created: 2026-06-24
updated: 2026-06-24
---

Instruction Design Principles for sub-agents state that instructions must have:
1.  **A narrow scope**: The task should be focused and well-defined.
2.  **Precise output formats**: This allows for predictable aggregation of results by the orchestrator.
3.  **Explicit success criteria**: This prevents incomplete processing and ensures the sub-agent knows when its task is truly done.

**Why it matters**: Well-designed instructions are crucial for reliable delegation to sub-agents.

**Connections**:
- [[permanent/subagent-definition]]
- [[permanent/well-defined-subtasks]]

**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-3-agentic-architecture-multi-agent-orchestration]]
