---
title: "Over-Escalation Anti-Pattern"
aliases: ["over-escalation"]
tags: [claude, agentic-architecture, anti-pattern]
created: 2026-06-24
updated: 2026-06-24
---
Over-Escalation is an anti-pattern in agentic systems where triggers are activated too frequently, causing unnecessary friction and hindering legitimate agentic workflows.

## Symptoms
- Escalation on low-stake, reversible tasks.
- Triggers activating based on "gut feel" or minor uncertainty rather than concrete policy violations.
- High volume of human review required for tasks that should be handled autonomously.

## Risk
- **Developer Friction**: Overloading human reviewers leads to alert fatigue.
- **Efficiency Loss**: Undermines the value proposition of agentic automation.

## Connections
- [[permanent/escalation-trigger|Escalation Trigger]]
- [[permanent/silent-failure|Silent Failure]]

## Sources
- [[literature/certified-claude-architect-masterclass-2026-section-4-agentic-architecture-reliability-human-oversight|Agentic Architecture - Reliability & Human Oversight]]
