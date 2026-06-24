---
title: "Human-in-the-Loop (HITL)"
aliases: ["hitl"]
tags: [claude, agentic-architecture, reliability]
created: 2026-06-24
updated: 2026-06-24
---
Human-in-the-Loop (HITL) is a deliberate architectural strategy used to mitigate risk by placing human reviewers at specific, defined checkpoints in an agentic workflow.

## Core Principle
- **Not a fallback of last resort**: HITL should be an integrated part of the system design to handle high-stakes decisions.
- **Targeted, not ubiquitous**: Effective HITL is applied to checkpoints where the cost of failure is high or where human judgment is essential to policy alignment, rather than applied to every interaction.

## Connections
- [[permanent/escalation-trigger|Escalation Trigger]]
- [[permanent/layered-enforcement-architecture|Layered Enforcement Architecture]]

## Sources
- [[literature/certified-claude-architect-masterclass-2026-section-4-agentic-architecture-reliability-human-oversight|Agentic Architecture - Reliability & Human Oversight]]
