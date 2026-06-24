---
title: "Hard Block vs Soft Warning"
aliases: ["hard-block", "soft-warning"]
tags: [claude, agentic-architecture, reliability]
created: 2026-06-24
updated: 2026-06-24
---
Enforcement patterns for validation gates depend on the criticality of the constraint being checked.

## Hard Block
- **Definition**: Rejects output immediately and halts or forces a retry.
- **Use Case**: Non-negotiable constraints (financial limits, permission boundaries, safety-critical actions).

## Soft Warning
- **Definition**: Flags and logs the output but permits execution to continue.
- **Use Case**: Advisory constraints where human judgment adds value or where the violation is not critical enough to halt the pipeline.

## Connections
- [[permanent/validation-gate|Validation Gate]]
- [[permanent/layered-enforcement-architecture|Layered Enforcement Architecture]]

## Sources
- [[literature/certified-claude-architect-masterclass-2026-section-4-agentic-architecture-reliability-human-oversight|Agentic Architecture - Reliability & Human Oversight]]
