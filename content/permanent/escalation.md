---
title: "Escalation (Agentic)"
aliases: []
tags: [agentic-architecture, loop-control]
created: 2026-06-24
updated: 2026-06-24
---

Escalation is a loop control mechanism where an agent passes a problem it cannot solve to a human operator or a fallback system.

**Why it matters**: Escalation is a critical safety valve that prevents an agent from getting stuck or failing silently. The agent's design must clearly define when to escalate, treating it as a planned safety feature, not a failure.

## Connections
- [[permanent/termination-conditions]]
- [[permanent/error-threshold]]
- [[permanent/silent-failure]]

## Sources
- [[literature/certified-claude-architect-masterclass-2026-section-1-agentic-architecture-foundation]]
