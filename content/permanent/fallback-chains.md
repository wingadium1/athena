---
title: "Fallback Chains"
aliases: ["fallback-chain"]
tags: [claude, agentic-architecture, reliability]
created: 2026-06-24
updated: 2026-06-24
---
A Fallback Chain is a systematic strategy for managing failures by defining ordered, pre-validated sequences of actions when a primary tool or path fails.

## Core Principle
Move from ad-hoc error handling to a deterministic flow:
1. Attempt primary tool.
2. Attempt pre-validated alternatives or cached results.
3. Escalate to human/system if all alternatives fail.

## Implementation Rules
- **Pre-validation**: All fallback mechanisms MUST be tested under realistic failure conditions; an untested fallback is not a safety net.
- **Explicit Signaling**: Systems must explicitly signal any degradation (e.g., partial results, fallback usage) to downstream consumers to prevent them from treating degraded output as authoritative.
- **Escalation Decision**:
    - Low Risk/Reversible: Degrade and continue.
    - High Risk/Irreversible: Abort and escalate.

## Connections
- [[permanent/validation-gate|Validation Gate]]
- [[permanent/layered-enforcement-architecture|Layered Enforcement Architecture]]

## Sources
- [[literature/certified-claude-architect-masterclass-2026-section-4-agentic-architecture-reliability-human-oversight|Agentic Architecture - Reliability & Human Oversight]]
