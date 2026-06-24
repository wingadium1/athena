---
title: "Authority Boundaries"
aliases: []
tags: [agentic-architecture, multi-agent-orchestration]
created: 2026-06-24
updated: 2026-06-24
---

Authority Boundaries define the scope of a sub-agent's permissions and access. The Principle of Least Privilege should be applied, meaning sub-agents should have no access to shared state or the ability to cause side effects outside their explicit task scope without the orchestrator's approval.

**Why it matters**: Clear authority boundaries prevent sub-agents from interfering with each other or the broader system, enhancing security and reliability.

**Connections**:
- [[permanent/subagent-definition]]
- [[permanent/context-isolation]]
- [[permanent/orchestrator-agentic]]

**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-3-agentic-architecture-multi-agent-orchestration]]
