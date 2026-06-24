---
title: "Independent Testability"
aliases: []
tags: [agentic-architecture, multi-agent-orchestration]
created: 2026-06-24
updated: 2026-06-24
---

Independent testability is a characteristic of a well-designed sub-agent. It means the sub-agent can be tested in isolation, relying only on its explicit inputs, without any hidden dependencies on the orchestrator's state.

**Why it matters**: This makes the system more modular, easier to debug, and more reliable, as individual components can be swapped and verified independently.

**Connections**:
- [[permanent/subagent-definition]]
- [[permanent/context-isolation]]

**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-3-agentic-architecture-multi-agent-orchestration]]
