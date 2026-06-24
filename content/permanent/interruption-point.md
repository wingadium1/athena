---
title: "Interruption Point"
aliases: ["interruption-points"]
tags: [claude, agentic-architecture, reliability]
created: 2026-06-24
updated: 2026-06-24
---
An Interruption Point is a defined location in an agentic workflow where execution pauses to wait for human input or review.

## Optimal Placement
- **Before Irreversible Actions**: Critical for preventing uncorrectable errors.
- **Phase Boundaries**: Natural points where agent scope or authority shifts.
- **On Anomaly Detection**: When unexpected inputs or behaviors are identified.

## Implementation Principles
- **State Preservation**: Persist task progress and decision branches to external storage (not in-context) to ensure resumption is possible.
- **Structured Handoff**: Messages must provide Context, the specific Decision required, the Consequence of the decision, and a Deadline.
- **Async Handling**: If escalation is asynchronous, the agent must define a timeout threshold and action (abort/fallback) to avoid silent hangs.

## Connections
- [[permanent/human-in-the-loop|Human-in-the-Loop (HITL)]]
- [[permanent/escalation-trigger|Escalation Trigger]]

## Sources
- [[literature/certified-claude-architect-masterclass-2026-section-4-agentic-architecture-reliability-human-oversight|Agentic Architecture - Reliability & Human Oversight]]
