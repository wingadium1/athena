---
title: "Escalation Trigger"
aliases: ["escalation-triggers"]
tags: [claude, agentic-architecture, reliability]
created: 2026-06-24
updated: 2026-06-24
---
Escalation Triggers are explicit programmatic conditions that cause an agent to hand off control to a human or a fallback system, rather than continuing to execute a task autonomously.

## Function
- **Preventative**: Stops the agent from executing when failure is imminent (e.g., policy violation) or when the agent is stuck in an ineffective loop.
- **Categorization**:
    - **Hard Triggers**: Based on clear policy violations (e.g., unauthorized access, exceeding financial limits).
    - **Soft Triggers**: Based on heuristics like low confidence scores, unusual anomaly patterns, or ambiguous constraints.

## Implementation
Practical patterns include:
- Counting tool errors/validation rejections.
- Deny lists for prohibited actions.
- Confidence gates (when confidence < threshold).

## Connections
- [[permanent/human-in-the-loop|Human-in-the-Loop (HITL)]]
- [[permanent/over-escalation-anti-pattern|Over-Escalation Anti-Pattern]]

## Sources
- [[literature/certified-claude-architect-masterclass-2026-section-4-agentic-architecture-reliability-human-oversight|Agentic Architecture - Reliability & Human Oversight]]
