---
title: "Layered Enforcement Architecture"
aliases: ["layered-enforcement", "permission model"]
tags: [claude, agentic-architecture, reliability]
created: 2026-06-24
updated: 2026-06-24
---
Layered Enforcement Architecture is a defense-in-depth strategy where no single point of failure compromises the entire agentic system.

## Layers
1. **Prompt Layer**: Provides behavioral guidance and intent; probabilistic.
2. **Pre-Execution Layer**: Ensures inputs are valid before action.
3. **Post-Execution Layer**: Validates results against business rules and detects anomalies before passing downstream.
4. **Runtime Layer**: Continuous monitoring and anomaly detection during action execution.

This depth ensures that if one layer is bypassed or fails, subsequent layers catch the error, enhancing overall system resilience.

## Connections
- [[permanent/validation-gate|Validation Gate]]
- [[permanent/hard-block-soft-warning|Hard Block vs Soft Warning]]

## Sources
- [[literature/certified-claude-architect-masterclass-2026-section-4-agentic-architecture-reliability-human-oversight|Agentic Architecture - Reliability & Human Oversight]]
