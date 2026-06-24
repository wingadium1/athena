---
title: "Validation Gate"
aliases: ["validation-gates"]
tags: [claude, agentic-architecture, reliability]
created: 2026-06-24
updated: 2026-06-24
---
Validation Gates are checkpoints within an agentic processing pipeline that inspect outputs against defined criteria before allowing execution to proceed.

## Responsibilities
- **Inspection**: Validate output against schema, range, completeness, and plausibility.
- **Decision**: Trigger appropriate response based on the findings:
    - **Retry**: Attempt the step again if transient failure is detected.
    - **Fallback**: Route to an alternative method.
    - **Escalate**: Surface to human review or higher-level monitoring.

## Types of Checks
- **Pre-Execution**: Ensure inputs are valid, authorized, and within range before the action begins.
- **Post-Execution**: Verify the output satisfies business rules and semantic constraints after the action.
- **Runtime Monitoring**: Continuous assessment of cost, scope, and anomalies during execution.

## Connections
- [[permanent/error-classification-framework|Error Classification Framework]]
- [[permanent/layered-enforcement-architecture|Layered Enforcement Architecture]]

## Sources
- [[literature/certified-claude-architect-masterclass-2026-section-4-agentic-architecture-reliability-human-oversight|Agentic Architecture - Reliability & Human Oversight]]
