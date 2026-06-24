---
title: "Infinite Loops"
aliases: []
tags: [agentic-architecture, loop-control]
created: 2026-06-24
updated: 2026-06-24
---

Infinite loops are a failure mode in agentic systems where an agent gets stuck in a cycle of repetitive actions that yield no meaningful progress towards its goal. They can stem from a lack of stop conditions, ongoing errors, or circular reasoning.

**Why it matters**: Infinite loops can lead to excessive costs and unpredictable behavior. Implementing proper termination conditions is vital to prevent them.

**Connections**:
- [[permanent/termination-conditions]]
- [[permanent/progress-vs-spinning]]

**Sources**:
- [[literature/certified-claude-architect-masterclass-2026-section-1-agentic-architecture-foundation]]
