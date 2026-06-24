---
title: "Graceful Degradation"
aliases: ["graceful-degradation"]
tags: [claude, agentic-architecture, reliability]
created: 2026-06-24
updated: 2026-06-24
---
Graceful Degradation is the strategy of returning partial or functionally reduced results when a system cannot fulfill the primary request, while explicitly signaling this status to downstream systems.

## Anti-Pattern: Silent Degradation
Returning a degraded result without signaling causes downstream components to operate under false assumptions, leading to cascading failures.

## Implementation Principles
- **Degradation Signals**: Systems MUST propagate status indicators alongside partial data to inform downstream consumers about the reduced quality.
- **Risk-Based Handling**:
    - Low-risk/reversible actions allow for partial/degraded propagation.
    - High-risk/irreversible actions require immediate abort and escalation to human review.

## Connections
- [[permanent/fallback-chains|Fallback Chains]]
- [[permanent/validation-gate|Validation Gate]]

## Sources
- [[literature/certified-claude-architect-masterclass-2026-section-4-agentic-architecture-reliability-human-oversight|Agentic Architecture - Reliability & Human Oversight]]
