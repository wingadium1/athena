---
title: "Context Isolation"
aliases: []
tags: [agentic-architecture, multi-agent-orchestration]
created: 2026-06-24
updated: 2026-06-24
---

Context Isolation is a design principle where sub-agents start with a "blank" context and only see the information that is explicitly handed over to them by the [[orchestrator|orchestrator]].

**Why it matters**: This practice avoids context bloat, prevents information leakage between agents, and eliminates dependency interference. It ensures that sub-agents are modular and independently testable.

**Connections**:
- [[permanent/subagent-definition]]
- [[permanent/orchestrator]]
- [[permanent/authority-boundaries]]
- [[permanent/handoff-messages]]

**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-3-agentic-architecture-multi-agent-orchestration]]
